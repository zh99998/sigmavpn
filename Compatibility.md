# Compatibility #

The following systems are compatible with SigmaVPN:

| **Operating System** | **Compatible** | **Notes** |
|:---------------------|:---------------|:----------|
| Mac OS X 10.6        | Yes            | Requires [Universal TUN/TAP driver](http://tuntaposx.sourceforge.net/) and Xcode 4.0.2 (earlier versions may work but untested)|
| Mac OS X 10.7        | Yes            | Requires [Universal TUN/TAP driver](http://tuntaposx.sourceforge.net/) and Xcode 4.1 (earlier versions may work but untested)|
| Linux 2.6+           | Yes            | Requires TUN/TAP to be enabled in the kernel, and for `/dev/net/tun` to exist|
| FreeBSD 9.0+         | Yes            | Seems to work out of the box |

Other systems are untested. Please feel free to let us know if you have any success or failures with SigmaVPN on systems not listed here.