# Configuration #

Sigma configuration files are in the fairly simple INI format.  Each section denotes a session, and then a list of key pairs will set the options for that protocol. An example configuration file is shown below.

## General Options ##

**Note:** SigmaVPN is sensitive to the order in which you specify configuration parameters in `sigmavpn.conf`. For example, `proto` should be defined before any other `proto_` options, `local` should be defined before any other `local_` options and `peer` should be defined before any `peer_` options.

| **Option** | **Possible values** | **Description** |
|:-----------|:--------------------|:----------------|
| `proto`    | `raw`, `nacl0`, `nacltai` |  Sets the encoding/encryption protocol to be used. (`nacltai` is equivalent to selecting `TAI64 nonce` on SigmaVPN for Android). |
| `local`    | `tuntap`, `udp`     |  Sets the local interface to use on your system (normally `tuntap`). |
| `peer`     | `tuntap`, `udp`     |  Sets the interface that should be used to communicate with your peer (normally `udp`). |

## `nacl0`/`nacltai`-specific options ##

| **Option** | **Possible values** | **Description** |
|:-----------|:--------------------|:----------------|
| `proto_privatekey` | 64-bit hex          | Your private key (this can be generated using the `naclkeypair` tool; place your private key here, and give the public key to your peer). |
| `proto_publickey` | 64-bit hex          | Your peer's public key  (your peer should give you their generated public key; place it here).|

## `udp`-specific options ##

| **Option** | **Possible values** | **Description** |
|:-----------|:--------------------|:----------------|
| `peer_remoteaddr` | IPv4/IPv6 addr.     | IP address to send traffic to (corresponds with `peer_localaddr` on the remote side).|
| `peer_remoteport` | `1` to `65535`      | UDP port number to send traffic to (corresponds with `peer_localport` on the remote side). |
| `peer_remotefloat` | `0` or `1`          | Automatically update the remote endpoint when receiving correctly encrypted packets. This allows the remote client to change IP address or port number, and still communicate with this tunnel. (If you are setting up a tunnel for use with SigmaVPN on Android, you will probably need to use this instead of specifying `peer_remoteaddr` and `peer_remoteport`). _Available since version 0.2. You may need to compile the latest source code from git for this option to be available._ |
| `peer_localaddr` | IPv4/IPv6 addr.     | IP address to listen for traffic on (corresponds with `peer_remoteaddr` on the remote side).|
| `peer_localport` | `1` to `65535`      | UDP port number to listen for traffic on (corresponds with `peer_remoteport` on the remote side). |
| `peer_ipv6` | `0` or `1`          | Specifies if the UDP connection should use IPv6. Both local and remote addresses must be specified in IPv6 format. If unspecified, IPv4 is assumed. (Please note that this option determines whether the tunnel itself should be carried over IPv6, not whether IPv6 traffic is contained inside the tunnel.) |

## `tuntap`-specific options ##

| **Option** | **Possible values** | **Description** |
|:-----------|:--------------------|:----------------|
| `local_interface` | String              | Depending on operating system, a valid network interface name (Linux 2.6+, i.e. `mytunnel`) or the path to a TUN or TAP device (Mac OS X, i.e. `/dev/tun0`).|
| `local_tunmode` | `0` or `1`          | Whether to use a TUN layer 3 adapter instead of a TAP layer 2 adapter (Linux 2.6+ only). TUN adapters carry only IP packets instead of Ethernet frames. Required if the other side is a SigmaVPN for Android client. |
| `local_protocolinfo` | `0` or `1`          | Whether to include the protocol information header when using TUN mode (Linux 2.6+ only). This is desirable - or maybe even necessary - if you wish to carry both IPv4 and IPv6 over a TUN layer 3 tunnel. Don't use this option if the other side is a SigmaVPN for Android client, even if tunnelling IPv6. |

## Example configuration ##
```
[peername]
proto = nacl0
proto_publickey = 1e22c6af59f23cd3c40464c29e307cbd616e3f6a743f17a33dd6bd0ae4c79e71
proto_privatekey = cf1d8756fdde0f73f0c06f7c3f4cf456c3d74596b9e559570cf27d8b34059121
local = tuntap
local_interface = /dev/tap0
peer = udp
peer_remoteaddr = 10.0.1.1
peer_remoteport = 4567
peer_localaddr = 10.0.2.1
peer_localport = 7654
```

On Linux 2.6+, it is likely that `local_interface` will just be a device name like "`tunnel`" rather than a full path like "`/dev/tap0`".