# Getting Started
There are many tools that dWeb developers have to have when developing dWeb-based applications and it can be a pain attempting to install each one individually. For that reason, we have released the `Lunar Installer` which will automatically handle the sometimes complex installation process. Some of the dWeb's utilities are written in JavaScript using the Node.JS framework and others are written in C++. For this reason, if you choose to install all of dWeb's utilities using the "Install All Tools" option, the install could take around an hour to complete, since many of the C++-based utilities like `arisecli`, `aos`, `awalletd` and `arisen-cpp` use Make and are substantial in size. Luckily, Lunar will will give you the option of installing individual utilities. Although, we do recommend that you install all of the tools, if you plan on developing websites or web applications on the dWeb.

## System Requirements
- CURL or WGET
- 8GB of RAM
- 4 vCPUs
- 200GB DISK
- MacOS or Linux

## How To Use The Lunar Installer

You can install Lunar by running the following shell command:

```
wget -qO- https://raw.githubusercontent.com/peepsx/lunar/master/installer.sh | bash
```

The installer will immediately reveal a menu of options, where the first option (1) allows you to install all developer tools and options 2-6 allows you to install individual tools. Simply enter the number (1-6) of the tool(s) you would like to install.


## Having Trouble?
Head on over to the [PeepsLabs Telegram](https://t.me/peepslabs) where you can discuss any potential issues you may be facing with developers in the dWeb community.


## What's Next?
Learn how to publish content, data, websites and web applications on the dWeb. [Click here to continue](publish-to-the-dweb/index.md).