# How To Publish To The dWeb
Publishing data, content, websites and web applications to the dWeb is a fairly easy task, thanks to tools like [dDrive Daemon](https://github.com/distributedweb/ddrive-daemon) and [dBrowser](https://github.com/dbrowser/dbrowser). In this documentation, I will use `dDrive Daemon` in my examples, although, `dBrowser` does provide a UI on top of the daemon that can be a lot easier to use for entry-level developers or those who simply prefer a UI over a shell-based environment.

Topics in this section:
- [Convert Site Or App To A dDrive](convert-site-or-app-to-a-ddrive-md)
- [Publishing And Seeding On The dWeb](publishing-and-seeding-on-the-dweb.md)
- [Pointing Centralized Domains To A dDrive](pointing-centralized-domains-to-a-ddrive.md)
- [Registering And Pointing A Decentralized Domain](registering-and-pointing-a-decentralized-domain.md)


## Preparing dDrive Daemon
There are a few setup tasks you will need to complete with `dDrive Daemon` before you can publish anything. Remember that you can easily install `dDrive Daemon` using the [Lunar Installer](https://github.com/peepsx/lunar/) as described in [Getting Started](../getting-started.md). If you simply want to install it using NPM, simply run the following:

```
npm install -g ddrive-daemon@latest
```

You can also install directly from GitHub, like so:

```
git clone https://github.com/distributedweb/ddrive-daemon.git
cd ddrive-daemon
npm install -g
```


## Starting The Daemon
`dDrive Daemon` runs as a background persistent process on your PC or server. It uses `PM2` for managing the process, so you can install `PM2` if you would like more control and statistics in regards to the process itself. The daemon is responsible for automatically replicating updates for any of your dDrives to the peers that are `seeding` them and updating any of the dDrive's you are personally seeding locally on your device. 

For the daemon itself to function, you must start it with the following command:

```
ddrive start
```

You should now see a message that shows the following:

```
Daemon started at http://localhost:3101
```

If you would like to stop the daemon, you can run:

```
ddrive stop
```


## FUSE Setup
If you're using MacOS or Linux, you can mount dDrives as directories and interface with them using standard file system syscalls. In order to follow along with this documentation, you will need to setup FUSE. Run the following command to do just that:

```
sudo ddrive fuse-setup
```

**NOTE**: As you can see above, running the `fuse-setup` command requires `SUDO` access. Without it, the FUSE setup would not complete successfully, so make sure you have SUDO access before using `ddrive-daemon`.

To insure that it has completed successfully, run the `status` command as follows:

```
ddrive status
```

This should show the following two items in the output:

```
Fuse Available: true
Fuse Configured: true
```

## Having Trouble? 
Head on over to the [PeepsLabs Telegram](https://t.me/peepslabs) where you can discuss any potential issues you may be facing with developers in the dWeb community.


## What's Next?
Learn how to convert a website or a website application into a dDrive and announce it on the dWeb.

[Click here to continue](convert-site-or-app-to-a-ddrive.md)