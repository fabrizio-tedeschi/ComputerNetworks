# Host to Network level

## Exercise 1

### Assignment

### Setup

Configure a network with the topology shown on the image above.
* Each host has only 1 NIC
* IP address are assigned for each host
* The interconnection element is a switch

Configure the network using the `/etc/network/interfaces` file and indicate machine names into the `/etc/hosts` using consistent names.

### Connection test

Execute the following commands and check the responses.

```
@h1

arping 192.168.1.2
ping 192.168.1.2

arping h2
ping h2
```

### Switch test

Launch the following command:

```
@h1

arping 192.168.1.2
```

Verify the network traffic using `tcpdump` command on @h3 and @h2.

### ARP cache

Launch the following command:

```
@h2

tcpdump arp
```

### Solution

## Exercise 2

## Exercise 3

## Exercise 4

## Exercise 5

## Exercise 6