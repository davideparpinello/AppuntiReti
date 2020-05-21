# Livello di collegamento: introduzione e servizi

A livello di collegamento definiamo come **nodi** host e router, che vengono collegati tra loro da **collegamenti (link)** di tipologia cablata, wireless o LAN. Le unità di dati scambiate a livello link sono chiamate **frame**.

In breve, i protocolli a livello di collegamento si occupano del trasporto di datagram lungo un singolo canale di comunicazione. Questi protocolli possono essere differenti sui vari collegamenti che seguirà il datagram e i servizi erogati possono essere differenti: non tutti i protocolli, ad esempio, forniscono consegna affidabile.

## Servizi offerti a livello di link

- **Framing**
  - I protocolli incapsulano i datagram del livello di rete in frame a livello di link
  - Il protocollo MAC controlla l'accesso al mezzo trasmissivo (il collegamento)
  - Viene utilizzato l'indirizzo MAC (diverso dall'IP) per identificare i nodi di origine e destinazione
- **Consegna affidabile**
  - Non necessaria sui collegamenti a basso numero di errori sui bit (fibra ottica, cavo coassiale, doppino intrecciato)
  - Utilizzata nei collegamenti soggetti a elevato tasso di errori (wireless)
- **Controllo di flusso:** non saturare il nodo ricevente
- **Rilevazione degli errori:** causati dall'attenuazione del segnale e dal rumore elettromagnetico, il ricevente individua gli errori grazie a un bit di controllo inserito nel frame
- **Correzione degli errori:** il ricevente determina l'errore e lo corregge
- **Half-duplex e full-duplex**

## Dov'è implementato il livello link

Viene implementato in tutti gli host grazie ad un adattatore (o **NIC**, network interface card) che implementa il livello di collegamento e fisico ed è una combinazione di hardware, software e firmware.

L'adattatore ha il ruolo in trasmissione di incapsulare i datagram nei frame impostando bit di controllo errori, trasferimento affidabile, controllo di flusso, ecc e dall'altro lato di individuare errori ed estrarre i datagram passandoli al nodo.

# Tecniche di rilevazione e correzione degli errori

La rilevazione degli errori non è attendibile al 100%, infatti è possibile che ci siano errori non rilevati e per ridurre che questo accada le tecniche più sofisticate prevedono un'elevata ridondanza.

## Controllo di parità

Nel caso di **unico bit di parità** è presente un solo bit che consente di riconoscere che si è verificato almeno un errore in un bit.

Nel caso della **parità bidimensionale** è possibile individuare e correggere il bit alterato.

*figura 5-12 bidimensionale*

# Protocolli di accesso multiplo

Esistono due tipi di collegamenti di rete:

- collegamento **punto-punto** (PPP), utilizzato per connessioni telefoniche o collegamenti punto-punto tra Ethernet e host
- collegamento **broadcast** (cavo o canale condiviso) come l'Ethernet tradizionale o wireless

Nel caso di una connessione a un canale broadcast condiviso è consentita la connessione anche a migliaia di nodi: si genera quindi una **collisione** quando i nodi ricevono più di un frame contemporaneamente. 

I protocolli ad accesso multiplo consentono quindi di definire le modalità con cui i nodi regolano le loro trasmissioni sul canale, la cui comunicazione utilizza il canale stesso (non è presente un canale fuori banda).

Il protocollo di accesso multiplo ideale sarebbe un protocollo decentralizzato e semplice che suddivide il tasso trasmissivo tra il numero di nodi che devono inviare dati. Ovviamente tale protocollo non è possibile

I protocolli di accesso multiplo esistenti vengono classificati come segue:

- Protocolli a **suddivisione del canale**: il canale è suddiviso in parti (slot di tempo, frequenza, codice) ed allocate a uno specifico nodo per utilizzo esclusivo
- Protocolli ad **accesso casuale**: nessuna divisione, si possono verificare collisioni, i nodi ritrasmettono ripetutamente i pacchetti
- Protocolli a **rotazione**: ogni nodo ha il suo turno di trasmissione, ma quelli che hanno molto da trasmettere potrebbero avere turni più lunghi

## Protocolli a suddivisione del canale

### TDMA: accesso multiplo a divisione di tempo

Il protocollo TDMA consiste in turni per accedere al canale, suddividendolo in intervalli di tempo. Gli slot non usati rimangono inattivi.

*figura 5-18*

### FDMA: accesso multiplo a divisione di frequenza

Il protocollo FDMA suddivide il canale in bande di frequenza e ad ogni nodo viene assegnata una banda di frequenza prefissata.

