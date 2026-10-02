<div align="center">

<img src="silab-logo.svg" width="96" alt="SILAB logo" />

# SILAB

### SecuryTik Interactive Laboratory

**MikroTik network labs in your browser: real RouterOS, Plug & Play, no VT-x required on Linux, one-command install**

[![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white&style=flat-square)](https://python.org)
[![QEMU](https://img.shields.io/badge/QEMU-KVM%20or%20TCG-FF6600?logo=qemu&logoColor=white&style=flat-square)](https://www.qemu.org)
[![RouterOS](https://img.shields.io/badge/RouterOS-CHR%207.15%2B-293239?logo=mikrotik&logoColor=white&style=flat-square)](https://mikrotik.com/download)
[![Platform](https://img.shields.io/badge/Platform-Ubuntu%20%7C%20Windows%20%28WSL2%29-E95420?logo=ubuntu&logoColor=white&style=flat-square)](https://ubuntu.com)
[![Web](https://img.shields.io/badge/Web-nginx-009639?logo=nginx&logoColor=white&style=flat-square)](https://nginx.org)

[**securytik.com**](https://securytik.com) &nbsp;·&nbsp; [Report a Bug](mailto:silab@securytik.com?subject=SILAB%20Bug%20Report) &nbsp;·&nbsp; [Request a Feature](mailto:silab@securytik.com?subject=SILAB%20Feature%20Request)

</div>

---

## Overview

SILAB is a network simulation lab built for **MikroTik trainers and students**, in the spirit of EVE-NG and
PNETLab but focused on one job and made simple. Draw a topology, press start, and every router is a real
RouterOS **Cloud Hosted Router (CHR)** you can reach with one double-click: a console in the browser, WinBox,
WebFig or SSH.

It runs on plain QEMU. When the host has KVM, SILAB uses it; when it does not (a cheap VPS, a laptop VM, a
nested cloud instance), it falls back to software emulation and still boots a router in about half a minute.
Resources are treated as a feature: routers share one base image on disk and identical memory pages in RAM,
and switches, PCs and Internet clouds cost almost nothing.

```
 browser ──► nginx ──► silab-web (UI, unprivileged) ──unix socket──► silab-engine (root)
    │                                                           │
    │  console (xterm.js)                                       ├─ QEMU + CHR per router  (systemd unit, CPU/RAM caps)
    │                                                           ├─ tc links between ports (STP / LACP / LLDP pass)
 WinBox / SSH / WebFig ──► host:port ──nftables DNAT──►        ├─ mgmt network (DHCP, isolated, VRF inside RouterOS)
                                                                └─ hubs, PCs, Internet NAT clouds
```

---

## Features

<table>
<tr>
<td valign="top" width="50%">

**🧪 Real routers, real labs**
- Every router is a genuine MikroTik CHR: the same RouterOS your students will meet in the field
- Router, Switch, Hub, PC and Internet nodes, cabled port to port (`ether1..N`) exactly as drawn
- Point-to-point cables pass every frame: STP, LACP, LLDP, MNDP and VLAN tags
- Add routers, switches and cables **while the lab is running**, no restart
- Startup configuration per router (`.rsc`), applied on first boot

**🖱️ One double-click away**
- Browser console that works even when the student has erased every IP
- WinBox, WebFig and SSH through fixed host ports, from the LAN or the Internet
- **Right-click → WinBox** opens WinBox already logged in (routers are `admin` / `admin`)
- **Right-click a cable → Capture in Wireshark**: that link's traffic, live, both directions
- **Virtual PCs**: DHCP client, static address or **PPPoE client** (with service name); a console with
  `ping`, `trace`, `nslookup`, `web` (a page as text), `arp`, `route` — no host shell behind it
- **Ping** next to **+ Connect**: click two devices; 5 pings run in the background and the result opens
  in a pop-up (the address on the subnet they share, else the target's first address)
- Management port kept in its own VRF (`vrf-mgmt`): out of the student's routing table and out of OSPF/BGP
- Ports named `ether1..N` inside RouterOS, matching the diagram

**🔌 Cables and power, like the real bench**
- Unplug / plug a cable, or impair it: delay, jitter, loss, bandwidth
- VMware-style power menu: power on, shut down, reboot, reset, **power off (pull the plug)**, wipe the disk
- Power-cycle confirms RouterOS device-mode changes (container, traffic-gen …)

</td>
<td valign="top" width="50%">

**⚡ Light on resources**
- No VT-x needed on Linux: KVM when present, QEMU TCG when not (Windows: WSL2 needs VT-x on)
- One base image per RouterOS version, a thin copy-on-write overlay per router
- Per-router CPU and RAM caps (systemd cgroups); one flooded router cannot freeze the host
- Hubs, PCs and Internet clouds are Linux bridges and namespaces, no VMs
- SQLite, no database server

**📦 Tiny, shareable labs**
- A `.silab` file is just the diagram plus one `.rsc` per router, usually a few KB
- The receiver's SILAB rebuilds the lab from its own RouterOS image
- Imported files are validated strictly: no host paths, no raw QEMU arguments

**🔄 RouterOS versions**
- CHR images downloaded from MikroTik at install, checksum-verified (never redistributed)
- Several versions side by side; SILAB tells you when a newer RouterOS is out
- A lab pins its version, so a shared lab behaves the same everywhere

**🧰 Trainer tools**
- **Bulk actions** on every router at once: clock sync, NTP, time zone, DNS, run RouterOS commands,
  RouterOS upgrade, reboot / hard reset / power cycle, reset the lab to its start
- **Auto Addressing**: pick a range and subnet sizes, preview the plan, and every cable gets its
  subnet and every port its address (typed addresses are kept, facing ports filled in)
- One click for the usual lab chores, each with its Remove: deploy **OSPF** (area 0, every network),
  **RIP**, **BGP** (eBGP per cable or iBGP between loopbacks), **VRRP** on shared segments, a default
  route, loopbacks, Internet access (DHCP client + NAT), MikroTik's default firewall toward the
  Internet, simple queues per PC subnet, DHCP servers for the PCs, switch-mode bridge, ports on/off,
  identities, checkpoints (backup / restore). SILAB's management port is never part of any of them
- Read the whole lab as tables (filter, copy, CSV): routing tables, addresses, interfaces, OSPF / RIP /
  BGP neighbours, a cabling check against the canvas, health, firewall counters, logs, traceroute
  and a ping-sweep matrix; right-click a router → **Show ›** for its routing table, addresses, DNS
  and info
- **Rename a router on the canvas** and its RouterOS identity follows, live
- Browser console that works even with no IP on the router

**🛡️ Like every SecuryTik product**
- One-command install; **signed updates** with What's new, automatic restore if an update fails
- **Backup & restore**: labs, accounts, settings, optionally router disks; daily automatic copies
- SecuryTik panel design, day and night themes, works down to phone width

</td>
</tr>
</table>

---

## Status

Verified on real RouterOS 7.24.4 on a host with no KVM:

| Check | Result |
|---|---|
| Clean 2-router lab, from `up` to both routers provisioned | 34 s |
| Router added to a running lab, booted and linked | 39 s, pings across the new cable |
| Idle router under TCG | ~220 MB RSS, 4–6 % of one core |
| Cable cut / reconnect, hard reset, power cycle | ✅ |
| PC → hub → router | ✅ |
| Browser console, WinBox / SSH / WebFig via host ports | ✅ |

**New: the Lab catalog.** Ready-made labs, downloaded when you want one: **learning labs** that show a
technology working and walk you through building it, and **diagnosis labs** that arrive broken, with 1–5
faults to find and fix and hints revealed one at a time. Each has a PDF guide and a **Check my work** button
that reads the routers and tells you what passes; your progress can be exported for a trainer. 29 learning labs
and 14 diagnosis labs, from IP addressing to MPLS L3VPN and a small-ISP capstone (the full list: [silab.securytik.com/docs](https://silab.securytik.com/docs)).

---

## Installation

SILAB installs everything it needs — **one command, one server, labs in minutes.**

```bash
curl -fsSL https://silab.securytik.com/install.sh | sudo bash
```

It downloads from our bucket, and from SILAB's GitHub release by itself when the bucket cannot be reached.
More at **https://silab.securytik.com/docs**.

The bootstrap verifies the release manifest's **signature** (SILAB's release key) and the bundle's
**SHA256** before anything runs, then asks you to accept MikroTik's licence and installs.

Open `http://<server>/` and sign in as **admin / admin** (change it under System → Profile).

### What the installer sets up

| Component | Details |
|---|---|
| **QEMU** | `qemu-system-x86` (or `-arm` on arm64 hosts) + `qemu-utils`; KVM used automatically when `/dev/kvm` is usable |
| **RouterOS CHR** | Latest stable image downloaded from MikroTik after you accept MikroTik's licence; SHA256-verified |
| **nginx** | Publishes the web UI on **port 80** (or 8088 when 80 is taken, e.g. by SAMM); SILAB's own vhost, added without touching other sites. |
| **silab-web** | Web UI as the unprivileged `silab` user, on 127.0.0.1 only (behind nginx) |
| **silab-engine** | Root service: routers, cables, management network, port forwards |
| **Timers** | Daily release check (`silab-updater.timer`), daily backup at 04:00 (`silab-backup.timer`), KSM (`silab-ksm`) |
| **Networking** | Its own bridges (`sl*`) and nftables table (`inet silab`); **never** touches your LAN interface |
| **Host firewall** | Where Docker or ufw drop forwarded traffic, `silab-fw.service` adds narrow `ACCEPT` rules (tagged `silab`) for the lab's port forwards, Internet clouds and hubs, and re-adds them at every boot. Install Docker or enable ufw **after** SILAB? Run `systemctl restart silab-fw` (or reboot). |

SILAB is **independent**: it installs on a clean Ubuntu box and sits beside anything else already on the host
without touching it.

**Removing SILAB:** `sudo bash /opt/silab/install.sh --uninstall` removes the app, its services, web site,
firewall rules and user, and keeps your labs, RouterOS images and backups; add `--purge` to remove those too.
QEMU, nginx and the other packages stay installed.

### Windows

SILAB runs on Windows 10 (2004+) and 11 in its own WSL2 instance, set up by one installer:

Open **PowerShell** — not Command Prompt (cmd) — and run the line below. If PowerShell is not running as
administrator, Windows asks for permission: click **Yes** and the installation continues in a new
administrator window.

```powershell
irm https://silab.securytik.com/install.ps1 | iex
```

It enables WSL2 (one restart the first time; setup then continues by itself), imports a dedicated
Ubuntu 24.04 instance named **SILAB** (checksum-verified; your other WSL distributions are untouched),
installs SILAB inside it with the Linux installer above, and adds a **SILAB** icon to the desktop and
Start menu: a small menu to **Start**, **Stop**, see the **Status**, **Open** it in the browser, and
**Share on the LAN** so other PCs (and their WinBox) can reach the lab. To remove it all:
`powershell -ExecutionPolicy Bypass -File "$env:ProgramData\SILAB\silab-windows.ps1" -Uninstall`.

WSL2 needs hardware virtualization (Intel VT-x / AMD-V) switched on in the BIOS; inside a virtual machine
it must be passed through to the guest (VMware: "Virtualize Intel VT-x/EPT"). The installer checks this
and says what to change.

### Prerequisites

- Ubuntu 22.04 / 24.04 / 26.04, amd64 or arm64
- Root / sudo access
- 2 cores and 4 GB RAM run a handful of routers; plan ~200–300 MB RAM per router

### Environment overrides

| Variable | Effect |
|---|---|
| `SILAB_HTTP_PORT=N` | Web UI port (default 80 when free, else 8088) |
| `SILAB_APP_PORT=N` | The app's own port on 127.0.0.1, behind nginx (default 8089) |
| `SILAB_ACCEPT_MIKROTIK_LICENSE=1` | Accept MikroTik's licence without the prompt |
| `SILAB_SKIP_IMAGE=1` | Offline host: install without downloading RouterOS (`silab image add <file>` later) |
| `SILAB_VERBOSE=1` | Raw command output instead of the progress display |

### Updating

System → Updates shows the installed version, the latest release and What's new. **Apply update**
downloads the signed release, backs up the install to `/var/backups/silab`, swaps in the new files and
restarts the engine and the web — running routers keep running. If anything fails, the previous version is
put back automatically.

After an Ubuntu release upgrade (22.04 → 24.04 → 26.04) the services rebuild SILAB's Python environment
themselves on their next start; re-running the installer also works.

Every release is signed by SecuryTik; installs and updates refuse a release whose signature does not
check out or has expired.

---

## Using SILAB

### The lab file

```yaml
silab: 1
name: OSPF basics
routeros: "7.24.4"
nodes:
  - {id: 1, name: R1, kind: router, x: 100, y: 100, config: R1.rsc}
  - {id: 2, name: R2, kind: router, x: 400, y: 100, packages: [user-manager]}
  - {id: 3, name: LAN, kind: hub,    x: 250, y: 300}
  - {id: 4, name: PC1, kind: pc,     x: 250, y: 450}
links:
  - {a: {node: 1, port: 1}, b: {node: 2, port: 1}, delay_ms: 20}
  - {a: {node: 2, port: 2}, b: {node: 3}}
  - {a: {node: 4}, b: {node: 3}}
```

### Extra RouterOS packages

A router can carry MikroTik's extra packages — `user-manager`, `container`, `dude`, `iot`, `wireless` and the
rest: right-click it → **Packages…**, tick them, save. SILAB downloads MikroTik's package archive for that
RouterOS version once, checks its SHA256, installs the packages and restarts the router once; `container` also
switches RouterOS container mode on.

The list is saved with the lab (`packages: [user-manager]` on the node), so an exported lab installs the same
packages wherever it is opened, before the router's configuration runs. A lab from an older SILAB that uses
`/user-manager`, `/container` and the like gets those packages the same way.

### Command line

| Command | What it does |
|---|---|
| `silab image latest` / `pull <ver>` / `list` | RouterOS versions from MikroTik |
| `silab lab load <file>` / `export <file>` | Open a lab / save it with every router's config |
| `silab up` / `down` / `status` | Start or stop the whole lab; routers, states, ports |
| `silab node reset\|poweroff\|cycle\|wipe R1` | Power a router like a real one |
| `silab link down\|up R1:1` | Pull or plug a cable |
| `silab link impair R1:1 --delay 50 --loss 2` | Make a link bad on purpose |
| `silab console R1` | Serial console in the terminal (`Ctrl-]` to leave) |
| `silab backup create [--disks]` / `list` | Backups from the command line |

### Pages

| Page | What it does |
|---|---|
| Lab → Lab catalog | Ready-made learning and diagnosis labs: install, open, export your progress |
| Lab → Topology | Draw and run the lab: add nodes, connect ports, double-click a router for console / WinBox / WebFig / SSH / power |
| Lab → Topology → **Lab** | In a catalog lab: its guide, Check my work, hints, Reset lab, Load solution |
| Lab → Bulk actions | One change on every router of the lab |
| System → Helpers | One-time helpers per app: WinBox (right-click a router) and Wireshark (right-click a cable) |
| System → Updates | Installed version, What's new, apply a signed update |
| System → Backup & restore | Create, download, upload and restore backups; daily automatic copies |
| System → Profile | Change the admin password |

### Reaching a router

| Way | Where |
|---|---|
| Console | Double-click the router → Console (browser) |
| WinBox | `<host>:20000 + id×10` (R1 → `:20010`) |
| SSH | `ssh -p <20000 + id×10 + 1> admin@<host>` |
| WebFig | `http://<host>:<20000 + id×10 + 2>` |

---

## Documentation

| Document | Contents |
|---|---|
| [silab.securytik.com/docs](https://silab.securytik.com/docs) | Installation, labs, bulk actions, FAQ |
| [Releases](https://github.com/mhdhaidarah/silab/releases) | Release notes for every version |

---

## Licence

SILAB is proprietary software by SecuryTik; all rights reserved: see [LICENSE](LICENSE).
Releases ship compiled (CPython 3.12, amd64 and arm64).
MikroTik, RouterOS and WinBox are trademarks of MikroTik. RouterOS CHR images are downloaded from MikroTik
under MikroTik's own licence and are never distributed with SILAB. Bundled fonts (Fira Sans, Fira Code,
Inter) are under the SIL Open Font License; xterm.js is MIT.

<div align="center">

Made by [**SecuryTik**](https://securytik.com), the team behind SAMM, SICO and SILA.

</div>
