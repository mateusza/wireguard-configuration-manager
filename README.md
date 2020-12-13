# Wireguard Configuration Manager

## Usage

### `new` - add new interface or peer
* To add new interface: `wgconfmgr new <IFACE>`
* To add new peer: `wgconfmgr new <IFACE> <PEER>`

Adding new interface or peer creates required directories and files:
- `private.key` is automaticaly generated (however it's OK to replace it later)
- `ip.conf`, `networks.conf` are created empty and need to be manually populated with desired values

### `conf` - display configuration file
* Interface configuration: `wgconfmgr conf <IFACE>`
* Peer configuration: `wgconfmgr conf <IFACE> <PEER>`

### `write-conf` - write configuration to a file
* All interfaces: `wgconfmgr write-conf`
* Specific interface: `wgconfmgr write-conf <IFACE>`

Interface configuration is saved to `/etc/wireguard/<IFACE>.conf.d/interface.conf`. This can be later copied, rename or symlinked as `/etc/wireguard/<IFACE>.conf`.

### `qr` - display QR code for quick peer configuration
* `wgconfmgr qr <IFACE> <PEER>`

(`qrencode` tools is used. UTF-8 enabled terminal is required.)

### `ping` - test connection
* `wgconfmgr ping` - ping all peers on all interfaces
* `wgconfmgr ping <IFACE>` - ping all peers on specific interface
* `wgconfmgr ping <IFACE> <PEER>` - ping single peer on specific interface (all addresses)

## Configuration files

### Interface
Interfaces' configuration directories are named `/etc/wireguard/<IFACE>.conf.d`.

#### `private.key`
Private key.

File should be readable only by root.

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

**At least one of `private.key` and `public.key` must be present.**

**The tool doesn't verify if `public.key` matches `private.key` when both are present.**

#### `ip.conf`
IPv4 or IPv6 addresses. One per line.

#### `networks.conf` (optional)
IPv4 or IPv6 networks connected to peer. One per line.

If the network is not a subnet of interface network, it should also be referenced in interface's `network.conf` file.

#### `dns.conf` (optional)
DNS-es to by used by peer. Defaults to interface's `dns.conf` or default values.

## Author
* Mateusz Adamowski

## License
The project is licensed under MIT License.

## See also
* [The Wireguard](https://www.wireguard.com/)
* [Wireguard Vanity Address](https://github.com/warner/wireguard-vanity-address) Tool to generate keypairs with easily recognizable public keys, like `c0ol/adDr/eM/ui8om59pAvR1KgLuhpOV5KC9kWiGGo=`
* [QR Encode](https://fukuchi.org/works/qrencode/) Tool to generate QR codes from command line, able to display them on text console using UTF-8 block characters. Simplifies provisioning of mobile devices.

