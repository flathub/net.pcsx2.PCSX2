# Flatpak for PCSX2 (on Flathub)
[PCSX2 Official Website](https://pcsx2.net)

## Install

(Your operating system's "app store" or application installer may provide a graphical way to install, run, and uninstall flatpaks, as an alternative to entering some of the commands below in a terminal window.)

0. [Dump your Playstation 2's BIOS](https://pcsx2.net/guides/basic-setup/).

1. [Set up Flatpak](https://www.flatpak.org/setup/).

2. Install PCSX2 from [Flathub](https://flathub.org/apps/details/net.pcsx2.PCSX2).

   `flatpak install -y net.pcsx2.PCSX2`

3. Start PCSX2.

   `flatpak run net.pcsx2.PCSX2`

4. Copy your PS2 BIOS files to PCSX2's data directory, which should be `$HOME/.var/app/net.pcsx2.PCSX2/config/PCSX2/bios`

   When the "PCSX2 First Time Config" dialog prompts you to "Select a BIOS rom", PCSX2 should find your BIOS.

5. Continue on to use PCSX2.

### Uninstall

To uninstall: `flatpak uninstall -y net.pcsx2.PCSX2`

## Usage notes

### Networking support
As of PCSX2 1.7, DEV9 support has been merged into the main codebase and can be used with some caveats.

#### Network permissions
Since Flatpak does not contain permissions to write to the network stack via libpcap at the user level, PCSX2 will likely need to be run as `root`
and will need the `--share=network` permissions. (Note: Running apps like this under root is not a good idea.)

If networking still does not work, or adapters do not show up, you will need to grant cap permissions to PCSX2
```sh
setcap cap_net_raw,cap_net_admin=eip /var/lib/flatpak/app/net.pcsx2.PCSX2/current/active/files/bin/PCSX2
```

#### Audio support
If you run the PCSX2 flatpak under root or some other user, you need to run PulseAudio in multi-user mode to have sound support:
```sh
flatpak run --system --socket=pulseaudio net.pcsx2.PCSX2`
```

### BIOS issues
PCSX2 tries to open some PS2 BIOS files with lowercase extensions ([PCSX2 issue 5954](/PCSX2/pcsx2/issues/5954)), so rename your PS2's BIOS files to lowercase extensions (`.NVM` -> `.nvm`, etc.)

PCSX2 may write to files in the PS2 BIOS directory. So if in "PCSX2 First Time Configuration" you point to some other directory than the default (the flatpak's data directory), you can grant the flatpak write access to that directory
```sh
flatpak override --user net.pcsx2.PCSX2 --filesystem=/path/to/my/PS2_BIOS
```

## Build

`flatpak-builder` is required, see [Building your first Flatpak](https://docs.flatpak.org/en/latest/first-build.html).

- Install the SDK

`flatpak install org.kde.Platform/x86_64/6.4 org.kde.Sdk/x86_64/6.4`

- Build PCSX2

`flatpak-builder --user --install --force-clean build-dir net.pcsx2.PCSX2.yml`
