Internet Protocol Security is a suite of tools that are used for securing network traffic at the IP layer. There are two protocols that provide different security assurances:
- Authentication Header (AH): Provides data intergrity (will know if data is modified between senders), data source authentication (will know if the source is not what is expected for that conncetion), and protects against replay attack.
- Encapsulating Security Payloads (EDP): Provides similar capabilites, plus confidentiality (someone in the middle cant see the data)

There also something called Security Associations (SA) which provide a bundle of algorithms to dynamically exchange keys and establish a secure conncetion over AH or ESP. **IKE is one of those.**

### Modes
Both ESP and AH can operate in two modes:

- Transport mode - Provides security services between two hosts, applied to the payload of the IP packet, but the IP headers are leeft in the clear for routing.
- Tunneling - The entire IP packet is encrypted and/or authenticated and it become the payload of a new IP packet with a header ro send it to the other end. At the other end, the packet is encrypted and send based on the decrypted headers

![transport vs tunnel](https://0xdf.gitlab.io/img/conceal-ipsec-modes.jpg)


For this we going to use `strongswan` client to connect to the VPN

```bash
sudo apt install strongwan
sudo apt-get install libstrongswan-extra-plugins
```

Edit Config files:

```bash
/etc/ipsec.conf
/etc/ipsec.secrets
```
[Resource](https://wiki.strongswan.org/projects/strongswan/wiki/Strongswanconf)
[Resource](https://blog.ruanbekker.com/blog/2018/02/11/setup-a-site-to-site-ipsec-vpn-with-strongswan-and-preshared-key-authentication/)

