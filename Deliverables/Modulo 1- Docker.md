### Immagine 
Una docker image rappresenta lo stato di un container. Un'immagine è composta a layer, ognuno identificato da un hash. Generalmente ogni comando produce un layer. I comandi necessari a produrre l'immagine che cambiano spesso vanno inseriti più in basso nell'immagine per evitare di rigenerare layer inutilmente.  
### Container
Un container è l'esecuzione di un'immagine. Concettualmente è molto simili ad una vm poichè possiede un filesystem totalmente separato, tuttavia differisce da una vm poichè non possiede un kernel proprietario ed effettivamente i proccessi vengono eseguiti e gestiti dal kernel della macchina host. 

### Registry
Registry, DockerHub serve per gestire la condivisione di immagini, si ci interagisce selezionando il nome dell'immagine e la versione, docker recupererà l'immagine per l'esecuzione. 

### Persistance
Spesso si perdono i dati siccome i dati scritti finiscono in un layer dell'immagine, smontando un container e rigenerdandolo da un immagine verrebbero cancellati.

Per implementare la persistenza si utilizzano volumes o bindmount. La differenza fra i due è che solitamente con i volumes si interagisce solo tramite container, mentre con le bindmounts è possibile l'interazione dalla host machine. 

### Compose
 Docker compose si usa perchè i  container potrebbero necessitare di altri container per funzionare, ad esempio odoo e postgress, il compose permette di definire che ad esempio odoo necessita che postgress sia avviato e funzionale prima di partire. Inoltre il compose ci permette di definire e creare le reti che collegheranno i vari container in unico file.
 
### Networking

 Networks. Esistono vari driver di rete che si comportano diversamente. 

- Bridge:
 Ogni container vede come collegata la sua porta ethernet su uno switch virtuale(bridge), si collegano all'host passando per un nat vituale. 
 
E' possibile raggiungere i container usando il loro nome, siccome docker crea un servizio dns interno.

Un container su rete bridge non è raggiungibile dall’esterno finché non pubblichi una porta, tuttavia a volte questo bypassa il firewall in esecuzione sull'host. 

- Host:
I container condividono lo stack di rete del'host, performance massime siccome nessuna isolazione, solitamente si preferisce bridge. 

- None: completamente isolati
  