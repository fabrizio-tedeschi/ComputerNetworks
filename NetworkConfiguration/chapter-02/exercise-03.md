# Esercizio 03

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

### Configurazione delle VLAN

1. Per permettere la separazione delle reti è necessario creare due VLAN distinte tramite i seguenti comandi (da lanciare su entrambi gli switch):

```bash
# @S1, @S2
vlan/create 10
vlan/create 20
```

2. Le porte connesse agli host lavorano in modalità *ACCESS LINK* e possono essere configurate lanciando i seguenti comandi:

```bash
# @S1
port/setvlan 1 10       # Dato che h1 appartiene alla VLAN 10
port/setvlan 2 20       # Dato che h3 appartiene alla VLAN 20

# @S2
port/setvlan 1 10       # Dato che h2 appartiene alla VLAN 10
port/setvlan 2 20       # Dato che h4 appartiene alla VLAN 20
```

3. Per consentire la comunicazione tra i vari switch si utilizzano due cavi cross, uno per ciascuna VLAN. Si può quindi impostare l'inoltro come segue:

```bash
# @S1, @S2
port/setvlan 3 10       # Il cao cross collegato alla porta 3 inoltra i dati della VLAN 10
port/setvlan 4 20       # Il cao cross collegato alla porta 4 inoltra i dati della VLAN 20
```

4. Il comando `vlan/print`, lanciato dopo la configurazione, permette di visualizzare quanto segue:

```
0000 DATA END WITH '.'
VLAN 0000
VLAN 0010
 -- Port 0001 tagged=0 active=1 status=Forwarding
 -- Port 0003 tagged=0 active=1 status=Forwarding
VLAN 0020
 -- Port 0002 tagged=0 active=1 status=Forwarding
 -- Port 0004 tagged=0 active=1 status=Forwarding
```

### Test delle comunicazioni

Si verificano i requisiti di connettività lanciando i seguenti comandi:

```bash
# @h1
ping h2         # OK
arping h2       # OK

# @h3
ping h4         # OK
arping h4       # OK

# @h1
ping h3         # KO: destination-host-unreachable
arping h4       # KO: Timeout
```