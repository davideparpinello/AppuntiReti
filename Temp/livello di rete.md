# Introduzione

## Funzioni chiave del livello di rete

Il livello di rete svolge due funzioni principali:

- **inoltro (o forwarding):** trasferimento dei pacchetti dall'input di un router all'output appropriato
- **instradamento (o routing):** determinare il percorso seguito dai pacchetti da origine a destinazione

Possiamo, per analogia, considerare l'inoltro come l'attraversamento di uno svincolo durante un viaggio e l'instradamento come la pianificazione dell'intero viaggio.

Per effettuare queste funzioni, viene inserito nell'header del pacchetto un'indicazione per l'instradamento, che verrà utilizzata dal router per fare un match con la sua tabella di routing.

*figura 4-5*

Una terza funzione importante del livello di rete è **l'impostazione della connessione**, viene utilizzata però solo in alcune reti (come ATM o X.25). Prima della trasmissione dei dati viene stabilita una connessione virtuale tra i due host.

## Modelli di servizio

Il livello di rete offre diversi modelli di servizio: possiamo ricordare il servizio **best effort** utilizzato sulla rete internet attuale e i modelli **CBR (Constant), VBR (Variable), ABR (Available), UBR** utilizzati in ATM, per diversi anni utilizzata alternativa a Internet.

# Reti a circuito virtuale e datagramma

Le reti a datagramma offrono solo il servizio senza connessione, mentre le reti a **circuito virtuale** offrono il servizio con connessione. Questa è realizzata da host a host e non si può scegliere (il livello di rete non può fornirli entrambi contemporaneamente).

## Reti a circuito virtuale

Le reti a circuito virtuale (come ATM) sono reti analoghe ai circuiti telefonici che consentono buone prestazioni e vedono il coinvolgimento della rete: il pacchetto ha un numero di VC nell'header utilizzato e sostituito dai router, questo numero consente di effettuare multiplexing.

I router di queste reti mantengono le informazioni sullo stato delle connessioni, infatti ogni router ha una tabella di inoltro con tutti i VC utilizzati.

*figura 4-12*

### Protocolli di segnalazione

I protocolli di segnalazione, non utilizzati sulla rete Internet, sono dei messaggi utilizzati per gestire la connessione sui VC quindi avvio, mantenimento e chiusura dei circuiti.

## Reti a datagramma

Nelle reti a datagramma non avviene l'impostazione di chiamata: i dati vengono inviati quando si vuole e quando la rete può li inoltrerà. In aggiunta, la rete è stateless (senza connessione) e i pacchetti vengono inoltrati utilizzando l'indirizzo di destinazione.

Le tabelle di inoltro sono costituite da 4 miliardi di indirizzi possibili (32 bit), queste tabelle spesso sono realizzate ad intervalli. Per semplificazione si preferisce effettuare il matching confrontando i prefissi tra datagram e tabella, nel caso in cui i bit siano uguali si aumenta il numero di bit confrontati.

## Confronto tra le due tipologie

- **Circuiti virtuali (ATM)**
  - eredita dalla telefonia
  - requisiti più stringenti in termini di tempo e affidabilità, utilizzate per servizi garantiti
  - i terminali sono stupidi: la complessità è interna alla rete (es. controllo di flusso)
- **Datagrammi (internet)**
  - scambio di dati tra calcolatori
  - i servizi sono elastici: pochi requisiti di tempo
  - L'interconnessione è semplice: adattabile, controllo degli errori, rete interna semplice ed ottimizzata alla velocità
  - svariati tipi di link, servizio non uniforme

# Cosa si trova all'interno dei router?

## Architettura

I router svolgono due funzioni principali:

- far girare i protocolli e gli algoritmi di instradamento (RIP, OSPF, BGP)
- inoltrare i datagram dagli input agli output

*figura 4-19*

## Porte d'ingresso

*figura 4-20*

Le porte d'ingresso dei router vengono utilizzate per effettuare una commutazione decentralizzata:

- determinare la porta d'uscita dei pacchetti utilizzando le informazioni della tabella d'inoltro
- obiettivo: completare l'elaborazione allo stesso tasso della linea
- si presenta un **accodamento** se il tasso d'arrivo dei pacchetti è maggiore del tasso d'inoltro

## Tecniche di commutazione

*figura 4-21*

