## (slide 1)

Nelle lezioni precedenti abbiamo usato per tutte le operazioni di Amazon Forecast dei processi manuali tramite la management console in quanto è il modo giusto di iniziare ad utilizzare un servizio su AWS e anche il migliore dal punto di vista didattico per iniziare ad imparare.

Però quando dobbiamo automatizzare il processo per un ambiente di produzione che esegua il deploy dei modelli e la generazione di nuove previsioni in modo continuo e del tutto automatico bisogna utilizzare sicuramente altri strumenti e oggi ne vediamo uno che uso molto spesso.

## (slide 2)

Vi elenco qui i due processi più comuni e un terzo che io uso spesso e che vi consiglio perché il risultato è di buonissima qualità. Il primo metodo è tramite il pacchetto AWS CLI che come abbiamo visto nelle lezioni passate possiamo installare su qualsiasi sistema operativo e con cui possiamo scrivere script.

Il secondo metodo è inserire le chiamata API nel vostro programma applicativo utilizzando un SDK specifico per il vostro linguaggio di programmazione. In questo modo potete creare qualsiasi flusso condizionale che necessitate. Lo strumento tra i 3 è sicuramente il più potente ma anche il più complesso.

Il terzo metodo che vi presento è STEP FUNCTION una macchina di stato in cui potete definire un flusso di lavoro e integrare ogni passo con i servizi AWS e chiamate API già preimpostate. Amazon Forecast fa parte di uno dei tanti servizi implementati in STEP Function e per questo uso questo strumento molto spesso.

## (slide 3)

Vediamo in una definizione abbastanza breve cosa è di preciso il servizio di STEP Functions.

Amazon Step Functions è un servizio di coordinamento di flussi di lavoro basato sul cloud offerto da Amazon Web Services il quale consente di orchestrare micro servizi, funzioni serverless e compiti automatizzati in flussi di lavoro visivi.

Con Step Functions, è possibile progettare e eseguire workshop che uniscono servizi come AWS Lambda, Amazon SNS, Amazon DynamoDB e molti altri, garantendo l'esecuzione affidabile di ciascun passo in sequenza o in parallelo e gestendo il passaggio di dati tra tutti i step definiti.

I singoli step sono definiti tramite un linguaggio JSON chiamato Amazon States Language, che permette di definire facilmente le transizioni di stato. Step Functions gestisce automaticamente gli errori, il retry e mantiene lo stato dell'applicazione, facilitando la creazione di applicazioni complesse e distribuite.

Vediamo velocemente, non entrerò nei dettagli perché l'obiettivo non è la spiegazione del servizio STEP Function ma l'indicazione che può essere lo strumento giusto per automatizzare le fasi di Amazon Forecast.

## (slide 4)

Qui vedete alcune funzioni che ho preparato proprio per eseguire le fasi principali di Amazon Forecast:

(1) Funzione TEST_Dataset è la funzione che controlla se esistono i dataset nel dataset group o se bisogna aggiornare lo schema a seguito di modifiche alla sua struttura.

(2) Funzione TEST_Dataset_Import per eseguire l'operazione di importazione o aggiornamento dei dataset nel dominio RETAIL come ad esempio TARGET, RELATED e METADATA.

(3) Funzione TEST_Predictor per la creazione del Predictor in base a delle opzioni presenti in un file di configurazione che viene passato a tutte le funzioni tramite una chiamata LAMBDA.

(4) Funzione TEST_Forecast per la generazione delle previsioni dall'ultimo predictor, che potrebbe essere l'ultimo appena creato o l'ultimo esistente creato già in precedenza.
Andiamo a vedere l'interno della funzione TEST_Dataset_Import per darvi una idea:

## (slide 5)

Come possiamo vedere abbiamo una parte laterale dove cercare i servizi che desideriamo, ad esempio cercando forecast troverete tutte le chiamate API disponibili nel servizio. Al centro potete trascinare queste funzioni e metterle in correlazione tra di loro come sequenze di elaborazione o come sequenze condizionali.

Ad esempio in questa slide potete vedere che inizio creando "Un Parallel State", chiamato Create Import JOB con cui posso definire operazioni di caricamento dataset in parallelo in modo da ottimizzare i tempi di elaborazione. Nella parte centrale abbiamo il flusso di lavoro di uno di questi JOB paralleli che è la operazione di Import che riguarda il dataset RELATED, il quale viene richiamato tramite lo step già disponibile in STEP Functions e una volta completato esce dal blocco.

## (slide 6)

Un altro esempio è la creazione del predictor che come potete vedere già abbiamo come step pronto per eseguire una chiamata API CreateAutoPredictor. Dopo aver lanciato questo comando che necessita diverso tempo, la nostra macchina di stato ogni 15 minuti controllerà se la creazione del predictor è terminata in caso positivo uscirà dal loop per terminare.

## (slide 7)

In questa slide vediamo ad esempio la creazione di un dataset in un dataset group dove possiamo specificare direttamente nel singolo step il codice JSON dello schema che sarà passato alla API interessata senza dover scrivere nessuno codice di programmazione. Un altro aspetto importante che una volta che le funzioni funzionano bene e sono state testate si può creare una funzione per orchestrare le chiamate.

## (slide 8)

Una volta che avete implementato ogni singolo passo con STEP Functions potete lanciare le funzioni manualmente o se volete definire uno schedulatore in base ad un evento o ad un'orario specifico vi consiglio di usare Amazon EventBridge dove potete definire delle azioni da schedulare in automatico.