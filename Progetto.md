
## Modulo 1: Docker

**Obiettivo:** capire cosa sono immagini e container e installare Docker Engine sul tuo Linux seguendo la documentazione ufficiale.

### Concetti da capire

- **Immagine, container e registry** (Docker Hub): cosa è cosa e come si relazionano.
- Differenza tra un **container** e una **macchina virtuale**.
- Dove finiscono i **dati scritti dentro un container** e perché spesso si perdono.
- **Volumi e bind mount**: a cosa servono e in cosa differiscono.
- La **rete tra container**: come un container ne raggiunge un altro.
- **Docker Compose**: perché si descrive un insieme di servizi in un file invece di avviarli uno a uno.
- Il **gruppo `docker`** sul sistema: cosa implica, per la sicurezza, farne parte.

### Cosa fare

1. Leggi le sezioni introduttive della documentazione di Docker e scegli tu il metodo di installazione adatto alla tua distribuzione, motivando la scelta nel diario.
2. Installa Docker Engine e il plugin Compose, poi verifica con l'immagine di prova indicata dalla documentazione.
3. Fai un esperimento con un servizio semplice a tua scelta: avvialo, scrivi qualcosa al suo interno, fermalo, rimuovilo, riavvialo. Osserva cosa si conserva e cosa no, **con e senza volume**.

### Cosa consegnare

Una pagina del diario con i concetti sopra spiegati con parole tue, un esempio tuo per ciascuno e uno **schema**, fatto con uno strumento di diagrammi a tua scelta, che mostri immagine, container, volume e rete.

### Domande di verifica

1. Che differenza c'è tra un'immagine e un container?
2. Cosa succede ai dati se cancello il container?
3. Perché un database in un container ha bisogno di un volume?
4. Come fa un servizio in Compose a raggiungerne un altro? Che ruolo ha il nome del servizio?
5. Che differenza c'è tra fermare i servizi, rimuovere i container e rimuovere anche i volumi? Quale di questi tre passaggi può far perdere dati?
6. Perché aggiungere il tuo utente al gruppo `docker` è una decisione da ponderare?

---

## Modulo 2: Odoo 20 e PostgreSQL in Docker

**Obiettivo:** scrivere da solo un file Compose che avvia Odoo 20 con il suo database, ricavando ogni informazione dalla documentazione ufficiale.

### Cose da scoprire leggendo le fonti

Le risposte vanno nel diario, con il link da cui le hai prese.