*figura 5-19*

## Protocolli ad accesso casuale

Nei protocolli ad accesso casuale ogni nodo che deve inviare dati trasmette alla massima velocità consentita dal canale, senza coordinazione tra i nodi. Se più nodi stanno trasmettendo si verifica una collisione. Il protocollo definisce quindi come rilevare le collisioni e come ritrasmettere in caso di avvenuta collisione.

Alcuni protocolli ad accesso casuale sono slotted ALOHA, ALOHA, CSMA, CSMA/CD, CSMA/CA.

### Slotted ALOHA

Nel protocollo slotted ALOHA si assume che tutti i pacchetti abbiano la stessa dimensione e il tempo sia suddiviso in slot, equivalenti al tempo di trasmissione di un singolo pacchetto.

Quando un nodo deve spedire, esso attenderà fino all'inizio dello slot successivo. Se nel mentre non si verifica una collisione il nodo potrà trasmettere il pacchetto nello slot successivo, altrimenti se avviene la collisione, essa verrà rilevata prima della fine dello slot e ritrasmetterà il pacchetto con probabilità *p* durante gli slot successivi.

Questo protocollo consente ai nodi di trasmettere continuamente alla massima velocità decidendo indipendentemente quando ritrasmettere (decentralizzazione). Dall'altro lato alcuni slot presenteranno collisioni andando sprecati ed altri rimarranno vuoti (inattivi)

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

*figura 5.36*

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

*figura 5.44*

## Struttura dei pacchetti Ethernet

Preambolo, MAC di destinazione, MAC di origine, tipo, payload dati e CRC (correzione degli errori).

- Il **preambolo** in specifico è costituito da 8 byte, i primi sette sono 10101010 e l'ultimo 10101011, e viene utilizzato per "attivare" il ricevitore quando viene inviato un pacchetto e per sincronizzare l'orologio con quello del trasmittente.
- Gli **indirizzi** sono costituiti da 6 byte. Se è presente l'indirizzo di destinazione o il broadcast il payload viene trasferito direttamente al livello di rete altrimenti il pacchetto viene ignorato.
- Il campo **tipo** consente a Ethernet di supportare i vari protocolli di rete (multiplexing).
- Il controllo **CRC** consente all'adattatore ricevente di rilevare la presenza di un errore nei bit del pacchetto.

*figura 5.45 pacchetto*

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

*figura 5-63*

### Switch e router a confronto

- Entrambi i dispositivi sono store-and-forward
  - Il router lavora a livello di rete (dominio di broadcast)
  - Lo switch lavora a livello di collegamento (dominio di collisione)
- I router mantengono tabelle d'inoltro e implementano algoritmi d'instradamento
- Gli switch mantengono tabelle di commutazione e implementano il filtraggio e algoritmi di autoapprendimento

# PPP: protocollo punto-punto

Protocollo estremamente semplice, prevede un solo collegamento tra mittente e destinatario. Non è necessario un protocollo di accesso al mezzo, non occorre un indirizzo MAC esplicito, veniva usato soprattutto nei collegamenti ISDN.

Sono presenti protocolli più complessi come **HDLC** (High-level Data Link Control).

## Requisiti di IETF (RFC 1547)

- **Framing dei pacchetti:** incapsulare il pacchetto di rete dentro una struttura riconoscibile a livello di link e a livello fisico
- **Trasparenza: **non porre nessuna restrizione alla configurazione dei dati
- **Rilevazione di errori**
- **Disponibilità della connessione: **rilevare eventuali guasti
- **Negoziazione degli indirizzi di rete: **necessario un protocollo per interagire con la rete per ottenere ad esempio un indirizzo IP

Le altre funzioni come correzione degli errori, controllo di flusso, riordinamento dei pacchetti **sono delegati ai livelli superiori.**

## Formato dei pacchetti dati PPP

- **Flag: **ogni pacchetto inizia e termina con un byte di valore **01111110**
- **Indirizzo: **unico valore **11111111** broadcast (solo due entità che comunicano)
- **Controllo: **unico valore, tutti i frame sono dello stesso tipo
- **Protocollo: **qual è il protocollo di livello superiore cui appartengono i dati incapsulati

*figura 5-70*

### Riempimento dei byte

Per il requisito di trasparenza non dev'essere possibile inserire nel campo informazioni la stringa flag **01111110**, poiché non sarebbe riconoscibile la fine del frame PPP.

Per ovviare a questo problema si ricorre alla tecnica del byte stuffing: si aggiunge un byte di controllo pari al flag prima di ogni byte di dati. In questo modo il destinatario riconoscerà la presenza di dati se sono presenti due byte **01111110** consecutivi.

