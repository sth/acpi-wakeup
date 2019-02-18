# acpi-wakeup

Command line script and minimal systemd service to configure ACPI wakeup
settings for USB or other devices.
## Usage

### acpi-wakeup command

When called without parameters, `acpi-wakeup` shows the current ACPI wakeup
status of all devices, for example:

    Device  S-state   Status   Sysfs node
    SIO1      S3    *disabled  pnp:00:00
    RP01      S4    *disabled  pci:0000:00:1c.0
    XHC       S4    *enabled   pci:0000:00:14.0
    ...

Wakeup status is enabled for `XHC`. `XHC` is my mouse, so this means the
computer will wake up when the mouse is moved.

To disable this behaviour:

    sudo acpi-wakeup -XHC

Now moving the mouse will no longer wake up the computer.

To enable it again:

    sudo acpi-wakeup +XHC

You can specify multiple configuration settings in a single call:

    sudo acpi-wakeup -XHC +USB1


### systemd service

The settings configured with `acpi-wakeup` are only active while the
computer is running, a reboot will reset them to their defaults.

The provided minimal systemd service can be used to automatically
configure the wakeup settings at startup. Edit the `ExecStart` line
in `etc/acpi-wakeup.service´ to enable/disable the desired devices
and install the service:

    sudo cp etc/acpi-wakeup.service /etc/systemd/system/
	 sudo systemctl enable acpi-wakeup


## Installation

The script can be installed by simply copying it to a `bin` folder, for
example:

	 sudo cp acpi-wakeup /usr/local/bin/

If you want to install the systemd service, copy the service file to a
place where systemd will find it and enable the service:

    sudo cp etc/acpi-wakeup.service /etc/systemd/system/
	 sudo systemctl enable acpi-wakeup

