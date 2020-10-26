# Pointing Centralized Domains To A dDrive

Using a dWeb key to access a dDrive would be painful for users of the dWeb, especially those who want to access a dDrive that contains a website or web application. Using domain names on the centralized web is a great workaround to having to use IP addresses directly and the same can be said for the dWeb and dWeb keys.

**Important Fact:** dBrowser uses the [dwebx-dns](https://github.com/distributedweb/dwebx-dns) module for resolving a record's dWeb key.

## Creating DNS Records
dBrowser looks for a `TXT` record of the same name that is passed to the dBrowser address bar. For example, if you type in the `dweb://www.dbrowser.com`, a DNS lookup is performed for a `TXT` record with the name `www` for the `dbrowser.com` domain. 

In this case, you would create a DNS record of the `TXT` type, with the name `www` and the RDATA (or host info) for that record would be the following:
`dwebkey=<dweb-key`

It's literally that simple. Now you can access your dWeb-based site using a centralized domain within dBrowser.

## Alternative To DNS
You could also create a folder on the `dbrowser.com` server (where the centralized website is) with details regarding the dWeb version of the website.

First create a folder called `.well-known` in the root folder of the web server. Then create a file called `dweb` in the folder, with the following content:

```
dwebkey=<dweb-key>
TTL=30
```

## Having Trouble?
Head on over to the [PeepsLabs Telegram](https://t.me/peepslabs) where you can discuss any potential issues you may be facing with developers in the dWeb community.

## What's Next?
Learn how to register and point a decentralized domain to a dDrive.

[Click here to continue](registering-and-pointing-a-decentralized-domain.md)