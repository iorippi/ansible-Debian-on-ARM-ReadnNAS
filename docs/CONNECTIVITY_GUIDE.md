# Connectivity Guide
This document covers all different ways required to communicate with NAS server.

**[Serial connection](#serial-connection)**
1. [Setting up for serial connection](#1-setting-up-for-serial-connection)
2. [Getting into bootloader via SERIAL connection](#2-getting-into-bootloader-via-serial-connection)
3. [Getting into Linux via SERIAL connection](#3-getting-into-linux-via-serial-connection)

**[SSH conenction](#ssh-connection)**
1. [Setting up for SSH connection](#1-setting-up-for-ssh-connection)
2. [Gettign into Linux via SSH connection](#2-gettign-into-linux-via-ssh-connection)

---

## Serial connection
### 1. Setting up for serial connection

ReadyNAS RN102 and RN104's pinout is, from top-left to bottom-right, 
RX, TX, GND and +3.3V.

```
------------------------
|  RX   |  TX          | 
------------------------
|  GND  |  VCC (3.3V)  |
------------------------
```

**Baud rate** is at `115200`

Note that if you're using other hardware other than RN102 and RN104, this pinout mapping may not be relevant. To avoid the risk of damaging the component.

Source:[Serial connection to the ReadyNAS 102 | Roberto's blog](https://blog.rebootr.nl/serial-connection-readynas-102/) and [connexion serie readynas 104 | Netgear community](https://community.netgear.com/discussions/fr-business-solutions-insight-forum/connexion-serie-readynas-104/2398573)

#### Example on Linux / macOS
`[tio](https://github.com/tio/tio)`
```
$ tio -l
> Find path to your serial connectivity

$ tio <path>
# ex: tio /dev/usbserial-A5069RR4
```

or `screen`
```
$ screen <path> <baud rate>
# ex: screen /dev/usbserial-A5069RR4 115200
```

See also: [Serial connection to the ReadyNAS 102 | Roberto's blog](https://blog.rebootr.nl/serial-connection-readynas-102/). This site includes instructions for Windows as well.

### 2. Getting into bootloader via SERIAL connection
You need to interrupt bootloading in order to get into the bootloader console.
Bootloader loads OS (ReadyNAS OS on flash NAND by default) in 3 seconds after the prompt:
"Hit any key to stop autoboot:  0"
and by deafult, it will only wait for 3 seconds.

Hitting space bar every 1 second would eventually let you into bootloader console.

Depends on the bootloader you have, you'll see either one of the below:

```
ALPINE_DB>

Marvell>>
```

It is possible that the bootloader with `ALPINE_DB` is the newer bootloader
introduced as part of Netgear's software / firmware update, so you way want to
try updating ReadyNAS OS if you're seeing `Marvell` or something else,
as this repository is primarily focused on the bootloader that says `ALPINE_DB`.
Please refer to [`/OFFICIAL_RESOURCES.md`](/OFFICIAL_RESOURCES.md) for htne links to software / firmware update files 
and official documentation.

You can compare the output example for bootloading screen, and some outputs at
- /config-backup/bootloader-2017 for `ALPINE_DB` (Source: self)
- /config-backup/bootloader-2015 for `Marvell` ([Source: NicolasBernaerts](https://github.com/NicolasBernaerts/readynas-102/blob/master/bullseye/README.md))

### 3. Getting into Linux via SERIAL connection
Looks like some bootloader lets you to go back to continue booting into Linux just with `boot` command in some circumstances, however it did not work for my stock configuration. In such case, just shutdown once and start once again without bootloader login interruption.

```
ALPINE_DB> shutdown

# Then press the power button until Linux login prompt appears.

# Default password is `password`
# otherwise the `admin` user you have set on ReadyNAS sholud be used.
# Below is an example for NAS host named `nas-B2-13-44` (may look different for yours).

nas-B2-13-44 login: root
Password: <password>
```

Now you're into Linux shell via serial console!

---

## SSH connection

### 1. Setting up for SSH connection
You can enable ssh connection on ReadyNAS OS either by

- Enabling from its UI, or
- Enabling from terminal

For the latter, you can log into root from Serial connection, 
and execute the following

```
# On ReadyNAS serial connection

# Wait until machine boots into Linux
# Username: `root`
# Password: Either `password` or password you have set for `admin` user on UI

~:# systemctl enable ssh
~:# systemctl start ssh

# Find IP address via teminal

~:# hostname -I
  > [IPv4 address] [IPv6 address]
```

### 2. Gettign into Linux via SSH connection
Once above is done, you should be good to go.

```
# On your remote ssh client

$ ssh root@<IPv4 address>
> root@<IPv4 address>'s password: <password>
```