*figura 5-72*

# Canali virtuali: una rete come un livello di link

## Il concetto di virtualizzazione

Per **virtualizzazione** si intende il processo di sostituzione di una versione fisica con una rappresentazione software. Consente di spezzare la dipendenza tra hardware e funzionalità software e una veloce innovazione, è più facile testare e progettare i servizi senza il bisogno dell'ambiente fisico. La virtualizzazione consente di isolare e partizionare le risorse disponibili, oltre alla possibilità di maggior controllo.

### Tipi di virtualizzazione

- **Virtualizzazione hardware:** astrarre la funzionalità logica dall'infrastruttura fisica (ad esempio programmazione con compilatore)
- **Virtualizzazione di rete: **reti virtuali basate su dispositivi virtuali, successivamente mappate su risorse fisiche
- **Cloud: **gestire al meglio grosse quantità di CPU, storage e rete fornendo un pool di risorse aggregate, con lo scopo di fornire le risorse in maniera scalabile (illusione di risorse infinite)

### Storia della virtualizzazione

La virtualizzazione inizia nell'era nei mainframe negli anni '60 dove le risorse di computazione venivano condivise tra molti utenti. Successivamente la virtualizzazione venne utilizzata nei datacenter, consentendo la suddivisione delle risorse disponibili, arrivando al giorno d'oggi con la virtualizzazione delle reti.

## Internet: virtualizzazione delle reti

Grazie all'indirizzamento IP e ai gateway divenne possibile collegare tra loro reti diverse per configurazione tra loro che prima non potevano comunicare. IP consente quindi di virtualizzare le reti.

### Architettura di Cerf e Kahn

Propone un primo tipo di virtualizzazione, dove il livello 3 (IP) rende tutto omogeneo, infatti il livello è necessariamente standard. La virtualizzazione ha consentito di spezzare l'indirizzamento a livello globale (livello 3) da quello locale (livello 2), virtualizzando le tecnologie di livello 2 (cavo, satellite, 56K).

### Ethernet e domini

- **Dominio di "collisione": **determinato dall'insieme degli host che possono risentire di una collisione generata da due postazione arbitraria
- **Dominio di "broadcast": **insieme degli host che ricevono i pacchetti in broadcast

*figura 5-82*

## LAN virtuali

Consente di definire più reti locali virtuali distinte utilizzando una stessa infrastruttura fisica. Ogni VLAN si comporta come una rete locale separata dalle altre:

- i pacchetti di broadcast sono confinati all'interno della VLAN
- la comunicazione a livello 2 è confinata all'interno della VLAN
- l'interconnessione tra più VLAN viene effettuata a livello 3 (è necessario un router)

Le VLAN sono definite nello standard 802.1q e nel 802.1d che riguarda la comunicazione tra diversi standard 802 attraverso bridge.

### Scopo delle VLAN

L'utilizzo delle VLAN consente:

- **risparmio: **definire più topologie virtuali utilizzando la stessa infrastruttura fisica. Non sono necessarie modifiche all'hardware
- **aumento di prestazioni: **costruire la rete in base alle esigenze del momento, limitando il traffico broadcast
- **aumento della sicurezza: **suddividere il traffico e isolarlo nelle varie VLAN
- **flessibilità: **più facile spostare un utente dal punto di vista logico da una VLAN a un'altra

### Requisiti sui bridge

Per implementare le VLAN è necessario che gli apparati supportino lo standard 802.1q, con il quale è possibile definire due tipologie di VLAN: la port based (privata) e quella tagged (802.1q).

Bisogna anche istruire bridge e switch perché riconoscano le VLAN, non è possibile farlo in autoapprendimento ma vanno preprogrammati.

### Funzioni del bridge in 802.1q

Per supportare le VLAN è necessario che i bridge svolgano le seguenti tre funzioni:

- **ingress: **l'apparato deve capire a quale VLAN appartiene il frame in ingresso
- **forwarding: **effettuare l'inoltro in base alla VLAN di appartenenza
- **egress: **deve poter trasmettere il frame in uscita in modo che la VLAN sia interpretabile dagli altri bridge

### Port based VLAN (untagged)

Tecnica abbastanza semplice, assegna in maniera statica ciascuna porta del bridge a una VLAN definita con la configurazione del bridge. Permette di costruire su un singolo apparato 2 o più bridge logici.

Le funzioni del bridge sono semplici:

