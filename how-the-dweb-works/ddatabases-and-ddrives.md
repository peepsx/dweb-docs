# dDatabases and dDrives
I like to say that the dWeb is made up of dDatabases and dDrives because it is, but in a way it's technically misleading. Technically speaking, the dWeb is made up of dDatabases, because a dDrive is literally an abstraction built on top of a dDatabase. In fact, as you will see in the [Distributed Database Frameworks](../distributed-database-frameworks/index.md) section, there are many different abstractions built on top of the dDatabase protocols, from distributed database management systems to distributed file systems. Essentially, everything you see distributed between peers on the dWeb is crammed into a single append-only log known as a dDatabase. As an example, a dDrive stores its files, their metadata and content as log entries in a dDatabase. 

A pseudo-representation of this would look like this:

```
Key                                     Value
====                                   =========
index.html                          {content:010110011110...}, {metadata: 1110100101...}
img/logo.png                      {content: 11010011110...}, {metadata: 0100110001...}
```

This way an entire file system can be moved efficiently between peers using a single file (the dDatabase itself). The `dDatabase Protocol` is designed in such a way, that the creator of a dDatabase can replicate a log across multiple peers in an instant. When the creator appends data to the log, those who are peering it, automatically receive the updated log. Even if a dDatabase creator stops broadcasting the log, as long as it is being peered by others, it can still be accessed - although the creator is the only peer who can append data to it. In other words, once the creator stops peering it or loses the cryptographic keys required to write to it, it in essence because an `archive`. It's important to note that you can fork any dDatabase or dDatabase-based abstraction as your own, which means you can edit it but it will be distributed under an entirely different dWeb key than the original.

If you simply think of the data on the dWeb as a bunch of append-only logs that are constantly replicated across the people who want to consume said logs, you will grasp how it works at a very basic level, but it is much more complex than that when you get down to the lower-level guts of the protocol, which I will describe in-depth, below.

## Merkle Trees
The dDatabase uses a custom encoding method when laying out dDatabase-based update logs into a Merkle tree. This particular encoding method positions hashes into a scheme called "binary in-order interval numbering" or just "bin" numbering for short. This is a deterministic method of positioning leaf-nodes in a Merkle tree. 

A Merkle tree with seven leaf nodes will always be laid out like this:

```
0L
     1L
2L
          3L
4L
     5L
6L
```

From a computer science perspective, dDatabases are binary append-only streams, the contents of which are cryptographically hashed and signed. Therefore, any dDatabase can be verified by anyone with access to the public key of the creator. Over the HTTP protocol, datasets are shared everyday, although, there is no built-in support for version control or the content-addressing of data. dDatabase became the solution for this, allowing multiple untrusted devices to act as a single virtual host. For the dWeb to work, it requires a data structure which authenticates the content's integrity and one that keeps a historical log of revisions - and dDatabase, as well as its protocol, certainly provide that.

## Append-Only Data Feed
A dDatabase could be seen as an append-only data feed, where the data within a data feed is an abstract "blob" of data.

**dDatabase feeds can:**
- Be distributed partially over the dWeb
- Be distributed fully over the network
- Be distributed to/received by multiple peers at once

dDatabases are identified internally by signed Merkle trees and identified publicly over the dWeb by a public key. A dDatabase's public key is used to verify the signature that relates to the received data. A dDatabase's internal Merkle tree is output as a "Flat In-Order Tree" or "FIO Tree".

## FIO Trees
FIO trees, per PP5P RFC 7574 are defined as "Bin numbers". These FIO trees allow for numerical-based identification of each leaf node within a binary-based Merkle tree and therefore creates a simplistic way of representing a binary tree as a list. These properties of FIO trees are used in the simplification of "wire protocols" (like NOISE) that use Merkle tree structures within distributed applications.

Below is an example FIO tree that is sized at 4 blocks of data:
```
0L
1P
2L
3P
4L
5P
6L
```

In the above FIO tree example, even numbers (0,2,4,6) represent leaf nodes on the tree and odd numbers represent parent-nodes that each contain two children. Using binary notation, we can count the total number of "trailing 1s" to calculate the depth of the tree's nodes. For example, the following numbers below are converted to binary:

