(slide 1)
Eccoci qui siamo nel punto finale della prima parte di questo corso. Dove vedremo come leggere le previsioni. Quindi senza perdere tempo andiamo a vedere cosa fare.
(slide 2)
Per interrogare le previsioni esistono 4 metodi principali, l’interfaccia della management console, i comandi disponibili su AWS CLI, le richieste di esportazione su Amazon S3 e il codice di programmazione con utilizzo di SDK. Qui un accenno alle caratteristiche di ogni metodo che nelle prossime slide analizzeremo:
    • (1) CONSOLE AWS: Utilizzo semplice e immediato utile per le fasi di impostazione.
    • (2) AWS CLI: Strumento potente e versatile da linea di comando e script.
    • (3) EXPORT S3: Esportare in una unica soluzione tutte le previsioni disponibili.
    • (4) DEVELOPER SDK: Strumento per integrare le previsioni nel proprio applicativo.
Passiamo ad un'analisi più dettagliata di quanto indicato in questa slide.
(slide 3)
Iniziamo con il metodo tramite CONSOLE AWS
Per interrogare un forecast basta selezionare come sempre il nostro dataset group dove questa volta troviamo al suo interno attivata l'opzione Query Forecast che ci permetterà di eseguire delle semplici query indicando il codice del prodotto (item_id) e il periodo di previsione.
Quindi preparate un codice prodotto che volete analizzare e di cui conoscete le caratteristiche in modo da controllare se le previsioni si avvicinano alla realtà che già conoscete.
Clicchiamo su Pulsante Query Forecast indicato dalla seconda freccia verde..
(slide 4)
Questa è la schermata di interrogazione Forecast messa a disposizione dalla console AWS. Nella parte superiori indicate le opzioni della query e nella parte inferiore il risultato della query.
La parte sinistra del grafico rappresenta gli ultimi periodi storici presenti nei nostri dataset (è l'area di color grigio) mentre la parte di destra rappresenta le previsioni per percentili. Vi indico qui i valori del percentile 50 che ci indicano 3.171 unità di vendita nella prossima settimana, 3.313 quella seguente e così via.
Per vedere i valori basta posizionarsi con il cursore sulla linea del percentile interessato e il periodo che volete vedere. Questo strumento è molto utile per fare alcuni test prima di andare in produzione o quando agli inizi stiamo controllando il modello e ci serve verificare alcuni dati. Però quando vogliamo eseguire analisi di produzione su diversi prodotti conviene utilizzare altri strumenti.
(slide 5)
Il secondo metodo per eseguire una query a Forecast è quella di usare lo strumento a linea di comando AWS CLI che ci permette di eseguire la stessa query fatta in console però ricevendo come risultato un file JSON. Utile per eseguire delle query mentre stiamo sviluppando e per eseguire degli script di verifica.
In questa schermata vedete a sinistra il comando che è
aws forecastquery query-forecast con i valori:
    • forecast-arn (che potete trovare sui dettagli della risorsa nella console)
    • start-date (la data del periodo iniziale di previsione o frequenze di tempo precedenti)
    • end-date (la data del periodo finale di previsione)
    • filters (il codice prodotto che volete analizzare)
Nella parte destra vediamo il risultato JSON della chiamata, vi sto indicando solo la parte del Percentile 50 in realtà vi verranno restituite tutte le previsioni per ogni Percentile disponibile e tramite un indice Array potete estrarre quello che vi interessa o creare voi stessi un piccolo summary a forma tabellare.
Ricordate sempre che se usate un access key o usate un ruolo collegato al server che emette il comando dovete sempre specificare l'autorizzazione all'utilizzo del servizio di Amazon Forecast nella risorsa IAM.
(slide 6)
OK passiamo adesso a un metodo molto utilizzato a livello aziendale, cioè quello di fare una esportazione massiva di tutti i prodotti e la loro previsione su Amazon S3 in formato CSV.
Per eseguire questa operazione selezioniamo il nostro Forecast in stato di active e dovremmo vedere la schermata dei dettagli del forecast dove c'è anche l'informazioni ARN che vi serve quando usate i strumenti da linea di comando o i codici di programmazione, in quanto ARN indica l'indirizzo univoco alla risorsa AWS.
Se andate più in basso alla schermata troverete la sezione Exports, che essendo la prima volta che la visitiamo non dovrebbe avere nessuna operazione di export eseguita.
Quindi clicchiamo sul tasto "Create Forecast Export".
(slide 7)
Per eseguire l'esportazione indichiamo il nome dell'esportazione e il ruolo che abbiamo già usato diverse volte in queste lezioni. Indichiamo CSV come tipo di esportazione e indichiamo il nome del file di output su S3. Il quale in seguito lo useremo per l'analisi con EXCEL o software alternativi compatibili con i file CSV. 
Confermiamo e aspettiamo qualche minuto la fine dell'elaborazione.
(slide 8)
In questa schermata potete vedere che il lavoro di esportazione apparirà nella sezione Exports e una volta che il lavoro passerà in stato di Active potrete eseguire il download del file interessato direttamente da S3. Portatelo in ambiente locale e aprite il file con il vostro strumento EXCEL o altro compatibile CSV.
(slide 9)
Qui un esempio di alcuni dati presenti nel file esportato. Il codice prodotto il periodo di previsione e i percentili, troverete anche altre colonne di informazione in base ai dati che erano presenti sul dataset METADATA. Ovviamente vi conviene fare gli arrotondamenti sui valori indicati, specialmente se la previsione deve essere un valore come unità di vendita. Molte volte dare agli utenti finali numero con 8 virgole può creare dei problemi è sempre meglio semplificare quando possibile.
(slide 10)
Un altro metodo di accedere alle previsioni è tramite codice di programmazione, io qui vi riporto un piccolo esempio dove eseguo un queryForecast con il framework Laravel, come potete notare la logica è semplice ho un oggetto client con dei metodi già pronti con cui accedere alle API di Amazon Forecast.
La chiamata al metodo (in questo caso queryForecast) mi ritorna un array con tutte le informazioni della previsioni, che si posso visualizzare in un'applicazione WEB o memorizzare su un database relazionale.
Nella prossima slide vi faccio vedere un esempio pratico in cui è stato applicato proprio questo codice PHP.
(slide 11)
Questa è la schermata di un mio cliente, come potete vedere è partito da una lista di prodotti suddivisi per stock, fornitore, categoria etc. Ha selezionato la icona di previsione su uno di questi prodotti e il programma tramite chiamata API ad Amazon Forecast gli prepara una tabella in cui indicare le previsioni del prodotto.
Quindi per questo prodotto su una previsione a 21 giorni rispetto allo stock attuale è consigliato l'acquisto di almeno altri 2 cartoni con i livelli di minimo e massimo impostati su 1 e 10. In questo caso ogni cartone contiene 8 unità e il consigliato sarebbe 2 cartoni x 8 = 16 unità da riordinare.
Con questo possiamo concludere questa sezione del corso. Spero che vi siate trovati bene e che l'esperienza vi sia tornata utile. Grazie per la partecipazione ci vediamo nelle prossime lezioni.
