# Third-Party Notices

Date: 2026-07-26

## Scope

This file records the current license/provenance checklist for the LN6001
BBFDM/iCWMP and bbf-agent IPK distribution. It is not legal advice and does not replace the
full upstream license texts.

## bbfdm

- Package: `bbfdm`
- Version: `7.5-1`
- Target: `arm_cortex-a7_neon-vfpv4`
- Local package license metadata: `BSD-3-Clause`
- Upstream source: `https://dev.iopsys.eu/bbf/bbfdm.git`
- Source revision: `a52d79b11fd0cf709d55baa15eee078aa38ddca1`
- Included license text: `notices/licenses/bbfdm-BSD-3-Clause.txt`
- Downstream patches applied in the LN6001 package feed:
  - `010-compat-old-ubus-status-codes.patch`
  - `020-include-sys-stat-for-utilities.patch`
  - `030-use-size-t-format-for-service-count.patch`

Release assets should reproduce the upstream BSD-3-Clause copyright and
license notice.

## icwmp

- Package: `icwmp`
- Version: `7.5-1`
- Target: `arm_cortex-a7_neon-vfpv4`
- Local package license metadata: `BSD-3-Clause`
- Upstream source: `https://dev.iopsys.eu/bbf/icwmp.git`
- Source revision: `eb60ced578efa949286df4860bd756b3da6b021c`
- Included license text: `notices/licenses/icwmp-BSD-3-Clause.txt`
- Downstream patches applied in the LN6001 package feed:
  - `010-include-sys-stat-for-mkdir.patch`
  - `020-fix-download-c99-switch-declarations.patch`
  - `030-use-json-object-get-ex.patch`
  - `040-fix-connection-request-accept-addrlen.patch`
  - `050-use-mxml-allocated-string-save.patch`

Release assets should reproduce the upstream BSD-3-Clause copyright and
license notice.

## mxml

- Package: `mxml`
- Version: `2.12-1`
- Target: `arm_cortex-a7_neon-vfpv4`
- Local QSDK package license metadata: `GPL-2.0`
- Local QSDK license file marker: `COPYING`
- Upstream source: `https://github.com/michaelrsweet/mxml.git`
- Source revision: `3aaa12c7d709d05286255d191998f29105dd407a`
- Included source license text: `notices/licenses/mxml-COPYING.txt`

Because this binary package is marked GPL-2.0 in the local QSDK package
metadata, but the exact upstream `COPYING` file at the shipped source revision
states the Mini-XML LGPL2-with-exceptions license, public binary distribution
should include the exact `COPYING` text and corresponding source availability
for the shipped build.

Note: current upstream Mini-XML materials describe newer Mini-XML releases as
Apache-2.0 with an optional exception, but this LN6001 package uses the older
`mxml` 2.12 source revision listed above.

## bbf-agent

- Package: `bbf-agent`
- Version: `26.4.0726`
- Target: `all`
- Local package license metadata: `Proprietary`
- Role: LN6001 identity mappings, BBF JSON descriptors, runtime links, ACS
  configuration helper, and first-Inform smoke/status commands

The repository MIT license applies only to repository-authored documentation
and metadata. It does not relicense the `bbf-agent` binary. The public release
provides the binary LN6001 companion package without granting a source-code
license or redistribution rights beyond the package owner's terms.

## Release Checklist

- Do not commit `.ipk` files to git history.
- Attach reviewed IPKs as GitHub Release assets only.
- Publish SHA-256 checksums beside release assets.
- Include full upstream license texts for open-source packages and an explicit
  proprietary notice for `bbf-agent`.
- Include source links and exact source revisions.
- Include downstream patch names or a source bundle for the exact shipped
  build, especially for `mxml`.
- Do not publish ACS URLs, credentials, device serials, MAC addresses, private
  logs, proprietary SDK contents, or customer/device-specific configuration.
