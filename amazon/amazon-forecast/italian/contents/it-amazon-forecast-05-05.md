## (slide 1)

Questa più che una nuova lezione è solo un consiglio basato sulla mia esperienza personale. Infatti sia se si usa forecast o anche nel caso in cui non lo utilizziamo possiamo ottenere dei grafici sulle previsioni in modo semplice utilizzando il servizio di Amazon Quicksight.

## (slide 2)

Amazon QuickSight è un servizio di business intelligence (molto spesso chiamato BI). È un servizio gestito e offerto da Amazon Web Services. Consente agli utenti di analizzare, visualizzare e condividere dati tramite dashboard interattive e report in formato PDF. Ideale per grandi quantità di dati grazie alla sua velocità.

Perché vi sto presentando questo servizio? Il perché è il fatto che Quicksight contiene al suo interno delle funzionalità di previsione che si posso integrare con Amazon Forecast sia se abbiamo un modello presente nel nostro account ma anche senza modello utilizzando quello di default di Quicksight.

In questa schermata ad esempio potete vedere dove ci sono le frecce verdi che la parte del grafico arancione sono le previsioni future dei dati storici rappresentati in colore celeste/blu.

## (slide 3)

Prima di andare a vedere QuickSight guardiamo una soluzione standard, dove le previsioni che generiamo con Amazon Forecast vengono esportate e successivamente integrate in un software applicativo, in questo scenario normalmente questo è il risultato. Una tabella dettagliata dove l'utente può cercare il suo prodotto o la risorsa sui cui vuole analizzare le previsioni e nel caso esportare questi dati su Excel per altre analisi.

Tramite Quicksight possiamo usare sempre i nostri dati che sono le previsioni generate da un nostro modello presente nel servizio di Amazon Forecast e creare facilmente dei grafici di analisi. Vediamo un esempio:

## (slide 4)

Ecco qui è lo stesso risultato con un grafico, al centro abbiamo le vendite reali con la linea verde, le ultime 3 settimane sulla destra sono a zero in quanto sono date future non abbiamo ancora i dati di vendita reali. Come possiamo vedere il percentile 50 e 70 sono quelli che si avvicinano sempre alle vendite reali.

Possiamo fare decine di queste analisi e anche con grafici diversi, istogrammi, linea, area etc. Strumento molto utile anche per capire se i valori di previsioni si avvicinano alla realtà e quindi capire se i nostri modelli sono qualitativi al punto giusto per essere messi in produzione e integrati con il proprio applicativo.

## (slide 5)

Però esiste anche un altro metodo per usare le Previsioni in Quicksight molto più semplice come utilizzo ma ovviamente meno personalizzato rispetto ai nostri dati specifici. Il metodo consiste nell'usare una serie storica senza aver neanche attivato il servizio di Amazon Forecast nel nostro account, semplicemente QuickSight userà un suo modello standard studiato per delle casistiche generali.

Per non andare troppo OFF-TOPIC a chi è interessato a questa soluzione consiglio di iniziare sempre dalla documentazione ufficiale dove trovate un capitolo dedicato proprio a questo aspetto. Per il momento vi faccio vedere velocemente il risultato finale con un semplice grafico delle vendite.

## (slide 6)

Quindi creiamo un grafico di tendenza, ad esempio le vendite dell'ultimo periodo andando a leggere direttamente il database delle vendita senza necessità di creare dataset, modelli etc. 

Nella parte superiore abbiamo il grafico classico con le vendite di un determinato periodo e aggregato su base mensile. A questo punto aggiungiamo previsione con il menu indicato dalla prima freccia verde.

E otteniamo il grafico indicato nella parte inferiore, dove viene aggiunta una linea gialla che rappresenta le previsioni per i periodi futuri , quest'ultima a sua volta è racchiusa in un'area gialla che rappresenta il valore minimo e massimo che puó raggiungere la previsione. In modo da avere una previsione variabile.

Stavo pensando di fare un corso in futuro su QuickSight però ho la paura di fare qualcosa per un target forse troppo limitato, in quanto non vedo richieste in questa direzione, però fatemi sapere nei commenti.. 