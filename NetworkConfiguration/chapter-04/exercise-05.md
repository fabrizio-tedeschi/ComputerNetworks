# Esercizio 05

### Configurazione delle macchine

Si impostano le configurazioni dell'interfaccia di rete di chiascun host finale come mostrato di seguito.

```bash
# @h1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.1
    gateway 192.168.1.254

# @h3 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.2.1
    gateway 192.168.2.254
```

Dato che il dispositivo `@h2` possiede indirizzi IP differenti su VLAN differenti è possibile utilizzare la notazione `ethX.Z` per specificare quale indirizzo IP una certa interfaccia `ethX` deve utilizzare all'interno di una certa VLAN `Z`.

```bash
# @h2 /etc/network/interfaces

auto eth0.10
iface eth0.10 inet static
    address 192.168.1.254

auto eth0.20
iface eth0.20 inet static
    address 192.168.2.254
```

### Configurazione dello switch

Lo switch deve essere configurato per smistare correttamente il traffico verso le varie VLAN. In particolare:
* Le porte 1 e 3 possono lavoare con *access link* poichè collegate ad una sola VLAN
* La porta 2 deve lavorare con *trunk link* poichè gestisce traffico appartenente sia alla VLAN 10 sia alla VLAN 20 (il gateway `@h2` ha IP diversi sulle VLAN)

```bash
# @S1
vlan/create 10
vlan/create 20

port/setvlan 1 10       # Dato che h1 appartiene alla VLAN 10
port/setvlan 3 20       # Dato che h3 appartiene alla VLAN 20

vlan/addport 10 2       # Dato che h2 appartiene sia alla VLAN 10 sia alla VLAN 20
vlan/addport 20 2
```

Si ottiene quindi:

```
0000 DATA END WITH '.'
VLAN 0000
VLAN 0010
 -- Port 0001 tagged=0 active=1 status=Forwarding
 -- Port 0002 tagged=1 active=1 status=Forwarding
VLAN 0020
 -- Port 0002 tagged=1 active=1 status=Forwarding
 -- Port 0003 tagged=0 active=1 status=Forwarding
```

### Abilitazione di IP forwarding

Per permettere ai vari host di comunicare è necessario abilitare IP forwarding nel dispositivo `@h2`. Tale opzione indica al dispositivo di far transitare fra le sue schede di rete anche il traffico non direttamente a lui indirizzato:

```bash
# @h2
sysctl -w net.ipv4.ip_forward=1
```

### Test di connettività

Si verificano i requisiti di connettività lanciando i seguenti comandi:

```bash
# @h1
ping 192.168.2.1    # OK
```

In caso di fallimento significa che ci sono problemi lungo la catena di inoltro, verificare la connettività con un dispositivo/VLAN per volta.