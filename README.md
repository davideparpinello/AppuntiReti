# AppuntiReti
 Appunti del corso di Reti tenuto dal prof. Fabrizio Granelli, A.A. 2019-20
 Dipartimento di Ingegneria e Scienze dell'Informazione, Unitn

## Contenuti
 Attualmente la dispensa contiene i seguenti capitoli:

1. [Roadmap](#roadmap)
   -   [Internet](#internet)
   -   [Ai confini della rete](#ai-confini-della-rete)
   -   [Il nucleo della rete](#il-nucleo-della-rete)
       -   [Esempio di commutazione di
           circuito](#esempio-di-commutazione-di-circuito)
       -   [Esempio di commutazione di
           circuito](#esempio-di-commutazione-di-circuito-1)
       -   [Confronto fra commutazione di pacchetto e di
           circuito](#confronto-fra-commutazione-di-pacchetto-e-di-circuito)
       -   [Struttura gerarchica](#struttura-gerarchica)
   -   [Ritardi, perdite e throughput nelle reti a comunicazione di
       pacchetto](#ritardi-perdite-e-throughput-nelle-reti-a-comunicazione-di-pacchetto)
       -   [Ritardo di un nodo](#ritardo-di-un-nodo)
   -   [Livelli di protocollo e i loro modelli di
       servizio](#livelli-di-protocollo-e-i-loro-modelli-di-servizio)
   -   [Reti sotto attacco: la
       sicurezza](#reti-sotto-attacco-la-sicurezza)
2. [Il livello Applicazione](#il-livello-applicazione)
   -   [Principi delle applicazioni di
       rete](#principi-delle-applicazioni-di-rete)
   -   [Web e HTTP](#web-e-http)
       -   [Connessioni non persistenti](#connessioni-non-persistenti)
       -   [Connessioni persistenti](#connessioni-persistenti)
       -   [Messaggi HTTP](#messaggi-http)
       -   [Cookies](#cookies)
       -   [Cache web (server proxy)](#cache-web-server-proxy)
       -   [HTTP/2.0](#http2.0)
   -   [FTP](#ftp)
       -   [Comandi comuni](#comandi-comuni)
       -   [Codici di ritorno comuni](#codici-di-ritorno-comuni)
   -   [Posta elettronica](#posta-elettronica)
   -   [DNS](#dns)
       -   [Protocollo DNS](#protocollo-dns)
   -   [Condivisione di file P2P](#condivisione-di-file-p2p)
       -   [Confronto tra architettura server client e
           P2P](#confronto-tra-architettura-server-client-e-p2p)
   -   [Cloud Computing](#cloud-computing)
       -   [CDN](#cdn)
3. [Il livello di Trasporto](#il-livello-di-trasporto)
   -   [Servizi a livello di
       trasporto](#servizi-a-livello-di-trasporto)
       -   [Demultiplexing senza
           connessione](#demultiplexing-senza-connessione)
       -   [Demultiplexing orientato alla
           connessione](#demultiplexing-orientato-alla-connessione)
   -   [Trasporto senza connessione:
       UDP](#trasporto-senza-connessione-udp)
   -   [Principi del trasferimento dati
       affidabile](#principi-del-trasferimento-dati-affidabile)
       -   [Rdt 1.0: trasferimento affidabile su canale
           affidabile](#rdt-1.0-trasferimento-affidabile-su-canale-affidabile)
       -   [Rdt 2.0: canale con errori nei
           bit](#rdt-2.0-canale-con-errori-nei-bit)
       -   [Rdt 2.1](#rdt-2.1)
       -   [Rdt 2.2: un protocollo senza
           NAK](#rdt-2.2-un-protocollo-senza-nak)
       -   [Rdt 3.0: canali con errori e
           perdite](#rdt-3.0-canali-con-errori-e-perdite)
       -   [Pipelining](#pipelining)
   -   [Trasporto orientato alla connessione:
       TCP](#trasporto-orientato-alla-connessione-tcp)
       -   [TCP: controllo di flusso](#tcp-controllo-di-flusso)
       -   [Gestione della connessione](#gestione-della-connessione)
   -   [Principi del controllo di
       congestione](#principi-del-controllo-di-congestione)
   -   [Controllo di congestione in TCP
       (AIMD)](#controllo-di-congestione-in-tcp-aimd)
       -   [Partenza lenta](#partenza-lenta)
       -   [Riassunto: controllo di
           congestione](#riassunto-controllo-di-congestione)
       -   [Throughput TCP](#throughput-tcp)
       -   [Equità di TCP](#equità-di-tcp)
4. [Il livello di Rete](#il-livello-di-rete)
   -   [Introduzione](#header-n0)
       -   [Funzioni chiave del livello di rete](#header-n2)
       -   [Modelli di servizio](#header-n13)
   -   [Reti a circuito virtuale e datagramma](#header-n15)
       -   [Reti a circuito virtuale](#header-n17)
       -   [Reti a datagramma](#header-n23)
       -   [Confronto tra le due tipologie](#header-n26)
   -   [Cosa si trova all'interno dei router?](#header-n48)
       -   [Architettura](#header-n49)
       -   [Porte d'ingresso](#header-n57)
       -   [Tecniche di commutazione](#header-n67)
       -   [Porte d'uscita](#header-n78)
       -   [Quale deve essere la capacità dei buffer?](#header-n80)
   -   [Protocollo Internet (IP)](#header-n86)
       -   [Formato dei datagrammi](#header-n88)
       -   [Indirizzamento IPv4](#header-n97)
       -   [ICMP](#header-n151)
       -   [IPv6](#header-n155)
   -   [Algoritmi di instradamento](#header-n165)
       -   [Stato del collegamento (link state)](#header-n179)
       -   [Algoritmo con vettore distanza](#header-n196)
       -   [Instradamento gerarchico](#header-n228)
   -   [Instradamento in Internet](#header-n235)
       -   [RIP (Routing Information Protocol)](#header-n244)
       -   [OSPF (Open Shortest Path First)](#header-n249)
       -   [BGP: instradamento inter-AS](#header-n267)
5. [Il livello di collegamento](#il-livello-di-collegamento)
   -   [Livello di collegamento: introduzione e servizi](#header-n0)
       -   [Servizi offerti a livello di link](#header-n4)
       -   [Dov'è implementato il livello link](#header-n30)
   -   [Tecniche di rilevazione e correzione degli errori](#header-n33)
       -   [Controllo di parità](#header-n35)
   -   [Protocolli di accesso multiplo](#header-n39)
       -   [Protocolli a suddivisione del canale](#header-n57)
       -   [Protocolli ad accesso casuale](#header-n64)
       -   [Protocolli MAC a rotazione](#header-n81)
       -   [Riepilogo dei protocolli](#header-n86)
   -   [Indirizzi a livello di collegamento](#header-n109)
       -   [Indirizzi MAC e ARP](#header-n110)
   -   [Ethernet](#header-n135)
       -   [Topologia](#header-n145)
       -   [Struttura dei pacchetti Ethernet](#header-n149)
       -   [Servizio senza connessione non affidabile](#header-n161)
       -   [Fasi operative del protocollo CSMA/CD](#header-n167)
       -   [Efficienza di Ethernet](#header-n180)
       -   [Ethernet 802.3](#header-n194)
       -   [Codifica Manchester](#header-n197)
   -   [Switch a livello di collegamento](#header-n200)
       -   [Hub](#header-n201)
       -   [Switch](#header-n208)
   -   [PPP: protocollo punto-punto](#header-n241)
       -   [Requisiti di IETF (RFC 1547)](#header-n244)
       -   [Formato dei pacchetti dati PPP](#header-n257)
   -   [Canali virtuali: una rete come un livello di
       link](#header-n272)
       -   [Il concetto di virtualizzazione](#header-n273)
       -   [Internet: virtualizzazione delle reti](#header-n285)
       -   [LAN virtuali](#header-n296)
       -   [ATM e MPLS](#header-n362)
