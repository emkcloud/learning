## (slide 1)

Benvenuto a questa nuova sezione del corso dedicata alla creazione dei modelli in Amazon Forecast. Questa fase rappresenta la parte centrale del servizio, infatti prima si preparano i dati, poi si creano i modelli e alla fine si generano le previsioni. Per essere precisi possiamo aggiungere le query sulle previsioni.

## (slide 2)

Se hai seguito tutte lezioni precedenti in questo momento ti dovresti trovare in questa condizione:

- Hai il tuo account AWS
- Hai scelto la regione geografica dove lavorare
- Hai creato il Ruolo di autorizzazione per il servizio di Amazon Forecast
- Hai creato un dataset group 
- Hai creato i 3 dataset di dati TARGET, RELATED e METADATA
- Hai eseguito l'upload di questi dataset su Amazon Forecast

Se hai fatto tutto questo siamo pronti per analizzare la creazione del modello di previsione chiamato "predictor" il quale si base sui dati dei dataset e sulle opzioni di creazione da noi indicate.

## (slide 3)

Prima di iniziare ad analizzare il predictor voglio discutere con voi di due cose.

La prima è rappresentata dallo screenshot in alto dove nelle lezioni precedenti avevamo visto che una volta caricati i dataset si sarebbe sbloccato il tasto di "predictor training" come indicato dalla prima freccia verde al contrario della generazione delle previsioni dove il tasto rimarrà disabilitato fino a quando non sarà disponibile un predictor. 

Quindi dopo l'upload del dataset in realtà non dobbiamo fare altro che eseguire START per la creazione di un predictor, indicare tutti i parametri di elaborazione e aspettare qualche ora per il suo completamento. Come potete vedere nello screenshot in basso vi indico solo alcune opzioni che bisognerà specificare.

Quindi dal punto di visto operativo in realtà creare un modello è la cosa più semplice del mondo, andiamo nella schermata AWS della creazione di un predictor inseriamo le opzioni e diamo conferma. 

Il problema è che Amazon Forecast è un servizio tutto automatico e molto facile da utilizzare ma questo non significa che non bisogna avere delle conoscenze solide su cosa in realtà Forecast va ad elaborare, il modo e il perché alcune volte dei dati possono essere importanti o semplicemente ignorati in base all'algoritmo scelto, senza parlare di molti aspetti che si possono personalizzare rispetto al funzionamento standard.

Quindi purtroppo anche se siamo pronti per fare START io direi andiamo prima a cercare di capire cosa succederà dal punto di vista logico quando lanceremo la creazione del predictor.

## (slide 4)

OK iniziamo con cos'è un predictor in Amazon Forecast. Iniziamo dalla definizione ufficiale di Amazon.
Un predictor è un modello di Amazon Forecast che viene addestrato utilizzando le serie temporali di destinazione, le serie temporali correlate, i metadati degli articoli e qualsiasi set di dati aggiuntivo che includiamo. Possiamo utilizzare i predictor per generare previsioni basate sui dati e sulle serie temporali.

Nella pratica esistono 3 tipi di predictor:

**(1) Legacy Manuale**

È il primo predictor che fu lanciato con Amazon Forecast, adesso ha il nome di Legacy perché è rimasto per compatibilità con il passato ma non è più consigliato utilizzarlo. Dalla console AWS non è più possibile utilizzarlo se lo si vuole creare bisogna utilizzare direttamente le API messe a disposizione dal servizio di Amazon Forecast. In questo predictor bisognava indicare l'algoritmo manualmente mentre gli iperparametri venivano settati in automatico direttamente da Forecast (HPO).

**(2) Legacy AutoML**

È il secondo predictor che fu lanciato con Amazon Forecast, adesso ha il nome di Legacy come il precedente perché è rimasto per compatibilità ma non è più consigliato utilizzarlo. In questo predictor l'algoritmo veniva scelto in automatico dal servizio tramite i backtest e grazie alle metriche ne veniva scelto uno il quale veniva usato come algoritmo di default. Quindi si risparmiava il tempo di valutazione dei differenti algoritmi riducendo il tempi di elaborazione e i costi generali.

**(3) AutoPredictor**

