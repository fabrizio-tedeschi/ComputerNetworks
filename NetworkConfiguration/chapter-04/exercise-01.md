# Esercizio 01

### Configurazione delle macchine

1. Si impostano le configurazioni dell'interfaccia di rete di chiascun host come mostrato di seguito ed in modo consistente fra i diversi host.

```bash
# @h1 /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.1
    netmask 255.255.255.0
    gateway 192.168.1.254
```

L'aggiunta dell'ultima riga permette di configurarae all'avvio le tabelle di routing impostando `@GW` come default gateway. In alternativa è possibile lanciare il comando seguente su ciascuna macchina:

```bash
# @h1
ip route add default via 192.168.1.254
```

2. Si impostano le configurazioni delle interfacce di rete del gateway.

```bash
# @GW /etc/network/interfaces

auto eth0
iface eth0 inet static
    address 192.168.1.254
    netmask 255.255.255.0

auto eth1
iface eth1 inet static
    address 192.168.2.254
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 192.168.3.254
    netmask 255.255.255.0

```

3. Si avviano tutte le interfacce di rete dei vari host eseguendo il comando:

```bash
# @h1, @h2, @h3, @GW
ifup -a
```

### Abilitazione di IP forwarding

Per permettere ai vari host di comunicare è necessario abilitare IP forwarding nel dispositivo `@GW`. Tale opzione indica al dispositivo di far transitare fra le sue schede di rete anche il traffico non direttamente a lui indirizzato:

```bash
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
```