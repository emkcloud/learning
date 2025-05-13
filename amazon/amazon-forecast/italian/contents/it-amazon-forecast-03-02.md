(slide 1)
Nella lezione precedente abbiamo visto come creare un "dataset group" in questa lezione invece andremo ad analizzare proprio i "dataset di dati" che sono necessari al servizio di previsione di Amazon Forecast.
(slide 2)
Prima di importare i dati in un domino RETAIL dobbiamo essere a conoscenza che esistono 3 file CSV che dobbiamo preparare e importare separatamente. Il primo file è obbligatorio e gli altri sono facoltativi, nel senso che sono informazioni per migliorare la previsione ma non sono un requisito necessario.
Anche se vi devo dire che quando si lavora su grandi dataset io per mia esperienza personale ho sempre avuto bisogno dei dati integrativi sono troppo importanti per tantissime casistiche. 
Il primo dataset è chiamato TARGET_TIME_SERIES e contiene nel nostro caso le vendite storiche dei prodotti, quindi ogni riga indica il periodo, il codice del prodotto e le vendite eseguite.
Il secondo dataset si chiamata RELATED_TIME_SERIES e contiene per ogni periodo e prodotto informazioni che siamo sicuri che influenzano la vendita, come il prezzo, le promozioni e lo stock. Come dicevo prima, questi dati non sono obbligatori per la previsione, ma indicare che ho venduto il doppio di unità in un periodo dove avevo uno sconto del 50% secondo me è essenziale al fine di generare previsioni corrette.
Il terzo dataset si chiama ITEM_METADATAe qui ci sono indicati a livello di prodotto i campi che possono essere collegati agli articoli similari, ad esempio la categoria, il brand, il colore, la taglia etc, questo può aiutare moltissimo nei prodotti che hanno pochissimo storico di vendita o anche nessuno in quanto le sue previsioni potranno essere generate dai prodotti similari in base alle caratteristiche in comune.
OK adesso passiamo a vedere gli stessi dataset però con lo schema dei campi necessari.
(slide 3)
Nel dataset TARGET_TIME_SERIES dobbiamo prevedere questi campi:
    • (1) item_id (di tipo string) nel nostro caso sarà il codice univoco del prodotto
    • (2) timestamp (di tipo timestamp) nel nostro caso indicherà il giorno delle vendite
    • (3) demand (di tipo float) nel nostro caso indicheremo qui le vendite in unità
Possiamo specificate le vendite giorno per giorno per ogni prodotto ma possiamo usare anche tecniche di aggregazione come ad esempio le vendite di un'intera settimana o mese. In questo caso la data deve rappresentare il primo giorno della settimana o il primo giorno del mese.
Il secondo dataset è chiamato RELATED_TIME_SERIES qui nel nostro esempio abbiamo specificato
    • (1) item_id (di tipo string) sempre codice del prodotto
    • (2) timestamp (stesso identico discorso fatto su TARGET_TIME_SERIES)
    • (3) price (di tipo float) rappresenta il prezzo normale del prodotto
    • (4) discount (di tipo float) rappresenta la % di sconto 10%,20%,50% etc
Nell'ultimo dataset chiamato ITEM_METADATA non abbiamo bisogno di specificare nessun periodo in quanto i dati sono collegati ad un codice prodotto. Nel nostro caso abbiamo indicato:
    • (1) item_id (di tipo string) codice de prodotto
    • (2) brand (di tipo string) la marca del prodotto (potete usare una stringa ma anche un codice marca)
    • (3) line (di tipo string) la linea merceologica del prodotto (ad esempio bevande)
    • (4) category (di tipo string) la categoria merceologica del prodotto (ad esempio acque minerali)
    • (5) family (di tipo string) la famiglia merceologica del prodotto (ad esempio acque minerali gassate)
