# Subnetting & supernetting

## Esercizio 01

Dati i seguenti indirizzi IP in formato CIDR calcolare i seguenti valori:
* Netmask in formato dotted decimal notation
* NetID
* Indirizzo di broadcast
* Indirizzi hostmin e hostmax
* Numero di host per chiascuna rete

|Indirizzo IP     |
|-----------------|
|`192.168.1.1/24` |
|`192.168.1.1/25` |
|`192.168.1.1/23` |
|`155.185.54.1/26`|
|`155.185.54.1/22`|
|`160.80.2.46/27` |
|`160.80.2.46/23` |
|`160.80.2.46/23` |
|`10.42.42.1/9`   |
|`10.42.42.1/12`  |
|`10.53.147.1/14` |
|`10.53.147.1/17` |

> [Soluzione](./exercise-01.md)

## Esercizio 02

Date le seguenti sottoreti che devono ospitare un certo numero di host indicare la netmask corrispondente in formato CIDR oppure dotted decimal.

|N_hosts |
|--------|
|10      |
|30      |
|50      |
|120     |
|500     |
|1000    |

> [Soluzione](./exercise-02.md)

## Esercizio 03

Si vogliono ripartire le seguenti reti in sottoreti come specificato di seguito.

* Ripartire `192.168.1.0/24` in tre diverse reti:
    * LAN_1: deve avere netmask /25
    * LAN_2: deve contenere 192.168.1.1
    * LAN_3: nessuna richiesta specifica

* Ripartire `192.168.54.0/24` in quattro diverse reti:
    * LAN_1: deve avere netmask /25
    * LAN_2: deve avere netmask /26
    * LAN_3: deve contenere 192.168.54.129
    * LAN_4: nessuna richiesta specifica

* Ripartire `192.168.30.0/24` in quattro diverse reti:
    * LAN_1: nessuna richiesta specifica
    * LAN_2: deve avere stessa dimensione di LAN_1 e conenere 192.168.30.1
    * LAN_3: nessuna richiesta specifica
    * LAN_4: nessuna richiesta specifica

* Ripartire `10.0.0.1/24` in tre diverse reti:
    * LAN_1: deve avere netmask /26
    * LAN_2: deve contenere 120 host
    * LAN_3: deve contenere 10.0.0.1

> [Soluzione](./exercise-03.md)

## Esercizio 04

Date le seguenti tabelle di routing ridurne la dimensione tramiite la tecnica dell'aggregazione.

> [Soluzione](./exercise-04.md)