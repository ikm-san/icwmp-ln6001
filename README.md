# icwmp-ln6001

LN6001/QSDK 12.2 IPK release area for the BBFDM and iCWMP runtime packages
used in early BBF/TR-069 bring-up.

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

## Install Order

Install `mxml` first, then `bbfdm`, then `icwmp`:

```sh
opkg install ./mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
```

These packages only provide the runtime components. ACS configuration,
credentials, service enablement, and smoke testing should be handled by the
owning LN6001/Kaname workflow, not by publishing secrets or device-specific
configuration in this repository.

## Source And Provenance

The package metadata used for this release points to:

- `bbfdm`: `https://dev.iopsys.eu/bbf/bbfdm.git`
  at `a52d79b11fd0cf709d55baa15eee078aa38ddca1`
- `icwmp`: `https://dev.iopsys.eu/bbf/icwmp.git`
  at `eb60ced578efa949286df4860bd756b3da6b021c`
- `mxml`: `https://github.com/michaelrsweet/mxml.git`
  at `3aaa12c7d709d05286255d191998f29105dd407a`

If binary assets are published, the release notes should include:

- exact filenames
- SHA-256 checksums
- build date and QSDK target
- applied downstream patches
- links to corresponding source and license texts
- any GPL source offer or source bundle required for `mxml`

## License

There is no single upstream license for all release assets. See
[`LICENSE`](LICENSE) and [`notices/THIRD_PARTY_NOTICES.md`](notices/THIRD_PARTY_NOTICES.md).
