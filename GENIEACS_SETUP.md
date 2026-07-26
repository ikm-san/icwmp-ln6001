# Connect Linksys LN6001 to GenieACS

This guide installs the LN6001 BBF/iCWMP runtime and performs a controlled
first CWMP Inform to GenieACS.

## Scope

Tested target:

- Linksys Velop WRT Pro 7 (LN6001)
- LN6001 QSDK 12.2 package target: `arm_cortex-a7_neon-vfpv4`
- GenieACS 1.2.16 first-Inform flow
- TR-181 root data model

This procedure proves outbound CPE-to-ACS registration and periodic Inform.
ACS-initiated connection requests are not yet part of the proven LN6001 slice;
see [Current limitations](#current-limitations).

## No LN6001 XML File Is Required

GenieACS does not need a separate LN6001 XML file for the first connection.

The LN6001 data-model setup is supplied by:

- UCI configuration in `/etc/config/cwmp`
- BBF service descriptors in `/etc/bbfdm/services/*.json`
- LN6001 identity mappings in
  `/usr/share/bbfdm/micro_services/sysmngr.json`
- BBF runtime plugins under `/usr/share/bbfdm/micro_services/`
- `mxml`, which iCWMP uses internally to create and parse CWMP SOAP XML

The `bbf-agent` package installs the LN6001-specific descriptors and seeds
the required identity values such as manufacturer, OUI, product class, model,
serial number, hardware/software version, and time status.

## Packages

Install all four packages from the
[latest GenieACS bundle](https://github.com/ikm-san/icwmp-ln6001/releases/tag/ln6001-q12.2-genieacs-20260726):

1. `mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk`
2. `bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk`
3. `icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk`
4. `bbf-agent_26.4.0726_all.ipk`

The first three packages provide the portable BBF/iCWMP runtime. The
`bbf-agent` package provides the LN6001 identity, JSON descriptors, runtime
plugin links, startup helper, and safe ACS configuration/smoke commands.

## Prepare GenieACS

Install and start GenieACS using the
[official installation guide](https://docs.genieacs.com/en/stable/installation-guide.html).

For a default lab installation:

- CWMP listener: TCP `7547`
- Web UI: TCP `3000`
- NBI API: TCP `7557`
- File server: TCP `7567`

The LN6001 must be able to reach the GenieACS host on TCP port `7547`.
Use an address reachable from the router, not `127.0.0.1` or a WSL-only NAT
address. Open the host firewall for the CWMP listener.

For the first isolated lab test, GenieACS accepts incoming CPE connections by
default. Configure `cwmp.auth` in GenieACS before using authentication in a
production or shared environment. See the
[GenieACS CPE authentication guide](https://docs.genieacs.com/en/stable/cpe-authentication.html).

## Download On A Workstation

```sh
BASE_URL="https://github.com/ikm-san/icwmp-ln6001/releases/download/ln6001-q12.2-genieacs-20260726"

curl -LO "$BASE_URL/mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk"
curl -LO "$BASE_URL/bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk"
curl -LO "$BASE_URL/icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk"
curl -LO "$BASE_URL/bbf-agent_26.4.0726_all.ipk"
curl -LO "$BASE_URL/SHA256SUMS-2026-07-26.txt"

sha256sum -c SHA256SUMS-2026-07-26.txt
```

Copy the packages to the router:

```sh
scp *.ipk root@ROUTER_IP:/tmp/
```

Replace `ROUTER_IP` with the LN6001 management address.

## Install On LN6001

Keep a local management path available during the first test.

```sh
ssh root@ROUTER_IP
cp /etc/config/cwmp "/etc/config/cwmp.before-genieacs.$(date +%Y%m%d-%H%M%S)" 2>/dev/null || true

opkg install /tmp/mxml_2.12-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install /tmp/bbfdm_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install /tmp/icwmp_7.5-1_arm_cortex-a7_neon-vfpv4.ipk
opkg install /tmp/bbf-agent_26.4.0726_all.ipk

/etc/uci-defaults/90_bbf_agent
```

The defaults script is idempotent: it fills missing LN6001 identity values and
keeps existing non-empty identity values.

## Configure The ACS

Replace `ACS_HOST` with an address reachable from the LN6001.

Review the plan first:

```sh
bbf-agent acs-config-plan --json \
  url=http://ACS_HOST:7547 \
  periodic_interval=300
```

Apply the lab profile:

```sh
bbf-agent acs-config-apply --json \
  url=http://ACS_HOST:7547 \
  periodic_interval=300
```

For a GenieACS deployment that requires CPE-to-ACS authentication, add:

```text
username=YOUR_CPE_USERNAME password=YOUR_CPE_PASSWORD
```

The helper maps those inputs to iCWMP's `cwmp.acs.userid` and
`cwmp.acs.passwd` UCI options. Do not paste real credentials into issues,
README files, screenshots, or support bundles.

Check non-secret configuration:

```sh
uci -q get cwmp.acs.url
uci -q get cwmp.cpe.enable
uci -q get cwmp.cpe.product_class
uci -q get cwmp.cpe.model_name
uci -q get cwmp.cpe.serial_number
uci -q get cwmp.cpe.time_status
uci -q get cwmp.lwn.enable
```

Expected identity includes `product_class=LN6001`,
`model_name=LN6001`, a non-empty serial number, synchronized time status, and
`lwn.enable=0`.

## Start The First Inform

Start the BBF data-model services:

```sh
/etc/init.d/bbfdm-sysmngr restart
/etc/init.d/bbfdmd restart
```

Review readiness:

```sh
bbf-agent acs-smoke-plan --json
```

Start the controlled smoke window:

```sh
bbf-agent acs-smoke-start --json
```

The smoke command restarts only:

- `bbfdm-sysmngr`
- `bbfdmd`
- `icwmpd`

It does not change WAN settings, reboot the router, enable XMPP/LWN, or expose
raw ACS credentials.

## Verify In GenieACS

Open the GenieACS UI:

```text
http://ACS_HOST:3000
```

Confirm that a device appears with LN6001 identity and a recent
`_lastInform` timestamp.

On the router, collect the redacted status:

```sh
bbf-agent status --json
bbf-agent device-info --json
bbf-agent management-server --json
bbf-agent runtime-smoke --json
```

A successful run should report:

- all four packages installed
- BBF services `bbfdm`, `bbfdm.core`, `bbfdm.icwmp`,
  `bbfdm.sysmngr`, and `tr069`
- all required descriptor files present
- TR-181 DeviceInfo reads available
- `acs_inform_accept_seen=true` or `first_inform_seen=true`

## Troubleshooting

### missing-runtime

Confirm that all four IPKs are installed and that the descriptor files exist:

```sh
opkg status mxml
opkg status bbfdm
opkg status icwmp
opkg status bbf-agent
bbf-agent status --json
```

### bootstrap-needed

The ACS URL is empty or the CPE is disabled. Re-run
`bbf-agent acs-config-plan` and `bbf-agent acs-config-apply`.

### inform-attempt-no-acs-accept

Check:

- LN6001 routing to `ACS_HOST:7547`
- GenieACS `genieacs-cwmp` service status
- host firewall rules
- HTTP versus HTTPS URL
- CPE-to-ACS authentication settings
- that the ACS URL does not use an unreachable WSL/private address

### icwmpd exits before sending HTTP

Use the `icwmp_7.5-1` package from this release. It includes the LN6001
QSDK 12.2 fixes for connection-request address length and Mini-XML string
serialization that were required for the successful GenieACS first Inform.

## Current Limitations

This release proves outbound first Inform and periodic Inform. It does not yet
claim complete TR-069 management parity.

- The CWMP firewall hook is intentionally non-mutating.
- ACS-initiated connection requests are not proven.
- XMPP/LWN connection requests are disabled.
- The initial custom writable TR-181 leaf is
  `Device.DeviceInfo.ProvisioningCode`.
- LN6001 identity, GatewayInfo, and Time status are read-only.
- Router settings, Wi-Fi mutation, package changes, raw UCI, and shell commands
  are not exposed as generic TR-069 writes.

Tasks created in GenieACS may therefore wait for the next periodic Inform
instead of waking the CPE immediately.

## Stop Or Roll Back

Stop and disable the CWMP client:

```sh
/etc/init.d/icwmpd stop
/etc/init.d/icwmpd disable
uci set cwmp.cpe.enable='0'
uci commit cwmp
```

To restore a saved configuration, identify the backup created before setup or
the protected backup recorded by `bbf-agent acs-config-apply`, restore it to
`/etc/config/cwmp`, and restart only the BBF/iCWMP services.

Do not publish the backup: it may contain ACS credentials.
