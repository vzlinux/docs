# Virtuozzo Linux 8 Quick Start Guide

## About Virtuozzo Linux

Virtuozzo Linux 8 is a 1:1 clone of Redhat Enterprise Linux (RHEL) 8. It
is free to download, use, and distribute.

The purpose of Virtuozzo Linux is to assist organizations and their
clients with an easy path to upgrade or move the existing estate from
CentOS, which has been announced to have a reduced lifecycle.

NOTE: Compared to CentOS 8, Virtuozzo Linux 8 uses iptables instead of
nftables by default, even though both sets of tools are provided.

## Installing Virtuozzo Linux

This chapter lists the system requirements of Virtuozzo Linux 8 and
explains how to install it in various modes.

### System Requirements

Virtuozzo Linux has the following hardware requirements:

-   RAM: 1.5 GiB minimum, 1.5 GiB per logical CPU recommended, 4 GiB
    recommended for installation via PXE
-   Disk: 10 GiB minimum, 20 GiB recommended

For more information, consult [Red Hat Enterprise Linux technology
capabilities and
limits](https://access.redhat.com/articles/rhel-limits).

### Obtaining the Distribution

You can download Virtuozzo Linux distribution ISO images from
<http://repo.virtuozzo.com/vzlinux/8/iso/>.

The package repository is at
<http://repo.virtuozzo.com/vzlinux/8/x86_64/os/>.

### Starting Installation

Virtuozzo Linux can be installed from:

-   USB drives
-   PXE servers
-   IPMI virtual drives
-   DVD discs

To start the installation:

1.  Prepare the installation source, e.g., mount a distribution ISO or
    plug in a bootable USB drive.
2.  Configure the server to boot from the installation source.
3.  Boot the server and wait for the welcome screen.

#### Choosing the Installation Type

You can install Virtuozzo Linux in one of the following modes:

-   Graphics (default, recommended), see
    `Installing in the Graphics Mode`.
-   Basic graphics (in case of issues with video card drivers), see
    `Installing in the Basic Graphics Mode`.
-   Graphics via VNC, see `Installing via VNC`.
-   Text, see `Installing in the Text Mode`.

Installation steps differ depending on the chosen mode. They are
described in the corresponding sections of this guide.

### Installing in the Graphics Mode

To install Virtuozzo Linux in the graphics mode, choose **Install**
**Virtuozzo Linux** on the welcome screen. After the installation
program loads, choose the language to use during the installation and
click **Continue**. You will be taken to the **Installation Summary**
screen. On it, you specify the parameters required to install Virtuozzo
Linux.

<img src="/images/vzlinux8.png" class="align-center align-centeralign-center" alt="image" />

In the **LOCALIZATION** section:

-   In **Keyboard**, optionally add keyboard layouts and choose a key
    combination to switch between them.
-   In **Language Support**, optionally add more languages to install
    support for.
-   In **Time & Date**, optionally adjust time, date and time zone, add
    NTP servers, and enable network time.

In the **SOFTWARE** section:

-   In **Installation Source**, choose a source to install Virtuozzo
    Linux from.

    If the machine is connected to the Internet, you can choose to
    install from the official repository that provides all available
    base environments and additional software (see
    `Obtaining the Distribution`).

    You may need to configure network in **SYSTEM** &gt; **Network &
    Host Name** first.

-   In **Software Selection**, select the desired base environment and
    additional software for it.

In the **SYSTEM** section:

-   In **Installation Destination**, select the disk(s) to install
    Virtuozzo Linux to. Choose **Automatic** in **Storage
    Configuration** to have the installer partition the disk(s).
    Otherwise, choose **Custom** in **Storage Configuration** to
    partition the disk(s) manually. In this case, it is recommended to
    have these partitions at least:
    -   `/boot` for OS kernel and bootstrap files, 1 GiB or more

    -   `/` (root), the top level of the directory structure, 10 GiB or
        more

    -   `/home`, for user data, 1 GiB or more

    -   `swap`, 1 GiB or more, depending on the system RAM:

        | RAM             | Recommended swap size              | Recommended swap size for hibernation |
        |-----------------|------------------------------------|---------------------------------------|
        | Less than 2 GiB | 2 times the RAM size               | 3 times the RAM size                  |
        | 2 to 8 GiB      | Same as RAM size                   | 2 times the RAM size                  |
        | 8 to 64 GiB     | 4 GiB to 0.5 times the RAM size    | 1.5 times the RAM size                |
        | Over 64 GiB     | 4 GiB or more, depends on workload | Hibernation not recommended           |

    -   `/boot/efi`, 200 to 600 MiB
-   In **KDUMP**, optionally toggle kernel crash dumping.
-   In **Network & Host Name**, configure the network and optionally
    edit the domain name.

When done, click **Begin Installation**. While the OS is being
installed, set a password for the root user and create more users. After
the installation is complete, click **Reboot** to boot to Virtuozzo
Linux. Make sure that the machine boots from the destination disk with
the `/boot` partition.

### Installing in the Basic Graphics Mode

If the installer cannot load the correct driver for your video card, you
can try to install Virtuozzo Linux in the basic graphics mode.

To select this mode, on the welcome screen, choose
**Troubleshooting--&gt;**, then **Install in the basic graphics mode**.

The installation process itself is the same as that in the default
graphics mode (see `Installing in the Graphics Mode`).

### Installing in the Text Mode

To install Virtuozzo Linux in the text mode, boot to the welcome screen
and do the following:

1.  Select the required installation option and press **E** to start
    editing it.

2.  Add `text` at the end of the line starting with
    `linux /images/pxeboot/vmlinuz`. For example:

        linux /images/pxeboot/vmlinuz inst.stage2=hd:LABEL=<vzlinux_ISO> quiet ip=dhcp text

3.  Press **Ctrl-X** to start booting the chosen installation option.

4.  When presented with a choice of starting VNC or proceeding to the
    text mode, press **2** for text mode.

5.  In the installation menu, at least edit settings marked `[!]`.

6.  Press **b** to begin installation.

7.  When installation ends, press **Enter** to reboot.

### Installing via VNC

To install Virtuozzo Linux via VNC, boot to the welcome screen and do
the following:

1.  Select the required installation option and press **E** to start
    editing it.

2.  Add `text` at the end of the line starting with
    `linux /images/pxeboot/vmlinuz`. For example:

        linux /images/pxeboot/vmlinuz inst.stage2=hd:LABEL=<vzlinux_ISO> quiet ip=dhcp text

3.  Press **Ctrl-X** to start booting the chosen installation option.

4.  When presented with a choice of starting VNC or proceeding to the
    text mode, choose **1** for VNC.

5.  When offered, enter a VNC password.

6.  In the output that follows look up the hostname or IP address and
    VNC port to connect to, e.g., `192.168.0.10:1`.

7.  Connect to said address in a VNC client. You will see the usual
    **Installation Summary** screen.

The installation process itself is the same as that in the default
graphics mode (see `Installing in the Graphics Mode`).

### Booting into Rescue Mode

If you experience problems with your system, you can boot into the
rescue mode to troubleshoot these problems.

To enter the rescue mode, do the following:

1.  Boot your system from the chosen media.
2.  On the welcome screen, click **Troubleshooting--&gt;**, then
    **Rescue system**.
3.  Once Virtuozzo Linux boots into the emergency mode, press **Ctrl+D**
    to load the rescue environment.
4.  In the rescue environment, choose one of the following options:
    -   Continue (press **1**): mount the Virtuozzo Linux installation
        in read and write mode under `/mnt/sysimage`.
    -   Read-only mount (press **2**): mount the Virtuozzo Linux
        installation in read-only mode under `/mnt/sysimage`.
    -   Skip to shell (press **3**): load shell, if your file system
        cannot be mounted; for example, when it is corrupted.
    -   Quit (Reboot) (press **4**): reboot the server.
5.  Unless you press **4**, a shell prompt will appear. In it, run
    `chroot /mnt/sysimage` to make the Virtuozzo Linux installation the
    root environment. Now you can run commands and try to fix the
    problems you are experiencing.
6.  After you fix the problem, run `exit` to exit the chroot
    environment, then `reboot` to restart the system.

## Updating Virtuozzo Linux

Virtuozzo Linux allows quick and easy updates with the `yum` utility
standard for RPM-compatible Linux operating systems.

The components you may need to update are utilities and libraries as
well as the kernel.

### Checking for Updates

Before updating any packages, you may want to see the list of available
updates. To do this, run

    # yum check-update

### Updating Everything

The easiest way to update all components of Virtuozzo Linux is to run

    # yum update

When executed, this command instructs the `yum` utility to do the
following:

1.  Access the remote Virtuozzo repositories.
2.  Check for available updates for the Virtuozzo Linux kernel,
    utilities, libraries.
3.  Install the available updates to your system.

To update one or more specific packages, run

    # yum update <package1> ... <packageN>

### Updating the Kernel

To update just the kernel of Virtuozzo Linux, run

    # yum update vzkernel vzkernel-devel

After updating, reboot the server and switch to the new kernel.
