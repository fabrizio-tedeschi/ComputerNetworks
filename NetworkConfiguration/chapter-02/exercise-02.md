# Esercizio 02

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

Dato che gli hos sono connessi allo stesso switch è necessario utilizzare delle VLAN per limitare le comunicazioni. Attraverso le impostazioni dello switch è possibile creare e modificare regole associate alle VLAN.

Ricordiamo che uno switch può applicare due diverse politiche di inoltro dei pacchetti:
* *ACCESS LINK*: una certa porta dello switch viene associata e opera solamente per una determinata VLAN
* *TRUNK LINK*: una certa porta dello switch inoltra pachetti appartenenti a VLAN differenti

Dato che nel caso in esame ogni host è associato ad una porta dello switch, si applica la modalità *ACCESS LINK*. I comandi riportati di seguito possono essere lanciati sul terminale dello switch oppure inseriti nel suo apposito file di configurazione (in questo ultimo caso saranno lanciati ad ogni avvio).

```bash
# Creazione di due distinte VLAN
vlan/create 10
vlan/create 20

# Impostazione delle porte 1 e 2 per lavorare solo su VLAN 10
port/setvlan 1 10
port/setvlan 2 10

# Impostazione delle porte 3 e 4 per lavorare solo su VLAN 20
port/setvlan 3 20
port/setvlan 4 20

# Stampa delle impostazioni delle VLAN
vlan/print
```

Il comando `vlan/print`, lanciato dopo la configurazione, permette di visualizzare quanto segue:

```
0000 DATA END WITH '.'
VLAN 0000
VLAN 0010
 -- Port 0001 tagged=0 active=1 status=Forwarding
 -- Port 0002 tagged=0 active=1 status=Forwarding
VLAN 0020
 -- Port 0003 tagged=0 active=1 status=Forwarding
 -- Port 0004 tagged=0 active=1 status=Forwarding
```

Il flag `tagged=0` specifica che la porta lavora in modalità *ACCESS LINK*.

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