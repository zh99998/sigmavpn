# Introduction #

SigmaVPN can support multiple concurrent tunnels, with flexible endpoint/protocol settings. Typically, configuration will be placed into a `sigmavpn.conf` file, in INI format.

## Protocols ##

Protocols specify how packets should be encoded/decoded or encrypted/decrypted before being sent and received. Right now there are a couple of protocols:
  * `nacltai`: strong temporal encryption based on `curve25519xsalsa20poly1305 `, as provided by the [NaCl library](http://nacl.cace-project.eu/)
  * `nacl0`: weaker encryption based on `curve25519xsalsa20poly1305 `, as provided by the [NaCl library](http://nacl.cace-project.eu/), but really should not be used and will probably be removed
  * `raw`: unencrypted raw packets

## Interfaces ##

Interfaces specify the inputs and outputs of the SigmaVPN executable. The following interfaces currently exist:
  * `udp`: the UDP protocol
  * `tuntap`: the TUN/TAP driver interface, for creating virtual network devices

## Local and Remote Interfaces ##

Typically you have two interfaces: a _local interface_, and a _remote interface_.

The _local interface_ is your entry point to the tunnel, and the remote interface generally carries the tunnel itself. Therefore, the local side is generally unencrypted/unencoded, and the remote side is encrypted/encoded.

Moving packets between the two interfaces, and encrypting/decrypting them, is handled by Sigma.

```
you <-> local <-> protocol <-> remote <-> ...... <-> remote <-> protocol <-> local <-> peer
```

## Protocols and Interfaces together ##

Typically, a configuration will have two interfaces (one local and one remote), and one protocol. Let's look at a typical encrypted VPN setup:

  * local `tuntap` interface
  * `nacltai` protocol
  * remote `udp` interface

This configures a tunnel so that:

  * packets from the local TUN/TAP interface will be encrypted using `nacltai`, and then sent to your peer via UDP
  * packets received from your peer via UDP will be decrypted using `nacltai`, and then sent to your local TUN/TAP interface