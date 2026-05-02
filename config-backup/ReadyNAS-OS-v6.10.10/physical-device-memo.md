# Physical device configuration

This is a memo for physical parts such as buttons, LEDs and fan configuraion
for the stock OS. Please refer to this for later customization
after installing your own Linux OS to this machine.

TL;DR: Most basic components such as power button and fan should work the same
right out of the box with the standard Linux installation.
However, other parts that are handled by the stock configuration is
not easy to migrate over to the original Linux installation.
Backup button functionality, LEDs on front panels needs custom setup.
Also, behavior of the power button on stock configuration can only be replicated
via custom setup.

See the followings for a bit more of depth.

## Buttons (Power, Backup)

You can rely entirely on standard Linux input semantics.

Button press
   ↓
gpio-keys driver
   ↓
input event (KEY_POWER / KEY_COPY)
   ↓
systemd / acpid / your script

Use below table for reuse.

| Button | Code | Symbol      |
| ------ | ---- | ----------- |
| Power  |  116 | `KEY_POWER` |
| Backup |  133 |  `KEY_COPY` |

---

## LED control

It seems to be controlled by `readynasd`

udev event (USB/disk)
      ↓
rnutil (wrapper / IPC client)
      ↓
readynasd (central logic)
      ↓
LED updates + system behavior

However, since portability of the binary and environment seems not worth the effort,
thus this implementation is substituted for the workaround provided elsewhere.

---

## Fan control

You should have full fan control capability, not a degraded case.
This NAS comes with..

*g762*
This is almost certainly your fan controller
* GMT G762 = hardware fan controller chip
* Common in embedded/NAS boards
* Provides:
    * fan speed control (PWM or DAC)
    * tachometer (RPM feedback)

*al_thermal*
Input for fan policy, not control
* Armada (Marvell) SoC thermal sensor
* Provides:
    * CPU temperature

```bash
root@nas-B2-13-44:~# cat /sys/class/hwmon/hwmon*/name
g762
al_thermal

root@nas-B2-13-44:~# ls /sys/class/hwmon/hwmon*
/sys/class/hwmon/hwmon0:
device      fan1_fault   fan1_target  power        pwm1_mode
fan1_alarm  fan1_input   name         pwm1         subsystem
fan1_div    fan1_pulses  of_node      pwm1_enable  uevent

/sys/class/hwmon/hwmon1:
name  power  subsystem  temp1_input  uevent
```
