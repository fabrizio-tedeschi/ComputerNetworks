# Esercizio 02

### Configurazione delle macchine

1. Si impostano le configurazioni dell'interfaccia di rete di chiascun host finale come mostrato di seguito.

```bash
# @h1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.1
    netmask 255.255.255.0
    gateway 192.168.1.254

# @h4 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.3.1
    netmask 255.255.255.0
    gateway 192.168.3.254
```

2. Si configurano le interfaccie di rete di ciascun gateway come mostrato di seguito.

```bash
# @gw12 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.254
    netmask 255.255.255.0

auto eth1
iface eth1 inet static
    address 192.168.2.254
    netmask 255.255.255.0
    post-up ip route add 192.168.3.0/24 via 192.168.2.254

# @gw23 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.2.254
    netmask 255.255.255.0

auto eth1
iface eth1 inet static
    address 192.168.3.254
    netmask 255.255.255.0
    post-up ip route add 192.168.1.0/24 via 192.168.2.253
```

>[!NOTE]
> I comandi post-up servono per inserire righe nelle tabelle di routing all'avvio dell'interfaccia. In particolare in questo caso:
> * Il dispositivo `@gw12` non *vede* la rete `192.168.3.0` perciò bisogna specificare che per raggiungerla è necessario passare da `gw23`
> * Il dispositivo `@gw23` non *vede* la rete `192.168.1.0` perciò bisogna specificare che per raggiungerla è necessario passare da `gw12`

3. Si avviano tutte le interfacce di rete dei vari host eseguendo il comando:

```bash
# @h1, @h2, @h3, @h4
ifup -a
```

In caso di errori nei file `etc/network/interfaces` si possono spegnere le interaccie di rete lanciando il comando:

```bash
# @h1, @h2, @h3, @h4
ifdown --force <dev>
```

### Abilitazione di IP forwarding

Per permettere ai vari host di comunicare è necessario abilitare IP forwarding nei dispositivi `@gw12, @gw23`. Tale opzione indica al dispositivo di far transitare fra le sue schede di rete anche il traffico non direttamente a lui indirizzato:

```bash
# @gw12, @gw23
sysctl -w net.ipv4.ip_forward=1
```

### Test di connettività

Si verificano i requisiti di connettività lanciando i seguenti comandi:

```bash
# @h1
ping 192.168.3.1    # OK
```

In caso di fallimento significa che ci sono problemi lungo la catena di inoltro, verificare la connettività con un dispositivo per volta.