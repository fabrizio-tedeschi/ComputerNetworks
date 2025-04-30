# Esercizio 05

### Configurazione

1. Si connettono i dispositivi usando cavi diretti come mostrato in figura.

2. Si configura l'indirizo IP di ogni host lanciando i seguenti comandi. 

```bash
# @h1
ip addr show dev eth0                   # Mostra l'assenza di indirizzo
ip addr add dev eth0 10.0.0.1/24        # Imposta l'indirizzo
ip addr show dev eth0                   # Mostra l'indirizzo impostato

ip link set dev eth0 up                 # Attiva l'interfaccia di rete
```

* Si noti l'importanza del CIDR `/24` che permette di specificare che gli host appartengono alla stessa rete. Nei file di configurazione è il default a meno che non venga specificata una particolare netmask.

### Test

1. Si lancia il comando `ping` da `@h1` verso `@h3` e si verifica lo scambio request/response.
2. Lanciando il comando `tcpdump` sugli altri host è possibile vedere il traffico della rete, in particolare:
    * I nodi coinvolti da `ping` mostrano request/response
    * I nodi non coinvolti ricevono solo la AROP request, poi lo switch indirizza il traffico verso la destinazione corretta