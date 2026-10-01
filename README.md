<div align="center">

<img src="silab-logo.svg" width="96" alt="SILAB logo" />

# SILAB

### SecuryTik Interactive Laboratory

**MikroTik network labs in your browser: real RouterOS CHR on plain QEMU, no VT-x required on Linux.**

[**silab.securytik.com**](https://silab.securytik.com) &nbsp;·&nbsp; [Report a bug](mailto:silab@securytik.com?subject=SILAB%20Bug%20Report)

</div>

---

This repository holds SILAB's **releases** only: the signed, compiled bundles that installs and updates
download. The source is not published.

## Install

**Ubuntu 22.04, 24.04 or 26.04 (amd64 or arm64)**, as root:

```bash
curl -fsSL https://silab.securytik.com/install.sh | sudo bash
```

**Windows 10/11** (WSL2), in PowerShell as administrator:

```powershell
irm https://silab.securytik.com/install.ps1 | iex
```

Full guide, labs and FAQ: [silab.securytik.com/docs](https://silab.securytik.com/docs).

## Releases

Each [release](https://github.com/mhdhaidarah/silab/releases) carries:

| File | What it is |
|---|---|
| `silab-<version>.tar.gz` | The bundle: SILAB compiled for Python 3.12, amd64 and arm64 |
| `manifest.jws` | The release manifest, signed with SILAB's release key; installs and updates verify it |
| `install.sh` | The Linux installer bootstrap (checks the manifest and the bundle's SHA256) |
| `install.ps1`, `silab-windows.ps1` | The Windows installer |

The same files are served from `dl.securytik.com/silab`; this page is the fallback the updater uses when
that mirror cannot be reached.

## Licence

SILAB is proprietary software by SecuryTik; all rights reserved: see [LICENSE](LICENSE). RouterOS CHR images
are downloaded from MikroTik under MikroTik's own terms and are never distributed with SILAB.
