---
layout: post
title: "NoSQL databases, a cautionary tale"
date: 2024-12-14 19:05:00 +0530
tag: NoSQL, CosmosDB
---

TL;DR: For most projects, don't use a NoSQL database unless you really know what you're doing.

I'm currently working with a team that is using CosmosDB, a NoSQL database that runs on Microsoft Azure, and it's not a great technology choice for the team. The way it's being used in this team it's slowing down the pace of development and making the application more prone to future problems than if they'd defaulted to SQL. It's not necessarily a problem with NoSQL itself, which could be a reasonable fit for their application in theory, it's that they're not using it in the right way and it lends itself more easily to be not used in the right way. To use an analogy from 10 pin bowling, if NoSQL is like regular bowling, SQL is like bumper bowls - i.e. there are inherent guardrails in place to make sure you don't do anything stupid.

<p>
<div>
  <img src="/assets/images/why-nosql-is-hard/karla-rivera-TS2_PvGhGgs-unsplash-sm.jpg" alt="Bumper Bowls Image">
  <figcaption>Photo by <a href="https://unsplash.com/@karla_rivera?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Karla Rivera</a> on <a href="https://unsplash.com/photos/bowling-balls-hitting-pins-TS2_PvGhGgs?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a></figcaption>
</div>
</p>

Tbf, they didn't make the initial decision to use it, that decision was made for them by a consultancy. However, it's a cautionary tale that tech decisions really need to take into account the skillset and experience of the team members over what suits the application on paper or is the shiny new thing to an outside party.

So what makes it a bad choice in this situation...

### Joins in application code

The application is a web app with a React frontend talking to an asp.net backend-for-frontend api, which uses CosmosDB as a data store. However, the api is making the cardinal sin of NoSQL of modelling the data as relational data in lots of separate containers (think tables in SQL) rather than trying to embed as much of the data in singular documents. This is what the [section on data modelling]((https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/modeling-data#reference-data)) in the CosmosDB docs has to say about it:

> We don't recommend building systems that would be better suited to a relational database in Azure Cosmos DB, or any other document database

As a result the api code is littered with software joins like the following fictional example. In fact, I'd say this kind of code accounts for about 80% of the code lines.

```csharp
var accounts = accountRepository.GetAll();

var accountIds = accounts
    .GetAll()
    .Select(x => x.Id)
    .ToList();

var invoices = invoiceRepository
    .Get(x => accountIds.contains(x.AccountId));

var invoiceProductIds = invoices
    .SelectMany(x =>
        x.LineItems
            .Select(y => y.ProductId))
    .ToList();

var products = productRepository
    .Get(x => invoiceProductIds.Contains(x => x.Id))
    .ToList();

var invoiceDtos = invoices
    .Select(x => new
    {
        Id = x.Id
        AccountName = accounts.FirstOrDefault(x => x.Id == x.AccountId)?.Name,
        ProductName = products.FirstOrDefault(x => x.Id == x.ProductId)?.Name
    })
    .ToList();

return invoiceDtos;
```

Of course this is the kind of thing SQL was born to do. When combined with an ORM like Entity Framework, none of this code is necessary.

The equivalent code in a single SQL query looks like this:

```sql
SELECT 
    i.Id AS InvoiceId,
    a.Name AS AccountName,
    p.Name AS ProductName
FROM Invoices i
JOIN Accounts a ON i.AccountId = a.Id
JOIN InvoiceLineItems li ON i.Id = li.InvoiceId
JOIN Products p ON li.ProductId = p.Id;
```

In addition, it's making multiple calls across the network to fetch all the data rather than one single call. Of course it's not meant to be this way... NoSQL is supposed to avoid this kind of thing altogether by embedding all the reference data in a single document so that all the data can be read quickly and efficiently in a single transaction. However, that requires taking the time to model the data using NoSQL thinking. If the nature of the data doesn't lend itself to this or it's too complicated given the skillset of the team, then NoSQL is a bad fit.

The developers in this case were still thinking in relational database terms and the result is complex and verbose code that is hard to read, takes longer to write, has more potential points of failure and is slower to run.


### Schema changes

One of the major features of NoSQL, that it is schemaless, is also be a curse if you're not aware of the dangers. A developer on the team renamed a property on the data model that already had records created against it in the database. It was a reasonable change because the property was poorly named in the first place and didn't describe what data it was storing. However, nulls started appearing in the application because all existing records that use the old property name were now broken.

With SQL this isn't a thing. You rename the column and all existing records automatically use the new column. Even if it's a more involved schema change, ORMs like Entity Framework won't let the app run until a migration has been applied to the database to reconcile the data model with the db schema. The bumpers prevent another gutterball.

With NoSQL though, something relatively simple like renaming a property/field requires the developer to be aware that this could break the application for existing records and that they need to either:
- Apply a migration script to update existing records, or
- Create a new version of the data model in the application and write code to map the old version to the new version for historical records

If devs aren't aware of this then it's a recipe for trouble. Having a schema with defined rules gives a less experienced team guard important guardrails that aren't there with a schemaless database.


### Container allocation

Unlike with SQL, where every business entity gets it's own table and no further thought is required, NoSQL, or CosmosDB specifically, requires a bit of thought and forward planning when it comes to containers and partitioning. You could just create a container for every entity like you would a table in SQL but this would mean having to manage throughput requirements separately for each one. This could both cost big $$ and make it hard to manage performance.

Alternatively, you could dump all documents in a single container and partition them by their type. While this might simplify container management it could result in issues like hot parititions, which affect performance and/or cost. It might rule out things like transactions, which require documents to be in the same container. Add to this the fact its difficult to change containers and partition keys once data is written to them and it quickly becomes a headache.
There are of course reasons why these features exist, namely to manage DBs at large scale, but if you don't have those kind of problems why give yourself the headache.

Basically it's not straightforward, and requires some degree of expertise and planning. Without this, what's likely to happen, as it has in this team, is a mish mash of approaches with some entities having their own containers and others ending up in random catch-all containers they clearly don't belong to. As well as making it difficult to find data it's also suboptimal from a cost and performance perspective. With SQL you don't have to deal with any of this.


### Transactions

A final gotcha, with CosmosDB specifically, is that you can only perform transactions, i.e. updating multiple documents in a single atomic transaction, on documents within the same partition. As well as being another reason why you need to give container and partition key selection some thought it's also likely that you'll have to update documents that aren't in the same partiion or even container. This means you'll have to live with not having transactions and either accept the risks or manage this in the application code. While this is not the end of the world and can be worked around it's also a limitation that you don't have to worry about with SQL databases.


### Conclusion

None of this is to say NoSQL or CosmosDB is a bad tech choice for all situations. It has its strengths and it has its weaknesses. However, it is the wrong tech choice for a team that doesn't have the expertise to avoid the pitfalls. In this siutation you can end up getting none of the benefits while incurring all the costs. SQL can also be difficult for an inexperienced team but it's much harder get the ball in the gutter relative to NoSQL.

So to conclude I'd say unless you _really_ know why you're choosing NoSQL then don't choose it. Just play it safe and stick with SQL. Of all the database options its the one most developers know how to use, has great tooling, is simple, safe and fast. Even if you think you know why you're choosing it, if you don't have at least one team member with experience using NoSQL in a production application then also don't choose it. Without that expertise it won't be worth it.