- Quale **immagine** e quale **tag** usare per Odoo 20, e cosa differenzia i tag che sembrano indicare la stessa versione.
- Quale **versione minima di PostgreSQL** richiede Odoo 20, e quale **versione di Python** (e perché, usando l'immagine ufficiale, non devi installarla).
- Quali **variabili d'ambiente** legge l'immagine di Odoo per collegarsi al database e quali ci vogliono sul container del database.
- Su quale **porta** risponde Odoo.
- Quali **cartelle** dentro il container contengono dati, configurazione e moduli aggiuntivi, e quali di queste devono stare su un volume.
- Cos'è la **master password** di Odoo e dove si imposta.
- Quale **versione di PostgreSQL** scegliere tra quelle ammesse (16, 17, 18): decidi tu e motiva. Controlla in particolare cosa cambia nell'immagine Docker di PostgreSQL a partire dalla 18 e cosa comporta per i volumi.

### Cosa fare

1. Scrivi il file Compose con due servizi (Odoo e database) usando **solo le fonti ufficiali**.
2. Avvialo, crea un database dall'interfaccia web e prova alcune app.
3. **Rompi apposta qualcosa** e spiega cosa vedi: per esempio una password del database diversa tra i due servizi, oppure un volume rimosso. Per ogni guasto: **sintomo, causa, rimedio**.
4. Individua dove stanno fisicamente i dati e prepara un **backup completo**, con ripristino su un database nuovo.

### Cosa consegnare

Il file Compose con un commento tuo su ogni riga o gruppo di righe (cosa fa e perché), il diario dei guasti provocati e la procedura di backup e ripristino scritta passo passo.

### Domande di verifica

1. Perché Odoo e PostgreSQL stanno in due container separati e non in uno?
2. Cos'è il filestore e perché un backup del solo database non basta?
3. Se cambi la password del database nel file Compose dopo il primo avvio, cosa succede e perché?
4. Perché la master password non va lasciata di default?
5. Perché la tua scelta della versione di PostgreSQL è giusta per questo progetto?

> [!info] Qui c'è il **primo incontro** con il tutor.

---

## Modulo 3: come è fatto Odoo

**Obiettivo:** capire come è organizzato Odoo e preparare l'ambiente per svilupparci sopra, fino a installare un tuo modulo vuoto. I piccoli sviluppi veri arriveranno dopo, assegnati dal tutor.

### Concetti da capire

- L'**architettura di Odoo**: browser, server Python, database PostgreSQL, filestore.
- Cos'è un **modulo (addon)** e da quali file è composto; cosa dice il file **manifest**.
- Cos'è l'**addons path** e come Odoo trova i moduli.
- Cos'è un **modello dell'ORM**, un **campo** e una **vista**, e come sono collegati.
- Cosa fanno la **modalità sviluppatore** e la **modalità debug**.
- Differenza tra **installare** e **aggiornare** un modulo.
- Dove si leggono gli errori.

### Cosa fare

1. Studia la sezione per sviluppatori della documentazione di Odoo 20 (in particolare i tutorial introduttivi) e leggi la struttura di un modulo standard del sorgente della versione 20.0.
2. Fai in modo che una cartella del tuo PC sia vista da Odoo come cartella dei moduli aggiuntivi. Devi capire tu quale parte del Compose cambiare.
3. Crea un modulo minimo, installalo e verifica nei log e nel database cosa è successo.
4. Modifica il modulo e ripetine l'aggiornamento, annotando **quali modifiche richiedono un riavvio** del servizio e quali no.

### Cosa consegnare

Uno schema dell'architettura disegnato da te, l'albero delle cartelle del tuo modulo con una riga di spiegazione per ogni file, e una nota su cosa hai osservato nei log all'installazione.

### Domande di verifica

1. Cosa succede nel database quando installi un modulo?
2. Perché una modifica a un file Python e una a una vista XML non si comportano allo stesso modo?
3. Cos'è l'addons path e cosa succede se il percorso è sbagliato?
4. Perché la cartella dei moduli e la cartella di configurazione sono montate dall'esterno e non copiate nell'immagine?
5. Dove guardi per capire perché un modulo non si installa?

---

## Modulo 4: React e l'API di Odoo

**Obiettivo:** costruire una piccola applicazione React che legge dati da Odoo 20, capendo cosa succede in ogni passaggio, dal clic nel browser fino al database.

### Concetti da capire

- **Richiesta e risposta HTTP**, metodi, codici di stato, formato JSON.
- Cos'è l'**API esterna di Odoo** e perché le vecchie **API RPC sono deprecate** nella versione 20: leggi cosa dice la documentazione e cosa deduci per i tutorial che trovi in rete.
- Come si **autentica** una chiamata con una chiave API e come Odoo applica i **permessi dell'utente** a cui la chiave appartiene.
- La **politica della stessa origine** del browser e il **CORS**: cosa sono, perché esistono e quali soluzioni ci sono.
- Le basi di **React**: componenti, stato, effetti collaterali e come un componente chiede dati a un server.
- Differenza tra il **server di sviluppo** di un progetto React e la **versione compilata** per la produzione.

### Cosa fare

1. Leggi la documentazione ufficiale dell'API esterna di Odoo 20.
2. Prima di scrivere React, prova una chiamata all'API con uno strumento a tua scelta (terminale o client HTTP) e capisci la struttura di richiesta e di risposta. Se non funziona, il problema è qui e non in React.
3. Crea un progetto React con lo strumento che preferisci, scegliendolo dopo aver confrontato almeno due opzioni.
4. Fai mostrare all'applicazione un elenco di dati letto da Odoo, con almeno una ricerca.
5. Incontrerai probabilmente un errore del browser legato al CORS: non cercare solo la scorciatoia, **spiega la causa** e scegli una soluzione, motivando perché va bene in sviluppo e se va bene in produzione.
6. **Gestisci gli errori.** Prova a spegnere Odoo, a usare una chiave non valida, a chiedere dati inesistenti e a simulare una rete lenta. L'applicazione deve mostrare messaggi chiari e non bloccarsi, e ogni caso va documentato con lo schema indicato all'inizio.

### Cosa consegnare

Il progetto in Git, uno schema che mostri il percorso di una richiesta dal browser a PostgreSQL e ritorno, e una nota di una pagina su CORS e sulla gestione della chiave API.

### Domande di verifica

1. Perché mettere una chiave API direttamente in un'applicazione che gira nel browser è un rischio? Quali alternative esistono?
2. Cosa dice la documentazione sulle API RPC rispetto alla nuova API, e come ti comporteresti con un tutorial che usa quelle vecchie?
3. Perché il browser blocca la chiamata e il terminale no?
4. Cosa succede se la chiave appartiene a un utente con pochi permessi?
5. Come si comporterebbe l'applicazione se Odoo fosse spento? Come la renderesti più robusta?

> [!info] Qui c'è il **secondo incontro** con il tutor.

---

## Modulo 5: misurare risorse ed energia

**Obiettivo:** progettare un metodo di misura corretto e ripetibile, prima di eseguire qualunque confronto. Il modulo si conclude con una prova di misura su un caso semplice.

### Concetti da capire

- Cosa misurano **CPU, memoria, disco e rete** di un container, e cosa non misurano.
- **Latenza e throughput**; perché si guardano **mediana e 95° percentile** e non la sola media.
- **Carico di lavoro**: cosa lo rende rappresentativo di un uso reale e cosa lo rende artificiale.
- **Riscaldamento (warm-up)**, cache e perché le prime richieste sono diverse dalle successive.
- **Ripetibilità**: cosa deve restare uguale tra due prove per poterle confrontare.
- **Potenza (W) ed energia (Wh)**: la differenza e come si passa dall'una all'altra.
- **Consumo a riposo** e **consumo attribuibile al carico**.
- **Misura dell'energia a muro** e **stima software**: cosa vede ciascuna e quali limiti ha.

### Cosa fare

1. Cerca nella documentazione e sul web gli strumenti con cui si misurano: risorse dei container, tempi di risposta sotto carico, query lente di PostgreSQL, energia. Per ogni grandezza scegli uno strumento e motiva la scelta confrontandolo con almeno un'alternativa.
2. Scrivi un **piano di misura** di una o due pagine: cosa misuri, con che strumento, per quanto tempo, quante ripetizioni, cosa tieni fisso, come tratti il riscaldamento.
3. Definisci le **formule** per ricavare dall'energia misurata l'energia per un certo numero di richieste, e il consumo attribuibile al solo carico.
4. Esegui una **prova generale** su un caso semplice (per esempio solo Odoo con pochi dati) per controllare che gli strumenti diano numeri sensati. Se due ripetizioni della stessa prova differiscono molto, trova il perché prima di andare avanti.

### Cosa consegnare

Il piano di misura, la prova generale con i numeri raccolti e un commento sull'affidabilità di ciascuno.

### Domande di verifica

1. Perché la media dei tempi di risposta può nascondere un problema?
2. Come ti accorgi che una misura è disturbata da altro (altri programmi, aggiornamenti, temperatura)?
3. Una stima software dell'energia della CPU può sostituire una misura a muro? In quali casi no?
4. Come ricavi l'energia consumata solo dal carico, sapendo che il PC consuma anche a riposo?
5. Cosa dovresti ripetere se cambiassi macchina?

---

## Modulo 6: dati in scala 1:100 e confronto delle architetture

**Obiettivo:** rispondere alla domanda del progetto: quanto costano in risorse ed energia Odoo e React, e conviene tenere Odoo e React nello stesso container applicazione o separarli, con il database sempre in un container a parte? E cosa possiamo dire della produzione, dove i dati saranno milioni, misurando su una frazione?

**Il rapporto 1:100.** Qui si intende un dataset di test pari a 1/100 dei dati attesi in produzione (se ne prevedono 5 milioni, nel test ne carichi 50.000). Il valore di produzione va fissato con il tutor.

### Concetti da capire

- Perché **moltiplicare per 100** un risultato misurato su 1/100 dei dati può dare un numero sbagliato.
- **Indici**, scansione completa di una tabella e **piano di esecuzione** di una query.
- **Cache e memoria**: cosa cambia quando i dati non entrano più in RAM.
- **Crescita lineare e non lineare** dei costi.
- **Dati sintetici e dati reali**: in cosa differiscono e quanto questo falsa le misure.
- Le **tre configurazioni** da confrontare (in tutte e tre il database sta in un container separato):
    - **(A)** due container: database e un container applicazione che contiene insieme Odoo e il web server per React;
    - **(B)** due container: database e Odoo, con React servito da Odoo;
    - **(C)** tre container: database, Odoo e un web server per React.
- **Pro e contro** di ciascuna, anche oltre le risorse (aggiornamenti, limiti per servizio, log, sicurezza).

### Cosa fare

1. Fissa con il tutor i **numeri di produzione**: record attesi, utenti contemporanei, operazioni tipiche.
    
2. Decidi come generare **dati sintetici credibili** e in tre dimensioni diverse (per esempio 1:1000, 1:100, 1:10). Scrivi tu lo script di generazione e motiva le scelte sulla forma dei dati.
    
3. **Prima di misurare**, scrivi un'ipotesi per ciascuna configurazione: cosa ti aspetti di RAM, CPU, tempi ed energia, e perché.
    
4. Realizza le configurazioni, esegui le misure secondo il piano del Modulo 5 e ripetile.
    
5. Confronta ipotesi e risultati. Dove le ipotesi erano sbagliate, spiega perché.
    
6. Distingui cosa si può **proiettare** ai milioni di record con ragionevole sicurezza, cosa no, e quale prova servirebbe per stabilirlo.
    
7. **Dimensiona le macchine.** Documenta quanto spazio e quante risorse di calcolo pensi di allocare a una macchina di test e a una di produzione. Per ogni valore indica da quali misure parte, quale margine di crescita hai previsto e quali ipotesi hai fatto; se un valore non deriva da una misura, dichiaralo. Come ipotesi di partenza per la produzione considera un **VPS OVHcloud** delle fasce VPS-3 o VPS-4, eventualmente con memoria aggiuntiva: leggi caratteristiche e opzioni di potenziamento sul sito ufficiale di OVHcloud, perché cambiano nel tempo e dipendono dal piano. Usa la tabella qui sotto come schema.
    
    |Risorsa|Macchina di test|Macchina di produzione|Su cosa si basa il valore|
    |---|---|---|---|
    |Processore (numero di core)||||
    |Memoria RAM||||
    |Disco per database e indici||||
    |Disco per allegati (filestore)||||
    |Disco per backup||||
    |Prestazioni del disco e della rete||||
    |Margine di crescita (per quanto tempo)||||
    |Energia stimata||||
    
8. **Verifica su un VPS di prova.** Prima di dare un'indicazione definitiva, ripeti le prove principali (stessa configurazione, stessi dati, stesso carico) su un VPS reale della fascia candidata, noleggiato per un periodo breve. Confronta i risultati con quelli del PC e calcola di quanto differiscono tempi, CPU, memoria e disco; poi correggi proiezioni e dimensionamento di conseguenza. Annota anche cosa non puoi controllare su un VPS (per esempio la condivisione delle risorse fisiche con altri clienti) e come ne tieni conto nei margini. Se servisse più memoria, verifica nella documentazione di OVHcloud quali potenziamenti sono possibili per il piano scelto.
    

### Cosa consegnare

- Lo **script di generazione dei dati** con la sua spiegazione.
- La **tabella dei risultati** e almeno un grafico per ciascuna grandezza principale.
- Il **confronto tra ipotesi e risultati**.
- Una sezione sulle **proiezioni** con il grado di fiducia dichiarato.
- La **tabella di dimensionamento** compilata per la macchina di test e per quella di produzione.
- Il **confronto tra le misure sul PC e quelle sul VPS di prova**.

### Domande di verifica

1. Perché misurare su tre dimensioni di dati e non su una sola?
2. Come capisci se una ricerca che va bene a 50.000 record sarà un problema a 5 milioni? Cosa ti dice il piano di esecuzione?
3. Quali grandezze ti aspetti che crescano meno o più dei dati, e perché?
4. Quando separare servizi in container diversi porta un vantaggio e quando è solo un costo?
5. Cosa cambia se la macchina di produzione ha meno memoria o un disco più lento del tuo PC?
6. Con che sicurezza daresti il tuo risultato a chi deve decidere un acquisto?
7. Perché la macchina di produzione ha i valori che hai indicato e non la metà o il doppio? Quali misure o ipotesi li sostengono?
8. Di quanto differiscono le misure sul PC da quelle sul VPS di prova, e cosa cambi nel dimensionamento di conseguenza?

> [!info] Qui c'è il **terzo incontro** con il tutor.

---

## Consegna finale e valutazione

Alla fine del percorso consegni **tre cose**:

1. **Una relazione scritta** di circa cinque pagine: obiettivo, metodo, risultati con tabelle e grafici, conclusione con la configurazione che consigli, il dimensionamento proposto per la macchina di test e per quella di produzione e i limiti delle proiezioni, più il registro di tutti i test eseguiti e delle scelte progettuali fatte, scritti con lo schema indicato all'inizio.
2. **Il repository Git** personale e privato, che condividi con il tutor alla consegna, con tutto il lavoro e un file README che permetta a un'altra persona di rifare le prove da zero.
3. **Una presentazione orale** di circa 15 minuti, seguita da domande.

### Cosa guarderà il tutor

|Aspetto|Cosa si controlla|
|---|---|
|**Comprensione**|Le risposte alle domande di verifica, date con parole tue|
|**Metodo**|Il piano di misura è corretto e le prove sono ripetibili|
|**Riproducibilità**|Un'altra persona riesce a rifare le misure seguendo il README|
|**Chiarezza**|La relazione si capisce senza spiegazioni a voce|
|**Onestà sui limiti**|Dichiari cosa non sai e cosa non si può proiettare; non mascheri risultati incerti|
|**Documentazione**|Ogni test e ogni scelta progettuale sono scritti con lo schema richiesto, compresi i test falliti e le alternative scartate|
|**Gestione di Git**|Repository ordinato, README completo, flusso dichiarato e rispettato, commit leggibili, nessun segreto nel repository, API documentate con uno standard|
|**Gestione degli errori**|Ambiente e applicazione gestiscono guasti e input non validi senza errori silenziosi; i casi gestiti sono provati e documentati|

---

## Fonti ufficiali da studiare

I link sono scritti per esteso perché il documento può essere stampato. Ogni link va controllato prima di usarlo, per questo ha accanto _(da verificare)_. Per Odoo apri sempre le pagine con `20.0` nell'indirizzo: molti articoli in rete riguardano versioni precedenti e le differenze contano.

### Modulo 1: Docker

- Installazione di Docker Engine: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/) _(da verificare)_
- Documentazione di Docker Compose: [https://docs.docker.com/compose/](https://docs.docker.com/compose/) _(da verificare)_
- Volumi in Docker: [https://docs.docker.com/engine/storage/volumes/](https://docs.docker.com/engine/storage/volumes/) _(da verificare)_

### Modulo 2: Odoo 20 e PostgreSQL in Docker

- Immagine ufficiale di Odoo su Docker Hub: [https://hub.docker.com/_/odoo](https://hub.docker.com/_/odoo) _(da verificare)_
- Repository dell'immagine Docker di Odoo: [https://github.com/odoo/docker](https://github.com/odoo/docker) _(da verificare)_
- Immagine ufficiale di PostgreSQL su Docker Hub: [https://hub.docker.com/_/postgres](https://hub.docker.com/_/postgres) _(da verificare)_
- Documentazione di PostgreSQL: [https://www.postgresql.org/docs/](https://www.postgresql.org/docs/) _(da verificare)_
- Documentazione Odoo 20.0, punto di partenza: [https://www.odoo.com/documentation/20.0/](https://www.odoo.com/documentation/20.0/) _(da verificare)_
- Amministrazione e installazione di Odoo 20: [https://www.odoo.com/documentation/20.0/administration.html](https://www.odoo.com/documentation/20.0/administration.html) _(da verificare)_
- Installazione da sorgente (requisiti e struttura): [https://www.odoo.com/documentation/20.0/administration/on_premise/source.html](https://www.odoo.com/documentation/20.0/administration/on_premise/source.html) _(da verificare)_

### Modulo 3: come è fatto Odoo

- Sezione Developer e tutorial: parti dalla documentazione Odoo 20.0, [https://www.odoo.com/documentation/20.0/](https://www.odoo.com/documentation/20.0/) _(da verificare)_
- Codice sorgente di Odoo, ramo 20.0: [https://github.com/odoo/odoo/tree/20.0](https://github.com/odoo/odoo/tree/20.0) _(da verificare)_

### Modulo 4: React e l'API di Odoo

- External JSON-2 API di Odoo 20: [https://www.odoo.com/documentation/20.0/developer/reference/external_json2_api.html](https://www.odoo.com/documentation/20.0/developer/reference/external_json2_api.html) _(da verificare)_
- External RPC API di Odoo 20, deprecata: [https://www.odoo.com/documentation/20.0/developer/reference/external_rpc_api.html](https://www.odoo.com/documentation/20.0/developer/reference/external_rpc_api.html) _(da verificare)_
- Documentazione di React: [https://react.dev/](https://react.dev/) _(da verificare)_
- Guida di Vite: [https://vite.dev/guide/](https://vite.dev/guide/) _(da verificare)_
- CORS, spiegato da MDN: [https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) _(da verificare)_
- Controlli di salute in Compose (`healthcheck`): [https://docs.docker.com/reference/compose-file/services/#healthcheck](https://docs.docker.com/reference/compose-file/services/#healthcheck) _(da verificare)_

### Modulo 5: misurare risorse ed energia

- Comando `docker stats`: [https://docs.docker.com/reference/cli/docker/container/stats/](https://docs.docker.com/reference/cli/docker/container/stats/) _(da verificare)_
- k6, strumento di carico: [https://grafana.com/docs/k6/latest/](https://grafana.com/docs/k6/latest/) _(da verificare)_
- Locust, strumento di carico alternativo: [https://locust.io/](https://locust.io/) _(da verificare)_
- cAdvisor, metriche dei container: [https://github.com/google/cadvisor](https://github.com/google/cadvisor) _(da verificare)_
- Prometheus: [https://prometheus.io/docs/](https://prometheus.io/docs/) _(da verificare)_
- Grafana: [https://grafana.com/docs/grafana/latest/](https://grafana.com/docs/grafana/latest/) _(da verificare)_
- Estensione `pg_stat_statements` di PostgreSQL: [https://www.postgresql.org/docs/current/pgstatstatements.html](https://www.postgresql.org/docs/current/pgstatstatements.html) _(da verificare)_
- Scaphandre, stima del consumo energetico: [https://github.com/hubblo-org/scaphandre](https://github.com/hubblo-org/scaphandre) _(da verificare)_

### Modulo 6: dati, architetture e dimensionamento

- Usare `EXPLAIN` in PostgreSQL: [https://www.postgresql.org/docs/current/using-explain.html](https://www.postgresql.org/docs/current/using-explain.html) _(da verificare)_
- Piani VPS di OVHcloud: [https://www.ovhcloud.com/en/vps/](https://www.ovhcloud.com/en/vps/) _(da verificare)_
- Potenziare le risorse di un VPS OVHcloud: [https://docs.ovhcloud.com/en/guides/bare-metal-cloud/virtual-private-servers/upgrade-resources](https://docs.ovhcloud.com/en/guides/bare-metal-cloud/virtual-private-servers/upgrade-resources) _(da verificare)_

### Git, documentazione e consegna

- Documentazione di Git: [https://git-scm.com/doc](https://git-scm.com/doc) _(da verificare)_
- Libro Pro Git: [https://git-scm.com/book](https://git-scm.com/book) _(da verificare)_
- GitHub flow, un flusso di lavoro con i rami: [https://docs.github.com/en/get-started/using-github/github-flow](https://docs.github.com/en/get-started/using-github/github-flow) _(da verificare)_
- Trunk Based Development, un flusso alternativo: [https://trunkbaseddevelopment.com/](https://trunkbaseddevelopment.com/) _(da verificare)_
- Conventional Commits: [https://www.conventionalcommits.org/](https://www.conventionalcommits.org/) _(da verificare)_
- Guida a Markdown, per il `README.md`: [https://www.markdownguide.org/](https://www.markdownguide.org/) _(da verificare)_
- OpenAPI Initiative: [https://www.openapis.org/](https://www.openapis.org/) _(da verificare)_
- Swagger UI: [https://swagger.io/tools/swagger-ui/](https://swagger.io/tools/swagger-ui/) _(da verificare)_
- Documentazione di Postman: [https://learning.postman.com/docs/](https://learning.postman.com/docs/) _(da verificare)_

---

## Checklist finale

Spunta ogni voce solo quando la consegna esiste davvero e l'hai riletta.

### Modulo 1: Docker

- [ ] Docker Engine e Compose installati e verificati
- [ ] Pagina del diario con i concetti, un esempio tuo per ciascuno e lo schema

### Modulo 2: Odoo 20 e PostgreSQL

- [ ] File Compose scritto da me e commentato
- [ ] Guasti provocati e documentati (sintomo, causa, rimedio)
- [ ] Backup e ripristino provati e descritti passo passo
- [ ] Primo incontro con il tutor fatto

### Modulo 3: come è fatto Odoo

- [ ] Modulo minimo installato e funzionante
- [ ] Schema dell'architettura, albero del modulo spiegato e nota sui log

### Modulo 4: React e API

- [ ] Applicazione React che legge dati da Odoo, con almeno una ricerca
- [ ] Schema del percorso di una richiesta, nota su CORS e gestione della chiave API
- [ ] Gestione degli errori provata e documentata
- [ ] Secondo incontro con il tutor fatto

### Modulo 5: misure

- [ ] Piano di misura scritto
- [ ] Prova generale eseguita, con numeri e commento sull'affidabilità

### Modulo 6: dati e architetture

- [ ] Numeri di produzione fissati con il tutor
- [ ] Script di generazione dei dati scritto e spiegato
- [ ] Ipotesi scritte prima di misurare
- [ ] Misure ripetute su tre dimensioni di dati
- [ ] Confronto tra ipotesi e risultati, con proiezioni e grado di fiducia dichiarato
- [ ] Tabella di dimensionamento per macchina di test e di produzione compilata
- [ ] Verifica su un VPS di prova e confronto con il PC
- [ ] Terzo incontro con il tutor fatto

### Trasversale e consegna

- [ ] Diario di lavoro aggiornato
- [ ] Tutti i test e le scelte progettuali documentati con lo schema richiesto
- [ ] Repository Git privato e ordinato: `README.md`, flusso dichiarato, commit leggibili, `.gitignore`, nessun segreto, tag di consegna
- [ ] API documentate con uno standard (OpenAPI o Postman)
- [ ] Relazione scritta di circa cinque pagine
- [ ] Presentazione orale preparata (circa 15 minuti)