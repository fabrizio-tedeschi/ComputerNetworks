# Esercizio 04

### Configurazione

1. Si connettono i dispositivi usando cavi diretti come mostrato in figura. Per connettere gli switch si utilizza un *cavo cross*.
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

3. Si esegue il comando `tcpdump` su `@h2` e su su `@h3` per verificare il traffico specificando l'interfaccia e lanciando le `arping` da `@h1` verso `@h2_eth0`.

```bash
# @h1
arping h2

# @h2
tcpdump -i eth0 arp

# @h3
tcpdump arp
```

* Sia il nodo `@h2_eth0` che il nodo `@h3` ricevono le richieste ARP ma solamente `@h2_eth0` invia una risposta.
* La scheda di rete `@h2_eth1` non riceve traffico ma la sua risposta viene gestita da `@h2_eth0`.

>[!NOTE]
> Collegando gli switch con un cavo cross si è creata anche una comunicazione diretta fra `@h1` e `@h3`. Non si verificano problemi di comunicazione fra gli host a differenza dell'esercizio precedente in cui `@h1` e `@h3` erano separati poichè connessi a switch dfferenti.