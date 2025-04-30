# Esercizio 02

### Configurazione

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

### Test

1. Si avviano le interfaccie di rete su ciascun dispositivo lanciando:

```bash
ifup eth0
```

2. Si verifica il corretto avvio lanciando il comando:

```bash
ip addr show dev eth0
```

3. Lanciando il comando `arping` da `@h1` verso `@h2` si può verificare il traffico osservabile da `@h2` e `@h3` usando il comando `tcpdump`:

```bash
# @h1
arping h2
```
```bash
# @h2 & h3
tcpdump arp
```

* La richiesta ARP è BROADCAST perciò viene inviata ed è osservabile da tutti i dispositivi
* La risposta ARP è UNICAST ma è visibile a tutti i dispositivi poichè **un HUB si comporta come un semplice cavo di rete** su cui si affacciano tutti i dispositivi.

4. Lanciando `ping` da `@h1` verso `@h2` si può verificare il traffico su `@h2` e `@h3` lanciando `tcpdump`:

```bash
# @h1
ping h2
```
```bash
# @h2 & h3
tcpdump
```

* Sia `@h2` sia `@h3` possono osservare tutto il traffico della rete, anche quello che non li riguarda.