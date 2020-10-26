# Convert Site Or App To A dDrive
First and foremost, all users must have a private "root" dDrive mounted at `~/DDrive`, into which additional sub-dDrives can be mounted and shared with others. `dDrive Daemon` does all of this for you when it is first initialized.  You can think of this root dDrive as the `home` directory on your computer, where you might have Documents, Photos and Video directories. You probably wouldn't want to share your entire `Documents` folder, but you might want to create a mounted dDrive at `Documents/website-files` to share with the world. `dDrive Daemon` allows you to easily accomplish by using a distributed filesystem where some folders and files are shareable and others are not.

## Setting Up Your Root dDrive
When creating a new dDrive, it will be mounted in the `~/DDrive` root dDrive as mentioned above. It is good practice to organize this folder so that you do not mix content, data, websites, apps or seeded dDrives within the same folder.

To create this sort of organization, run the following:

```
cd ~/DDrive \
mkdir Content && mkdir Databases && mkdir Sites && mkdir Apps && mkdir Mnts
```


## Creating A New dDrive For A Website or App
There are two ways to create a shareable dDrive inside your root dDrive:
- `ddrive create [path]` - This will create a new shareable dDrive at `path` (where `path` must be a subdirectory of `~/DDrive`).
- `ddrive mount [path] [dweb-key]` - This will mount a remote peer's dDrive at `path`.

Obviously when creating a new dDrive, the `ddrive create` command must be used.

To start, lets create a new "Hello World" static dWeb-based website by first creating a new dDrive, as follows:

```
ddrive create ~/DDrive/Sites/hello-world
```

This should output the following:

```
Path: /root/DDrive/Sites/hello-world
Key: <dweb-key>
Seeding: true
```


## To Seed Or Not To Seed
Unless you use the `--no-seed` flag, all new dDrives will be seeded publicly by default and announced on the [dWeb DHT](https://dwebx.net).

You can also manually seed the dDrive by using the following command:
`ddrive seed ~/DDrive/Sites/hello-world`


## Mounting From A Remote Device
On a separate device, the dDrive you create can be downloaded and mounted to that device's root dDrive like so:

```
ddrive mount ~/DDrive/hello-world
```

**NOTE:** Keep in mind, there is nothing in this folder currently but we will change that in the next step.

## Creating Our Hello World Website
Within the `hello-world` folder, create a file called `index.html` and place the following in the file:

```
<html>
  <head>
  </head>
  <body>
    <h1>Hello, world!</h1>
  </body>
</html>
```

This is a super exciting website :)


## Live Syncing
The moment you created `index.html`, the remote device that mounted the `hello-world` dDrive should have received this file, almost instantaneously. Like magic right? As I mentioned previously, dDrive Daemon is a persistent process and is constantly replicating the dDrives you create, to the peers who have mounted them on their devices. Those peers and their dDrive Daemon process are constantly checking for new updates from the original dDrive creator, and receive those updates the moment the creator adds new files to the dDrive or makes updates to any of the files that already exist within it.

Now our `hello-world` website has been packaged into a dDrive, announced on the dWeb DHT and can now be accessed by anyone via the dWeb key we received after running the `ddrive create` command. We can use that key in various ways, as we'll discuss over the next several sections.



## Having Trouble?
Head on over to the [PeepsLabs Telegram](https://t.me/peepslabs) where you can discuss any potential issues you may be facing with developers in the dWeb community.



## What's Next?
Learn more about publishing and seeding dDrives.

[Click here to continue](publishing-and-seeding-on-the-dweb.md)