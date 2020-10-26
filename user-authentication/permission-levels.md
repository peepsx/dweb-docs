# Permission Levels
ARISEN uses `role-based permission management` to determine if account-based `Actions` are authorized. ARISEN provides a "declarative permission management system" that gives accounts high-level control over who can do what and when. It is critical that authentication and permission management be standardized and separated from the business logic of an application, which enables the development of tools that manage permissions, generally, while also giving way to a significant boost in performance. An account can be controlled by multiple other accounts at once. Each one of these "controlling accounts" can have different weighted permissions, so that accounts have "multi-user control". Every account can be controlled by any weighted combination of other accounts and private keys. This ultimately forms a hierarchical authority structure that reflects how permissions are organized in reality and makes multi-user control easier than ever. Multi-user control is the single biggest contributor to security and when used properly, can greatly reduce the risk of theft due to hacking. ARISEN's software allows accounts to decide which keys and accounts are allowed to send a particular `Action` type to another account. For example, it's possible to have one key for a user's social media account and another for access to a decentralized exchange dApp. You could also give one account the ability to act on behalf of another account, without assigning a single key.

## Vanity Permission Levels (VPLs)
ARISEN accounts are capable of creating `vanity permission levels` whose origins are that of higher-level vanity permissions. Each vanity permission or VPL for short, must define an `authority`. Authorities on ARISEN are essentially `multi-signature threshold checks` that are comprised of keys and/or VPLs of other ARISEN accounts. For example, an ARISEN account could have a VPL known as "Friend", that could be correlated to a specific `Action` on the account and could be mutually controlled by any of the account's friends. The Steem blockchain (home of the Steemit social network) has three hard-coded VPLs known as `owner`, `active` and `posting`, where the `posting` VPL can only perform social actions like voting or posting, whereas the `active` VPL can do anything but change the `owner` permission, which can ultimately do anything. ARISEN allows users to create their own custom-named (vanity) permission levels, by allowing account holders to define their own hierarchy of permissions, as well as the customized grouping of `Actions`.

## Permission Mapping
With ARISEN, each account is able to outline a specific `mapping` between a contract/action or contract of any other ARISEN account, along with their own VPL. A user using a social media dApp on ARISEN, could `map` his/her social media application to the `Friend` VPL. This would allow his/her friends to post on behalf of this user, to his/her social media page. While the friend's post still appears as the account holder, they would still be identified by their keys that were ultimately used to sign the `Action`, therefore, you could easily identify users who are abusing their permissions.

## Default Permission Levels (DPLs)
ARISEN has two default permission levels or DPLs. The DPLs are known as `owner` and `active`. The `owner` permission can do anything on the network, while the `active` DPL can do anything except change the `owner` DPL (similar to Steem). VPLs derive from the `active` DPL.

## Evaluating Permission Levels
If a VPL is used, which doesn't exist, the `active` DPL is automatically used to authorize an `Action`. Once a mapping is determined, then the signing authority is validated using the threshold multi-signature process, as well as the authority associated with the VPL. If this is unsuccessful, it traverses up to the `active` DPL and ultimately the `owner` DPL. 

## Parallel Evaluation Of Permissions
The process behind the permissions evaluation process is immutable, where transactions based around the change of permissions, are not completed until the end of the block, so that all key/permission evaluations for every transaction can be completed in parallel, the quick validation of permissions is possible without costly contract executions (application logic) that would ultimately be rolled back and that transaction permissions can be evaluated as pending transactions are received, so they don't need to be re-evaluated when they're confirmed. Parallelizing the permissions process, while making it "read-only", dramatically increases performances on the network, because of the significant percentage of computation that is required to validate permission-based transactions. To add to this, when `replaying` the blockchain from a log of `Actions`, the permissions to not need to be re-evaluated. This also significantly lowers the computational load brought by the replaying of a constantly growing blockchain.

## Actions With Mandatory Delay
ARISEN enables application developers to allow for certain `Actions` to be delayed before being applied in a block. This is so these delayed `Actions` can be ultimately cancelled. EOS.IO first introduced this security feature, so that `Actions` happening on an immutable blockchain could be reversed before they became `read-only` within a network's log of `Actions`. For example, developers could allow a user to receive notice via email or text message that a specific action occurred so that the user could retract that `Action`. The specific amount of time surrounding this specific time period, could be seconds or years. The specific delay period is determined by the application developer.

## Stolen Key Recovery
ARISEN allows users to restore control of their ARISEN account when their keys are stolen. An account owner can use the stolen `owner` key along with approval of a designated `account recovery partner`. It is important to note that the account recovery partner cannot reset control of the account without the help of the user that possesses the `owner` key for the stolen account. Even if a hacker went through this process, it would be pointless, due to the fact that they already control the account, although, even if the hacker did go through the process - the `account recovery partner` would probably demand some sort of two-factor authentication (phone and email). That is, if the account recovery partner is a dApp itself. A recovery partner has no authority over day-to-day transactions, rather, the recovery partner is only a `party` to the recovery process. This will ultimately reduce legal liabilities, costs and lost currency that would be unrecoverable on blockchains that lack this sort of feature. 


## Creating Custom Permission Levels For Specific Actions
```shell
arisecli -u https://greatchains.arisennodes.io \
set action permission account contractName actionName permissionName \
-p account@permissionName
``` 

## Set New Key To Permission
```shell
arisecli set account permission testaccount active PUBLIC_KEY owner -p testaccount
```

## Set An Account As Authority For Permission
```shell
arisecli set account permission testaccount active diffaccount \
owner -p testaccount@owner
```

## Weight/Thresholds
```shell
arisecli set account permission testaccount active '{"threshold": 100, "keys": [], "accounts": [{"permission": {"actor": "user1", "permission":"active"}, "weight": 25}, {"permission": {"actor": "user2", "permission":"active"}, "weight": 75}]}' owner -p testaccount@owner
```

## Having Trouble?
If you're facing issues, join the [Peeps Labs Telegram](https://t.me/peepslabs) where our community developers can help you figure things out!

## What's Next?
Learn about authenticating users with ARISEN.

[Click here to continue](authenticating-users.md)