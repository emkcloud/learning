(slide 1)
In questa lezione ci concentreremo su alcuni aspetti che riguardano il funzionamento del servizio.
Nella lezione precedente abbiamo visto alcune operazioni che possiamo fare con Amazon Forecast tramite la console, anche se in realtà ne esistono moltissime di più per diverse operazioni specifiche.
(slide 2)
Anche se questa slide che sto per presentare in realtà è valida per quasi tutti i servizi di Amazon ritengo utile dedicarci qualche minuto specialmente per chi non ha grandissima esperienza con i servizi AWS. 
Nella lezione precedente abbiamo visto proprio l'essenza del servizio, cioè eseguire l'upload del dataset, creare un predictor e generare le previsioni.
In realtà esistono decine di funzioni che possiamo svolgere, ad esempio cancellare un dataset, crearne uno nuovo o semplicemente aggiornare quello esistente, eseguire un nuovo addestramento su un predictor esistente, aggiungere delle previsioni, eseguire esportazioni di controllo, eseguire il monitoring, ed altro ancora. 
La domanda è come si fa ad eseguire tutte queste funzionalità ?
La risposta è che normalmente esistono 3 metodi diversi per accedere alle API di Amazon e quindi eseguire comandi o interrogare dati tramite un servizio di Amazon AWS. 
(1) Console AWS
La prima che viene scoperta immediatamente dopo la creazione di un account è proprio la console AWS, un'interfaccia WEB che ci permette di interagire con la maggior parte dei servizi. Questo strumento è indispensabile per le operazioni giornaliere ma è un po' limitante nelle azioni ricorrenti o nella creazione di risorse che hanno bisogno di molti parametri di configurazione.
Ad esempio nello stesso servizio di Amazon Forecast durante i test e l'avvio delle prime previsioni è sicuramente lo strumento più adatto, ma quando la procedura è stabile e bisogna eseguirlo in modo schedulato e ripetere l'operazione per aggiornare i dati, creare nuovi modelli e previsioni diventa un po' limitante e conviene automatizzare il tutto con degli script.
(2) AWS CLI 
Il secondo strumento è AWS CLI che è possibile usare in quasi tutti i sistemi operativi, ci permette da linea di comando di eseguire qualsiasi azione su Amazon AWS ed elaborare tutte le operazioni che normalmente facciamo da console. Con il vantaggio che possiamo automatizzare i processi e creare un flusso condizionale in base ad alcune informazioni.
Un altro aspetto per cui conviene AWS CLI è che molte volte ci sono funzioni particolari che non troviamo nella console ma che sicuramente le troviamo in AWS CLI. Ad esempio nel servizio di Amazon Forecast al momento della registrazione di questo corso non posso aggiornare un dataset in modo incrementale, (INCREMENTAL UPLOAD) devo ricaricare tutto il dataset ed eseguire un (FULL UPLOAD) mentre ad esempio da linea di comando con AWS CLI posso farlo tranquillamente.
(3) SDK
Il terzo metodo per accedere a tutte le azioni del servizio è quello di usare un SDK legato al proprio linguaggio di programmazione ed ottenere delle librerie che ti permettono di accedere tramite codice di programmazione alle varie API senza dover sviluppare la logica di interscambio con il servizio. 
Quindi a prescindere dal linguaggio utilizzato, la logica è quella di importare la libreria interessata, creare l'oggetto client del servizio che volete gestire e richiamare i metodi per eseguire le azioni previste nella classe. Gli SDK vanno aggiornati quando escono nuove funzionalità o vengono rilasciati nuovi servizi.
Ok adesso andiamo a vedere un po' più in dettaglio questo aspetto:
(slide 3)
Partiamo dalla console dove non perderei molto tempo in quanto è uno strumento intuitivo che non ha bisogno di molte spiegazioni. è lo strumento iniziale con cui conoscere tutti i servizi AWS. È uno strumento che ha molte funzionalità, ricerca rapida, servizi usati di frequente, assistente virtuale per cercare i servizi adatti in base alle soluzioni da sviluppare etc etc..
La unica cosa che vi posso ricordare è controllate sempre la regione selezionata e controllate bene le regioni abilitate al servizio che volete usare.
Ad esempio io ho molti clienti che non usano la regione geografica Italia per la mancanza di servizi molto importanti e si preferisce usare Irlanda che ha quasi sempre tutto. Però come continuerò sempre a dire verificate sempre, le cose cambiano velocemente e in AWS cambiano ancora più velocemente.
Ovviamente sui servizi di Base come Amazon EC2, Amazon S3 etc non dovreste aver nessun problema ma se usate un servizio abbastanza nuovo controllate sempre la regione e se ci sono i requisiti aziendali per poterla utilizzare, tipo regole sulla protezione dei dati, certificazione del servizio ed altro ancora.
Per quanto riguarda Amazon Forecast basta cercare nella barra di ricerca dei servizi il termine "forecast" e selezionare la pagina che vi porterà alla dashboard iniziale del servizio.
Dovreste visualizzare la pagina del servizio simile a questa indicata in questa slide sulla destra.
(slide 4)
Il secondo metodo per utilizzare le funzionalità AWS è da linea di comando tramite il pacchetto chiamato AWS CLI, il quale può essere installato in diversi ambienti. Per installare il pacchetto andate alla documentazione ufficiale e seguite le istruzioni in base al sistema operativo utilizzato. MAC, Linux o Window. Ricordatevi di utilizzare un access key autorizzata.
Dopo aver eseguito correttamente l'installazione provate i seguenti comandi :
aws forecast help
aws forecastquery help
dovreste vedere l'elenco di tutti i comandi disponibili nel servizio di Amazon Forecast da usare sulla linea di comando senza l'utilizzo della console e puoi utilizzarli per la creazione di script personalizzati che si possono schedulare direttamente sul vostro sul server.
Come potete vedere abbiamo a disposizione diversi comandi (ad esempio):
    • create-dataset per creare un dataset
    • list-predictors per listare dei predictor esistenti
    • delete-dataset-group per cancellare un dataset group
    • update-dataset-group per modificare un dataset group
