# Airlock Depot

A **Virtual Bytes** tool · https://virtualbytes.io · https://tools.virtualbytes.io

Airlock Depot is a virtual appliance (Photon OS 5) that productizes the
VMware Cloud Foundation 9.x **offline depot**: a connected appliance
downloads and packages depot content, signed transfer media crosses the
air gap, and a dark-site appliance verifies every chunk and serves VCF
consumers (SDDC Manager, VCF Installer) inside the gap.

## Downloads

Grab the OVAs from the **[Releases](../../releases)** page:

- `airlock-depot-<version>.ova` — dual-configuration OVA; pick
  **Connected** or **Disconnected (Dark Site)** in the vSphere deploy
  wizard.
- `airlock-depot-<version>-darksite.ova` — restricted single-configuration
  descriptor for separately delivered high-side media. Same disk bits;
  only the descriptor differs.

### Verify before deploying

Each OVA ships with `.sha256` and `.sha512` files attached to the release:

```
sha256sum -c airlock-depot-<version>.ova.sha256
```

## What's new in 2.0.4

Since 2.0.0 the appliance gained:

- **In-place service patching.** Settings → Appliance updates takes a signed
  service patch, verifies it against the key built into the appliance,
  snapshots what it replaces, installs atomically, restarts the API and
  health-checks it, rolling back on its own if the new version does not come
  up. Roll back is one button; every step is audited. The depot on `/data`,
  the database, configuration and TLS material are never touched. Photon OS
  packages are not covered; those arrive with a new OVA.
- **GUI accounts.** Administrators add, remove and reset `admin` or
  `operator` accounts from Settings. Operators can do everything except
  account management, appliance updates and the classification banner, and
  they list and revoke only the API tokens they created.
- **Classification banner by role.** Until an administrator selects the
  network once, every screen (including sign-in) shows a grey
  **SAMPLE — CLASSIFICATION NOT SET** band; it is a placeholder, not a
  marking, and disappears once the level is set. A connected
  (Internet-facing) depot offers *None* and *UNCLASSIFIED* only; the
  dark-site deployment offers the full marking table (UNCLASSIFIED, SECRET,
  TOP SECRET, TOP SECRET//SAP). Only administrators can set or change it.
- **SSH maintenance window** opened from the GUI as an audited action and
  closed again on time; SSH stays key-only.
- **Resilience.** `/data` is mounted by UUID with `nofail`, so a detached or
  re-enumerated data disk no longer stops the next boot in emergency mode.
- **Exports** show the on-disk package directory, and the build result stays
  on screen after a build.
- **Build transparency.** The image build produces an SBOM of the sealed
  image (Photon packages plus the API's Go modules) and scans it; the API is
  built with a current Go toolchain.

Appliances deployed from an earlier OVA keep working; redeploy from the
2.0.4 OVA to get the patching mechanism.

## Deploying

Deploy the OVA in vCenter, choose the deployment configuration (role), and
fill in the vApp properties (FQDN, IP/CIDR, gateway, DNS, NTP, admin
password). The role is fixed by the configuration you pick — first boot
stamps it, configures the network, and generates keys and TLS. The
appliance UI walks you through the rest via Express Setup.

## Disclaimer

Airlock Depot is provided by Virtual Bytes **AS IS**, without warranty of
any kind, express or implied. **No support, maintenance, updates, or
service levels are offered or implied.** Use at your own risk. Virtual
Bytes accepts no liability for any damages arising from its use. This is
a community tool, not a Virtual Bytes product offering; see
[LICENSE](LICENSE) for the full terms.

## Licensing

[MIT License](LICENSE). The appliance bundles third-party components under
their own licenses, inventoried in
[THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md).
