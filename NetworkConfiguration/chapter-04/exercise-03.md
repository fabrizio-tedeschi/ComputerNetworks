# Esercizio 03

### Configurazione delle macchine

1. Si impostano le configurazioni dell'interfaccia di rete di chiascun host finale come mostrato di seguito.

```bash
# @h1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.1
    gateway 192.168.1.254

# @h2 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.2.1
    gateway 192.168.2.254

# @h3 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.3.1
    gateway 192.168.3.254
```

2. Si configurano le interfaccie di rete di ciascun gateway come mostrato di seguito.

```bash
# @GW1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.254

auto eth1
iface eth1 inet static
    address 10.0.0.1
    post-up ip route add 192.168.2.0/24 via 10.0.0.2
    post-up ip route add 192.168.3.0/24 via 10.0.0.3

# @GW2 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.2.254

auto eth1
iface eth1 inet static
    address 10.0.0.2
    post-up ip route add 192.168.1.0/24 via 10.0.0.1
    post-up ip route add 192.168.3.0/24 via 10.0.0.3

# @GW3 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.3.254

auto eth1
iface eth1 inet static
    address 10.0.0.3
    post-up ip route add 192.168.1.0/24 via 10.0.0.1
    post-up ip route add 192.168.2.0/24 via 10.0.0.2
```

>[!NOTE]
> I comandi post-up servono per inserire righe nelle tabelle di routing all'avvio dell'interfaccia. In particolare in questo caso:
> * Il dispositivo `@GW1` *vede* la rete `192.168.1.0/24` e `10.0.0.0/8` perciò può *vedere* i tutti i gateway e `@h1`
> * Il dispositivo `@GW2` *vede* la rete `192.168.2.0/24` e `10.0.0.0/8` perciò può *vedere* i tutti i gateway e `@h2`
> * Il dispositivo `@GW3` *vede* la rete `192.168.3.0/24` e `10.0.0.0/8` perciò può *vedere* i tutti i gateway e `@h3`
> Per permettere la comunicazione è necessario che ogni gateway inoltri i pacchetti al gateway corretto a seconda della rete a cui i pacchetti sono idirizzati.

3. Si avviano tutte le interfacce di rete dei vari host eseguendo il comando:

```bash
# @h1, @h2, @h3, @GW1, @GW2, @GW3
ifup -a
```

In caso di errori nei file `etc/network/interfaces` si possono spegnere le interaccie di rete lanciando il comando:

```bash
# @h1, @h2, @h3, @GW1, @GW2, @GW3
ifdown --force <dev>
```

### Abilitazione di IP forwarding

Per permettere ai vari host di comunicare è necessario abilitare IP forwarding nei dispositivi `@GW1, @GW2, @GW3`. Tale opzione indica al dispositivo di far transitare fra le sue schede di rete anche il traffico non direttamente a lui indirizzato:

```bash
# @GW1, @GW2, @GW3
sysctl -w net.ipv4.ip_forward=1
```

### Test di connettività

Si verificano i requisiti di connettività lanciando i seguenti comandi:

```bash
# @h1
ping 192.168.2.1    # OK
ping 192.168.3.1    # OK

# @h2
ping 192.168.3.1    # OK
ping 10.0.0.1       # OK
```

In caso di fallimento significa che ci sono problemi lungo la catena di inoltro, verificare la connettività con un dispositivo per volta.