È il predictor attuale e consigliato nell'utilizzo di Amazon Forecast, le previsioni sono molto più accurate dei precedenti. A differenza di AutoML, AutoPredictor non si limita a selezionare un singolo algoritmo di previsione. Invece, utilizza un approccio più sofisticato che può combinare più algoritmi e tecniche diverse per generare le previsioni. 

AutoPredictor è progettato per gestire automaticamente complessità come la selezione delle feature, la trasformazione dei dati e l'ottimizzazione dei modelli, tutto ciò contribuisce a creare una soluzione di previsione che può essere più adattabile a vari scenari di dati senza un'intensa configurazione manuale.

Detto questo andiamo alla prossima slide per fare un passo in avanti:

## (slide 5)

OK in questa slide iniziamo ad analizzare alcuni parametri che dovremmo specificare nella creazione del predictor. Per cercare di creare il primo predictor il più velocemente possibile mi concentrerò solo sui parametri più importanti e rimanderemo le personalizzazioni nelle lezioni più avanzate.

Il primo parametro richiesto è il nome del predictor che deve essere indicativo delle sue caratteristiche per poi essere individuato facilmente in futuro tra la lista dei predictor disponibili. Io uso molte volte dei processi automatici per la generazione di tutto il lavoro di previsione, quindi uso spesso le date e gli orari nel nome.

Il secondo parametro richiesto è la frequenza che rappresenta il tipo di previsione come aggregazione di tempo che deve essere generata quando richiederemo la generazione dei forecast. Il valori sono:

- 1 Minuto
- 1 Ora
- 1 Giorno
- 1 Settimana
- 1 Mese
- 1 Anno

Però possiamo anche usare valori multipli, tipo previsioni di 3 giorni in 3 giorni o 4 mesi in 4 mesi etc. Quindi posso specificare il livello di tempo ma anche i periodi che devono essere aggregati. 

Questa opzione è direttamente collegata a quella dell'orizzonte dove viene indicato quante volte deve essere ripetuta la previsione. Facciamo un esempio che è sicuramente più chiaro:

Nella frequenza indico 1 Mese e In orizzonte 3. Questo significa che avrò nella linea del tempo futuro 3 previsioni dove ognuna rappresenta il valore "demand" di un mese totale. Nel nostro caso le vendite di un mese ripetute 3 volte per i mesi successivi. Quindi se è GENNAIO avrò le previsioni delle vendite di un singolo prodotto per Febbraio, Marzo e Aprile.

Nella prossima slide vi farò uno schema per chiarire questo concetto.

Dopo la frequenza vediamo un altro campo a tendina chiamato "allineamento di tempo" , questo serve in quei casi dove vogliamo specificare esattamente da dove inizia l'aggregazione indicata in frequenza, ad esempio se ho specificato come frequenza 1 Giorno e ho un dataset orario potrei indicare a Forecast che non voglio che il giorno inizi da mezzanotte ma dalle 8:00 del mattino, quindi il mio giorno sarà dalle 8:00 alle 8:00 seguenti no quello corrispondente all'orario solare.

Cosi lo potrei fare anche per un anno, dove magari non voglio aggregare l'anno da Gennaio ma ad esempio da Marzo a Marzo successivo. Infatti come potrete vedere quando userete questa schermata l'elenco della tendina cambia in base alla frequenza selezionata, se selezionate giorni nell'allineamento del tempo troverete nell'elenco degli orari, mentre se selezionate in frequenza settimane troverete nella tendina i valori dei giorni e cosi via.

L'ultima opzione di questa schermata è la dimensione che in questo momento non vado ad approfondirla in quanto dovremmo avere solo item_id e possiamo ignorare questa opzione lasciando il suo valore di default.

OK a questo punto andiamo alla prossima slide per vedere graficamente la frequenza.

## (slide 6)

La frequenza: facciamo conto che siamo nel bel mezzo di natale e le previsioni future inizieranno a gennaio. Nel primo caso abbiamo frequenza 3 MESI quindi si divideranno i mesi in blocchi da 3 e ogni blocco sarà una sola previsione con il totale di quel periodo. Mentre l'orizzonte è il numero delle previsioni totali che abbiamo impostato a 2. Quindi avremmo 2 previsioni:

La prima: Le vendite sommate di GEN FEB e MAR (orizzonte 1 di 2)
La seconda: Le vendite sommate di APR MAG e GIU (orizzonte 2 di 2)
Da qui in avanti non ci saranno più previsioni.

