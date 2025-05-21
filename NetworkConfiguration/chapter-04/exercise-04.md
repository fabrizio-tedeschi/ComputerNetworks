# Esercizio 04

### Configurazione delle macchine

1. Si impostano le configurazioni dell'interfaccia di rete di chiascun host finale come mostrato di seguito.

```bash
# @h1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.1
    netmask 255.255.255.0
    gateway 192.168.1.2

# @external /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 2.2.2.2
    netmask 255.255.255.255
    post-up ip route add 1.1.1.1/32 dev eth0
    post-up ip route add 192.168.1.0/24 via 1.1.1.1
```

>[!NOTE]
> Il dispositivo `@external` *vede* solamente la rete `2.2.2.2/32` perciò è necessario indicare che
> * Per raggiungere la rete `1.1.1.1/32` bisogna utilizzare l'interfaccia `eth0`
> * Per raggiungere la rete `192.168.1.0/24` bisogna passare per l'indirizzo `1.1.1.1` (che si raggiunge tramite la regola precedente)

2. Si configurano le interfaccie di rete del gateway come mostrato di seguito.

```bash
# @internal /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.2
    netmask 255.255.255.255

auto eth1
iface eth1 inet static
    address 1.1.1.1
    netmask 255.255.255.255
    post-up ip route add 2.2.2.2/32 dev eth1
```

>[!NOTE]
> Il dispositivo `@internal` NON *vede* la rete `2.2.2.2/32` perciò è necessario indicare che per raggiungerla bisogna utilizzare l'interfaccia `eth1`

3. Si avviano tutte le interfacce di rete dei vari host eseguendo il comando:

```bash
# @h1, @internal, @external
ifup -a
```

In caso di errori nei file `etc/network/interfaces` si possono spegnere le interaccie di rete lanciando il comando:

```bash
# @h1, @internal, @external
ifdown --force <dev>
```

### Abilitazione di IP forwarding

Per permettere ai vari host di comunicare è necessario abilitare IP forwarding nel dispositivo `@internal`. Tale opzione indica al dispositivo di far transitare fra le sue schede di rete anche il traffico non direttamente a lui indirizzato:

```bash
# @internal
sysctl -w net.ipv4.ip_forward=1
```

### Test di connettività

Si verificano i requisiti di connettività lanciando i seguenti comandi:

```bash
# @h1
ping 2.2.2.2    # OK
```

In caso di fallimento significa che ci sono problemi lungo la catena di inoltro, verificare la connettività con un dispositivo per volta.