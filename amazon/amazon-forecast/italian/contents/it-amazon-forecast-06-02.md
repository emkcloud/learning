(slide 1)
In questa lezione facciamo un ripasso sul metodo utilizzato fino a questo momento e analizziamo come ottimizzare il processo di elaborazione dei dati nei vari passi di Amazon Forecast.
(slide 2)
In questo corso su Amazon Forecast abbiamo seguito sempre l’approccio indicato in questa slide il quale è suddiviso in 3 passaggi fondamentali. La creazione del dataset, la creazione del predictor che è il nostro modello e la generazione delle previsioni chiamate forecasts.
E ogni volta che vogliamo ottenere delle nuove previsioni basta ripetere questo ciclo ogni volta. L'importante è aggiornare i dati con le nuove informazioni e generare le previsioni con dei nuovi periodi di tempo.
Però in verità esiste anche un metodo diverso per aggiornare le previsioni di Amazon Forecast senza dover ricreare tutti i dati e rigenerare tutte le risorse presenti. Vediamo il primo aspetto su questo tema:
(slide 3)
Esistono due modi per caricare un dataset in Amazon Forecast.
Il metodo FULL che abbiamo usato fino adesso, anche perché è l'unico metodo permesso tramite la management console e il metodo INCREMENTAL che possiamo utilizzare andando a richiamare direttamente le API con lo strumento di AWS CLI o codice di programmazione tramite SDK.
In questa slide vi riporto l'esempio con AWS CLI dove possiamo usare il comando create-dataset-import-job e dove dobbiamo indicare 3 valori obbligatori:
    • Il nome del job.
    • l'indirizzo ARN del dataset .
    • Il percorso dei dati su Amazon S3.
