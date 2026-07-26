# Linksys Velop WRT Pro 7 (LN6001) iCWMP and GenieACS IPKs

The Linksys Velop WRT Pro 7 (LN6001) is an OpenWrt based Wi-Fi 7 router for
home labs, SOHO networks, small offices, and integrators who want Linksys
hardware with LuCI, SSH, opkg packages, and scriptable network configuration.
Linksys positions the Japanese LN6001-JP model as a home and business
OpenWrt based Wi-Fi 7 router with tri-band wireless, a 1.5 GHz quad-core
Qualcomm platform, a 2.5 Gbps internet port, and four 1 Gbps Ethernet ports.

Product page: [Linksys Velop WRT Pro 7 OpenWrt Router WiFi 7 LN6001-JP](https://support.linksys.com/kb/article/6274-jp/)

This repository publishes the complete LN6001/QSDK 12.2 first-Inform package
set for BBFDM and iCWMP. The three runtime IPKs provide BBFDM, iCWMP, and
Mini-XML. The companion `bbf-agent` IPK adds the LN6001 identity mappings,
JSON service descriptors, runtime plugin links, and safe ACS setup/smoke
commands required for the proven GenieACS connection path.

Setup guide: [Connect Linksys LN6001 to GenieACS](GENIEACS_SETUP.md)

This is not a firmware image, a complete TR-069 management platform, or an ACS
server. It provides the runtime packages plus source, checksum, setup, and
license materials needed to install and audit the LN6001 first-Inform stack.

## Download IPKs

Latest complete bundle:
[`ln6001-q12.2-genieacs-20260726`](https://github.com/ikm-san/icwmp-ln6001/releases/tag/ln6001-q12.2-genieacs-20260726)

Direct downloads:

- [`mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-genieacs-20260726/mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk)
- [`bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-genieacs-20260726/bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk)
- [`icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-genieacs-20260726/icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk)
- [`bbf-agent_26.4.0726_all.ipk`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-genieacs-20260726/bbf-agent_26.4.0726_all.ipk)
- [`SHA256SUMS-2026-07-26.txt`](https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-genieacs-20260726/SHA256SUMS-2026-07-26.txt)

Download from a shell:

```sh
BASE_URL="https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-genieacs-20260726"

wget "$BASE_URL/mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk"
wget "$BASE_URL/bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk"
wget "$BASE_URL/icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk"
wget "$BASE_URL/bbf-agent_26.4.0726_all.ipk"
wget "$BASE_URL/SHA256SUMS-2026-07-26.txt"

sha256sum -c SHA256SUMS-2026-07-26.txt
```

## Packages

| Package | Version | Target | Role | License metadata |
| --- | --- | --- | --- | --- |
| `mxml` | `2.12-1` | `arm_cortex-a7_neon-vfpv4` | CWMP XML library | QSDK metadata: `GPL-2.0`; source `COPYING`: LGPL2 with exceptions |
| `bbfdm` | `7.5-1` | `arm_cortex-a7_neon-vfpv4` | TR-181 data-model backend | `BSD-3-Clause` |
| `icwmp` | `7.5-1` | `arm_cortex-a7_neon-vfpv4` | TR-069/CWMP client | `BSD-3-Clause` |
| `bbf-agent` | `26.4.0726` | `all` | LN6001 identity, descriptors, and ACS helpers | `Proprietary` |

## Install

Install the runtime packages first and the LN6001 companion package last:

```sh
opkg install ./mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./bbf-agent_26.4.0726_all.ipk
```

Continue with [GENIEACS_SETUP.md](GENIEACS_SETUP.md) to seed LN6001 identity,
configure the ACS URL, start the controlled smoke window, and verify the first
Inform in GenieACS.

Do not publish ACS credentials, router credentials, serial numbers, MAC
addresses, private logs, or customer data.

## What Provides The LN6001 Data Model

A separate LN6001 XML file is not required for the first connection. The
package set uses:

- `/etc/config/cwmp` for ACS and CPE identity values
- `/etc/bbfdm/services/*.json` for BBF service registration
- `/usr/share/bbfdm/micro_services/sysmngr.json` for LN6001 TR-181 mappings
- `mxml` for internal CWMP SOAP XML serialization/parsing
- `bbf-agent` for identity defaults, runtime links, readiness, and smoke proof

## Source And Provenance

The release includes IPKs, SHA-256 checksums, upstream source archives, and the
LN6001 downstream package-feed patch bundle.

- `bbfdm`: `https://dev.iopsys.eu/bbf/bbfdm.git`
  at `a52d79b11fd0cf709d55baa15eee078aa38ddca1`
- `icwmp`: `https://dev.iopsys.eu/bbf/icwmp.git`
  at `eb60ced578efa949286df4860bd756b3da6b021c`
- `mxml`: `https://github.com/michaelrsweet/mxml.git`
  at `3aaa12c7d709d05286255d191998f29105dd407a`

The `icwmp` build includes the LN6001 QSDK 12.2 connection-request address
length and Mini-XML serialization fixes required by the successful GenieACS
first-Inform proof.

## Current Scope

The current release proves outbound first Inform and periodic Inform. The
CWMP firewall hook remains intentionally non-mutating, so ACS-initiated
connection requests are not yet claimed as ready. Tasks may wait for the next
periodic Inform.

Generic TR-069 writes are also intentionally restricted. See
[GENIEACS_SETUP.md](GENIEACS_SETUP.md) for the exact supported slice.

## License

There is no single upstream license for all release assets. Repository-authored
documentation and metadata use the MIT terms in [LICENSE](LICENSE). Binary
IPKs retain their package-specific licenses. The `bbf-agent` package metadata
is `Proprietary` and is distributed as a binary LN6001 companion; no source
license is granted by this repository.

See [Third-Party Notices](notices/THIRD_PARTY_NOTICES.md) and
[upstream license texts](notices/licenses/).
