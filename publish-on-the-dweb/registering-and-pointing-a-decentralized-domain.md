# Registering & Pointing A Decentralized Domain To A dDrive
Centralized domains do have issues, one of which is the fact that they can be taken away by the registrar for political reasons. That's why we invented dDNS and created decentralized top-level domains (dTLDs) for use with the dWeb and dDrives themselves. Registering a decentralized domain and creating dDNS records is fairly simple. To continue, you will need to install `ddns-cli` by running `npm install @ddns/cli@latest`. You can also install `ddns-cli` using the `Lunar Installer` via the [Getting Started](../getting-started.md) section.

## Register A Decentralized Domain
In order to register a domain, go to [dNames](https://dnames.network), where the process should be pretty straight forward. Each domain has two permission levels (`owner` and `active`) by default, each of which are associated with their own keypair. The dName registration system will automatically generate a 12-word mnemonic phrase which is used to derive a keypair for each permission level. You will need to save both the mnemonic phrase and both keypairs in a safe place, as you will use them for many things, including registering a NameDrive with dWeb's `Root dDNS System`.

## Creating A NameDrive
A `NameDrive` is the dWeb's decentralized alternative to a "NameServer". To create a `NameDrive` or `ND` for short, you need to run the following:

`ddns creatend`

This will prompt you to enter a `name` for the `ND`. This will output the dWeb key for the NameDrive (since a NameDrive is simply a dDrive that contains domain databases), which we will use in a later step when registering the `ND` with the `Root System`.

**NOTE:** You may notice that your newly created `ND` is now being managed by `dDrive Daemon`and can also be accessed via dBrowser. That's because `ddns-cli` uses dDrive Daemon's Node.JS client to communicate with the daemon when creating an `ND`. 

## Adding A Domain To The NameDrive
To add your newly registered domain to your NameDrive, simply run:

`ddns adddomain`

This will prompt you to enter the following:
- `ND Name` - Name of the ND where domain will be added.
- `Domain Name` - Domain being added to the ND (i.e `domain.dcom`)

## Adding A Record To The Domain
To add a record to the domain, simply run:

`ddns addrecord`

This will prompt you to enter the following:
- `ND Name` - ND where domain is
- `Domain` - Domain that record is being added to
- `Record Name` - Name of record, i.e `www`
- `RData` - dWeb key associated with this record
- `Type` - Record type, i.e `D` (like an A record on the centralized web)
- `Class` - Record class (almost always will be `DW`)
- `TTL` - Time to live (i.e `30`)

**NOTE:** For more information on `Record Types`, read the [dDNS Specification](https://github.com/peepsx/ddns-whitepaper).

## Registering A NameDrive
You will now need to register your newly created `ND` with the dWeb's `Root dDNS System`. To do that, you will need to install `arisecli`. You  can easily install `arisecli` using the `Lunar Installer`. Once installed, you will need to create a keystore for your domain's keys. To do that, run the following in a `screen`:

`awalletd --http-server-address=0.0.0.0:12618`

Then create the keystore outside the screen:

`arisecli wallet create`

**NOTE:** This will output a long key-like password that you need to save. It is required to lock/unlock the keystore. 

Then you need to import both the `owner` and `active` private key for the domain into the keystore, like so:

```
arisecli wallet import --private-key <owner-private-key>
arisecli wallet import --private-key <active-private-key>
```

Then run the following command to register your `ND`:

```
arisecli --url https://greatchains.arisennodes.io push action ddns regnd '["domain.dcom", "nd1", "<dweb-key>"]' -p domain.dcom@active
```

In the above command, I am registering `nd1.domain.dcom` and pointing it to the `dweb-key` for the NameDrive I created with `ddns-cli`. 

**NOTE:** The name I gave to the `ND` using `ddns-cli` does not have to be the same name provided to the `Root dDNS System`, since dDrive names are only used locally.

## Accessing The Domain
You should now be able to access the record you created with `ddns-cli` via `dBrowser` by simply typing in `dweb://domain.dcom`. For more information on how dBrowser performs `ND` lookups, please read the [dDNS Specification](https://github.com/peepsx/ddns-whitepaper).

## Having Trouble?
Head on over to the [PeepsLabs](https://t.me/peepslabs) where you can discuss any potential issues you may be facing with developers in the dWeb community. 

## What's Next?
Learn how the dWeb works inside and out.

[Click here to continue](../how-the-dweb-works/index.md)