# Release Template

## LN6001 iCWMP Runtime

Target: `arm_cortex-a7_neon-vfpv4`

### Assets

Attach reviewed IPK files as release assets. Do not commit IPKs to git.

| File | SHA-256 |
| --- | --- |
| `mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk` | `TODO` |
| `bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk` | `TODO` |
| `icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk` | `TODO` |

### Install

```sh
opkg install ./mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install ./icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
```

### Source

- `bbfdm`: `https://dev.iopsys.eu/bbf/bbfdm.git`
  `a52d79b11fd0cf709d55baa15eee078aa38ddca1`
- `icwmp`: `https://dev.iopsys.eu/bbf/icwmp.git`
  `eb60ced578efa949286df4860bd756b3da6b021c`
- `mxml`: `https://github.com/michaelrsweet/mxml.git`
  `3aaa12c7d709d05286255d191998f29105dd407a`

### Notices

Include the license and notice text required by `notices/THIRD_PARTY_NOTICES.md`.

### Safety

This release does not include ACS credentials, device-specific configuration,
customer data, private logs, firmware images, proprietary SDK contents, or
router secrets.
