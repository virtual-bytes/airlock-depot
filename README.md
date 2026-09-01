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
