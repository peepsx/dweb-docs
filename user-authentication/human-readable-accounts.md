# Human-Readable Accounts
One of ARISEN's major features is the ability to have accounts. Account names can be thought of as usernames which transactions can be sent to, like addresses on other blockchains. They are also human-readable and typically 12 characters long. This is a major difference to the long hashes associated with other blockchain `users`. An account on ARISEN must be created by an already existent account due to the fact that the creator must pay a small RIX (native ARISEN currency) fee to store the newly created account data on the network itself. Every account needs a minimum of 5KB of RAM and 3.0 RIX "staked" for CPU (critical for processing transactions) - which is really nothing when it's all said and done.

There are 2 types of ARISEN accounts: standard and premium. Standard accounts are 12 character long (Latin alphabet and numbers 1-5) and cannot contain a dot. Premium accounts are less than 12 characters and can contain a dot, thus enabling sub-accounts. Premium accounts are only sold via auction and can be thought of as a suffix, in a way a domain ending (TLD) is a suffix, giving the ability to have many names under a central name (jared.dweb). As an example, the @peeps account on ARISEN won the auction for the "dweb" account, which means we can register any account with the `.dweb` suffix. As a result, `.dweb` is one of the dTLDs we offer at [dNames](https://dnames.network).

## Registering An ARISEN Account Via The Web
- https://signup.arisen.network
- https://signup.peepsid.com
- dweb://signup.arisen.network

## Registering An ARISEN Account Via `arisecli`
```shell
arisecli --url https://greatchains.arisennodes.io system new account \
-x 1000 --stake-net "0.1 RIX" --stake-cpu "0.1 RIX" \
--buy-ram-kbytes 4 --transfer existingaccount newaccount \
OWNER_PUBLIC_KEY ACTIVE_PUBLIC_KEY
```

## Creating An Account With [ArisenJS](https://github.com/arisenio/arisenjs)
```javascript
arisenjs = require('arisenjs');
const keyProvider = 'private-key-of-existing-account';
const httpEndpoint = null, chainID  = null;

arisen = arisenjs({httpEndpoint, chainID, keyProvider});
const wif = 'private-key';
const pubkey = 'public-key'; // there are libraries like arisenjs-ecc for generating keys programmatically

arisen.transaction( tr => {
  tr.newaccount({
    creator: 'existingaccount',
    name: 'newaccount',
    owner: pubkey,
    active: pubkey
  });
  tr.buyrambytes({
    payer: 'existingaccount',
    receiver: 'newaccount',
    bytes: 8192
  });
  tr.delegatebw({
    from: 'existingaccount',
    receiver: 'newaccount',
    stake_net_quantity: '10.0000 RIX',
    stake_cpu_quanity: '10.0000 RIX',
    transfer: 0
  });
});
```

## Creating An Account Auction
```shell
arisecli --url https://greatchains.arisennodes.io \
system bidname existingaccount newname "10 RIX"
```

The above command creates an auction for "newname" and starts it off with `10 RIX`.

## Check Auction Status
```shell
arisecli --url https://greatchains.arisennodes.io \
system bidnameinfo newname
```

If you win the auction, you can create sub-accounts of this account, i.e. `jared.newname`.

## Actions & Handlers
Each account on ARISEN has its own private on-chain database associated with it, that can only be accessed by its own action handlers. Action handlers are scripts that send actions from one account to another. ARISEN is able to define smart contracts through the combination of action handlers and automated action handlers. Each account can ultimately send structured "Actions" to other accounts and may define scripts to handle Actions when they are received. To support parallel execution, each ARISEN account can also define any number of scopes within their database. The block producer will schedule transactions in such a way that there is no conflict over memory access to scope and therefore they can be executed in parallel. 

## Having Trouble?
If you're facing issues, join our [Peeps Labs Telegram](https://t.me/peepslabs), where our community developers can help you figure things out!

## What's Next
Learn about account-based permission levels.

[Click here to continue](permission-levels.md)