# Ansible playbook for Debian on ARM ReadyNAS models
Configuring ARM ReadyNAS for Debian installation with Ansible semi-automatically

_This project is inspired and forked from [readynas-102](https://github.com/NicolasBernaerts/readynas-102) project by @NicolasBernaerts._

Netgear seased to support their ReadyNAS hardwares, and its operating system (ReadyNAS OS) and extensions on it is no more updated. This project aims to replace its OS with the latest Linux distribution (Debian) and keep it reconfigurable via ssh just like any Linux server.
Methodology emplyed in this project also tries to minimize the configuration change to the hardware so it can be brought back to factory state rather easily and safely, whilst providing standard functionality such as RAID, SAMBA server, encryption and more as you wish.

## Abstract
This method reconfigures bootloader to boot from the disk in the SATA cage where Debian will be installed, and both Debian and the data storage will be encrypted, but partial encryption is also possible (as in leaving one partition unencrypted). Decryption can be done either by having Tang server, PIN via serial console or ssh connection. RAID is confiugured with mdadm and configured automatically. Disks will be exposed to network using either SAMBA, NFS or both.
As the primary testing model RN102 severely lacks compute and memory resource, this project does not have GUI, however if you plan to add one somehow, you can of course do so after the basic provisioning is done.

## Preparation
Check if your hardware is tested 
**Currently tested:**
- ReadyNAS RN102 (with ReadyNAS OS 6.10.10)
	This likely works for RN104, RN202 and RN204 without any modification, however it is not tested.

Please let your test result known in Issues of this respository for update.

## Requirement
Please prepare these before starting.
- ReadyNAS with the latest software/firmware update. (See [`/OFFICIAL_RESOURCES.md`](/OFFICIAL_RESOURCES.md) for the links.)
- At least one disk to be used for both Debian installation and NAS RAID
- Serial console connector (v3.3)
- Workstation (Linux or macOS recommended. Windows involves a little more manual configuration.)
- LAN for NAS and workstation to communicate
