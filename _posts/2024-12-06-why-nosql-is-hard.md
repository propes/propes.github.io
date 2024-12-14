---
layout: post
title: "NoSQL databases, a cautionary tale"
date: 2024-12-14 19:05:00 +0530
tag: NoSQL, CosmosDB
---

Don't use a NoSQL database unless you really know what you're doing.

I'm currently working with a team that is using CosmosDB, a NoSQL database that runs on Microsoft Azure, and it's not a great technology choice for the team. In the way it's being used it's slowing the pace of development and making the application more prone to future problems that if they'd stuck with something like SQL server. It's not necessarily a problem with CosmosDB itself, which could still be a reasonable theoretical fit for the application they're working, it's that they're not using it in the right way and getting caught out by gotchas that come with a lack of experience with NoSQL databases and the different thinking required to use it effectively.

Tbf, they didn't make the initial decision to use it, that decision was made for them by a previous consultancy. However, it's a cautionary tale that tech stack decisions really need to match the skillset and experience of the team members and not just the needs of the application as suggested by an outside party.

So what are some of the issues I'm seeing...

### Joins in application code

The application itself is a web app with a React frontend talking to an asp.net backend-for-frontend api, which uses CosmosDB as a data store. The api code is littered with code like the following entirely fictional example:

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

In other words, in-memory document joins in application code. Something that SQL eats for breakfast any day of the week and, when combined with an ORM like Entity Framework, gracefully and in a highly optimised fashion makes these kind of problems vanish.

In fact it's fair to say that most of the code mileage, and complexity, in the backend is made up by these kind of joins. And of course it's making multiple calls across the network to fetch all the data rather than one single call. Of course it's not meant to be this way... NoSQL is supposed to avoid this kind of thing altogether by embedding all the reference data in a single document so that all the data can be read quickly and efficiently in a single transaction. However, for that to work it requires a commitment to the NoSQL way of thinking. The developers here are clearly still thinking in relational database terms and the result is complex and verbose code that takes longer to write, is harder to get right and is unperformant.


### Schema changes

What is a feature of NoSQL, that it is schemaless, can also be a curse if you're not aware of the dangers. A developer on the team renamed a property in the data model that is persisted to the database. This was a reasonable change because the property name didn't reflect accurately what the data the property was storing. However, nulls started appearing in the application because all existing records that use the old property name are now broken.

With SQL this isn't a thing. You rename the column and all existing records automatically use the new column. Even if it's a more involved schema change, ORMs like Entity Framework won't let the app run until a migration has been applied to the database to reconcile the data model with the db schema.

With NoSQL though, something relatively simple like renaming a property/field requires the developer to be aware that this could break the application for existing records and that they need to either:
- Apply a migration script to update existing records, or
- Create a new version of the data model in the application and write code to map the old version to the new version for historical records

If devs aren't aware of this then it's a recipe for trouble. Having a schema with defined rules gives a less experienced team guard important guardrails that aren't there with a schemaless database.


### Container allocation

Unlike with SQL, where every business entity gets it's own table and no further thought is required, NoSQL, or CosmosDB specifically requires a bit of thought and forward planning when it comes to containers and partitioning. You could just create a container for every entity just like you would have a table for every entity in SQL but this will mean having many containers and having to manage throughput requirements separately for each one. This could make it both expensive and make it hard to manage performance.

Alternatively, you could dump all documents in a single container and partition them by their type. While this might simplify container management it could result in issues like hot parititions, which affect performance and/or cost. There also might be scenarios where it doesn't make sense to use the entity type as the partition key because you want to partition on another property, e.g. to take advantage of transactions.

The point is that it's not straightforward, and requires some degree of expertise and planning. It's also difficult to change containers and partition keys once they're in prod. Without this, what's likely to happen, as it has in this team, is a mish mash of approaches with some entities having their own containers and others ending up in random catch-all containers they clearly don't belong to. As well as making it difficult to find data it's also suboptimal from a cost and performance perspective. With SQL all of these problems disappear.


### Transactions

Another gotcha with CosmosDB specifically is that you can only perform transactions, i.e. updating multiple documents in a single atomic transaction, on documents within the same partition. As well being another reason why you need to give container and partition key selection some thought it's also likely that you'll have to update documents that aren't in the same partiion or even container. This means you'll have to live with not having transactions and either accept the risks or manage this in the application layer. While this is not the end of the world and can be worked around it's also a limitation that SQL databases don't have.


### Conclusion

None of this is to say NoSQL or CosmosDB is a bad tech choice in all situations. It has its strengths and it has its weaknesses. However, it is the wrong tech choice for a team that doesn't have the experience to avoid the pitfalls. In this case you end up getting none of the benefits while suffering all the costs. SQL can also be difficult for an inexperienced team but it's much harder to kick own goals relative to NoSQL.

So to conclude I'd say unless you _really_ know why you're choosing NoSQL then don't choose it. Just play it safe and stick with SQL. Of all the database options its the one most developers know how to use, has great tooling, is simple, safe and fast. I'd even go as far as to say if you don't have at least one team member with experience using NoSQL in a production environment then also don't use it. Without that expertise it won't be worth it.