```
5 = 101
3 = 011
4 = 100
```

The number 5 has one trailing 1, the number 3 has two trailing 1s and the number 4 has zero trailing 1s. In the FIO tree example, 1 is the parent node of (0, 2), 5 is the parent node of (4, 6) and 3 is the parent of (1, 5). The FIO tree would only have a single root if the leaf node count is a power of 2, otherwise a FIO tree will always have more than one root. 

Below is another FIO tree (pseudo-representation) with a total of 6 leaf-nodes:
```
0
1
2
3
4
5
6
7
8
9
10
```

In the above examples, the roots are 3 and 9.

## More About Merkle Trees
A Merkle tree is best described as a tree of binary-based data, where every "leaf" (an even-numbered tree node that has no children) is a hash of a data block and every "parent" (an odd-numbered tree node that has two children) is the hash of both of its children. dDatabases are feeds represented by Merkle trees that are ultimately encoded with "bin numbers".

For example, a dDatabase that consists of 4 values would always map to 0, 2, 4 and 6 leaf nodes. Below is the pseudo-representation:
```
parent0 => leaf0
parent1 => leaf2 
parent2 => leaf4
parent3 => leaf6
```

When converting to a FIO tree-style notation, a Merkle tree spanning these data blocks looks like:

```
0 = hash(0+2)
2 = hash(parent1)
3 = hash(1+5)
4 = hash(parent2)
5 = hash(4+6)
6 = hash(parent3)
```

The even and odd nodes store different types of information:
- `Even Numbers` - List of data hashes [d0, d1, d2...]
- `Odd Numbers` - List of Merkle hashes (hashes of child even nodes) [h0, h1, h2...]

The root node within a Merkle tree hashes the entire dataset. In the example above, Node #3 hashes the entire dataset and Node #3 is used to verify the rest of the dataset. Although, the root node will change every time data is added to a dDatabase.

```
0
1
2
3 (root node)
4
5
6
```

FIO-based Merkle trees can have multiple roots. If we expand to six leafs from four, this will ultimately result in two root nodes:
```
0
1
2
3 (root node)
4
5
6
7
8
9 (root node)
10
```

The Merkle tree's nodes are calculated as follows:
```
0 = hash(leaf0)
1 = hash(0+2)
2 = hash(leaf1)
3 = hash(1+5)
4 = hash(leaf2)
5 = hash(4+6)
6 = hash(leaf3)
8 = hash(leaf4)
9 = hash(8+10)
10 = hash(leaf5)
```

When there are multiple root hashes, it is convenient to capture the entire state of a dDatabase as a `fixed-sized hash` by hashing all of the root hashes into one single hash, where in the example above:

`root = hash(9+3)`

or 

`tr = h(r1+r2)`
- Key:  (tr = top root; h = hash; r1 = root1; r2 = root2)

## Root Hash Signatures
Merkle trees are utilized by a dDatabase to create a way of identifying the content of a dataset through hashes. The concept is simple, if the underlying content of a dDatabase changes, the hash changes. For example, a dDatabase acts as a list that calls the `append()` mutation, when an entry is added to the dDatabase feed, thereby adding a new leaf to the tree, which ultimately generates a new root hash. 

When a dDatabase is created, a keypair is generated. The public key is used as a public identifier for data addressability, whereas the private key is used to sign the root hash, every time a new one is generated. The signature is always distributed with the root hash to verify its integrity. 

## Data Validation
Data that is received belongs to a dDatabase goes through the following process:
1. The root hash's signature is verified
2. The received data is hashed with `ancestor hashes` in order to reproduce the `root hash`.
3. If the root hash that was calculated is an exact match of the signed root hash that was received, then the data has been verified.

In an example of a dDatabase containing 4 values, our tree of hashes would look like this:
```
0
1
2
3 (root node)
4
5
6
```

If we want to verify the data for 0 (leaf0), we first need (2), which is the sibling hash, (5) which is the uncle hash, and (3) which is the signed root hash. 

