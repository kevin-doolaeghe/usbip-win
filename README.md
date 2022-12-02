# USB/IP for Windows

## References

* [`usbip-win` Github repository](https://github.com/cezanne/usbip-win)

## Download

See the [Releases](https://github.com/cezanne/usbip-win/releases) page on the Github repository to download the tool.

## Installation

### Server setup

*Note :* All the instructions below must be performed as an administrator.

1. Install `usbip_test.pfx` :

The password for the certificate is `usbip`.  
The certificate should be installed into :
* `Trusted Root Certification Authority` in `Local Computer`,
* `Trusted Publishers` in `Local Computer`.

2. Enable `Test Signing` :
```
bcdedit.exe /set TESTSIGNING ON
```
*Note :* the system must be restarted to apply the changes.

3. List all the USB devices :
```
usbip.exe list -l
```
Get the ID of the USB device from the list.  
It should be of the following form : `1-XXX`

4. Bind the selected USB device to the `usbip` stub :
```
usbip.exe bind -b 1-XXX
```

5. Run `usbipd` server :
```
usbipd.exe -d -4
```
*Note :* TCP port `3240` should be allowed by firewall.

*Extra steps :* Add the USB/IB installation directory to the `PATH` environment variable :
```
$Path = [Environment]::GetEnvironmentVariable("PATH", "Machine") + [IO.Path]::PathSeparator + "USBIP_INSTALLATION_PATH"
[Environment]::SetEnvironmentVariable("Path", $Path, "Machine")
```

### Client setup

*Note :* All the instructions below must be performed as an administrator.

1. Install `usbip_test.pfx` :

The password for the certificate is `usbip`.  
The certificate should be installed into :
* `Trusted Root Certification Authority` in `Local Computer`,
* `Trusted Publishers` in `Local Computer`.

2. Enable `Test Signing` :
```
bcdedit.exe /set TESTSIGNING ON
```
*Note :* the system must be restarted to apply the changes.

3. Install USB/IP VHCI driver :
```
usbip.exe install
```

4. Attach the remote USB device :
```
usbip.exe attach -r <USBIP_SERVER_IP> -b 1-XXX
```
*Note :* The current directory must be USB/IP installation folder (in order to find `attach.exe` file).

## Uninstall

1. Uninstall VHCI driver :
```
usbip.exe uninstall
```

2. Disable `Test Signing` :
```
bcdedit.exe /set TESTSIGNING OFF
```
*Note :* the system must be restarted to apply the changes.