e cosi via per molte altre operazioni. Ok adesso passiamo agli SDK.
(slide 5)
Gli SDK invece vi permettono di integrare le funzioni dei servizi AWS e nel nostro caso le funzioni di Amazon Forecast direttamente nel codice sorgente del vostro programma.
Questo grazie al fatto che esistono tantissimi SDK per moltissimi linguaggi di programmazione. Ad esempio sicuramente ci sono gli SDK per C++, Javascript, PHP, .NET, Python, Ruby, GO e Java.
Cercate nel vostro motore di ricerca preferito AWS SDK e troverete la pagina ufficiale con tutti i linguaggi di programmazione disponibili. Eseguite il download del pacchetto e integratelo nel vostro progetto.
Vedrete che l'implementazione tramite API è semplicissima e nello stesso tempo molto potente, in quanto potete scrivere delle vere e proprie procedure ed eseguire tantissime funzioni presenti su tutti i servizi AWS e non solo su Amazon Forecast.
(slide 6)
Un altro metodo che vorrei aggiungere è quello di visitare il repository di Github dedicato agli esempi di Amazon Forecast, dove potete provare diverse funzionalità da utilizzare in un Notebook Jupyter. Qui vi lascio il link per accedere alla pagina. Sono delle risorse molto interessanti in quanto è possibile analizzare un processo completo di Amazon Forecast passo passo con esempi e documentazione.
(slide 7)
Qui vi faccio vedere un esempio di un'analisi What-IF la quale inizia con un upload su Amazon S3 di un dataset di dati. Tutte cose che vedremo in seguito, pero useremo la console no un notebook. In quanto è più semplice dal punto di vista didattico e non creiamo dei limiti di accesso alle persone iscritte.
Con questa slide concludiamo l'introduzione generale e dalla prossima lezione entriamo nel vivo del servizio con la creazione del nostro primo dataset. 
L'avventura vera inizia adesso, un grande saluto e grazie per esserti iscritto al mio corso...