Ovviamente la struttura dei dataset può cambiare in base alle vostre esigenze e dall'analisi dei dati eseguita. Molti dati sono anche dipendenti dal settore merceologico dell'attività. Se ad esempio vendiamo prodotti di abbigliamento le informazioni come colore, taglia, età sono molto importanti mentre quasi insignificanti ad esempio per la vendita di prodotti alimentari.
Spero di avervi chiarito la struttura dei dataset che dobbiamo creare e adesso passiamo alle prossime slide dove vi presenterò il contenuto di ogni dataset per capire bene quale deve essere il contenuto di ognuno.
(slide 4)
Questo è il contenuto parziale di un dataset TARGET_TIME_SERIES, abbiamo il codice prodotto 8 con tre giorni di vendita e a seguire il codice prodotto 64. Ad esempio la ultima riga rappresenta l'informazione che il prodotto numero 64 il giorno 1 Gennaio 2011 è stato venduto per 101 unità.
Questo file ovviamente dovrà contenere migliaia di righe con lo storico di vendita di ogni prodotto e deve essere formattato in formato CSV, quindi le informazioni separate da virgola.
Un altra informazione che vi ho messo su questa slide è la colonna location, che nel nostro esempio non usiamo che però vi voglio spiegare a cosa serve nel caso ne abbiate bisogno.
Con questa colonna è possibile dividere le vendite per punto di vendita o regione geografica, ad esempio le vendite dei negozi del LAZIO e a seguire le vendite dei negozi presenti in LOMBARDIA e cosi via. 
Io ad esempio ho assistito un cliente che ha diviso le vendite per WEB (vendita online ecommerce) dalle vendite PVE che indicavano le vendite dei negozi fisici. Quindi se vi doveste trovare in questo scenario potete sempre aggiungere questa colonna al dataset e generare previsioni suddivise per location.
(slide 5)
Questo è il contenuto parziale di un dataset RELATED_TIME_SERIES, nella prima colonna abbiamo il codice del prodotto, ad esempio qui riportiamo il prodotto 6, 64, 82 e 88. Mi raccomando se li giocate al lotto per favore ricordatevi anche di me...
Nella seconda colonna abbiamo il timestamp che indica il primo giorno della settimana, in quanto i valori sono ponderati in un range di 7 giorni. Poi vi spiego nelle slide successive il perché di questa scelta.
A seguire abbiamo le colonne del prezzo e la % di sconto. 
Ovviamente voi non dovete per forza seguire questo schema l'importante è che indicate i valori che secondo voi influiscono sulle vendite, cioè al cambio di questi valori sicuramente avrete un cambio sulle vendite.
Ad esempio il prezzo con un offerta al 50% ovviamente dovrebbe aumentare il volume delle vendite e quando il prezzo ritorna al suo valore iniziale le vendite dovrebbe abbassarsi. Quindi dare questa informazione al modello significa sicuramente migliorare le previsioni.
Ad esempio ho visto alcune persone che indicano anche il numero di giorni di apertura nella settimana, potrebbe essere una buona idea, io in questo caso non ne ho bisogno dato che parliamo di una realtà aperta 7 su 7. Però se le aperture settimanali sono molte volte diverse sarebbe buono indicare questo valore.
Un altro consiglio che vi posso dare è che quando vi trovate di fronte a valori molto granulari cercati di aggregarli creando dei range in modo che il modello possa valutare i dati con più semplicità.
Ad esempio nel mio caso il valore sconto è intero in quanto tutti gli sconto 20.50% o 11,25% diventeranno dei numeri interi e molte volte aggregati, tipo 4.66% 4.75% 5.25% etc magari li possiamo tutti mettere nel range degli sconti del 5%.. 
Ovviamente tutto questo lavoro va fatto prima di preparare i dataset per amazon forecast, in quanto il servizio al momento non ha tantissime funzioni per la trasformazione dei dati, questi devono già essere divisi e aggregati a monte in base alle previsioni che si vogliono ottenere.
(slide 6)
Questo invece è il contenuto parziale di un dataset ITEM_METADATA e serve per associare al prodotto delle caratteristiche che lo possono mettere in associazione con altri prodotti similari.
Questo in modo che si possano analizzare oltre allo storico e al trend del singolo prodotto anche i prodotti che fanno parte ad esempio dello stesso brand o appartengono alle stesse categorie.
Nel nostro caso abbiamo scelto di indicare la colonna del brand, il livello merceologico su tre livelli, linea, categorie e famiglia, in quanto il dataset sviluppato voleva dare molto importanza a questo aspetto, però magari nella maggior parte dei casi potrebbe essere sufficiente una sola colonna categoria. 
Tutto dipende dal vostro cliente o dalla vostra necessità legata alla particolarità dei prodotti che si vogliono elaborare.
Questo dataset aiuta la previsione sia perché si può analizzare il trend del prodotto ma anche quello di tutti i prodotti similari. E cosa ancora più importante per quei prodotti che non hanno uno storico di vendita perché sono dei nuovi inserimenti e sui cui la previsione sarebbe difficile se non in presenza di articoli simili.
Ho cercato di riportare questo esempio proprio per chiarire il concetto che in realtà le informazioni non sono fisse e prestabilite ma si posso indicare in base alla propria analisi e agli obiettivi di previsione. Oltretutto anche il settore merceologico di vendita può cambiare molte cose nei file related e metadata.
(slide 7)
Nella slide precedente ho indicato nelle colonne proprio i nomi delle risorse associate per essere più chiaro però in verità molte volte vengono utilizzati solo i codici del brand e dei livelli merceologici come in questo esempio. Brand 001,051,072 categoria A101,A0502, CA01 etc etc
Specialmente se le previsioni verranno lette con delle API lavorare con i codice ha diversi vantaggi e si può sempre decodificare la descrizione con la lettura di un database relazionale.
(slide 8)
Abbiamo visto la struttura e degli esempi di dataset che dobbiamo caricare nel servizio di Amazon Forecast, però c'è un aspetto molto importante sui cui conviene investire un po' di tempo. Il valore del timestamp e la sua eventuale aggregazione di dati. Partiamo da questo schemino:
Prima di analizzare le limitazioni partiamo da un concetto assoluto, la data indicata nel timestamp non deve per forza rappresentare le vendite di un giorno, puó rappresentare le vendite orarie, giornaliere, settimanali, mensili e annuali, e la data rappresenterà il primo elemento dell'aggregazione, quindi può indicare il giorno stesso, come il primo giorno della settimana o il primo giorno del mese e cosi via.
Poi quando eseguiremo la funzione di upload indicheremo questa informazione ad Amazon Forecast in modo che sappia interpretare correttamente l'aggregazione e il significato del timestamp indicato.
Perché possiamo usare aggregazione diverse? La risposta puó essere molto estensiva cerchiamo di fare un esempio per semplificare. Se ad esempio ho un dataset TARGET di 20 milioni di record giornalieri e quello che ho bisogno sono previsioni settimanali o mensili forse aggregare i dati prima di passarli al servizio di Amazon Forecast potrebbe abbassare i costi e i tempi di elaborazione del servizio stesso.
È anche vero però che aggregare troppo potrebbe far perdere informazioni importanti di comportamento che possono incidere sulla qualità del modello e la successiva generazione delle previsioni.
In ogni caso a prescindere dall'aggregazione che scegliamo ci sono dei limiti che sono riportati in questa tabella. Se ad esempio Indico come nella quarta riga di questa tabella un valore Giornaliero sia sul dataset TARGET che RELATED non posso generare previsioni settimanali e potrebbe in diversi casi essere un problema, mentre negli altri casi posso avere situazioni diverse ma compatibili con la previsione.
Nel nostro caso andremo ad utilizzare la riga indicata dalla freccia verde, un DATASET TARGET a livello giornaliero perché voglio mantenere un'alto dettaglio del dato e un DATASET RELATED settimanale perché mi interessa ottenere solo previsioni a livello settimanale e più precisamente le prossime 3 settimane.
(slide 9)
Un altro aspetto importante da apprendere sono le caratteristiche del dataset RELATED che possono essere gestite in modi differenti tramite indicazione di valori futuri direttamente nel timestamp del dataset.
Guardiamo questa slide: 
Nel primo grafico quello giallo abbiamo i dati storici TARGET e quindi nel nostro caso le vendite passate, magari posso avere anche 10 anni di storico ma posso arrivare al massimo fino a ieri. Da li in avanti saranno le previsioni che riempiranno la "Forecast window" con i dati futuri tipo domani e dopo domani.
IL RELATED puó seguire la stessa logica e associare ai periodi presenti in TARGET delle informazioni aggiuntive, tipo il prezzo di vendita in quel periodo, lo stock disponibile, lo sconto di un'offerta ed altro ancora. Queste informazioni preziose possono aiutare il modello a capire meglio il comportamento dei volumi di vendita presenti nei dati storici del dataset TARGET.
Questa serie che vi ho appena indicato è quella rappresentata in bianco sulla ultima riga, come potete vedere i dati indicati iniziano e finiscono come la riga gialla e non entrano nella "Forecast Window". 
Mentre esiste un'altro modo di preparare il dataset RELATED chiamato Forward-looking dove possiamo inserire dei timestamp futuri che corrispondano alle previsioni che saranno richieste. Infatti non posso sapere le vendite del futuro però posso sapere il prezzo della prossima settimana, gli stock in arrivo e le offerte speciali che saranno pubblicate, quindi posso anche indicarle in anticipo sul dataset.
In questo caso il timestamp del dataset come potete vedere nella seconda riga del grafico, quello centrale, va oltre la linea di fine del dataset TARGET ed entra nella "Forecast Windows", questo è uno dei problemi principale per cui il predictor quando viene creato per previsioni settimanali vuole che anche i dati del dataset RELATED siano settimanali mentre quelli del dataset TARGET possono rimane giornalieri. 
Ok, sperando di aver chiarito questo aspetto passiamo alla slide succesiva.
(slide 10)
Abbiamo visto i dataset richiesti e lo schema che bisogna rispettare per i dati da preparare. Ma quali sono i modi di preparazione più efficienti per svolgere il lavoro della creazione dei nuovi dataset.
Analizziamo insieme i più popolari.
Il primo metodo, che è quello che incontro più spesso nelle aziende, è quello di dare lo schema dei 3 dataset al programmatore incaricato il quale procede all'elaborazione dei dati e li aggrega in base alle specifiche indicate e ci consegna i file CSV già pronti all'uso per essere memorizzati su un bucket di Amazon S3.
Un altro metodo molto più semplice ma bisogna avere a disposizione un database ben organizzato e normalizzato è quello di creare un file CSV con una query. Quindi generare una query specifica per ogni dataset da creare e dirigere l'output della query direttamente in un file CSV su Amazon S3.
L'ultimo metodo che vorrei elencare è quello di usare servizi AWS come Glue per la trasformazione dei dati e Athena per la lettura di file di testo tramite sintassi SQL. Ho visto anche situazioni dove è stato usato il servizio di Database Migration Service per portare in ambiente di cloud dati presenti su sistemi IBM Mainframe e dopo una volta migrati sviluppate le QUERY di estrazione per i dataset richiesti.
Ovviamente esistono anche altri metodi, possono esistere situazioni con dati separati in motori di database diversi su sistemi operativi multipli etc. Quindi ogni azienda può essere un mondo a sé.
L'importante che prima di iniziare ad eseguire upload su Amazon Forecast si analizzino bene i dataset, quali sono i livelli di aggregazione e i metodi di preparazione.
Una volta che si ottiene questo risultato, l'uso del servizio è in discesa, ma è importante eseguire questa fase con disciplina, attenzione e con il giusto tempo di analisi che è necessario..
(slide 11)
OK finalmente siamo arrivati all'ultima lezione per la preparazione dei dataset, purtroppo ancora abbiamo visto poco il servizio di Forecast, questo è dovuto al fatto che la preparazione dei dati avviene sempre prima dell'utilizzo del servizio il quale inizia nel momento stesso che abbiamo i dati.
Penso che sia importante analizzare questo aspetto prima di iniziare ad utilizzare il servizio, in quanto essere consapevoli di tutto il lavoro di preparazione dei dati, la definizione degli schemi è essenziale per poi sfruttare a pieno le capacità di Amazon Forecast e cosa importante minimizzare i costi.
Quindi se volete un consiglio spassionato, prima di passare alla prossima sezione del corso in cui eseguirò il primo upload dei dati, fate voi stessi un'analisi dei dataset, create 3 schemi di dataset in base alle vostre esigenze e che potete tenere pronti da usare per il prossimo upload nella sezione successiva.
Quindi prendete un foglio EXCEL ad esempio o altro strumento e elencate lo schema di:
(1) TARGET_TIME_SERIES: 
dove avrete item_id, timestamp e demand. decidete qui se il timestamp rappresenta un giorno, una settimana, un mese ed etc. In più decidete se aggiungere una locazione con cui suddividere le vendite.
(2) RELATED_TIME_SERIES: 
dove avrete item_id e timestamp stesso concetto del dataset TARGET, però qui potete aggiungere tutte le informazioni che secondo voi influenzano il volume delle vostre vendite.
(3) ITEM_METADATA: 
dove specificare tutti i valori di aggregazione per i dati comuni, come categoria, colore etc
Una volta che avete lo schema dei tre dataset e siete sicuri di quello che avete fatto, allora questa è una buona notizia perché significa che avete capito lo scopo preciso degli schemi dei dataset.
A questo punto dovete generare dei dati che rispecchiano il formato del vostro schema.
Vi potete basare sui vostri dati reali se ne avete accesso o potete auto generali, l'importante è ottenere tre file CSV che hanno una logica e che possiamo caricare nel nostro servizio di Amazon Forecast.
(slide 12)
Purtroppo il dataset che userò in questo corso essendo visibile solo parzialmente nelle slide non ho nessun problema a farlo, ma non posso darvi il dataset completo per le prove in quanto devo ovviamente tutelare il diritto di privacy del mio cliente, anche se i dato sono completamente anonimizzati.
Io vi consiglio vivamente di preparare voi stessi il vostro dataset perché è il modo più sicuro per imparare tutti gli aspetti che si incontrano durante la preparazione dei dati.
In ogni caso in alternativa vi ho appena preparato un repository su github dove metterò alcuni dati anonimi con cui potete fare alcune prove di upload e generare delle previsioni.
Quindi anche se non so se farete parte della categoria dei studenti che seguiranno il mio consiglio o semplicemente di quello che lo ignorerà per passare avanti, ci vediamo in ogni caso alla prossima lezione e grazie ancora per la vostra partecipazione a questo corso.
