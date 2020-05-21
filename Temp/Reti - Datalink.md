### Efficienza di Slotted ALOHA

**Efficienza** definita come la frazione di slot vincenti in presenza di un elevato numero di nodi attivi. Nel caso migliore solo il 37% degli slot compie lavoro utile.

### ALOHA Puro

Aloha puro è più semplice e non sincronizzato: quando arriva il primo pacchetto lo trasmette immediatamente e integralmente nel canale broadcast. Ci sono però elevate probabilità di collisione (i pacchetti si sovrappongono tra loro).

L'efficienza di ALOHA puro (18%) è peggio dello slotted.

### Accesso multiplo a rilevazione della portante (CSMA)

CSMA si pone in ascolto prima di trasmettere: se il canale è libero trasmette l'intero paccheto, se sta già trasmettendo aspetta un altro intervallo di tempo.

Possono ancora verificarsi collisioni: il ritardo di propagazione fa sì che due nodi non rilevino la reciproca trasmissione. Se avviene una collisione, non appena rilevata il nodo cessa immediatamente la trasmissione. La distanza e il ritardo di propagazione sono fondamentali per calcolare la probabilità di collisione.

#### CSMA/CD (collision detection)

Rilevamento della portante differito, come in CSMA: rileva la collisione in poco tempo e annulla la trasmissione non appena si accorge che c'è un'altra trasmissione in corso. La collisione è di facile rilevazione nelle LAN cablate e difficile nelle LAN wireless.

## Protocolli MAC a rotazione

Prendono il meglio dai protocolli precedenti, cercando di ereditare dalla suddivisione di canale la condivisione equa del canale evitando congestione, mentre dai protocolli ad accesso casuale ereditano l'efficacia con carichi non elevati.

Si basano sul protocollo **polling**: un nodo principale sonda a turno gli altri. Vengono eliminate collisioni e slot vuoti, si introduce però il ritardo di polling e il problema che se il nodo master si guasta il canale resta inattivo.

### Protocollo token-passing

Un messaggio di controllo circola fra i nodi con un ordine prefissato e chi ne è in possesso può trasmettere.  Questo protocollo è decentralizzato, altamente efficiente ma il guasto di un nodo può mettere fuori uso l'intero canale.

## Riepilogo dei protocolli

Cosa si può fare con un canale condiviso?

- **Suddivisione del canale:** per tempo, frequenza o codice (TDM, FDM)
- **Suddivisione casuale** o dinamica:
  - ALOHA, S-ALOHA, CSMA, CSMA/CD (Ethernet)
  - Rilevamento della portante: facile per tecnologie cablate, difficile con wireless
  - 802.11 utilizza la variante CSMA/CA
- **A rotazione**
  - Polling di un nodo principale o passaggio di un token
  - Completa decentralizzazione ed elevata efficienza
  - Usati in Bluetooth, FDDI, IBM Token Ring

# Indirizzi a livello di collegamento

## Indirizzi MAC e ARP

L'indirizzo MAC (o fisico o Ethernet), è analogo al numero di codice fiscale di una persona: ha una struttura piatta e non dipende dalla rete in cui si è collegati. Dipende dal produttore della scheda di rete e solitamente ha 48 bit. Viene scritto in esadecimale usando 6 coppie di cifre esadecimali. L'indirizzo broadcast di livello 2 ha tutti 1 (FF-FF-FF-FF-FF-FF).

Sono gestiti dalla **IEEE** che vende alle società costruttrici di adattatori i blocchi di spazio di indirizzi. La società dovrà garantire l'unicità degli indirizzi.