La tecnica di commutazione **in memoria** è la più semplice ma la meno performante, la tecnica **a crossbar** è invece la più efficace.

### Commutazione in memoria

La tecnica di commutazione in memoria veniva utilizzata nelle prime generazioni di router, i quali erano tradizionali calcolatori. La commutazione veniva controllata dalla CPU: il pacchetto veniva copiato in memoria e successivamente mandato in uscita con una frequenza totale di **B/2**.

### Commutazione tramite bus

La commutazione tramite bus si basa sull'utilizzo di un bus condiviso tra tutte le porte: siamo in presenza di una **contesa per il bus**, la cui banda limita di conseguenza la banda della commutazione.

Un esempio di router è il Cisco 5600, che opera con un bus da 32 Gbps, sufficiente per reti d'accesso o aziendali.

### Commutazione attraverso rete d'interconnessione

Questa tecnica supera il limite di banda di un bus condiviso. Viene utilizzato un **crossbar switch**, ovvero una rete d'interconnessione di *2n* bus che collegano *n* porte d'entrata a *n* porte d'uscita.

Un esempio come il Cisco 12000 raggiunge i 60 Gbps nella struttura di commutazione.

## Porte d'uscita

Le porte d'uscita implementano le funzioni di accodamento (se la frequenza di pacchetti in arrivo è superiore a quella del collegamento uscente) e di schedulatore di pacchetti (stabilire l'ordine di trasmissione dei pacchetti).

## Quale deve essere la capacità dei buffer?

Secondo la regola spannometrica della RFC 3439 la capacità deve essere
$$
media \space RTT \space*\space capacita \space C
$$
dove C è la capacità del collegamento.

Secondo attuali raccomandazioni, dati N flussi la capacità dei buffer deve essere
$$
\frac{RTT*C}{\sqrt{N}}
$$

# Protocollo Internet (IP)

*figura 4-28*

## Formato dei datagrammi

*figura 4-30*

I datagrammi IP utilizzano 16 byte, hanno quindi una dimensione massima di 64 KB. Il campo del protocollo di livello superiore indica il protocollo utilizzato dal pacchetto trasmesso, quindi TCP/UDP. Avendo a disposizione 32 bit per gli indirizzi abbiamo in totale poco più di 4 miliardi di indirizzi.

### Frammentazione dei datagrammi

La frammentazione dei datagrammi è una funzione importante poiché consente il trasporto da parte del livello sottostante (collegamento) di datagrammi più grandi di quanto ne potrebbe portare.

Se la **MTU** (Maximum Transmission Unit, quantità massima di dati che un frame a livello di collegamento può portare) è più bassa della dimensione del datagram IP, il livello 3 frammenta i datagram troppo grossi in altri datagram, che verranno riassemblati solo a destinazione. Ogni router della rete può frammentare in base alle sue esigenze.

Quando viene effettuata la frammentazione viene settato a 1 il flag relativo nell'header di ogni pacchetto inviato (tranne l'ultimo) e in ognuno viene indicato lo spiazzamento (utilizzato per riposizionare il datagramma nel giusto payload e in posizione corretta).

*figura 4-32*

Il destinatario ricostruirà il datagram originale riconoscendo gli ID comuni, sapendo che se lo spiazzamento è uguale a 0 sarà il primo, mentre se il flag sarà uguale a 0 sarà l'ultimo.

## Indirizzamento IPv4

L'indirizzamento IPv4 consente il posizionamento dei dispositivi nella rete globale: infatti, ogni **interfaccia** di ciascun host collegato su Internet ha un IP globalmente univoco. Questo indirizzo è costituito da 32 bit.

L'interfaccia è il confine tra host e collegamento fisico: i router devono avere almeno due interfacce (2 indirizzi IP) mentre gli host, in genere, hanno una sola interfaccia.

### Sottoreti

Le sottoreti consentono di dividere l'indirizzamento in Internet in parti a sè stanti. Per dare una definizione appropriata possiamo considerare le sottoreti come delle reti isolate i cui punti terminali sono collegati all'interfaccia di un host o un router.

Questo è possibile suddividendo gli indirizzi IP in due parti: una parte di **sottorete**, condivisa fino a un certo punto, e una parte di **host**.

*figura 4-36*

