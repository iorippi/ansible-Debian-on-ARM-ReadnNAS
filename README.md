# Ansible playbook for Debian on ARM ReadyNAS models
Configuring ARM ReadyNAS for the latest Debian installation with Ansible (mostly)

_This project is inspired and forked from [readynas-102](https://github.com/NicolasBernaerts/readynas-102) project by @NicolasBernaerts._

Netgear seased to support their ReadyNAS hardwares, and there are no method to patch or add new functionality in safe and reliable way anymore. With this procject, while it keeps modification to the NAS hardware itself, it'll boot into the latest Debian installation on SATA disk, encrypt RAID and make it discoverable via Samba. This would function literally as simple Debian server, so you can reconfigure via ssh just like any other server.
Ansible playbook in this project also takes care of cooling fan, LEDs and button behaviors as well.

Design principles are as follws:
- Use standard Linux tools only
- Prefer simplicity over cleverness
- Optimize for recovery over convenience
- Avoid unnecessary layers and abstractions

## Requirement
Please prepare these before starting.
- ReadyNAS with the latest software/firmware update. (See [/docs/OFFICIAL_RESOURCES.md](/docs/OFFICIAL_RESOURCES.md) for the links.)
- At least one disk to be used for both Debian installation and NAS RAID
- Serial console connector (v3.3)
- Workstation (Linux or macOS recommended. Windows involves a little more manual configuration.)
- LAN for NAS and workstation to communicate

## Design abstracts

### Disk structure
_RAID configuration is configurable via config file - This is the default example_
- /boot (unencrypted)
- RAID1 -> LVM (3 partitions)
  - LUKS (encrypted)
    - / (NAS OS general)
    - /data-encrypted
      - human user data (ex.: Media, Documents)
      - machine user data (ex.: Logfiles, database)
  - Plain (unencrypted)
    - /data-plain
      - Encrypted client backup storage (ex.: Apple Timemachine backup)
      - Cache storage (ex.: APT, PYPI, container images)
      - Audiovisual media storage (ex.: Movies)

#### Bootloader configuration
- Bootloader:
  - Keep existing U-Boot
  - Do NOT replace or flash
  - Only minimal env changes (bootargs, bootcmd)
- Boot source:
  - SATA disks (no permanent USB dependency)
- Access:
  - SSH for normal operation
  - UART (FTDI) for recovery

*Boot order*
1. USB
2. Disk1's `/boot`
3. Disk2's `/boot`
4. Stock OS in NAND flash (ReadyNAS OS)

#### Data security
Data on disks are encrypted with clevis & tang
- LUKS applied on RAID (not per-disk)
- `/boot` remains unencrypted

**Key management**
- Mandatory:
  - Passphrase
- Optional:
  - Clevis + Tang (auto unlock)
- Fallback:
  - Dropbear SSH (initramfs)
  - UART console

