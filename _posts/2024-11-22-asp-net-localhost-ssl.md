---
layout: post
title: "Getting SSL to work for asp.net localhost running on Ubuntu"
date: 2024-11-22 16:15:00 +0530
tag: ssl, asp.net, localhost
---

Recently I had the need to get my browsers to connect to asp.net running on local host. Up till now I had been relying working around this by setting the `allow-insecure-localhost` flag in the browsers that I use (namely Brave and Chromium on Ubuntu 20.04) via: `chrome://flags/#allow-insecure-localhost`. However, following a recent update of these browsers that flag is no longer available.

As a result when my react app was trying to connect to my locally running api I was getting the dreaded `ERR_CERT_AUTHORITY_INVALID` error.

To get this working I needed to do the following:

#### 1. Install `certutil`

The `certutil` tool is part of the NSS (Network Security Services) suite used by browsers like firefox and chromium-based browsers on linux, and is required to add the dev certificate created by asp.net to the system's or browser's trusted certificate store.

According to ChatGPT:
> The `dotnet dev-certs https --trust` command uses `certutil` under the hood to add the certificate to the trusted store for browsers like Firefox. If you’ve already run the command, it should now work without errors if `certutil` is available on the path.

```sh
sudo apt install libnss3-tools
```

#### 2. Run the `dotnet dev-certs` tool again

```
dotnet dev-certs https --trust
```

<br />

### Alternatives considered

Neither of these alternatives are necessary but I'm documenting them just for completeness.

#### Alternative 1 - Use http instead of https

I didn't get very far with this. First, if `app.UseHttpsRedirection()` is called in the api setup code you'll get a CORS error on when it tries to redirect to https similar to:
```
Access to XMLHttpRequest at 'http://localhost:5001/api/User' from origin 'http://localhost:3000' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: Redirect is not allowed for a preflight request.
```

You can prevent that line of code from running locally but then I ran into another issue with Azure B2C authentication:
```
Error: Bearer token authentication is not permitted for non-TLS protected (non-https) URLs.
```

Fair enough.


#### Alternative 2 - Run chrome ignoring certificate errors

You can chromium from the command line with a flag to ignore certificate errors.

```sh
chromium --ignore-certificate-errors
```

It works but you get a "You are using an unsupported command-line flag; --ignore-certificate-errors. Stability and security will suffer" warning banner at the top of the browser window, which is not ideal.


Reference
- https://aka.ms/aspnet/https-trust-dev-cert