- ingress: un frame in ingresso appartiene alla VLAN a cui è assegnata la porta
- forwarding: frame inoltrato solamente verso le porte appartenenti alla stessa VLAN (forwarding database distinto per ogni VLAN)
- egress: determinata la porta il frame viene trasmesso così com'è

Le VLAN untagged non richiedono di modificare i pacchetti Ethernet poiché viene definito tutto sul bridge (che dev'essere compatibile con lo standard 802.1q).

### VLAN 802.1q (tagged)

Con questo standard è possibile far condividere lo stesso link fisico da tra VLAN differenti, stampando nel pacchetto di livello 2 la VLAN di appartenenza, aggiungendo nel frame Ethernet 4 byte che trasportano le informazioni sulla VLAN. Questo identificativo (VLAN tag) deve essere uguale per tutti i bridge che saranno tutti programmati per riconoscere tale VLAN.

#### Frame Ethernet 802.1q

Vengono aggiunti 4 byte dopo gli indirizzi di sorgente e destinazione i quali conterranno il **TPI** (Tag Protocol Identifier), che specifica che il frame è aderente a 802.1q e il **TCI** (Tag Control Information) che trasporta informazioni relative alla VLAN (priorità, interconnessione e VLAN tag).

*figura 7-90*

#### Considerazioni sul frame

La modifica proposta da 802.1q richiede anche la modifica della dimensione massima di 1518 bytes con l'aggiunta di due byte. Inoltre, il campo TPI ha un valore non utilizzato come "protocol type" nei frame Ethernet ordinari in modo che sia riconoscibile che il frame è di tipo 802.1q ma una scheda non compatibile non scarti il frame.

### VLAN con switch/router

Molti produttori consentono di implementare all'interno degli switch funzionalità di routing (livello 3), permettendo di interconnettere tra loro diverse VLAN mantenendo la separazione dei domini di broadcast.

Utilizzando porte configurate in modalità TRUNK è possibile trasportare su un unico cavo i dati di più VLAN, lasciando agli switch il compito di inoltrare i pacchetti correttamente.

### Porte tagged e untagged

Negli switch 802.1q tutte le porte devono essere associate a una o più VLAN: se la porta è associata a una VLAN untagged i frame ricevuti non trasporteranno tag ne lo trasporteranno i frame in uscita, il link su tali porte si chiama *access link*.

Se la porta è in modalità tagged, il link si chiamerà *trunk link* e la VLAN di appartenenza del frame sarà definita dal valore nel tag.

### Protocol based VLAN

Esiste la possibilità di assegnare un frame a una VLAN in maniera dinamica, sulla base di diversi parametri opportunamente configurati negli apparati (richiesti particolarmente evoluti).

Viene effettuato il packet filtering in base a delle regole come IP del mittente, protocol type, indirizzo Ethernet. Può essere definita una associazione statica, che avrà precedenza sulle altre regole.

Alcuni protocolli proprietari consentono di configurare le regole dinamiche in maniera centrale, importando le configurazioni tramite la rete, ad esempio non consentire l'accesso alle VLAN se il MAC address dell'host non è stato registrato dall'amministratore di rete.

###  Default VLAN

Gli switch 802.1q sono preconfigurati con una default VLAN assegnata col tag 1 e tutte le porte assegnate ad essa in modalità untagged, permettendo al primo accesso di tale switch un funzionamento tradizionale. Per poter modificare il VLAN ID associato a ciascuna porta bisognerà eliminare tale porta dalla VLAN per poi poterla riconfigurare.

### VLAN di management

In ogni rete IP sono presenti due piani di funzionalità: un piano dati (o data plane) dove circola il traffico degli utenti e un piano di controllo (control plane) relativo al traffico di controllo della rete (BGP).

Usando le VLAN è interessante poter creare una VLAN relativa alla gestione della rete, utilizzando la stessa infrastruttura.

## ATM e MPLS

ATM e MPLS sono delle soluzioni che consentono di generare circuiti virtuali utilizzando l'approccio a commutazione di pacchetto, consentono l'allocazione delle risorse per i flussi e la gestione della qualità del servizio. Queste tecnologie possono essere integrate nelle reti IP, attualmente sono utilizzate per interconnettere alcune zone della rete e non sono visibili all'utente.

### Trasferimento asincrono: ATM

ATM è nato verso metà anni 80 con l'obbiettivo di estendere la tecnologia delle reti telefoniche in modo tale da essere utilizzata per reti dati, progettando reti in grado di trasportare file audio e video in tempo reale e supportare file di testo e immagini. Può essere considerata la tecnologia telefonica di ultima generazione e viene tutt'ora usata nelle reti ADSL, consente la realizzazione di circuiti virtuali e l'implementazione del QoS.

#### Architettura ATM

*figura 5-102*

Segue la struttura TCP/IP, i terminali hanno 3 livelli mentre gli switch 2. I 3 livelli sono:

- **AAL (ATM adaptation layer): **presente nei dispositivi alla periferia della rete, svolge una funzione analoga al livello di Trasporto quindi segmentazione e riassemblaggio dei pacchetti e mappatura dei flussi nelle tipologie di QoS
- **ATM:** fulcro dell'architettura, considerato "livello di rete", definisce la struttura della cella ATM (pacchetto) e tutti i suoi campi
- **Livello fisico**

La rete ATM inizialmente era concepita come una rete stand-alone, che fosse in grado di trasportare dati "da una scrivania a un'altra", venendo considerata una tecnologia di rete. Dopo l'affermazione dello stack TCP/IP come standard di Internet, per far si che ATM si potesse integrare nella reti IP venne messo al suo di sotto, diventando un livello 2, o livello di link commutato (utilizzato solo in alcune reti al di sotto di IP).

*figura 5-103*

##### AAL: ATM Adaptation Layer

Presente solo negli host terminali, adatta i livelli superiori al livello ATM sottostante frammentandoli adeguatamente (come nella segmentazione TCP in pacchetti IP).

Ci sono diverse tipologie (o profili) AAL che suggeriscono il QoS richiesto:

- **AAL1:** servizio a tasso costante, CBR, come nei servizi tradizionali per il traffico telefonico
- **AAL2:** servizio a tasso variabile, VBR, adeguato per la trasmissione di video MPEG
- **AAL5:** servizio dati (datagram IP)

##### Livello ATM

Offre il servizio di trasporto di celle attraverso la rete ATM, analogo al livello di rete IP con servizi però molto differenti.

*tabella 5-106*

Il livello ATM implementa una rete a pacchetto a circuiti virtuali, chiamati **canali virtuali (VC)**, percorsi con un collegamento diretto fra sorgente e destinazione. Ciascun pacchetto viene marchiato con l'indicatore **VCI** cosicché ogni switch saprà come interpretarlo. Al canale virtuale possono essere riservate **risorse dedicate** come banda e buffer per garantire il QoS.

I canali virtuali possono essere **permanenti**, per connessioni di lunga durata, utilizzati tra zone della rete lontane tra loro, oppure **dinamici** (creati su richiesta).

L'utilizzo di canali virtuali ha il vantaggio di poter controllare prestazioni e QoS, controllando la congestione della rete, ma ha gli svantaggi che potrebbe non esserci un profilo per ogni tipologia di dato da trasportare, e avendo un numero limitato di canali virtuali non è del tutto scalabile. Nel caso di connessioni di breve durata la costruzione del VC richiede tempo riducendo le prestazioni percepite.

La **cella ATM** è costituita da un'intestazione da 5 byte (VCI, Payload Type, Priorità sulla perdita di cella, e byte di controllo errore) e un carico utile da 48 byte, che consente una trasmissione più efficace avendo dimensione fissa e un ritardo minore data la piccola dimensione.

##### Livello fisico ATM

Suddiviso in due parti:

- **Transmission Convergence Sublayer (TCS):** adatta la cella creata da ATM al livello fisico sottostante creando il checksum, effettuando la delineazione della cella e consentendo la strutturazione del canale trasmettendo celle inattive se non ci sono dati da inviare (il canale fisico è come un nastro trasportatore di celle)
- **Physical Medium Dependent:** dipende dal mezzo fisico utilizzato

Il livello fisico di ATM consente il funzionamento con diverse tecnologie come SONET/SDH (reti ottiche sincrone), T1/T3 (fibra, microonde e cavo) o mappare le celle ATM su canali non strutturati (grazie alla delineazione di ATM).

#### IP su ATM

*figura 5-114*

Quando si raggiunge un router di ingresso nella rete ATM, questo router dovrà determinare come trasferire il datagramma attraverso la rete, utilizzando l'indirizzo IP di destinazione per capire dove inoltrarlo e utilizzando ARP per chiedere alla rete ATM di costruire il percorso relativo. 

Raggiunto il router di uscita verrà risalita la pila protocollare, incontrando AAL5 che permetterà di ricostruire il datagramma IP e di conseguenza la risalita del pacchetto IP e sua consegna.

Per il corretto funzionamento di IP su ATM sarà necessario quindi un protocollo ARP dedicato ad ATM e dei router dotati di uno stack protocollare e un'interfaccia di rete compatibili con lo standard.



