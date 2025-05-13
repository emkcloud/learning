(slide 1)
Adesso che abbiamo creato il nostro predictor in teoria siamo già pronti per generare le previsioni. Però io dedicherei un po di tempo per analizzare il modello che è stato creato e quello che in linea di massima è successo durante la fase di creazione del nostro primo predictor.
Non entrerei nel dettaglio di tutto perché ci leverebbe troppo tempo per arrivare alle previsioni. Quindi vediamo solo gli aspetti principali e casomai rimandiamo a lezioni successive gli aspetti più avanzati.
(slide 2)
In questa slide cercheremo di analizzare i passi che la creazione del modello ha dovuto eseguire prima di pubblicare il predictor. Infatti in diverse ore di elaborazione computazionale cosa è successo di preciso?
(1) Analisi e processo dei dataset
Il primo processo è stato sicuramente quello di leggere i dati dai nostri dataset ed eventualmente creare nuove aggregazioni. Infatti se vi ricordate noi abbiamo indicato la frequenza di tempo sia sulla creazione del dataset che sulla creazione del predittore. (questo perché possono essere uguali ma anche diverse).
Infatti se la frequenza è la stessa "come ad esempio settimanale" il predictor non deve ricalcolarsi i valori sulla linea del tempo, se invece ho un dataset a livello giornaliero e voglio creare un predictor che generi previsioni a livello settimanale quest'ultimo dovrà prima aggregare i valori e dopo elaborarli.
(2) Scelta algoritmi e tecniche di previsione
In questa fase vengono provati gli algoritmi predefiniti di Amazon Forecast e come detto precedentemente gli AutoPredictor non sono come gli AutoML e non scelgono un solo algoritmo per il modello ma utilizzano un approccio più sofisticato che può combinare più modelli e diverse tecniche. Quindi Amazon stessa elaborerà i dataset con diversi algoritmi a sua disposizione e genererà la soluzione perfetta per i nostri dati.
(3) Esecuzione backtest e calcolo delle metriche
Il secondo passo consiste nel dividere i dati in due parti, una per il training e l'altra per i test. Normalmente Amazon usa la stessa divisione che avremo nelle previsioni reali, se ad esempio abbiamo indicato un orizzonte di 3 settimane, amazon prenderà le ultime 3 settimane dei nostri dati storici e userà i restanti dati per il training del modello che genererà delle previsioni che saranno testate su questo blocco.
AutoPredictor fornisce diverse metriche per ogni modello perché utilizza un approccio olistico alla costruzione del modello di previsione, integrando più algoritmi e tecniche.
Queste metriche aiutano a capire come il modello si comporta sotto diversi aspetti, come l'accuratezza, la precisione, la sensibilità, ecc. Offrendo una visione dettagliata di queste metriche, AutoPredictor permette agli utenti di avere una comprensione più completa delle prestazioni del modello su vari fronti.
(slide 3)
Riprendendo il concetto di aggregazione introdotto precedentemente:
In questa slide vediamo un esempio di aggregazione dati nel caso in cui avessimo un dataset su base oraria e vogliamo creare il predictor per previsioni su base giornaliera. Come potete vedete per default in Amazon Forecast viene utilizzata la somma del valore demand.
Però come vedremo successivamente è un aspetto personalizzabile.
Quindi tutte le serie appartenenti alla data 03 Marzo 2018 sono sommate 120+180=300 e viene creata una sola riga che verrà poi analizzata durante la creazione del modello.
Ovviamente se la frequenza è la stessa come nel nostro caso, la creazione del predictor non dovrà eseguire questo processo che molte volte può ridurre i costi del servizio.
Dato che stiamo discutendo su questo aspetto vi voglio ricordare anche un parametro del predictor che abbiamo lasciato al valore di default ma in ogni caso avevo spiegato il suo funzionamento.
L'opzione allineamento del tempo serve quando vogliano aggregare in modo diverso. Ad esempio qui potrei voler indicare che il giorno inizia alle ore 8:00 quindi se indico questo valore l'entrata del 15 Gennaio 2018 alle ore 7:00 apparterebbe al giorno precedente.
(slide 4)
In questa slide cercherò di chiare il concetto di backtest e più precisamente la suddivisioni dei dati per la fase di training e per la fase di test. Iniziamo con la definizione ufficiale di Amazon:
Forecast utilizza il backtesting per calcolare le metriche. Se esegui più backtest, Forecast calcola la media di ogni metrica su tutte le finestre di backtest. Per impostazione predefinita, Forecast calcola un solo backtest, con la dimensione della finestra di backtest (set di test) uguale alla lunghezza dell'orizzonte (finestra di previsione).
Ok adesso che abbiamo indicato la definizione passiamo allo schema:
Usando una configurazione personalizzata è possibile impostare sia la lunghezza della finestra di backtest che il numero di scenari di backtest durante l'addestramento di un predictor.
Quindi se abbiamo un dataset completo che va da una data iniziale a una data finale, come indicato in questa slide, Amazon per default divide questo FULL DATASET come nello scenario 1, cioè prende l'ultima parte reale dei dati storici e allena il modello con i restanti usando quest'ultimi come test sulle verifiche. 
Se vogliamo aumentare l'affidabilità del modello e la precisione delle previsioni potremmo aumentare il numero dei backtest, questo però significa anche più tempo di elaborazione e costi aggiunti del servizio.
In questo caso possiamo aggiungere finestre di backtest come scenario 2, scenario 3 e cosi via.. mi sembra se non sbaglio che ci sia un limite di 5 backtest.
(slide 5)
Se siete dei curiosi incurabili potete approfondire la fase dei backtest eseguendo il lavoro di esportazione backtest results utilizzando il pulsante apposito che trovate nella scheda del predictor. Il pulsante è indicato in questa slide sotto la freccia verde. Una volta che cliccate su questo pulsante la richiesta di esportazione è estremamente facile non serve che vi faccio un tutorial, dovete indicare un nome del lavoro e il percorso su Amazon S3 per la memorizzazione del risultato tramite multipli file CSV.
Quando il lavoro di esportazione completa la sua elaborazione troveremo due tipi di risultati che riguardano il debug dei backtest, il primo risultato riguarda per ogni serie temporale e item il valore della previsione e il valore reale, mentre il secondo risultato ci da per ogni serie il valore delle metriche calcolate.
Andiamo a vedere velocemente i due esempi di risultato che ho già in un foglio EXCEL.
(slide 6)
In questo caso abbiamo per ogni item_id il periodo di previsione che sono 3 settimane, il valore reale delle vendite e in fondo i percentili calcolati. Facciamo un esempio: le prime 3 righe appartengono al codice prodotto 204538 e ogni riga rappresenta la previsione di una settimana quindi 5 unità la prima settimana e 3 unità e 5 unità per quelle successive. Se prendiamo ad esempio la colonna della previsione mean possiamo notare che siamo andati molto vicini al valore reale.
Ovviamente le righe di questo file sono migliaia e nella maggior parte dei casi non arriverete a fare questa analisi specialmente su modelli già in produzione, però devo dire che ci sono state due volte minimo che sono riuscito a scovare alcuni problemi che avevo su analisi problematiche andando ad analizzare alcuni articoli che sapevo come funzionavano e quello che mi dovevo aspettare.
(slide 7)
L'altra informazione che viene generate dalla funzione di export backtest è una lista di serie temporali e items sui cui possiamo controllare il valore calcolato delle metriche. Anche qui non vi consiglio all'inizio di perdere molto tempo su questo tipo di analisi, però se dovesse servire sapete che sta qui...
Direi di chiudere il discorso del backtest e passiamo agli algoritmi:
(slide 8)
Qui vi riporto qui gli algoritmi che utilizza Amazon Forecast per creare il suo modello. Non è fondamentale conoscere gli algoritmi, in quanto il valore aggiunto di Amazon Forecast è che noi non dobbiamo avere la questa conoscenza per utilizzarlo proprio perché su questo aspetto ci ha pensato Amazon a creare una selezione automatica in base a complessi backtest e misurazione dei risultati tramite metriche.
Però sapere almeno l'elenco degli algoritmi usati è sempre positivo, quindi vi lascio qui di seguito la lista degli algoritmi presenti al momento di questo corso su Amazon Forecast.
Potete notare che ogni algoritmo ha un indirizzo ARN che serve ad indicare in modo univoco una risorsa presente su Amazon AWS. In questo modo in futuro potreste usare questi algoritmi anche al di fuori del servizio di Forecast ad esempio con Amazon Sagemaker o semplicemente con un Notebook JUPYTER.
Gli algoritmi indicati al momento sono 6 secondo la documentazione ufficiale di Amazon e sono:
CNN-QR DeepAR+ Prophet NPTS ARIMA e ETS, i primi due sono su rete neurale.
(slide 9)
Questa è la pagina ufficiale di Amazon dove potete trovare alcune informazioni se per caso volete approfondire questa conoscenza per desiderio personale, in quanto come detto in precedenza non è indispensabile per utilizzare il servizio di Amazon Forecast.
Qui ad esempio possiamo vedere il primo che il primo indicato è CNN-QR è un algoritmo di apprendimento automatico proprietario di Amazon per la previsione di serie temporali utilizzando reti neurali.
Poi abbiamo DeepAR+ sviluppato da Amazon ed è un metodo avanzato basato su reti neurali ricorrenti per la previsione di serie temporali, impara da molteplici serie per migliorare la precisione delle previsioni.
A seguire abbiamo Prophet che è un algoritmo di previsione per dati di serie temporali sviluppato da Facebook che adesso è Meta. È progettato per essere flessibile, facile da usare e in grado di gestire i dati di serie temporali con tendenze stagionali forti. Prophet funziona bene con dati giornalieri e con serie temporali che presentano effetti stagionali multipli, tendenze non lineari che cambiano nel tempo.
E cosi via potete trovare informazioni anche sugli altri algoritmi, sulla documentazione di amazon trovate una descrizione funzionale dell'algoritmo e gli ambienti dove lavora meglio mentre se cercate informazioni che entrano nell'aspetto matematico bisognerà cercare documentazione più scientifica.
(slide 10)
In questa slide possiamo vedere anche un confronto di funzionalità di ogni algoritmo in base anche ai dati che analizza, ad esempio potete notare che solo i due algoritmi di amazon analizzano i metadati di un item come ad esempio colore, brand etc), gli altri algoritmi faranno calcolo diversi ma non useranno questi dati.
Questa è una tabellina che vi consiglio da tenere da parte in quanto molte volte quando avrete la sensazione che alcuni campi non sono stati presi in considerazione o ad esempio il dataset aggiuntivo come le previsioni del tempo non incidono, molte volte dipende proprio dagli algoritmi utilizzati.
Detto questo passerei alla slide delle metriche.
(slide 11)
Le metriche visualizzate su un AutoPredictor in Amazon Forecast rappresentano diversi indicatori statistici utilizzati per valutare la precisione e l'efficacia delle previsioni generate dal modello.
Queste metriche danno una valutazione su quanto bene il modello predice i dati reali, consentendo agli utenti di comprendere le prestazioni del modello, come la tendenza, la variabilità e la precisione.
Anche per le metriche vale lo stesso discorso degli algoritmi, non abbiamo necessità di sapere il calcolo matematico di ogni metrica, Amazon stessa ha selezionato le metriche più adatte ai modelli creati.
Le metriche disponibili in amazon sono:
    • WQL = Perdita quantile ponderata
    • WAPE = Errore percentuale assoluto ponderato
    • RMSE = Errore quadratico medio principale
    • MAPE = Errore percentuale assoluto medio
    • MASE = Errore scalabile assoluto medio
