# Configuration Backup instruction

You can collect the basic configuration via serial I/O.

See [/docs/CONNECTIVITY_GUIDE.md](/docs/CONNECTIVITY_GUIDE.md) for connectivity instruction for
- **2. Getting into bootloader via SERIAL connection**
- **3. Getting into Linux via SERIAL connection**

## Bootloader configuration
```
# Gather info
ALPINE_DB> printenv
```
You can find and compare with examples at:
[/config-backup/bootloader-2017](/config-backup/bootloader-2017)

## Stock OS configuration
```
# Check OS version
# cat /etc/os-release

# Check installed packages
# dpkg -l

# Check hardware component controls
# Fan, LEDs and thermal sensor
```

You can find and compare with examples at:
[/config-backup/ReadyNAS-OS-v6.10.10](/config-backup/ReadyNAS-OS-v6.10.10)