Il vantaggio dell'orizzontalità del MAC è la portabilità delle schede da una rete all'altra (cambierà solo l'IP)

### Protocollo per la risoluzione degli indirizzi (ARP)

Ogni nodo IP nella LAN ha una tabella ARP, la quale contiene la corrispondenza tra indirizzi IP e MAC.

```
<Indirizzo IP, Indirizzo MAC, TTL>
```

Il TTL indica quando bisognerà eliminare una voce nella tabella (tipicamente 20 minuti).

#### ARP nella stessa sottorete

ARP è plug-and-play: la tabella si costruisce automaticamente, non è necessario l'intervento dell'amministratore di rete. Un nodo trasmette in broadcast il messaggio di richiesta ARP, richiedendo l'indirizzo di un secondo nodo. Quando quest'ultimo riceve il pacchetto ARP risponderà comunicando il proprio indirizzo MAC.

#### Invio verso un nodo esterno alla sottorete

Se si vuole inviare un pacchetto da A a B attraverso un router R, il router stesso avrà due tabelle ARP, una per ciascuna rete IP (LAN) a cui è collegato.

1. Il nodo A controlla qual'è la destinazione del suo datagram, se è nella sua stessa sottorete
2. A deve inoltrare il datagram al router R, deve quindi trovare il MAC dell'interfaccia del router utilizzando ARP
3. A quindi incapsulerà il datagram in un frame di livello 2 indirizzato a R, il quale toglierà l'header di livello 2, leggerà l'IP di destinazione e cercherà il percorso nella sua tabella di routing.
4. Il router R dovrà poi cercare l'indirizzo MAC del destinatario sulla seconda sottorete sempre con ARP
5. Infine il router R incapsulerà a sua volta il datagram in un frame di livello 2 inviandolo al destinatario.

*figura 5-41*

# Ethernet

Ethernet detiene una posizione dominante nel mercato delle LAN cablate:

- è stata la prima LAN ad alta velocità con vasta diffusione
- Più semplice e meno costosa di token ring, FDDI e ATM
- Al passo coi tempi con il tasso trasmissivo: da 10 Mbps finoa 10 Gbps

Il progetto originale di Ethernet fù ideato da Bob Metcalfe.

## Topologia

La topologia originale era quella a Bus con cavo coassiale, diffusa fino alla metà degli anni 90. 

Le reti odierne seguono la topologia a stella, ogni nodo è collegato a un hub o commutatore (*switch*) permettendo di eseguire in ogni nodo un protocollo Ethernet separato non entrando in collisione con altri.

## Struttura dei pacchetti Ethernet

Preambolo, MAC di destinazione, MAC di origine, tipo, payload dati e CRC (correzione degli errori).

- Il **preambolo** in specifico è costituito da 8 byte, i primi sette sono 10101010 e l'ultimo 10101011, e viene utilizzato per "attivare" il ricevitore quando viene inviato un pacchetto e per sincronizzare l'orologio con quello del trasmittente.
- Gli **indirizzi** sono costituiti da 6 byte. Se è presente l'indirizzo di destinazione o il broadcast il payload viene trasferito direttamente al livello di rete altrimenti il pacchetto viene ignorato.
- Il campo **tipo** consente a Ethernet di supportare i vari protocolli di rete (multiplexing).
- Il controllo **CRC** consente all'adattatore ricevente di rilevare la presenza di un errore nei bit del pacchetto.

## Servizio senza connessione non affidabile

- **Senza connessione**: non è prevista nessuna forma di handshake preventiva prima di inviare un pacchetto.
- **Non affidabile**: non esiste riscontro, il flusso dei datagram non è garantito poiché il compito viene delegato ai protocolli di livello superiore.

## Fasi operative del protocollo CSMA/CD

1. L'adattatore che riceve un datagram da livello 3 prepara il frame e ascolta il canale.
2. Se è inattivo inizia la trasmissione, altrimenti resta in attesa.
3. Durante la trasmissione, verifica se ci sono altri segnali provenienti da altri host.
4. Se rileva altri segnali interrompe la trasmissione e invia un segnale di disturbo (*jam*) per avvisare della collisione.
5. L'adattatore si mette in attesa e viene definito uno slot di attesa pari a 512 bit; se viene messo in attesa successivamente l'intervallo raddoppia ogni volta. Dopo 10 volte raggiungerà il valore massimo, quando scade l'attesa se il canale è inattivo si potrà trasmettere nuovamente.

Il segnale *jam* viene trasmesso a un voltaggio più elevato ed è lungo 48 bit. L'attesa è esponenziale con l'obiettivo di stimare quanti sono i nodi in attesa coinvolti.

## Efficienza di Ethernet

- Tempo di propagazione: tempo massimo che occorre al segnale per propagarsi tra due host
- Tempo di trasmissione: tempo necessario per trasmettere un pacchetto della maggior dimensione possibile

$$
efficienza = \frac{1}{1+5t_{prop}/t_{trasm}}
$$

- Se il tempo di propagazione tende a 0, l'efficienza tenderà a 1 (100%, efficienza massima)
- Per aumentare l'efficienza, in alternativa, si può aumentare il tempo di trasmissione.
- Molto meglio di ALOHA: decentralizzato, semplice e poco costoso.

## Ethernet 802.3

Sono presenti diversi standard Ethernet: il MAC e il frame sono solitamente standard. Abbiamo però differenti velocità e differenti mezzi trasmissivi (fibra o cavo).

*figura 5-51*

## Codifica Manchester

*figura 5-52*

La codifica Manchester veniva utilizzata in 10BaseT: durante la ricezione di ciascun bit si verifica una transizione, permettendo di sincronizzare gli orologi di trasmittenti e riceventi. L'operazione veniva effettuata a livello fisico.

# Switch a livello di collegamento

## Hub

L'hub è un dispositivo "stupido" che opera sui singoli bit:

- riproduce un bit incrementandone l'energia trasmettendolo su tutte le interfacce, anche se su alcune c'è un segnale (avverrà una collisione)
- non implementa rilevazione di portante né CSMA/CD

## Switch

Lo switch è un dispositivo a livello di link, è più intelligente di un hub e svolge un ruolo attivo:

- filtra e inoltra i pacchetti
- ha una tabella e sa a quale porta inoltrare un pacchetto
- è trasparente agli host
- è un componente plug-and-play con autoapprendimento, non richiede l'intervento salvo configurazioni particolari

Lo switch consente più trasmissioni simultanee: i pacchetti vengono infatti bufferizzati e il protocollo Ethernet viene implementato su ciascun collegamento in entrata, evitando collisioni ma consentendo il full-duplex. Grazie allo switching avvengono quindi più trasmissioni simultaneamente.

### Tabella di commutazione

Componente software utilizzato dallo switch per sapere dove inoltrare i pacchetti ricevuti. Contiene il MAC del nodo di destinazione e l'interfaccia associata, assomiglia alle tabelle di instradamento con la differenza che può essere generata automaticamente dallo switch.

#### Autoapprendimento

Quando riceve un pacchetto, lo switch impara l'indirizzo del mittente: registrerà quindi nella tabella di switching la coppia indirizzo/porta.

### Collegare gli switch

Gli switch possono essere interconnessi tra loro: gli host avranno la sensazione di essere sulla stessa rete di livello 2 ma saranno su differenti domini di collisione.

### Esempio di rete di un'istituzione

*figura 4-63*

### Switch e router a confronto

- Entrambi i dispositivi sono store-and-forward
  - Il router lavora a livello di rete (dominio di broadcast)
  - Lo switch lavora a livello di collegamento (dominio di collisione)
- I router mantengono tabelle d'inoltro e implementano algoritmi d'instradamento
- Gli switch mantengono tabelle di commutazione e implementano il filtraggio e algoritmi di autoapprendimento

