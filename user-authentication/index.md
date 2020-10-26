# User Authentication
Unquestionably, some applications will want to associate data with a particular user and store/validate the data users submit to these applications. That's where blockchains come into play when developing dWeb-based applications. The [ARISEN](https://arisen.network) blockchain is my blockchain of choice, because it offers:
- `Human Readable Accounts`
- `Smart Contracts`
- `Vanity Permission Levels`
- `Multi-Index Database`
- `Over 300,000 Transactions Per-Second`

Although, it's important that I say that it doesn't matter which blockchain platform you use because dWeb is truly blockchain agnostic. It doesn't care what you use. When it comes to using ARISEN or other blockchains, the goal should NEVER be to store large amounts of data on these platforms. Blockchains simply aren't designed for that. In fact, it's a better idea to simply store a dDrive or a dDatabase discovery key that contains specific data that's associated with the user, rather than storing the actual data `on-chain`. As an example, we're developing a social network called [dSocial](https://dsocial.network), where we use ARISEN for @accounts and validating user interactions on the social network. When a user creates a post, the 140 character post is stored alongside the user `on-chain`, but if media (photos or videos) are attached to a post, they're not stored `on-chain`. Instead, they're placed in a dDrive `off-chain` and the dWeb key for the dDrive is stored alongside the user. This way dSocial can easily locate data associated with a user, without having to bog down the the blockchain by storing the data itself `on-chain` - peers of the dWeb can handle that instead. 

Within this section, I'm going to discuss the ins-and-outs of how ARISEN can be used for user authentication. The following topics are discussed in this section:
- [Human Readable Accounts](human-readable-accounts.md)
- [Permission Levels](permission-levels.md)

**NOTE**: How data can be stored `on-chain` will be discussed in [Smart Contracts](../smart-contracts/index.md), whereas, how data can be stored `off-chain` will be discussed in [Distributed Database Frameworks](../distributed-database-frameworks/index.md).

## Having Trouble
If you're facing issues, please join our [Peeps Labs Telegram](https://t.me/peepslabs) where out community's developers can help you figure things out!

## What's Next?
Learn more about ARISEN accounts and human-readable names.

[Click here to continue](human-readable-accounts.md)