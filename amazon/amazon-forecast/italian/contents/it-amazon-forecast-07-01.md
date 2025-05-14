## (slide 1)

Siamo arrivati alla fine del corso, ringrazio moltissimo le persone che mi hanno seguito e spero in futuro di potervi offrire dei nuovi contenuti sui servizi AWS come Amazon Personalize, Quicksight o altro ancora. 

Ho diverse idee e sto decidendo quali proporre. Se mi volete dare una mano scrivete qualche consiglio nei commenti e i contenuti che vi possono interessare di più in questo momento.
In queste slide conclusive facciamo una sintesi dei punti trattati.

## (slide 2)

La prima cosa che sicuramente avete imparato specialmente se avete già creato il vostro primo progetto è che Amazon Forecast esegue una buona parte del lavoro ma non tutto. Ci sono diverse parti importanti che bisogna preparare al di fuori del servizio di Amazon Forecast. Vediamo qui un schemino:

La prima fase è la preparazione dei dati: 

Questo lavoro è fondamentale per raggiungere l'obiettivo finale, quindi vi consiglio di non farvi prendere dall'ansia di elaborare il modello fino a quando non avete chiarito bene i dati e gli obiettivi che devono raggiungere, e cosa importante scegliere il tipo di aggregazione e solo i valori correlati che sono utili.

La seconda fase è l'utilizzo del servizio di Amazon Forecast vero e proprio.

Quindi upload dei dati, creazione dei predictor, generazione delle previsioni (forecasts), analisi What-IF, monitoring del modello etc etc. Tutte cose che ormai conoscete. 

La terza fase è l'integrazione delle previsioni nei strumenti di analisi o nell'applicativo aziendale.

Questa fase è di vitale importanza in quanto rappresenta il risultato finale al cliente in cui le previsioni danno un vero valore aggiunto. Sia dal punto di vista effettivo che percepito.

Quindi quando affronterete un nuovo progetto di previsione tenete in mente e fate ben presente ad un eventuale vostro cliente che il servizio è solo un parte della soluzione e che bisogna svolgere tantissimo lavoro ai lati del progetto con decisioni che devono essere prese dal cliente stesso.

## (slide 3)

Adesso vi ripresento la slide che vi ho presentato nell'introduzione del corso in una delle prime lezioni. La slide è la stessa identica però voi a questo punto dovreste esseri diversi. Ed essendo voi che avete cambiato adesso questo schema vi deve essere chiarissimo rispetto alla prima volta che lo avete visto:

Quindi abbiamo sulla sinistra i nostri dataset, che sono divisi in dati storici (chiamati target) e dai relazionati che sono il dataset RELATED e il dataset METADATA. Questi dataset vengono inviati ad Amazon Forecast tramite la funzione di upload che come abbiamo visto può essere di due tipi FULL o INCREMENTAL.

Una volta che i dati sono stati caricati nel servizio possiamo creare il predictor che è il modello. Una volta creato possiamo usare questo modello per creare le previsioni le quali una volta messe a disposizione le possiamo leggere in diversi modi, con la management console, con le funzioni di export CSV e quindi fogli EXCEL e con le API tramite programmazione.

## (slide 4)

Oltre alle fase del servizio standard abbiamo dedicato un po' di tempo anche ad alcuni aspetti aggiuntivi. Ad esempio ricordatevi l'aspetto dell'automazione. In quanto il processo di generazione lo dovrete automatizzare in qualche modo non potete utilizzarlo sempre con passaggi manuali sulla console.

Io vi ho presentato STEP Function ma potete usare anche tantissimi altri strumenti di sviluppo sia con script di sistema che con linguaggi di programmazione.

Abbiamo anche visto alcuni passaggi e una spiegazione di quello che possiamo fare con una analisi What-IF che in previsioni specialmente come quelle di vendita molti clienti hanno proprio la necessità di simulare alcune condizioni prima di prendere delle decisioni di acquisto o di stoccaggio a lungo tempo.

Abbiamo visto un accenno sul fatto che il predictor non sempre deve essere creato, possiamo utilizzare lo stesso per creare previsioni su nuovi dati fino a quando i risultati sono affidabili. Uno strumento per aiutarci a fare questo è la funzionalità di "monitoring" che trovate nella definizione del predictor creato.

## (slide 5)

Con questo possiamo ritenere concluso questo corso anche se troverete una o due lezioni a seguire dove vi indicherò delle risorse aggiuntive che vi possono tornare utili per continuare ad apprendere.

Ovviamente il mio compito è quello di mettervi sulla strada giusta e di darvi una direzione su quello che è il cammino per imparare bene questo servizio. Ma tutto il resto dipende da voi, dai progetti che affronterete e dai studi di approfondimento, le letture dei blog ufficiali di AWS, i video di aggiornamento etc etc.

Solo così arriverete ad essere padroni del servizio.