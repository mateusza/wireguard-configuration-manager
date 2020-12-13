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

Interface configuration is saved to `/etc/wireguard/<IFACE>.conf.d/interface.conf`.

### displaying QR code for peer configuration
* `wgconfmgr qr <IFACE> <PEER>`

(`qrencode` tools is required)

## Configuration files

### Interface

#### `private.key`
Private key. File should be readable only by root.

#### `public.key` (optional)
Public key.

If missing, `wg pubkey` command will be used in runtime.

#### `ip.conf`
IPv4 or IPv6 address. One per line.

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

#### `private.key` (optional)
Private key. Optional.

If missing, a placeholder will be printed in peer's config file.

Required for QR code generation.

#### `public.key` (conditionally optional)
Public key. Optional, if `private.key` is provided.

**At least one of `private.key` or `public.key` must be provided.**

**The tool doesn't verify if `public.key` matches `private.key` when both are provided.**

#### `ip.conf`
IPv4 or IPv6 address. One per line.

#### `networks.conf` (optional)
IPv4 or IPv6 networks that are connected to peer. One per line.

If the network is not a subnet of interface network, it should also be included in interface's `network.conf` file.

#### `dns.conf` (optional)
DNS-es to by used by peer. Defaults to interface's `dns.conf` or defaults values.

## Authors

* Mateusz Adamowski

## License

The project is licensed under MIT License.
