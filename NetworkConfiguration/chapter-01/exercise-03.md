# Esercizio 03

### Configurazione

1. Si connettono i dispositivi usando cavi diretti come mostrato in figura.
2. Si configura l'indirizo IP di ogni host usando il file `/etc/network/interfaces`. 

```bash
# @h2 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.20

auto eth1
iface eth1 inet static
    address 192.168.1.21
```

3. Per configurare gli alias su ogni host si usa il file `/etc/hosts`.

```bash
# @h1 /etc/hosts

192.168.1.20 h2_eth0 h2     # Prima interfaccia
192.168.1.21 h2_eth1        # Seconda interfaccia

192.168.1.30 h3

# @h2 /etc/hosts

192.168.1.10 h1
192.168.1.30 h3

# @h3 /etc/hosts

192.168.1.21 h2_eth1 h2     # Prima interfaccia
192.168.1.20 h2_eth0        # Seconda interfaccia

192.168.1.10 h1
```

### Test

1. Si avviano le interfaccie di rete su ciascun dispositivo lanciando:

```bash
ifup -a
```

2. Si verifica il corretto avvio lanciando il comando:

```bash
ip addr show dev eth0
```

3. Si esegue il comando `tcpdump` su `@h2`per verificare il traffico specificando l'interfaccia e lanciando le `arping` da `@h1` verso `@h2_eth0` e poi verso `@h2_eth1`.

```bash
# @h1
arping h2_eth0

# @h2
tcpdump -i eth0 arp
tcpdump -i eth1 arp
```

* Il nodo `@h3` è escluso dalla comunicazione.
* La scheda di rete `eth0` vede il traffico ARP, mentre la scheda di rete `eth1` non lo vede.
* Situazione complementare avvine lanciando la richiesta ARP da `@h3` al posto di `@h1`.

4. Si esegue ora il comando `arping` da `@h1` verso `@h2_eth1`.

```bash
# @h1
arping h2_eth1

# @h2
tcpdump -i eth0 arp
tcpdump -i eth1 arp
```

* Il nodo `@h3` è escluso dalla comunicazione.
* La scheda di rete `eth0` vede il traffico ARP, mentre la scheda di rete `eth1` non lo vede.
* La scheda `eth0` risponde alla ARP request inviando l'indirizzo MAC della scheda `eth1` poichè esse sono sulla stessa macchina.

>[!NOTE]
> Eseguendo `arping` da `@h2` verso `@h3` non si ottengono risposte poichè **viene usata di default la prima interfaccia attivata** ossia `eth0`. Per risolvere il probema è necessario forzare l'utilizzo di `eth1` attraverso `arping -i eth1 192.168.1.30`.