Anche per le metriche se siete dei curiosi e volete approfondire questo argomento potete iniziare dalla documentazione di amazon dove viene dedicato molto spazio alla spiegazione matematica della metrica e vengono messe in risalto le caratteristiche principali della metrica stessa. 
Per andare oltre dovete passare a documentazione tecnico matematica.
(slide 12)
In questa slide potete vedere i valori delle metriche del nostro predictor. In poche parole questo è il risultato della media generale di tutte le metriche che sono state calcolate nella fase di backtest.
Vi ricordate quel foglio EXCEL con tutti gli item e i periodi con il calcolo delle metriche su ogni riga. Ecco qui vedete il risultato globale di quelle migliaia di operazioni eseguite su ogni riga del dataset principale.
Normalmente più i valori delle metriche sono bassi e più la qualità del modello è migliore per quella specifica metrica. Ricordatevi questo concetto per quanto vedremo nella parte avanzata come controllare se un modello sta perdendo di qualità e bisogna rieseguire il ciclo di allenamento.
(slide 13)
Vi ho lasciato i riferimenti alla documentazione degli algoritmi e delle metriche utilizzate però voglio sottolineare ancora una volta che il servizio di Amazon Forecast è nato e ha senso proprio in quanto non abbiamo necessità di questa conoscenza matematica. Il lavoro si deve concentrare sulla precisione della preparazione dei dati, stabilire i valori che aiutano la previsione e cosa importante saper integrare la previsione ottenuta in strumenti che danno il vero valore aggiunto al cliente o alla vostra azienda.
Per fare un esempio è come un programmatore Java, deve conoscere la sintassi del linguaggio, le tecniche, il funzionamento delle classi, dei metodi etc. Ma non è necessario sapere l'algoritmo o il codice di esecuzione di come JAVA internamente gestisce la memoria RAM come pulisce le variabili non utilizzate etc etc. ma basta sapere como lo fa e che in condizione questo può avvenire.
Infatti la conoscenza tecnica di questi aspetti sarà compito degli sviluppatori di JAVA che hanno un obiettivo diverso dal nostro che dobbiamo sviluppare una soluzione applicativa.
OK siamo arrivati alla fine di questa lezione, passiamo adesso alla lezione sulla spiegabilità per poi chiudere la sezione del corso dedicata al predictor e passare alle previsioni vere e proprie.
