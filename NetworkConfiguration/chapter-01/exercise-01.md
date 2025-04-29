# Exercise 01

### Configuration

1. Connect devices with direct cable like shown on the image
2. Configure each host IP address using the `/etc/network/interfaces` and `/etc/hosts` files:

For example in `@h1` add the following lines to file and repeat the operation for the other nodes changing IP and names.

```bash
# @h1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.1

# @h1 /etc/hosts

192.168.1.2 h2
192.168.1.3 h3
```

3. Try to lounch `arping` from `@h1` to `@h2` and verify the taffic shown on `@h2` and `@h3` using `tcpdump`:

```
@h1
arping h2
```
```
@h2 & h3
tcpdump arp
```
* The ARP request is BROADCAST so the switch forwards it to all devices
* The ARP reply is UNICAST so the switch forwards it only to the sender (other nodes like `@h3` doesn't see it)

4. Try to lounch `ping` from `@h1` to `@h2` and verify the taffic shown on `@h2` and `@h3` using `tcpdump`:

```
@h1
arping h2
```
```
@h2 & h3
tcpdump
```

* `@h3` receives only the first ARP request.
* `@h2` receives the ARP request and send an ARP reply, after that IP requests and replies are send between `@h1` and `@h2`.