La **maschera di rete** viene utilizzata per indicare quanti bit da sinistra contengono il prefisso condiviso tra tutti gli host della sottorete.

#### CIDR

CIDR, o (Classless Interdomain Routing), è una strategia di assegnazione degli indirizzi che definisce il prefisso comune agli apparati in una rete.

Ha una struttura del tipo *a.b.c.d/x* dove x indica il numero di bit della prima parte dell'indirizzo (parte subnet).

*figura 4-38*

La strategia CIDR facilita ai router la costruzione delle tabelle di inoltro e consente a un PC di sapere se un host si trova nella sua stessa sottorete (facendo l'AND tra indirizzo e maschera di rete troverà il prefisso che confronterà).

### Assegnazione degli indirizzi

L'assegnazione degli indirizzi può avvenire in modalità manuale o mediante **DHCP**. Il DHCP (Dynamic Host Configuration Protocol) consente di ottenere dinamicamente un IP da un server (plug-and-play), l'IP avrà una scadenza e potrà essere rinnovato, consente il riuso e il risparmio di indirizzi.

*figura 4-41*

*figura 4-42*

L'amministratore di rete o l'ISP ha a disposizione un blocco di IP da utilizzare nelle subnet: se sono privati non si pone alcun problema, mentre se sono pubblici questi dovranno essere comprati da altri ISP. A sua volta, un ISP o un amministratore può dividere il blocco a sua disposizione in altri sottoblocchi contigui.

#### Indirizzamento gerarchico

*figura 4-45*

#### Indirizzi IP alla fonte

Come fa un ISP a ottenere indirizzi IP? Si rivolge all'ICANN (Internet Corporation for Assigned Names and Numbers), ente che si occupa di allocare i blocchi di indirizzi, gestire i server DNS radice, assegnare e risolvere dispute sui domini.

### Traduzione degli indirizzi di rete (NAT)

La traduzione degli indirizzi di rete consente ai router di apparire a Internet con un unico indirizzo IP, quindi tutto il traffico della sottorete avrà lo stesso indirizzo. Si utilizzano all'interno classi di IP private come *10.0.0.0/8* e *192.168.0.0/16*.

Utilizzando il NAT il router abiulitato nasconde i dettagli della rete domestica al mondo esterno, portando alcuni vantaggi:

- non serve allocare intervalli di indirizzi da un ISP
- è possibile riconfigurare la rete privata senza comunicarlo a Internet
- è possibile cambiare ISP senza modificare la configurazione della rete privata
- i dispositivi interni non sono indirizzabili e visibili dall'esterno (sicurezza)

Per implementare il NAT il router, all'arrivo di un datagramma, genera una nuova porta d'origine e sostituisce  l'IP di origine con il proprio IP sul lato WAN e la porta di origine iniziale con il nuovo numero.

*figura 4-50*

Il numero di porta è costituito da 16 bit, quindi sono possibili più di 60000 connessioni simultanee. NAT è contestato per diversi motivi:

- i router dovrebbero lavorare solo a livello 3+
- viola il punto-punto, causando interferenze a P2P (serve una specifica configurazione)
- si dovrebbe usare alternativamente IPv6

#### Collegamenti dall'esterno

Un problema importante del NAT è l'impossibilità di effettuare collegamenti dall'esterno a un host interno alla rete (come un server web). Per effettuare questo si possono implementare alcune soluzioni

- impostare delle configurazioni statiche per inoltrare le richieste entranti a determinate porte dell'host (tabelle di forwarding)
- utilizzare **UPnP** (Universal Plug n Play), parte integrante di IGD (Internet Gateway Device protocol), che consente agli host nascosti da un NAT di chiedere in automatico di scrivere una riga nella tabella di forwarding
- **relay** (utilizzato da Skype), prevede un punto di riferimento a cui entrambi i client si collegano

*figura 4-54*

## ICMP

ICMP (Internet Control Message Protocol) consente lo scambio di informazioni relative al controllo della rete, come errori o ping. ICMP è considerato parte di IP, i messaggi hanno un campo tipo e un campo codice mentre l'intestazione e i primi 8 byte sono uguali al datagram IP.

### Traceroute e ICMP

Il programma invia più datagrammi con un TTL incrementale, ogni router scarterà il datagram e invierà all'origine un'allerta ICMP la quale calcolerà il RTT. Ogni router viene calcolato per 3 volte per avere una media e il programma si fermerà una volta arrivato a destinazione, ovvero quando l'host invierà un pacchetto ICMP di porta non raggiungibile.

## IPv6

L'esigenza principale che ha portato allo sviluppo di IPv6 è stato lo spazio di indirizzamento IP a 32 bit che stava cominciando ad esaurirsi. Oltre a questo, altre motivazioni erano un header più leggero per velocizzare elaborazione e inoltro, e un agevolazione del QoS.

### Formato dei datagrammi

I datagram IP sono costituiti da 40 byte di header a lunghezza fissa e non è consentita la frammentazione.

*figura 4-60*

Alcuni campi nuovi sono la **priorità di flusso**, l'**etichetta di flusso** che identifica flussi particolari (non è chiaro il concetto di flusso) e l'**intestazione successiva** che identifica il protocollo di destinazione dei contenuti.

Inoltre, è stato rimosso il checksum poiché ridondante, il campo opzioni non è più parte dell'intestazione ma può venire indicato in "intestazioni successive" ed è stato introdotto **ICMPv6** con nuovi codici che assume le funzionalità di IGMP (gestisce l'ingresso e l'uscita di host dai gruppi multicast).

### Passaggio a IPv6

Non è possibile aggiornare simultaneamente tutti i router, poiché servirebbe stabilire una giornata "di passaggio", attualmente impossibile. Per questo si utilizza il **tunneling**: IPv6 viene trasportato come payload in datagram IPv4.

*figura 4-63*

# Algoritmi di instradamento

Gli algoritmi di instradamento sono implementati nelle reti a commutazione di pacchetto, grazie all'inserimento di un valore nell'header di livello 3, che verrà letto e confrontato con le varie tabelle di instradamento.

*figura 4-67*

Possiamo assumere una rete come un grafo dove i nodi sono i router e gli archi sono i collegamento. Gli algoritmi di instradamento si occupano di calcolare il cammino a costo minimo tra due router.

Questi algoritmi possono essere classificati in più modalità: globali o decentralizzati e statici o dinamici:

- un algoritmo **globale** riceve in ingresso tutti i collegamenti tra nodi e loro costi, un esempio sono gli algoritmi **link-state**
- in un algoritmo **decentralizzato** ogni nodo elabora un vettore di stima dei costi verso tutti gli altri nodi, quindi il cammino a costo minimo viene calcolato in modo distribuito e iterativo. Un esempio sono gli algoritmi **distance-vector**
- in un algoritmo **statico** i cammini cambiano molto raramente
- gli algoritmi **dinamici** determinano gli instradamenti in base al traffico e alla topologia della rete

## Stato del collegamento (link state)

Un esempio di algoritmo link state è l'**algoritmo di Dijkstra** il quale prevede che la topologia di rete e i costi siano noti a tutti i nodi mediante il "link-state broadcast" e che tutti i nodi dispongano delle stesse informazioni.

L'algoritmo calcola il cammino a costo minimo dall'origine a tutti gli altri nodi, creando una **tabella d'inoltro** per quel nodo. Essendo iterativo, dopo la k-esima iterazione i cammini a costo minimo sono noti a k nodi di destinazione.

I costi di ogni collegamento possono essere:

- predefiniti dal provider
- tutti uguali
- costi effettivi (satellite)
- definiti dal ritardo di collegamento

La regola è evitare che i costi dipendano dal routing.

La sua complessità con n nodi è data da
$$
O(nlogn)
$$
Può presentare delle oscillazioni ad esempio nel costo del collegamento in base alla quantità di traffico trasportato.

## Algoritmo con vettore distanza

Un esempio di algoritmo con vettore distanza è la **formula di Bellman-Ford** (o a programmazione dinamica) che definisce il costo del percorso a costo minimo da x a y
$$
d_x(y) = min_v\{c(x,v) + d_v(y)\}
$$
dove min_v riguarda tutti i vicini di x, c(x,v) comprende il costo di tutti i collegamenti diretti da x a v e d_v(y) è il percorso a costo minimo da v a y.

L'algoritmo viene eseguito nel modo seguente: il nodo x contatta tutti i vicini v e si fa dare da ognuno di essi ogni costo per andare a y, successivamente valuterà tutte le alternative.

L'idea di base è che ogni nodo invia una copia del proprio vettore distanza a ogni vicino, quando un nodo riceve un nuovo vettore distanza da un vicinolo salva e usa la formula B-F per aggiornare il proprio DV. Finché tutti i nodi continuano a cambiare i propri DV in maniera asincrona, ciascuma stima Dx(y) coverge a d_x(y).

L'algoritmo con vettore distanza è **iterativo ed asincrono**, poiché ogni iterazione locale è causata dal cambio del costo di un link locale o dalla ricezione di un DV aggiornato; è **distribuito** poiché ogni nodo aggiorna i suoi vicini solo quando il suo DV cambia.

### Modifica dei costi

Quando un nodo rileva un cambiamento nel costo dei collegamenti allora aggiorna il proprio vettore distanza e lo trasmetterà ai suoi vicini, secondo il principio che "le buone notizie viaggiano in fretta".

### Confronto tra algoritmi LS e DV

- Complessità dei messaggi
  - LS con n nodi, E collegamenti implica l'invio di O(nE) messaggi
  - DV richiede scambi tra nodi adiacenti, quindi il tempo di convergenza può variare
- Velocità di convergenza
  - LS: l'algoritmo O(n<sup>2</sup>) richiede O(nE) messaggi, ci possono essere oscillazioni di velocità
  - DV può convergere lentamente, può presentare cicli d'instradamento e può presentare il problema del conteggio all'infinito
- **Robustezza:** cosa avviense se un router funziona male
  - LS: un router può comunicare via broadcast un costo sbagliato per uno dei suoi collegamenti (non per altri), i nodi si occupano di calcolare solo le proprie tabelle
  - DV: un nodo può comunicare cammini a costo minimo errati a tutte le destinazioni, la tabella di ogni nodo può essere usata da altri quindi <u>un calcolo errato si diffonde per l'intera rete</u>

## Instradamento gerarchico

Fino ad ora abbiamo visto la rete come un insieme di router interconnessi, con una visione omogenea, ma nella pratica le cose non sono così semplici. Nella realtà ci sono 200 milioni di destinazioni, quindi archiviare tutte le informazioni di instradamento su ogni host sarebbe impossibile data l'enorme quantità di memoria necessaria e l'elevato traffico (bloccherebbe il resto) che si creerebbe.

Per questo, conviene impostare la rete Internet con una **autonomia amministrativa**, secondo la quale idealmente ciascuno sarebbe in grado di amministrare la propria rete connettendola alle altre.

Nella realtà è possibile organizzare i router in **sistemi autonomi** (AS), dove in ogni gruppo autonomo i router eseguono lo stesso algoritmo di instradamento (protocollo **intra-AS**) mentre i **router gateway** hanno il compito di inoltrare i pacchetti a destinazioni esterne.

Ogni sistema autonomo sa come inoltrare i pacchetti lungo il percorso ottimo verso qualsiasi destinazione interna al gruppo, mentre per trasferire dati tra sistemi autonomi differenti (i quali potrebbero usare protocolli differenti) si utilizza l'instradamento **inter-AS**.

*figura 4-91*

*tabella 4-94*

# Instradamento in Internet

I protocolli d'instradamento *intra-AS* sono noti come protocolli gateway interni (**IGP**), i più comuni sono:

- **RIP**: Routing Information Protocol
- **OSPF**: Open Shortest Path First
- **IGRP**: Interior Gateway Routing Protocol (proprietario Cisco)

## RIP (Routing Information Protocol)

RIP è un protocollo a vettore distanza incluso in UNIX BSD dagli anni '80. Effettua un conteggio degli hop come metrica di costo con un massimo di 15 hop.

I router adiacenti si scambiano aggiornamenti ogni 30 secondi mediante l'annuncio RIP (*RIP advertisement*), contenente un elenco di fino a 25 sottoreti di destinazione interne all'AS, insieme alla distanza tra il mittente ed esse.

Se un router non riceve notizie dal vicino per più di 180 secondi il nodo viene considerato spento, con il ricalcolo della tabella e la propagazione dell'informazione agli altri vicini. L'utilizzo dell'*inversione avvelenata* evita i loop (16 hop).

RIP viene implementato a livello applicazione con messaggi su socket standard e protocollo di trasporto standard.

## OSPF (Open Shortest Path First)

Essendo open, le specifiche del protocollo sono pubblicamente disponibili. Protocollo a link-state, utilizza il flooding di informazioni di stato del link e l'algoritmo di Dijkstra per determinare il percorso a costo minimo. Ogni volta che si verifica un cambiamento su un link il router inoltra l'informazione a tutti i router (all'intero AS) utilizzando il flooding, i messaggi sono trasportati da IP.

OSPF presenta alcuni vantaggi rispetto a RIP:

- **sicurezza**: scambi tra router autenticati
- **multipath**: consente l'utilizzo di più percorsi con uguale costo
- per ogni link possono esserci più costi in base al servizio (es. satellite costo elevato)
- **supporto unicast e multicast**: viene utilizzato OSPF Multicast
- **supporto alle gerarchie** in un dominio d'instradamento

### OSPF strutturato gerarchicamente

*figura 4-107*

La struttura gerarchica in OSPF consente di impostare due livelli: area locale e dorsale, nelle quali i messaggi LS sono solo all'interno dell'area e ogni nodo conosce la direzione verso le reti nelle altre aree.

I **router di confine d'area** appartengono sia alla dorsale che a un'area generica, i **router di dorsale** effettuano l'instradamento interno alla dorsale, i **router di confine** scambiano informazioni con router di altri AS.

## BGP: instradamento inter-AS

Il protocollo BGP (Border Gateway Protocol) è l'attuale standard *de facto* per l'instradamento inter-AS, che mette a disposizione di ciascun AS le seguenti funzionalità:

1. ottenere informazioni sulla raggiungibilità delle sottoreti da parte di AS confinanti
2. propagare le informazioni di raggiungibilità a tutti i router interni di un AS
3. determinare percorsi "buoni" verso le sottoreti sulla base delle informazioni di raggiungibilità e delle politiche dell'AS

In breve, BGP consente alle sottoreti di comunicare la propria esistenza alla rete Internet.

### Fondamenti di BGP

Due router che si scambiano messaggi BGP sono chiamati **peer BGP** mentre la connessione TCP è detta **sessione BGP**. Quando un AS annuncia un prefisso ad un altro AS, sta in realtà "promettendo" di inoltrare i datagrammi sul prefisso stabilito, un AS può aggregare più prefissi in un annuncio.

*figura 4-111*

Le sessioni BGP interne sono utilizzate per distribuire i prefissi a tutti i router del AS, mentre le sessioni esterne scambiano informazioni sulla raggiungibilità dei prefissi. 

#### Attributi del percorso

Quando viene annunciato un prefisso, nel messaggio vengono aggiunti anche degli attributi BGP come:

- **AS-PATH** che elenca gli AS che ha attraversato l'annuncio
- **NEXT-HOP** che indica l'eventuale collegamento fisico su cui viene inoltrato il pacchetto

Ogni router gateway ha delle **politiche di importazione** per decidere se accettare o filtrare la rotta.

*figura 4-114*

#### Selezione dei percorsi

Nel caso in cui siano presenti più rotte si seguono alcune regole di eliminazione:

1. si preferiscono le rotte con dei valori di preferenza locale più alti
2. si seleziona la rotta con AS-PATH più breve
3. si seleziona la rotta con il router NEXT-HOP più vicino, instradamento a *patata bollente*
4. se avanzano più rotte, ci si basa sugli identificatori BGP

#### Messaggi BGP

- **OPEN**: apre la connessione TCP e autentica il mittente
- **UPDATE**: annuncia il nuovo percorso
- **KEEPALIVE**: mantiene la connessione attiva in mancanza di UPDATE
- **NOTIFICATION**: riporta gli errori del precedente messaggio; usato anche per chiudere il collegamento.

### Differenze tra i protocolli inter-AS e intra-AS

- **Politiche**
  - Inter-AS: l'amministrazione vuole controllare l'instradamento del traffico e chi instrada attraverso le sue reti
  - Intra-AS: un solo controllo amministrativo, rotte interne scelta senza questioni di politica importanti
- **Scala**: l'instradamento gerarchico fa risparmiare sulle tabelle d'instradamento riducendo il traffico di aggiornamento
- **Prestazioni**: Intra-AS orientato alle prestazioni, Inter-AS le politiche possono prevalere sulle prestazioni