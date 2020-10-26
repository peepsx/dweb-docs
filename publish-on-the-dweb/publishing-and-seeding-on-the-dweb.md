# Publishing And Seeding On The dWeb
While our example `hello-world` project was published and seeded on the dWeb in the previous step, it's important to understand what else is happening in the background and how we can access our website outside of just the command-line itself. 

## The Network Folder
Within your root dDrive, you'll find the `Network` directory. This is a `virtual directory` (a non-existent directory inside the dDrive), but it provides read-only access to useful information like storage and networking statistics for any dDrive under the daemon's management.

## Global dDrive Paths
For any dDrive being announced on the dWeb's DHT, `~/DDrive/Network/<dweb-key>` will contain the dDrive's contents, which will give us a shareable link with other peers for specific files. For example, if a friend is already peering our `hello-world` dDrive and we are chatting with that friend over an IRC chat, we could easily copy+paste `cat ~/DDrive/Network/<dweb-key>/file.txt` and the contents of the file would appear for everyone in that chat that was peering that particular dDrive. The reason to use the `Network` folder rather than a folder like `Sites` or `Content` (that we created within our root dDrive in previous steps), is that mount folders can vary by name from device to device. The `Network/<dweb-key>` location is available to any peer who has mounted the dDrive referenced by the `dweb-key` in the path. In realistic scenarios, you will probably never use this feature, but it's definitely worth mentioning.

## Storage And Networking Statistics
Within `~/DDrive/Network/Stats/<dweb-key>` you'll find two files:
- `storage.json` - Byte representation of the dDrive's metadata and content feeds (Explained more in `How The dWeb Works`). 
- `networking.json` - Contains network-based information, like the current amount of peers that the dDrive currently has.

_NOTE_: `storage.json` is dynamically computed every time the file is read. If you have a dDrive that contains millions of files, this can be an expensive operation.

## Accessing Via dBrowser
You can also access dDrive that contain websites and web applications via [dBrowser](http://dbrowser.com), by simply typing in `dweb://<dweb-key>`. In the sections that follow, we'll discuss how to point a centralized and decentralized domain to a dDrive, so that domains (`dweb://domain.com` or `dweb://domain.dcom`) can be used to access a dDrive, rather than the 64-character dWeb key itself.

## Having Trouble?
Head on over to the [PeepsLabs Telegram](https://t.me/peepslabs) where you can discuss any potential issues you may be facing with developers in the dWeb community.

## What's Next?
Learn more about how to point a centralized domain name to a dDrive. 

[Click here to continue](pointing-centralized-domains-to-a-ddrive.md)