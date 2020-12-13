# Wireguard Connection Manager

## Usage

### adding new
* Adding new interface: `wgconfmgr new <IFACE>`
* Adding new peer: `wgconfmgr new <IFACE> <PEER>`

### displaying configuration files
* Interface configuration: `wgconfmgr conf <IFACE>`
* Peer configuration: `wgconfmgr conf <IFACE> <PEER>`

### writing configuration to file
* All interfaces: `wgconfmgr write-conf`
* Specific interface: `wgconfmgr write-conf <IFACE>`

Interface configuration is saved to `/etc/wireguard/<IFACE>.conf.d/interface.conf`. This can be later copied, rename or symlinked as `/etc/wireguard/<IFACE>.conf`.

### displaying QR code for peer configuration
* `wgconfmgr qr <IFACE> <PEER>`

(`qrencode` tools is required)

### testing connection to peers
* `wgconfmgr ping`
* `wgconfmgr ping <IFACE>`
* `wgconfmgr ping <IFACE> <PEER>`

## Configuration files

### Interface

Interfaces' configuration directories are named `/etc/wireguard/<IFACE>.conf.d`.

#### `private.key`
Private key. File should be readable only by root.

#### `public.key` (optional)
Public key.

If missing, `wg pubkey` command will be used in runtime.

#### `ip.conf`
IPv4 or IPv6 addresses. One per line.

#### `networks.conf`
IPv4 or IPv6 networks. One per line.

List should include networks matching assigned interfaces and optionally additional networks that should be accessible by peers.

#### `endpoint.conf`
Endpoint hostname or IP address of the server.

#### `port.conf` (optional)
Listening port. Defaults to `51820`

#### `fwmark.conf` (optional)
Firewall mark id. Defaults to `0xca6c`

#### `dns.conf` (optional)
DNS-es to by used by peers. Defaults to `1.1.1.1` and `8.8.8.8` (Cloudflare and Google public nameservers).

#### `opts.conf` (optional)
Optional entries to be concatenated into interface configuration file.

Example:
```
SaveConfig = false
```

### Peer

Peers' configuration directories are named `/etc/wireguard/<IFACE>.conf.d/peers.d/<PEER>`

#### `private.key` (optional)
Private key. Optional.

If missing, a placeholder will be printed in peer's config file.

Only required for full configuration file generation on a server, including QR code generation.

#### `public.key` (conditionally optional)
Public key. Optional, if `private.key` is provided.

**At least one of `private.key` or `public.key` must be provided.**

**The tool doesn't verify if `public.key` matches `private.key` when both are provided.**

#### `ip.conf`
IPv4 or IPv6 addresses. One per line.

#### `networks.conf` (optional)
IPv4 or IPv6 networks that are connected to peer. One per line.

If the network is not a subnet of interface network, it should also be included in interface's `network.conf` file.

#### `dns.conf` (optional)
DNS-es to by used by peer. Defaults to interface's `dns.conf` or defaults values.

## Author

* Mateusz Adamowski

## License

The project is licensed under MIT License.

## See also
* [The Wireguard](https://www.wireguard.com/)
* [Wireguard Vanity Address](https://github.com/warner/wireguard-vanity-address) Tool to generate keypairs with easily recognizable public keys, like `c0ol/adDr/eM/ui8om59pAvR1KgLuhpOV5KC9kWiGGo=`
* [QR Encode](https://fukuchi.org/works/qrencode/) Tool to generate QR codes from command line, able to display them on text console using UTF-8 block characters. Simplifies provisioning of mobile devices.
