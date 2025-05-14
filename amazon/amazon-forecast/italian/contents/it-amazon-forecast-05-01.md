## (slide 1)

Benvenuti a questa nuova sezione dove finalmente siamo pronti per generare le previsioni. Abbiamo preparato i nostri dati, creato il modello e adesso non resta che lanciare le previsioni.

## (slide 2)

Per prima cosa andiamo nella dashboard di Amazon Forecast scegliamo il nostro dataset group, operazione ormai standard per voi. Una volta dentro il dataset group troveremo il tasto di "generazione forecast" attivo in quanto il servizio si è accorto che abbiamo un predictor disponibile.

Iniziamo la richiesta di creazione dei Forecast facendo clic su START. (seconda freccia verde sulla slide)

## (slide 3)

Nella prima serie di parametri dobbiamo specificare il nome delle previsioni MyForecastName a seguire dobbiamo scegliere il predictor creato nelle lezioni precedenti e come quantili lasciate il campo di default che significa che le previsioni saranno generate con i quantili indicati nella creazione del predictor.

Se per qualche motivo volete previsioni solo per un quantile o solo su alcuni potete specificarli in questo campo manualmente. Ok adesso passiamo alla seconda parte di parametri richiesti.

## (slide 4)

In questa schermata possiamo vedere che ci vengono chiesti gli items per cui vogliamo generare la previsione, per default Amazon Forecast ci propone "All Items" ma io non userei mai questa opzione, specialmente duranti i test, sarebbe un costo inutile.

Nella fase di addestramento è giusto avere più dati possibili in modo da migliorare le previsioni ma quando facciamo i testi dobbiamo creare le previsioni per pochi prodotti che poi andiamo a testare manualmente.

Quindi create un file molto semplice dove su ogni riga ci sarà un codice prodotto, scegliete 50, 100 anche 1000 prodotti se volete fare TEST approfonditi, non serve ad esempio generare e pagare 11.000 previsioni di prodotti per fare dei TEST. Caricate questo file di elenco su Amazon S3 e indicate il suo percorso sull'opzione specifica che vedete in schermata nella seconda freccia verde.

Per far comparire questa opzione ricordate di selezionare "selected items" come opzione e no "all items", una volta indicato il percorso del file contenente la lista degli items indicate il Ruolo che abbiamo usato diverse volte ormai e andiamo avanti sulla pagina per analizzare le opzioni successive. 

## (slide 5)

In questa schermata ci viene indicato di definire lo schema che identifica i prodotti da selezionare, 

in realtà il file contiene solo il codice item_id, uno per riga quindi possiamo lasciare lo schema predefinito che viene indicato come default. A questo punto possiamo confermare la generazione del Forecast e aspettare la conclusione dell'elaborazione. Come sempre armatevi di pazienza...

## (slide 6)

Andate nel menu principale della dashboard di Amazon Forecast selezionate forecasts e dovrete vedrete come in questa schermata il primo blocco di previsioni in "pending". Aspettate diversi minuti fino a quando lo stato passa ad Active come nell'immagine successiva.

Bene a questo punto abbiamo già pronte le nostre previsioni non ci resta che vedere come gestirle, analizzarle o integrarle nel nostro applicativo.