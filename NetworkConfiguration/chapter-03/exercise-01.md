# Esercizio 01

## Metodo

Si consideri l'indirizzo IP `192.168.1.1/25`.

### 1) Netmask

La netmask in notazione decimale può essere ricavata considerando i bit pari ad `1` indicati dal CIDR. Nell'esempio trattato i primi 25 bit hanno valore `1` mentre i restanti `0` perciò si ottiene:

```
11111111.11111111.11111111.10000000
```

I valori con tutti gli 8 bit ad `1` corrispondono a `255` mentre per i restanti gruppi di bit si può applicare la traduzione binario -> decimale. 

```
(10000000) = 1*2^7 + 0*2^6 + ... = (128)
```

Pertanto la netmask sarà `255.255.255.128`

### 2) Indirizzo di broadcast

L'indirizzo di broadcast è il piò alto indirizzo di una certa sottorete. Per calcolarlo si applicano i seguenti passaggi:
* Si lasciano invariati i bit del NetID
* Si impostano tutti i bit del HostID pari ad `1`

Si può considerare l'indirizzo `192.168.1.1/25` in cui ibit associati al NetID sono 25.

`11000000.10101000.00000001.0000001` => `11000000.10101000.00000001.0`**`111111`**

Traducendo in notazione decimale si ottiene `192.168.1.127`

### 3) Host_min e Host_max
* Host_min è definito come il primo indirizzo della rete
* Host_max è definito come `broadcast-1`

La rete ha NetID `11000000.10101000.00000001.0000001`

|Host_min      |Host_max       |
|--------------|---------------|
|`192.168.1.1` |`192.168.1.126`|

### 4) Numero di host

Il numero di host di una certa rete può essere calcolato secondo la formula `N_hosts = 2^N - 2` dove `N` è il numero di bit del HostID. Si noti che la sottrazione si riferisce alla presenza dell'indirizzo di rete e dell'indirizzo di broadcast.

`N_hosts = 2^(32-25) - 2 = 126`

## Soluzioni

Per verificare la correttezza delle soluzioni è possibile lanciare su un terminale linux il comando `ipcalc` seguito dall'indirizzo IP fornito e confrontare il risultato con i valori ottenuti manualmente.

|Indirizzo IP     |Netmask          |Broadcast        |Host_min         |Host_max         |N_hosts  |
|-----------------|-----------------|-----------------|-----------------|-----------------|---------|
|`192.168.1.1/24` |
|`192.168.1.1/25` |`255.255.255.128`|`192.168.1.127`  |`192.168.1.1`    |`192.168.1.126`  |126      |
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