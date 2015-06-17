# Configuring a secure tunnel #

Sigma allows easy secure tunnels when compiled with the appropriate libraries. This quick how-to guide explains how to create a secure tunnel using the [NaCl cryptographic library](http://nacl.cace-project.eu/) over UDP transport.

## 1. Build with `nacltai` ##

Check that you have the `proto_nacltai.o` file in your modules folder, and the `naclkeypair` command available. Otherwise, rebuild using the `--with-nacl` option.

## 2. Prepare your configuration file ##

Find and open your `sigmavpn.conf` file, and create a new section using the following template:
```
[peername]
proto = nacltai
proto_publickey =
proto_privatekey =
local = tuntap
local_interface = 
peer = udp
peer_remoteaddr =
peer_remoteport =
peer_localaddr =
peer_localport =
```

## 3. Generate and swap keys with your peer ##

Run the `naclkeypair` command, and two keys will be generated: a `PRIVATE` and a `PUBLIC` key. Set `proto_privatekey` to your `PRIVATE` key in your configuration file, and give the `PUBLIC` key to your peer.

Your peer should also generate a keypair, and do the same. You should put the public key that your peer has given you into `proto_publickey`.

## 4. Swap IP and port details with your peer ##

Choose an address and port that SigmaVPN should listen on, and then set `peer_localaddr` and `peer_localport` as appropriate. You should then give this address and port to your peer.

Your peer should also do the same, and give their address and port number to you. You should place your peer's address and port into `peer_remoteaddr` and `peer_remoteport` in your configuration.

## 5. Negotiate the tunnel type with your peer ##

Decide whether you would prefer to use a TAP tunnel (Layer 2/Ethernet) or a TUN tunnel (Layer 3/IP):

  * **Linux:** If you are using TUN mode, you should set `local_tunmode` to `1` in your config. Otherwise, leave it unset. If you wish to use multiple protocols, like IPv6, you should also both set `local_protocolinfo` to `1`.
  * **Mac OS X:** If you are using TUN mode, you should set `local_interface` to the path to a `tunX` device. Otherwise, you should use the path to a `tapX` device.

## 6. Complete the configuration ##

Finish off the configuration by completing the following:

  * Replace `peername` at the top of the section with the desired session name. This session name does not affect the protocol, and is used purely for reporting purposes.
  * **Linux:** If you have not already chosen an interface name, you can select one by setting `local_interface` to an interface name.

## 7. Save your configuration file and run SigmaVPN ##

Save your configuration file into the desired location, perhaps `/usr/local/etc/sigmavpn.conf` or your preferred location. (If you place `sigmavpn.conf` into `/usr/local/etc/sigmavpn.conf`, then you do not need to specify `-c` when starting SigmaVPN.)

Then, start SigmaVPN:
```
sigmavpn -c "path/to/sigmavpn.conf" 
```

**Note:** You may need to change the permissions of your TUN/TAP interface or run SigmaVPN as root if you receive errors.

## 8. Check the interface, and assign addresses ##

Run `ifconfig interfacename`:
  * **Linux:** `ifconfig interfacename`
  * **Mac OS X:** `ifconfig tap0` or `ifconfig tun0`, etc.

If the interface is shown, then assign an address:
```
ifconfig interfacename 192.168.6.1/24
```
... and then mark the interface as up:
```
ifconfig interfacename up
```

You should then be able to ping the address on the other side of the tunnel.