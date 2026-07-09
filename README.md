# Linksys Velop WRT Pro 7 (LN6001) iCWMP IPKs

The Linksys Velop WRT Pro 7 (LN6001) is an OpenWrt based Wi-Fi 7 router for
home labs, SOHO networks, small offices, and integrators who want Linksys
hardware with LuCI, SSH, opkg packages, and scriptable network configuration.
Linksys positions the Japanese LN6001-JP model as a home and business
OpenWrt based Wi-Fi 7 router with tri-band wireless, a 1.5 GHz quad-core
Qualcomm platform, a 2.5 Gbps internet port, and four 1 Gbps Ethernet ports.

Product page: [Linksys Velop WRT Pro 7 OpenWrt Router WiFi 7 LN6001-JP](https://support.linksys.com/kb/article/6274-jp/)

This repository publishes LN6001/QSDK 12.2 IPK release assets for BBFDM and
iCWMP. These packages add the runtime components used for TR-181/TR-069/CWMP
remote-management bring-up, including ACS connectivity testing and
managed-WiFi integration work.

This is not a firmware image, a full managed-WiFi stack, or an ACS
configuration package. It only provides the runtime IPKs plus the source,
checksum, and license materials needed to install and audit the BBF/iCWMP
components on a compatible LN6001 build.

## Download IPKs

Latest release: [`ln6001-q12.2-20260709`](https://github.com/ikm-san/icwmp-ln6001/releases/tag/ln6001-q12.2-20260709)

Direct downloads:

- [`mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-20260709/mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk)
- [`bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-20260709/bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk)
- [`icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-20260709/icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk)
- [`SHA256SUMS-2026-07-09.txt`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-20260709/SHA256SUMS-2026-07-09.txt)

Download from a shell:

```sh
wget https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-20260709/mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk
wget https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-20260709/bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
wget https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-20260709/icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
wget https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-20260709/SHA256SUMS-2026-07-09.txt
sha256sum -c SHA256SUMS-2026-07-09.txt
```

## Packages

The current LN6001 set is:

| Package | Version | Target | License metadata |
| --- | --- | --- | --- |
| `bbfdm` | `7.5-1` | `arm_cortex-a7_neon-vfpv4` | `BSD-3-Clause` |
| `icwmp` | `7.5-1` | `arm_cortex-a7_neon-vfpv4` | `BSD-3-Clause` |
| `mxml` | `2.12-1` | `arm_cortex-a7_neon-vfpv4` | `GPL-2.0` in the QSDK package metadata |

## Install

Install `mxml` first, then `bbfdm`, then `icwmp`:

```sh
opkg install ./mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
```

After installation, ACS URL, credentials, service enablement, and smoke testing
should be handled by the owning LN6001/Kaname workflow. Do not publish
device-specific ACS configuration, router credentials, serial numbers, MAC
addresses, private logs, or customer data in this repository.

## Source And Provenance

The release includes IPKs, SHA-256 checksums, upstream source archives, and the
LN6001 downstream package-feed patch bundle. Source revisions used for the
published packages:

- `bbfdm`: `https://dev.iopsys.eu/bbf/bbfdm.git`
  at `a52d79b11fd0cf709d55baa15eee078aa38ddca1`
- `icwmp`: `https://dev.iopsys.eu/bbf/icwmp.git`
  at `eb60ced578efa949286df4860bd756b3da6b021c`
- `mxml`: `https://github.com/michaelrsweet/mxml.git`
  at `3aaa12c7d709d05286255d191998f29105dd407a`

Release materials include:

- exact filenames
- SHA-256 checksums
- build date and QSDK target
- applied downstream patches
- links to corresponding source and license texts
- source archives for the shipped upstream revisions

## License

There is no single upstream license for all release assets. See
[`LICENSE`](LICENSE) and [`notices/THIRD_PARTY_NOTICES.md`](notices/THIRD_PARTY_NOTICES.md).
