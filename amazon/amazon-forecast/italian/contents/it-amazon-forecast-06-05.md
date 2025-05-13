(slide 1)
In questa lezione vediamo come usare il servizio di CloudWatch per avere delle statistiche sull'utilizzo del servizio, come ad esempio quanto occupano nel tempo di nostri dataset, quanti punti di previsione abbiamo generato in un certo periodo, quanti predictor abbiamo creato, etc.
(slide 2)
Il servizio di CloudWatch è uno dei servizi più importanti in AWS in quanto è possibile tenere sotto controllo moltissime metriche legate a tutti i servizi presenti in AWS. Il servizio è molto vasto non possiamo fare una panoramica completa però possiamo cercare di elencare alcuni punti importanti.
Tramite CloudWatch ad esempio possiamo centralizzare i logs sia dei servizi AWS che delle nostre applicazioni, possiamo avere logs per le nostre funzioni lambda, del nostro server web come apache o nginx, degli accessi alle politiche del nostro firewall etc etc
In più abbiamo a disposizione centinaia di metriche con decine di servizi AWS già integrati, come ad esempio controllare la CPU media delle nostre istanze EC2, la memoria RAM utilizzata dalle nostre istanze di database RDS come MYSQL o DB/2. Con tutto questo possiamo creare anche degli allarmi.
Ad esempio possiamo essere avvisati se i dataset di Forecast sorpassano una determinata soglia, o se gli endpoint generati da forecast non arrivano a determinati limiti per controllare i costi generali, etc. Il servizio da veramente tantissimi strumenti di controllo sui cui possiamo anche creare delle dashboard personali.
(slide 3)
Nel nostro caso quello che ci interessa è Amazon Forecast e tutte le metriche messe a disposizione di CloudWatch per analizzarle in un determinato periodo di tempo che possiamo selezionare. Quindi selezionate dal menu principale le Metriche (prima freccia verde alla sinistra) e ricercate il servizio di AWS/Forecast nell'elenco dei servizi che sono attivi sul vostro account (freccia verde sulla destra).
Una volta individuata la sezione di AWS/Forecast cliccate per entrare nelle metriche.
(slide 4)
Queste sono le metriche divise per gruppi che sono presenti in cloudWatch al momento di questo corso e che sono collegate al servizio di Amazon Forecast,
(1) DATASET / IMPORT JOB (ARN)
Analisi della metrica di cloudwatch DatasetSize per ogni Job di importazione eseguito su un Dataset. Con questa metrica possiamo controllare le dimensione dei dataset per tutti i job di import utilizzati.
(2) FORECAST / PREDICTOR (ARN)
Analisi della metrica di cloudwatch ForecastDataPointsGenerated in modo che si possa analizzare tutti gli endpoint generati dalle funzioni di Forecast per ogni associazione Forecast / Predictor.
(3) FORECAST (ARN)
Analisi della metrica di cloudwatch CreateForecastExecutionTime in modo che si possano analizzare tutti i tempi di esecuzione che riguardano la generazione delle previsioni Forecast.
(4) MONITOR (ARN)
Analisi della metriche utilizzate nel monitoraggio del predictor in modo da tener sotto controllo i cambi di qualità del modello e la variazione delle metriche a seguito della generazione di nuove previsioni.
(5) PREDICTOR (ARN)
Analisi della metrica di cloudwatch ExcecutionTime e ForecastDataPointsGenerated per analizzare i tempi di esecuzione della creazione di un modello e i punti di previsione successivamente generati.
(6) METRIC WITH NO DIMENSION
Analisi delle metriche generali che eseguono conteggi su tutte le risorse presenti in Amazon Forecast.
Scegliamo la sezione delle metriche generali per vedere cosa troviamo al suo interno e per provare a vedere qualche grafico che ci possa indicare delle informazioni importanti su Amazon Forecast.
(slide 5)
Nelle lista delle metriche disponibili ad esempio selezioniamo DataSetSize dovremmo vedere la dimensione dei nostri dataset durante le operazioni di importazione. In realtà io vi ho semplificato molto questa slide perché mi interessava trasmettervi il concetto che su cloudwatch trovate molte informazioni che vi possono aiutare a capire il volume delle operazioni che eseguite con Amazon Forecast.
Amazon cloudwatch e l'interrogazione delle metriche è molto più completo e complesso, potete settare i valori del grafico per medie, minimi, massimi, somme. Cambiare il tipo di grafico tra linea, area, barre etc, inoltre potete eseguire delle query complesse sulle metriche e volendo si possono attivare gli allarmi.
(slide 6)
Se volete conoscere qualcosa di più sul servizio di cloudwatch che è molto vasto come argomento vi consiglio di partire dalla documentazione ufficiale per poi passare al blog AWS come approfondimento.
Grazie di cuore a tutti quelli che hanno resistito fino a questa lezione, ci vediamo alle prossime lezioni per la chiusura del corso. Qualsiasi commento o consiglio è ben accetto.. ciao a tutti.. 
