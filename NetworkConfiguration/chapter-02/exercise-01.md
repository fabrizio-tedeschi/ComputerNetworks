# Esercizio 01

### Configurazione delle macchine

1. Si impostano le configurazioni dell'interfaccia di rete di ciascun host come mostrato di seguito ed in modo consistente fra i diversi host.

```bash
# @h1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 10.0.0.1


# @h1 /etc/hosts

10.0.0.2 h2
10.0.0.3 h3
10.0.0.4 h4
```

2. Si avviano le interfacce di rete dei vari host eseguendo il comando:

```bash
# @h1
ifup eth0
```

3. Si verificano i requisiti di connettività lanciando i seguenti comandi:

```bash
# @h1
ping h2
arping h2
```

```bash
# @h3
ping h4
arping h4
```

4. Per verficare l'assenza di connettività fra le coppie di host si eseguono i seguenti comandi aspettandosi i risultati di `Timeout` oppure `host-unreachable`.

```bash
# @h1
ping h3         # destination-host-unreachable
arping h4       # Timeout
```

5. Verificare anche che gli host non coinvolti non ricevano richieste ARP.

```bash
# @h1
arping h4       # Timeout

# @h2
tcpdump arp     # ARP request

# @h3
tcpdump arp     # ...
```