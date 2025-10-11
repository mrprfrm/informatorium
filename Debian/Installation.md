---
authors:
  - Anton Petrov
status: Note
tags:
  - debian
---

Debian installation is often treated as a routine checklist of screens and confirmations. Yet beneath each of these choices lies a network of configuration mechanisms that define how the system will function long after setup.

Options such as network selection, disk partitioning, and package mirrors do more than populate menus — they determine how the base system is assembled, how services initialize, and how storage and networking behave under real workloads.

The following notes clarify installation steps whose underlying mechanisms are not immediately apparent.

## Prerequisites

We are going to use the **netinst** Debian distribution, so let's make sure the system has internet access via Ethernet. There is a chance that our wireless adapter requires a proprietary driver. During installation, we can select any language or installation mode we prefer — here, we focus only on the steps and options that don't seem obvious.

## Network Configuration

The netinst installer requires a working network connection, so we keep this step deterministic and wired. On hardware with multiple NICs, we'll typically see entries like:

```
enp2s0: Realtek PCI Express Gigabit Ethernet Controller
eno1:   Realtek PCI Express Gigabit Ethernet Controller
wlp4s0: MediaTek 7921 Wireless Adapter
```

We select the **Ethernet** interface with a cable attached — in our case, `enp2s0`. DHCP completes automatically if the router provides addresses; that's enough for installation and first boot. We intentionally avoid Wi-Fi here to eliminate firmware prompts and unstable links during a net install.

After the link is up, the installer asks for identity. For the hostname, we use a short, functional name such as `server`. For the domain, we don't rely on external DNS — we assign a simple local tag that only matters to this machine and its logs, for example, `local`.

Both values can be changed later if necessary:

```bash
sudo hostnamectl set-hostname server
sudo hostnamectl set-domain local
```

This produces a local FQDN like `server.local`. It defines only how the host identifies itself internally. Without a DNS or mDNS service on the network, other machines won't resolve this name automatically — they can still connect by IP address or by adding a static entry to their own `/etc/hosts`.

## Disk Partitioning

Partitioning defines not just where data lives, but how the system can evolve later. In our case, the host runs on a single SSD (`/dev/nvme0n1`) and will manage LXC/LXD containers. We don't know how many containers there will be or how large they might grow, and some will be ephemeral — destroyed and recreated frequently. That uncertainty makes pre-allocating fixed partitions meaningless.

We need a layout that stays simple now but doesn't block future changes. During installation, we stick to the entire-disk partitioning scheme using a single partition and LVM:

```
Guided – use entire disk and set up LVM
```

...

and then:

```
All files in one partition (recommended for new users)
```

This combination lets Debian create a small EFI partition for boot and allocate the rest of the disk to an LVM physical volume. Inside that volume group (`server-vg`), two logical volumes are created automatically — one for the root filesystem (`/`) and one for swap. The result is a clean, flexible layout with no unnecessary fragmentation.

LVM provides the ability to resize filesystems or create new volumes later without reinstalling the system. That matters for a container host, where `/var` holds container storage (`/var/lib/lxd` or `/var/snap/lxd/common/lxd`) and grows unpredictably. By keeping everything inside one logical volume, we avoid situations where `/var` runs out of space while other partitions remain empty. At the same time, the configuration stays simple enough to recover or rebuild quickly if needed.

We deliberately did not enable encryption, since this node operates headless and must boot unattended after restarts. Requiring a passphrase at every boot would complicate remote maintenance. Of course, we can choose differently — enable encryption and follow another partitioning scheme if our environment requires it.

### Alternative Options

- **Use entire disk (no LVM)** — the simplest possible layout, but it locks us into fixed partition sizes. Fine for desktops, less so for an evolving lab host.
- **Encrypted LVM** — secures data at rest but adds a boot-time dependency we'd rather avoid on a remote machine.
- **Manual partitioning** — offers isolation between directories but forces us to guess size limits up front.
- **Btrfs or ZFS** — provide native snapshots and compression, but both add configuration overhead we decided to postpone.

## Package Repository

For a netinst setup, we want a mirror that is complete, fast, and not tied to a single region. The country prompt only seeds the list — it doesn't restrict what we can access. We pick **United States** to expose Debian's CDN entry and then select **deb.debian.org**.

`deb.debian.org` is a global redirector backed by multiple CDNs. It routes to a healthy mirror automatically and carries all components, so we avoid partial or stale country mirrors. When prompted for an HTTP proxy, we leave it empty; there's no proxy in this environment, and direct HTTPS to the CDN is preferred.

After installation, the installer writes `sources.list`. For a neutral, complete setup on Debian 13 (**trixie**), it should look like this (HTTPS, full components, security, updates, and optional backports):

```bash
# /etc/apt/sources.list
deb https://deb.debian.org/debian trixie main contrib non-free-firmware
deb https://deb.debian.org/debian trixie-updates main contrib non-free-firmware
deb https://security.debian.org/debian-security trixie-security main contrib non-free-firmware
# optional, enable when needed
# deb https://deb.debian.org/debian trixie-backports main contrib non-free-firmware
```

This layout ensures unrestricted access to the full archive (including firmware), timely security patches, and minor point updates — without relying on any local DNS or regional mirror behavior.

### Alternative Options

- **Country-specific mirror** — acceptable if we know a fast, reliable host, but we risk lag during syncs.
- **Corporate proxy** — only if our network requires it; otherwise, keep it blank and use HTTPS directly.
- **No backports** — safest default; enable the `trixie-backports` line only when we need a newer package for a specific task.

## Remaining Steps

All next steps are quite transparent, so we won't pay more attention here. Next, we may select software to install before the first boot and install the GRUB boot loader as the master boot record.

> Next packages can be installed using the package manager — `apt` in our case. For those sources that were added by default, we can modify `/etc/apt/sources.list` or `/etc/apt/sources.list.d/*.list` manually. Also, there is an option to use the `add-apt-repository` tool, but it's a port of an Ubuntu convenience tool and it's not tightly integrated with Debian's repository management philosophy.

> GRUB boot loader also may be installed after installation if needed.