```
0 = hash(leaf0)
2 = (hash received)
1 = hash(0+2)
5 = (hash received)
3 = hash(1+5)
```

IF what we calculate for 3 is equal to the signed root hash we received for 3, then leaf0 is valid. It's important to note that all new signatures verify the entire dDatabase, since the signature spans all data in the Merkle tree. A dDatabase is considered corrupt if a signed mutation results in a conflict against previously-verified trees. When a dDatabase is considered corrupt, the `dDatabase Protocol` is designed to stop distribution.

## Cryptography Specifications
- The hash function uses BLAKE2B-256 encryption, while signatures are ed25519 with the SHA-512 hash function.
- Hash function inputs are prefixed with different "constants" based on the type of data being hashed. These constants include:
- `0x00` - Leaf
- `0x01` - Parent
- `0x02` - Root

This protects against a "second preimage attack".

- Hashes a lot of the time will include the sizes and indexes of their content, so that the structure of a tree can be described along with the tree's data. 

## Data Replication
The easiest way to describe how dDatabase replication works with pseudo-code examples, is in the content of a file system. So we'll use the dDrive abstraction in the following examples.

Let's say we create a dDrive and add two files to it:
- `jefferson.png`
- `trump.png`

Once these files are added to the dDrive, they are split into fragments and represented within a constructed data file, along with the metadata for both files. Assuming `jefferson.png` and `trump.png` are both split into three fragments apiece that each equate to a file size of 124KB, the representation would be placed in a list like this:

```
jefferson-1.png
jefferson-2.png
jefferson-3.png
trump-1.png
trump-2.png
trump-3.png
```

These fragments of the two image files each get hashed and those hashes are organized into a Merkle tree, like the example below:

```
0L               - hash(jefferson-1)
     1L          - hash(0L + 2L)
2L               - hash(jefferson-2)
          3L     - hash(1L + 5L)
4L               - hash(jefferson-3)
     5L          - hash(4L + 6L)
6L               - hash(trump-1)
8L               - hash(trump-2)
     9L          - hash(8L + 10L)
10L             - hash(trump-3)
```

The next part of the process is to calculate the root hashes of our Merkle tree, which are `3L` and `9L`, then hash the number that derives from that. Lastly, we will cryptographically sign the `combined root hash` (sH). The signed hash is utilized to validate all other leaf nodes (parent/child) within the tree, where the signature easily proves who this dDrive was publicly by.

The pseudo-tree above is for the hashes that derive from a representation of the files and their metadata, known as `metadata.log`.

An example of a metadata log-based Merkle tree is below:
```
0L - hash(contentLog: {'4f21f567...'})
1L - hash(0+2)
2L - hash({"jefferson.png", first:0, length:3})
4L - hash({"trump.png", first:3, length:3})
```

A few notes about the above pseudo-representation of the metadata log's Merkle tree:
- The 1st entry in this log is the metadata entry pointing to the dWeb key of the second log (the contentLog that the first Merkle tree represented).
- Note that the 3rd Leaf Node is not included. That's because 3 is the hash of 1+5 and 5 does not exist yet, so it will be written at a later update.

All that's left in the replication process is to send the metadata to the other device (peer). The replicating peer-to-peer low-level messaging protocol is tasked with communicating between peers over what is known as a `duplex binary channel`. Below is a play-by-play of the communication between both peers (Bob and Alice):
- 1. Bob sends the first message, known as the `Announce` message, that includes a Discovery key (which we will discuss further in the next topic) that derives from the dDatabase's public key.
- 2. Alice sends Bob an `Ask` message, signaling that Alice wants all Leaf nodes in the metadata log of the dDrive Bob is announcing.
- 3. Alice sends three `Pull` messages, one for each leaf node (0, 2, 4).
- 4. Bob sends back 3 `Data` messages. The data messages contain the `contentLog` key, the hash of the sibling which in this case is (2), the hash of the uncle root (4), as well as the signatures for the root hashes (in this case - 1,4).
- 5. Alice can validate the integrity of the first data message by hashing the metadata received for the `contentLog` metadata to produce the hash for `leaf0`. They then hash `leaf0` with the hash (2) that was included to reproduce hash (1) and hashes (1) with the value of (4). Lastly the digital signature that was received is used in order to verify it was the same dDrive that was originally requested.
- 6. When the next `Data` message is received, a similar replication process to the one shown above is performed to verify the content of the dDrive again. Alice now has a full list of the files in the dDrive and can choose which ones she wants to download.