Nel secondo esempio invece abbiamo La FREQUENZA a un mese. Quindi tutto il periodo futuro sarà suddiviso in blocchi da un mese e dato che l'orizzonte in questo esempio è 4 avremo 4 previsioni:

La prima: Le vendite totali del mese di GEN (orizzonte 1 di 4)
La seconda: Le vendite totali del mese di FEB (orizzonte 2 di 4) e cosi via per MAR e APR
Da qui in avanti non ci saranno più previsioni.

Sperando di essere stato chiaro su questo concetto passerei al secondo gruppo di opzioni presenti sulla schermata dedicata alla creazione di un predictor.

## (slide 7)

In questa schermata vengono indicati i quantili che devono essere disponibili nella generazione delle previsioni. Se non sapete cosa sono i quantili cercherò di spiegarvelo nel modo più semplice possibile, in ogni caso se avete la necessità di approfondire questo concetto statistico vi consiglio di cercare tra l'enorme documentazione presente in internet.

Anche io personalmente quando stavo alle prime armi ho letto parecchi articoli che mi hanno chiarito molti concetti che mi sono tornati utili in diversi servizi AWS. Vi consiglio di fare la stessa cosa in quanto trovate persone che sulla statistica sono molto più preparate di me e hanno generato contenuti di alta qualità.

In ogni caso, i quantili sono valori che dividono un set di dati ordinato in parti uguali in base alla loro distribuzione e rappresentano i punti di taglio che suddividono i dati in segmenti con la stessa probabilità o in proporzioni specifiche.

Ad esempio, la mediana è un quantile che divide i dati in due parti uguali, con il 50% dei dati sotto e il 50% dei dati sopra. Ad esempio nello screenshot della slide ci sono 3 quantili indicati, che sono 0.10 0.50 e 0.90 questo significa che il modello genererà queste previsioni: Assumiamo che le previsioni sono unità di vendita:

- Una previsione con il quantile 0.10 genererà un forecast in percentile 10 il quale indicherà un valore di riferimento in cui il 90% delle volte le vendite saranno maggiori di questo valore e il 10% delle volte saranno uguali o minori di questo valore. 

- Cosi per il quantile 0.50 che genererà una previsione in percentile 50 per cui le vendite potranno essere al 50% minori della previsioni e al 50% maggiori delle previsioni.

- Cosi per il quantile 0.90 che genererà un forecast in percentile 90 il quale indicherà un valore di riferimento in cui il 10% delle volte le vendite saranno maggiori di questo valore e il 90% delle volte saranno uguali o minori di questo valore. 

Questo valore ovviamente lo dovete discutere con il vostro cliente o analizzare l'obiettivo della vostra azienda. Ad esempio nella mia esperienza ho visto che quasi nessun cliente lavora con il percentile 50, in quanto preferiscono un po' di sbilanciamento sul fatto di avere più magazzino e non perdere vendite.

Ad esempio su un percentile 70 ho un previsione che mi permette di stoccare in anticipo un certo numero di unità per le quali solo il 30% delle volte posso rimanere senza stock e perdere la vendita. Però come accennato dipende molto dalle richieste, dal settore merceologico e dagli investimenti, quindi dovete analizzarlo con chi è responsabile di questa decisione.

## (slide 8)

Ok cerchiamo di rivedere il tutto, prendiamo le vendite di 100 giorni, uso il valore 100 per far si che i numeri sono semplici da calcolare, sto barando un po' però lo faccio per semplificare perdonatemi.

A questo punto ordiniamo questi numeri (che sono le vendite mensili di un prodotto) in modo crescente in una tabella. Per prendere il valore corrispondente al quantile usiamo la formula di "arrotondamento all'indice intero più vicino", ci sono anche modi diversi come ad esempio il "metodo di interpolazione lineare" che calcola la media delle due posizioni più vicine etc però non complichiamoci la vita e usiamo la formula più semplice.

- Il quantile 0,10 è in posizione 10 dove troviamo 205 unità di vendita
- Il quantile 0,50 è in posizione 50 dove troviamo 280 unità di vendita
- Il quantile 0,65 è in posizione 65 dove troviamo 314 unità di vendita
- Il quantile 0,90 è in posizione 90 dove troviamo 389 unità di vendita