Come possiamo vedere possiamo usare un parametro nuovo che è --import-mode il quale è impostato come default su FULL, quindi i dati vengono ricaricati da capo, ma possiamo indicare anche incremental, a questo punto il servizio aggiunge le nuove serie temporali ai dataset già esistenti senza ricaricare tutto.
Ottimo e sicuramente da prendere in considerazione su dataset molto grandi, ad esempio 5 anni o 10 anni di dati storici e dove dobbiamo aggiungere solo una o due settimane di dati nuovi.
Adesso passiamo a vedere la fase successiva della creazione del predictor che anche li ci sono delle novità.
(slide 4)
Dopo aver creato i dataset con FULL o INCREMENTAL possiamo pensare che dobbiamo rieseguire la creazione del predictor con i nuovi dataset. In realtà questo non è obbligatorio, vediamo insieme il perché.
Ho cercato di riassumere in questo schema le due possibilità:
in alto abbiamo il flusso FULL e in basso quello INCREMENTAL.
Nella linea del caricamento FULL abbiamo i seguenti punti:
PUNTO 1: dobbiamo prima eseguire un upload dei dataset completi non importa quello che avevamo già i file CSV devo contenere lo storico completo e essere ricaricati. 
PUNTO 2: dobbiamo creare un predictor che chiamiamo per convenzione (A)
PUNTO 3: eseguiamo la creazione di un Forecast per le nuove previsioni che dovrà utilizzare il predictor appena creato. Non ci sono scorciatoie dobbiamo eseguire tutti i passi completi.
Nella linea del caricamento INCREMENTAL abbiamo i seguenti punti:
PUNTO 1: dobbiamo prima eseguire un upload dei dataset parziali che si andranno ad aggiungere a quelli che avevamo già nei file CSV caricati con il precedente upload FULL. 
PUNTO 2: possiamo creare un nuovo predictor (B) ma anche passare direttamente alla generazione delle previsioni utilizzando il predictor (A) creato durante l'operazione FULL.
PUNTO 3: eseguiamo la creazione di un Forecast per le nuove previsioni che potrà utilizzare il predictor (A) o il predictor (B) in base alla scelta fatta nel PUNTO 2.
OK adesso che abbiamo visto lo schema facciamo qualche riflessione:
Non creare un nuovo predictor ogni volta che generiamo delle previsioni significa risparmiare tempo e non avere costi di elaborazione aggiuntivi. Quindi quando usare uno o l'altro? La risposta è che possiamo usare lo stesso predictor fino a quando questo è affidabile.
Ad esempio se ho creato un predictor con uno storico di 5 anni di vendite, con molta probabilità il modello avrà la stessa affidabilità anche se aggiungiamo solo due settimane di vendita e così via. In questo caso possiamo risparmiare il tempo di creazione del nuovo predictor ma generare le previsioni direttamente.
Però ovviamente ad un certo momento sarà necessario rieseguire l'allenamento del modello completo con tutto il dataset a disposizione. Perché una settimana alla volta alla fine il periodo si allarga.
(slide 5)
Adesso la domanda sorge spontanea. Come faccio a sapere quando un modello non è più affidabile? Io nel mio caso a dirvi la verità uso quasi sempre un tempo determinato, tipo da un cliente ogni 6 settimane lo aggiorno, non sarà super preciso però è veloce, affidabile e il cliente non ha costi aggiuntivi di analisi.
Però, levato questo, Amazon Forecast ci mette a disposizione un potente strumento per eseguire questo controllo e la funzionalità si chiama monitoring qui vi faccio vedere la pagina della documentazione solo a scopo informativo in quanto lo vedremo più in dettaglio nelle lezioni successive.
(slide 6)
Ok ritorniamo al filo principale della creazione FULL o INCREMENTAL.
Vi ricordate questa è la slide del resoconto che abbiamo fatto dopo la creazione FULL dei nostro dataset, dove avevamo indicato i dati principali per ogni dataset come le righe totali (chiamate entrate), il numero di prodotti unici (item_id) ed altre informazioni riguardanti il contenuto dei dataset.
Ecco questo schema vi puó tornare utile anche per provare la vostra prima operazione INCREMENTAL, infatti possiamo caricare dei dataset solo con gli ultimi dati aggiornati, eseguire l'UPLOAD tramite linea di comandi e controllare i nuovi i dati dei dataset per incrementare questo schema rispetto ai risultati della nuova importazione e vedere se sono coerenti con quello che ci aspettiamo.
Ad esempio adesso nel dataset TARGET abbiamo circa 9 Milioni di entrate con 10.929 codici prodotto univoci e il timestamp minimo è del 02 Gennaio 2021 mentre quello massimo è del 25 FEB 2024. Questi dati dovrebbero modificarsi anche se di poco dopo un caricamento di tipo INCREMENTAL. 
Andiamo insieme a fare questa operazione per poi controllare il risultato:
(slide 7)
Prima di tutto vediamo quali sono i dati che possiamo aggiungere. Andiamo al calendario cosa che con il servizio di Amazon Forecast imparerete a usare spesso. Oggi siamo al 13 Marzo 2024 e rispetto allo schema di riepilogo l'ultima vendita elaborata era il 25 Febbraio 2024 indicata con il cerchio arancione. 
Quindi la unica cosa che possiamo fare è elaborare un dataset con le due settimane seguenti indicate dai cerchi verdi in quanto il mio cliente elabora le settimane dal Lunedì alla Domenica.
Quindi procedo a creare la stessa struttura del dataset FULL però solo con queste due settimane di dati che si dovranno andare a sommare a quelli che già esistono nel nostro dataset attuale.
(slide 8)
OK ho lanciato la mia elaborazione parziale e come potete vedere ho una parte del dataset risultato (in questo caso quello TARGET) con le vendite giornalieri dei prodotti dal 26 Febbraio 2024 in avanti come avevamo stabilito. Il RELATED avrà le stesse informazioni però aggregate settimanalmente.
Perché se è vero che aggiornando i dati in modo INCREMENTAL dobbiamo solo aggiungere le informazioni nuove, comunque dobbiamo rispettare lo schema del caricamento precedente e anche le aggregazioni devo essere le stesse, nel nostro caso giornaliere su TARGET e settimanali su RELATED. 
Prima di passare alla slide successiva, vorrei fare una precisazione, secondo la documentazione ufficiale se nel dataset INCREMENTAL finisco dei dati vecchi che si sovrappongono non dovrebbe succedere nulla in quanto Amazon va in update su quelli che già esistono. Purtroppo non ho mai provato questa casistica, normalmente io faccio i FULL all'inizio e poi aggiungo i dati nuovi con INCREMENTAL però sono sicuro che se la documentazione riporta questo in modo chiaro sicuramente funzionerà così.
(slide 9)
Come detto in precedenza purtroppo al momento il valore INCREMENTAL è possibile farlo da linea di comando o tramite SDK, quindi eseguiamo questi 3 comandi per aggiornare i nostri dataset. 
Il primo comando create-dataset-import-job esegue l'aggiornamento del dataset TARGET, il secondo eseguo l'aggiornamento del dataset RELATED e l'ultimo il dataset METADATA.
Una volta lanciati gli aggiornamenti aspettate la fine dell'elaborazione che potete controllare sulla management console che vi avviserà mettendo in ACTIVE i dataset quando questi sono pronti.
(slide 10)
Una volta pronti i dataset, create un nuovo forecast utilizzando lo stesso predictor di prima non abbiamo bisogno di crearne un altro. Il passaggio della creazione di un forecast ormai sapete benissimo come farlo è inutile che vi faccio vedere tutte le schermate e i parametri.
Io come potete vedere da questa slide ne ho creato uno che si chiama MyForecastUpdate, adesso andiamo a fare una query su questo forecast e vediamo se troviamo la previsione per le settimane successive alle due settimane nuove cha abbiamo inserito nel dataset.
(slide 11)
Ok ho fatto una query con il codice prodotto che avevamo visto nel dataset di aggiornamento e le date di previsione appartengono alle 3 settimane prossime esattamente come ci aspettavamo, quindi abbiamo aggiornato il dataset con le ultime due settimane senza ricaricare tutto e abbiamo generato le previsioni senza dover creare un nuovo predictor. 
Lasciamo in sospeso per le prossime lezioni quando bisognerà fare un nuovo TRAIN del predictor e quali sono le differenze tra creare un nuovo predictor, usare lo stesso o fare un RETRAIN di uno esistente.
Grazie ancora di aver partecipato a questo corso ci vediamo nella prossima lezione.