## The Simplicity Of A Log
Remember, a dDrive is a custom abstraction built on top of a dDatabase, where it stores the content and metadata of the files within it, within a Merkle tree-based representation. It's important to understand that a dDatabase is format agnostic and can hold something as simple as:

```
Key                Value
====              =====
Hello              World
```

As crazy as it sounds, the dWeb is simply a bunch of peers exchanging logs and validating that the actual data received, derived from the signer of the data itself (the dDatabase creator).

## dDrives
A dDatabase as mentioned previously is format agnostic, meaning it will accept any kind of input data. Therefore it will continue to function with any sort of streamed binary data. A dDrive uses a dDatabase to store a binary-based representation of a set of files. The beauty of using an append-only log for this enables a dDrive to keep every version, of every file. Since each binary-based entry in a dDatabase is cryptographically hashed, signed and can be verified by any consumer who has the public key, the same goes for a dDrive.

A dDrive uses two logs known as `content` and `metadata`, where the content log contains the files within a dDrive, while the metadata log contains the name, size, last modified time and other file-related metadata. This ultimately creates an entire file system structure. Both the content and metadata logs are replicated when syncing between dWeb peers. It is also important to understand that the dDatabase protocol is able to fully or partially sync streaming binary data in a distributed system, even if the stream is being appended to. This is accomplished by utilizing a P2P messaging protocol to traverse the Merkle tree of remote dDatabases and to fetch a strategic set of leaf nodes. Due to the low-level, message-oriented design of the replication protocol, different node traversal strategies can be implemented.

### Metadata Versioning
When adding a file to a dDrive, dDrive uses a sorted, depth-first recursion to list all the files in the file system tree. For each file dDrive finds, it fetches the file system metadata (file name, Stat object, etc.) and performs a check if a filename with the precise metadata already exists in the dDrive's dDatabase-based metadata log. If the metadata is different than any of the metadata currently being stored, then a new metadata entry will be appended to the metadata log as the "newest" version of that particular file. 

### File Versioning
While a dDrive is designed to store metadata by keeping a historical record of the dDrive file system itself, but it does not store metadata in this format by default. Unlike protocols like GIT, dDrive does not store a copy of every possible iteration of a file - but it can. dDrive utilizes several storage modes, where instead of using `metadata.log`, it can use `content.log` instead, where a copy of every single file is kept from the time the dDrive was created. This way, a dDrive can be referenced by a version. Each time the dDrive is updated, this state of the dDrive itself is considered as a "version". For example, the first time you publish a dDrive, the version would be `1`. What is great about this, is that past versions of a website can be shown by simply referencing the version within dBrowser's address bar, like so: `dweb://<key>?version=4`.

## The Big Picture
It's important that the dDatabase or dDrive you receive is the exact version that you requested. Malicious peers on a distributed network could in fact send information that is entirely different than the information you had originally requested. The dDatabase protocol places the importance of integrity, as it applies to data, above anything that's distributed over the protocol itself. Being able to verify that you're receiving exactly what you requested is paramount in a protocol that is based around distributed systems like the dWeb. This starts with a consumer being able to reference specific versions of a website, web application or file, by way of hash, which should ultimately derive from the content of the website or web application itself.

dWeb can easily transport massive file hierarchies and datasets while doing so at global scale. The restrictions brought on by web host-related resource limits are no longer a limitation placed on developers, considering dWeb's protocol is based entirely around a distributed peer-to-peer network architecture. 

## Having Trouble?
If you're facing issues, please join our [Peeps Labs Telegram](https://t.me/peepslabs) where our community's developers can help you figure things out!

## What's Next?
Learn about how a dDatabase or dDrive are announced on dWeb's distributed hash table (DHT). 

[Click here to continue](dweb-dht.md)