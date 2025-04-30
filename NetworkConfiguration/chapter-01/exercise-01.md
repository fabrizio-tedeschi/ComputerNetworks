# Exercise 01

### Configuration

1. Si connettono i dispositivi usando cavi diretti come mostrato in figura
2. Si configura l'indirizo IP di ogni host usando il file `/etc/network/interfaces`. 
3. Per configurare gli alias su ogni host si usa il file `/etc/hosts`.

* Per esempio, configurando `@h1` si inseriscono le seguenti linee nei rispettivi file.

```bash
# @h1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.1

# @h1 /etc/hosts

192.168.1.2 h2
192.168.1.3 h3
```

* Nei dispositivi rimanenti si inseriscono le stesse linee modificando alias e IP.

4. Lanciando il comando `arping` da `@h1` verso `@h2` si può verificare il traffico osservabile da `@h2` e `@h3` usando il comando `tcpdump`:

```bash
# @h1
arping h2
```
```bash
# @h2 & h3
tcpdump arp
```

* La richiesta ARP è BROADCAST perciò viene inviata ed è osservabile da tutti i dispositivi
* La risposta ARP è UNICAST perciò lo switch la indirizza solamente al dispositivo destinatario (altri nodi come `@h3` non la vedono)

5. Lanciando `ping` da `@h1` verso `@h2` si può verificare il traffico su `@h2` e `@h3` lanciando `tcpdump`:

```bash
# @h1
ping h2
```
```bash
# @h2 & h3
tcpdump
```

* `@h3` riceve solamente la ARP request.
* `@h2` riceve la ARP request e invia una ARP reply, successivamente prosegue lo scambio di dati fra `@h1` e `@h2`.