Ora avete sicuramente capito perché ho scelto 100 elementi per rendermi la vita semplice :D

In questo esempio il valore 205 rappresenta quel valore in cui il 10% delle vendite è il 10% delle volte uguale o minore alla previsione e stesso discorso per tutti gli altri valori.

Questo su molti aspetti è più interessante di avere un solo valore fisso perché ti ha una idea di ampiezza su cui lavora la previsione. Ad esempio in molte soluzioni di riordino automatico facciamo vedere all'operatore diversi valori tra consigliato minimo e massimo in modo che possa scegliere anche analizzando altri fattori come l'offerta di acquisto, la scarsità anomala del mercato attuale, il pericolo di eventi probabili che potrebbe far mancare in futuro la disponibilità del prodotto, etc etc...

In ogni caso esiste anche la possibilità di lavorare con una sola previsione fissa e gestire i vostro programmi applicativi solo con quel valore. Il valore si chiama "mean" e deve essere specificato nelle opzioni. Vi faccio vedere nella prossima slide esattamente dove si trova.

## (slide 9)

Ritorniamo alla schermata precedente dove erano indicati i quantili, aggiungendo con il pulsante apposito un nuovo quantile potete vedere che uno dei valori ammessi è proprio "mean".

La definizione di questo valore è la seguente: 
La specifica "mean" è comunemente utilizzata quando l'obiettivo principale è prevedere il valore più probabile o il "centro" della distribuzione degli esiti futuri. Questo approccio è utile per molte applicazioni di previsione standard in cui si è interessati principalmente al risultato medio o più comune, piuttosto che agli estremi della distribuzione.

Anche in questo caso lavorare con un unico valore medio o con degli estremi tipo 10/90 e con dei percentili di riferimento come possono essere 50/70 dipende dal vostro cliente, dalla vostra azienda o dal vostro business in generale.

## (slide 10)

OK, passiamo al seguente gruppo di opzioni che dovranno essere specificate durante la creazione di un predictor. In questa parte al momento le cose sono semplici perché non useremo nessuna di queste opzioni e nella creazione del nostro predictor lasceremo i valori cosi di default come li troviamo nella schermata.

Però riprenderemo questo aspetto nelle lezioni successive che riguardano la parte avanzata dei predictor, in quanto sono opzioni di controllo che servono quando abbiamo una certa confidenza con i modelli e hanno anche dei costi aggiuntivi rispetto a quelli di elaborazione durante la creazione di un predictor.

Quindi passiamo adesso all'ultimo gruppo di opzioni:

## (slide 11)

Queste opzioni sono molto interessanti, in quanto sono messe a disposizione da Amazon senza dover creare dei dataset complessi e costosi da manutenere. Oltretutto Amazon ha promesso che rilascerà in futuro altre feature che miglioreranno sempre di più le previsioni che sono influenzate da eventi esterni.

Al momento abbiamo WEATHER che ci permette di associare ai nostri dati le previsioni del tempo, in quanto Amazon ha per diverse parti del mondo (non tutte, quindi vedete sempre la documentazione per i paesi abilitati) e può capire se un giorno di forte pioggia può cambiare il trend di vendita di un prodotto sia in positivo che in negativo.

Nel mio caso l'opzione è disabilita perché comunque per renderla attiva bisogna mettere nel dataset anche un campo con le coordinate geografiche che associa la vendita ad una zona geografica. E dato che nel nostro dataset non esiste questo campo la funzione non viene messa a disposizione e viene disattivata per default.

Un'altro dataset integrativo messo a disposizione da Amazon è Holidays che uso quasi sempre nelle mie previsioni, in quanto ha memorizzate tutte le festività divise per singolo paese e quindi è possibile migliorare le previsioni di tutti quei prodotti che hanno delle alte vendite in momenti specifici come natale, pasqua etc..

Invece la configurazione avanzate indicata a fine schermata è una sezione con un file JSON di configurazione che vedremo nella parte avanzata del corso, al momento la lasciamo così di default.

## (slide 12)

Prima di chiudere questa lezione vi faccio vedere che nella documentazione ufficiale di Amazon trovate le coordinate geografiche ammesse per il servizio di previsione con WEATHER. Selezionate in alto la regione e controllate.