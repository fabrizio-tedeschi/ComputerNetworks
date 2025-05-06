# Esercizio 02

## Metodo

Per ricavare la netmask corrispondente alle reti è necessario:
1. Ricavare il numero di bit dell'HostID attraverso formula inversa
2. Ricavare i bit del NetID tramite la formula `NetID = 32 - HostID`
3. Ricavare la netmask ponendo tutti i bit del NetID a `1` ed i restanti a `0`.

Consideriamo per esempio una rete avente `N_hosts = 10`.
* La potenza del due più piccola che può contenere 10 è `N_hosts = 2^4 - 2 = 14` pertanto `HostID = 4`
* Tramite la formula si ricava `NetID = 32 - HostID = 32 - 4 = 28`
* Si scrive una netmask di 28 bit in binario e la si traduce in decimale:

`11111111.11111111.11111111.11110000` => `255.255.255.240`

## Soluzioni

|N_hosts    |NetID bit|Netmask          |
|-----------|---------|-----------------|
|10         |/28      |`255.255.255.240`|
|30         |/27      |`255.255.255.224`|
|50         |/26      |`255.255.255.192`|
|120        |/25      |`255.255.255.128`|
|500        |/23      |`255.255.254.0`  |
|1000       |/22      |`255.255.252.0`  |