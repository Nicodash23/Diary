## Guasti riscontrati

18-11
### Error: Database is uninitialized and superuser password is not specified.
Fix: set superuser password correctly into .env

20-11
### Error: Database connection failure: could not translate host name "db" to address: Name or service not known

Fix ipotizzata: il nameserver ora trova il container db sul nome database, ho aggiornato .env e compose, tuttavia sospetto il problema sia odoo.conf che sovrascrive le variabili d'ambiente.
 
 Come ipotizzato trovate le seguenti linee in odoo.conf:
 db_host = db

db_port = 5432

db_user = odoo
Eliminate, preferisco passare queste varibili dal file .env del docker compose.


### browser non riceve risposta da odoo
2026-11-20 10:54:34,971 
 INFO ? odoo.service.server: HTTP service running on 127.0.0.1:8069

Sospetto erronea configurazione della rete.

La rete era configurata correttamente.
 
Fix: siccome ho passato odoo.conf come bindmount il container ha riscritto l'intero .cfg di default, nella versione da me passata avevo omesso la riga http_interface = 0.0.0.0, ed aveva defaultato a 127.0.0.1, quindi il container ascoltava solo sul container stesso al posto di ascoltare da tutte le interfacce. 


## Procedura Backup e ripristino

### Backup
Per effettuare un backup basta salvare la cartella Data ed il file .env presente nella cartella Docker su una macchina diversa dall'host e possibilmente collocata in un altro edificio.
Essa contiene tutti i dati persistenti relativi al database ed ad odoo. 
### Ripristino 

Nella cartella Docker eseguire:

docker compose down 

Ciò fermerà i container. 

Passiamo a riconfigurare il .env, selzionare la versione rispettiva al backup e sostituirla nella cartella Docker.

Impostare la variabile:
BASEPATH= 
Alla directory contenente il backup da ripristinare.

Eseguendo nella cartella Docker: docker compose up -d il servizio tornerà online allo stato ripristinato 

## Checklist scoperte 
### Tag e immagini
20 e 20.0 indicano la stessa versione, i tag con il timestamp sono una build creata in quella data della versione specificata. Latest seleziona sempre l'ultima release disponibile tuttavia una nuova versione potrebbe rompere compatibilità. 
https://hub.docker.com/_/odoo 

### Postgress min version 
Python 3.12, PostgreSQL 16.
Usando la docker image ufficiale non dobbiamo manualmente installarlo noi siccome gli sviluppatori l'hanno già fatto al posto nostro. Vedi gli apt-get install in (https://github.com/odoo/docker/blob/master/20.0/Dockerfile)

https://www.odoo.com/documentation/20.0/administration/on_premise/source.html

### Variabili d'ambiente
Queste sono le variabili d'ambiente necessarie alla connessione con il db. 
PGHOST
PGPORT
PGUSER
PGPASSWORD
https://github.com/odoo/docker/blob/master/20.0/entrypoint.sh
Nella documentazione ufficiale di postgress è esplicitato che l'unica variabile d'ambiente richiesta è la:`POSTGRES_PASSWORD`, le restanti sono opzionali,
https://hub.docker.com/_/postgres


### Porte odoo
Dal dockerfile ufficiale vediamo che il container di odoo espone le seguenti porte: 
8069 8071 8072
Tuttavia 8072 è legata all'odoo chat e 8071 apparentemente deprecata ma ancora esposta.
La porta principale che serve l'https è 8069

### File persistenti odoo
Dalla versione base di odoo.conf ho visto ci sono due path configurati.

addons_path = /mnt/extra-addons
data_dir = /var/lib/odoo

La prima è relativa alla cartella degli addons
La seconda è il filestore
Mentre la configurazione stessa vive in /etc/odoo


### Odoo master password
https://www.odoo.com/documentation/20.0/administration/on_premise/deploy.html#change-old-password

Da qui possiamo vedere che la master password è contenuta nell'odoo.conf
Tuttavia preferire cambiarla da interfaccia siccome così è salvato solo un hash di essa

La master password di odoo ci consente di accedere agli strumenti di gestione del database internamente ad odoo. 


### Versione postgres
Ho scelto la versione 19 siccome supporta il maggior numero di feature e sarà mantenuta per più tempo.
A partire dalla versione 18 è stato cambiato dove l'immagine docker monta il volume e la cartella dove risiedono i dati ora è legata alla versione.

[Cambaimenti al volume](https://hub.docker.com/_/postgres#pgdata)

### Perchè due container? 
Per il principio di separazione ed isolazione. Cio rende più facile controllare chi ha accesso al database, inoltre il database può essere online anche senza odoo. Possiamo salvare le configurazioni separatamente in modo da semplificarci la manutenzione dei servizi.  

### Cosa succede se cambi la password? 
Nel primo tentativo nulla, siccome ho collegato le varibili in un singolo .env 
Riavviando non è successo nulla anche cambiando variabili intenzionalemente tra odoo e db, sospetto facendo ripartire il progetto da docker desktop ha ripreso la vecchia versione del compose.
Verifico listando le varibili d'ambiente sul container 

Come ipotizzato runnando printenv sui container le variabili non sono state aggiornate. Elimino i container e riprovo.

Answer- Odoo non si avvia, dai log compare auth faild for user(dbusername) odoo prova a riconnettersi al db 65 volte prima di andare in errore.

### cosa è il filestore? 
Il filestore è la directory dove odoo salva i file. nel databse sono presenti solo i dati strutturati.
è la tipica implementazione siccome è spesso inutile caricare file direttamente nel database, appesantirebbero solo le query ed il db.  

https://www.odoo.com/documentation/20.0/administration/odoo_sh/advanced.html#directory-structure
 
