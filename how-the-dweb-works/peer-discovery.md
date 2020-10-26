Now that you understand where dDatabase identifiers and their corresponding peer data is stored and how they're distributed, in this section I'll explain how dDatabases are announced on dWeb's DHT, how they're discovered and how they're downloaded by peers.

## Discovery Keys, Announcements & Lookups
In [dDatabases and dDrives](ddatabases-and-ddrives.md), we discussed how each dDatabase is associated with a keypair, which is used to sign the root hash and therefore validate the authenticity of the data itself. What I didn't discuss is `Discovery Keys` (dWeb keys), which are best described as network identifiers for any type of dDatabase. These network identifiers can also be referred to as `topics` and are the `keys` in dWeb's DHT key-value store.

A dDatabase and its peers are stored in a single entry on the DHT, like so:

```
Key              Value
====            =======
<dweb-key>  [{peer1Host, peer1Port}, {peer2Host, peer2Port},...]
```

This way, peer discovery is as simple as performing a lookup on dWeb's DHT for a particular dWeb key. After traversing the DHT and locating the dWeb key, it will return the peer information (value), associated with the dWeb key (key). 

## An Example Announcement & Lookup Using [@dwebswarm/dht](https://github.com/distributedweb/dht)
```javascript
const dht = require('@dwebswarm/dht');
const crypto = require('crypto');

const node = dht({});
const topic = crypto.randomBytes(32);

node.announce(topic, {port: 88888}, function (err) {
  if (err) throw err;
  
  node.lookup(topic)
    .on('data', console.log)
    .on('end', () => {
      node.unannounce(topic, {port: 88888}, () => {
        node.destroy();
      });
    });
});
```

This outputs the following:
```
{
  node: {host, port}, // DHT node returning data
  peers: [{host, port}, ...], // List of peers seeding this particular dWeb Key
  localPeers: [{host, port}, ...] // List of LAN peers seeding this particular dWeb Key
}
```

## Download Data From Peers
Peers exchange a dDatabase via a messaging protocol called [dDatabase Protocol](https://github.com/distributedweb/ddatabase-protocol). In other words, two or more peers stream data between each other. Perhaps the greatest feature of a dDatabase, is `sparse replication`, which allows a peer to only download the data that they want, rather than the entire log itself. Logs can be downloaded using the `download()` method, can retrieve root hashes of a log using the `rootHashes()` method, can get the associated signature using the `signature()` message and can verify the authenticity of the `lastSignedBlock` and the `signature` using the `verify()` method. You can also audit all of the data in the log using the `audit()` method. 

For more information on these methods and others, please refer to [dDatabase's Documentation](../developer-ecosystem/ddatabase.md) and [dDatabase Protocol's Documentation](../developer-ecosystem/ddatabase-protocol.md).

## Having Trouble?
If you're facing trouble, please join our [Peeps Labs Telegram](https://t.me/peepslabs) and our community of developers can help you figure things out!

## What's Next?
Learn more about how these systems are being used by companies like [Peeps](https://peepsx.com) to reimagine everything.

[Click here to continue](the-sky-is-the-limit.md)