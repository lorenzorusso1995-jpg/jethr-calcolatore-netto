# Calcolatore RAL → netto · 2026

**[Provalo qui](https://lorenzorusso1995-jpg.github.io/jethr-calcolatore-netto/)**

Prototipo per la selezione AI Builder di Jet HR.

Inserisci la RAL e ottieni il netto annuo e mensile, con il dettaglio di ogni trattenuta e il calcolo passo per passo. Funziona anche al contrario, dal netto che vuoi in busta alla RAL da chiedere.

## Cosa fa

Calcola il netto annuo e quello mensile, tenendo separati il mese ordinario e la tredicesima/quattordicesima (dal momento che sulle mensilità aggiuntive non si applicano le detrazioni).

Fa anche il percorso inverso, che è poi la domanda che si pongono alcuni utenti che stanno trattando un'offerta: che RAL devo chiedere per avere 1.800 € netti al mese?

Ogni voce del risultato si può aprire e leggere nel dettaglio, con la base imponibile, l'aliquota, il calcolo svolto sui numeri inseriti e il riferimento normativo.

L'addizionale comunale è quella del comune scelto: ci sono tutti i 7.904 comuni italiani, importati dalla tabella ufficiale del MEF. Delibera 2026 se pubblicata, altrimenti l'ultimo dato in vigore, come prevede la norma. Le addizionali regionali sono tutte e 21 (le 19 regioni più Trento e Bolzano, che deliberano separatamente).

Nelle impostazioni avanzate si trovano fondo pensione complementare, carichi familiari e agevolazioni (sono tutte visibili e si possono disattivare).

C'è anche l'aliquota marginale, cioè quanto ti resta davvero di 1.000 € di aumento. E il calcolo si può condividere con un link, perché lo stato dei campi finisce nell'URL.

## Come l'ho costruito

### Le fonti

Per ogni voce sono partito dalla norma invece che dai riassunti: legge di bilancio 2026 (L. 199/2025) per gli scaglioni IRPEF, artt. 12 e 13 TUIR per le detrazioni, L. 207/2024 per il taglio del cuneo, circolare INPS n. 6 del 30 gennaio 2026 per minimali, massimali e prima fascia pensionabile.

Le voci contributive minori (CIGS, FIS, EBINTER, EST, COVELCO) vengono dal CCNL Terziario e Commercio.

Per le addizionali comunali la fonte è il MEF.

### Feedback su altri calcolatori online

Prima di mettere mano all'interfaccia ho letto, con l'aiuto dell'AI, i thread di r/ItaliaPersonalFinance, r/techcompenso e r/Italia dove gli autori di due calcolatori hanno chiesto feedback pubblico. In tutto circa 130 commenti, tra critiche, bug segnalati e richieste.

I problemi più segnalati sono:
- il massimale INPS che non viene considerato sulle RAL alte
- il calcolo diretto e quello inverso che danno risultati diversi
- i bonus applicati a tutti anche a chi non ne ha diritto.

Mentre le richieste che tornavano più spesso sono:
- poter dedurre il fondo pensione
- scegliere il numero di mensilità
- scrivere la RAL a mano invece di usare uno slider
- il tema scuro
- poter vedere la formula sotto il risultato.

Inoltre, l'addizionale comunale fissata allo 0,80% è la critica che ho trovato più volte, con la stessa osservazione: "ci sono comuni allo 0% e altri sopra l'1%".

Infine, molto presente anche la diffidenza verso gli strumenti fatti in fretta con l'AI, riconosciuti da segnali precisi: interfacce tutte uguali, emoji al posto delle icone, link rotti nel footer.

### I calcolatori che già esistono

Ho provato quelli che la community usa e cita: stipendee.it, che è il riferimento per parecchi, poi calcolastipendionetto.it, tuttocalcolato.it, calcolonetto.it, e i calcolatori pubblicati da Jet HR.

Mi sono serviti per due cose. La prima è controllare i miei numeri: se il mio risultato si discostava dal loro volevo capire perché, e in un paio di casi la differenza mi ha fatto trovare qualche imprecisione. La seconda è capire cosa si aspetta chi arriva su una pagina del genere, perché ormai alcune cose sono uno standard: le card di sintesi in alto, la ripartizione del lordo, la scelta delle mensilità. Quelle sono state mantenute perché è ciò che l'utente si aspetta di trovare.

In più ho aggiunto quello che gli utenti chiedevano e che non trovavo negli altri calcolatori: la spiegazione del calcolo e il dato comunale preciso.

## Cosa ho deciso di conseguenza

Ho importato le aliquote reali di tutti i 7.904 comuni dagli elenchi MEF, con soglie di esenzione e scaglioni, invece di usare lo 0,80% medio nazionale.

Ogni riga del risultato si apre sul calcolo fatto con i tuoi numeri e cita la norma. Non è un testo scritto a parte: lo produce la stessa funzione che calcola, quindi se cambio il calcolo cambia anche la spiegazione e le due non possono andare fuori sincrono.

Il netto → RAL non è una seconda formula. È una ricerca per bisezione sulla stessa funzione del RAL → netto, così le due direzioni coincidono per costruzione.

Ho modellato la prima fascia pensionabile (56.224 €, oltre la quale scatta l'1% aggiuntivo) e il massimale contributivo (122.295 €), che sono i due punti dove le RAL alte danno risultati sbagliati.

Trattamento integrativo e taglio del cuneo sono opzioni che si vedono e si possono spegnere, con le condizioni di spettanza calcolate e spiegate una per una.

Ho aggiunto il tema scuro, che era richiesto spesso.

Il campo della RAL si scrive, senza slider.

Sull'aspetto ho cercato di evitare i segnali che nei commenti facevano storcere il naso: niente emoji usate come icone, niente sezioni decorative.

Il design riprende il sistema visivo di Jet HR (colori, tipografia, raggi, ombre).

In testa alla pagina ci sono l'anno fiscale e la data di aggiornamento, e per ogni comune è dichiarato da dove arriva il dato. Un calcolatore fiscale invecchia in fretta e chi lo apre deve sapere se è aggiornato o meno.

## Il calcolo

```
RAL
 − contributi INPS a carico del dipendente   (IVS 9,19%, +1% oltre la prima fascia,
                                              CIGS, FIS, enti bilaterali, con massimale)
 − fondo pensione complementare              (deducibile fino a 5.164,57 €)
 = imponibile IRPEF
 − IRPEF netta                               (scaglioni 23% / 33% / 43%, meno le
                                              detrazioni da lavoro, cuneo e familiari,
                                              mai sotto zero)
 − addizionale regionale e comunale          (si calcolano sull'imponibile, non
                                              sull'IRPEF, e non si pagano in incapienza)
 + trattamento integrativo e somma esente    (sono crediti, quindi si sommano)
 = NETTO ANNUO
```

Il motore è una funzione pura che restituisce insieme i totali e la spiegazione di come ci è arrivata. Ogni riga del dettaglio nell'app è quella struttura mostrata a schermo.

Leggendo i feedback online ho provato a lavorare su alcuni punti che risultavano poco chiari negli strumenti già disponibili:

L'incapienza. Se le detrazioni azzerano l'IRPEF, le addizionali non sono dovute. L'app scrive perché sono a zero invece di mostrare uno zero e basta.

La differenza tra soglia e franchigia. A Milano l'esenzione fino a 23.000 € è una soglia secca: a 22.999 € non paghi niente, a 23.001 € paghi lo 0,80% sull'intero imponibile, non sulla parte eccedente. È uno scalino che sorprende parecchie persone.

Gli scaglioni comunali. Nei comuni con più aliquote ogni aliquota vale solo per la sua fascia di reddito, come nell'IRPEF.

Tredicesima e quattordicesima. Non cambiano il netto annuo, cambiano solo come viene distribuito nell'anno.

## Limiti

Li scrivo qui, e l'app li dice a chi la usa.

Circa 600 delibere comunali prevedono esenzioni legate a categorie di reddito o a platee specifiche (pensionati, fasce ISEE). Sono scritte in testo libero e non si possono modellare: il calcolatore le segnala comune per comune e calcola il caso generale.

Dove manca la delibera 2026 resta valida l'ultima in vigore, come prevede la norma. Il calcolatore fa così e lo dichiara. Milano è in questa situazione.

Quindici comuni nati da fusioni recenti non hanno ancora una riga negli elenchi MEF e ricevono uno 0,80% indicativo, con un avviso dedicato.

Il caso trattato è quello di un lavoratore dipendente a tempo indeterminato per l'anno intero con contratto CCNL Terziario e Commercio. Restano fuori conguagli di fine anno, premi di risultato detassati e welfare aziendale.

TFR e buoni pasto non entrano nel netto, perché non sono soldi che vedi in busta ogni mese.

Non l'ha rivisto un consulente del lavoro. È un prototipo, e il footer lo dice.

## Note tecniche e debugging

Il calcolatore è un unico file HTML: una pagina web che si apre con un doppio click, senza installare niente e senza bisogno di connessione. Dentro quel file ci sono la grafica, i dati di tutti i comuni e i calcoli. Non usa pezzi già pronti presi da altri: i grafici e la ricerca dei comuni sono scritti lì dentro.

Per i controlli ho confrontato i risultati con gli altri calcolatori online, e ho fatto prove a campione su decine di comuni diversi per verificare l'addizionale comunale.

È così che è saltato fuori il caso di Napoli. Il dato diceva 1,00%, sopra il tetto ordinario dello 0,8%, e sembrava un mio errore nella lettura del file del MEF. Sono andato a controllare la delibera ed era corretto: i comuni in squilibrio finanziario possono superare il tetto. Stessa cosa per Genova, che arriva all'1,2%.

## Fonti

Normativa

- Legge di bilancio 2026 (L. 199/2025), scaglioni e aliquote IRPEF
- artt. 12 e 13 TUIR, detrazioni per carichi di famiglia e per lavoro dipendente
- L. 207/2024, ulteriore detrazione e trattamento integrativo
- Circolare INPS n. 6 del 30 gennaio 2026, minimali, massimali e prima fascia pensionabile
- CCNL Terziario, Distribuzione e Servizi (Confcommercio), enti bilaterali e fondo sanitario

Dati locali

- [MEF, elenchi generali delle addizionali comunali](https://www1.finanze.gov.it/finanze2/dipartimentopolitichefiscali/fiscalitalocale/addirpef_newDF/download/tabella.htm), i CSV ufficiali aggiornati ogni giorno
- Portali regionali per le addizionali regionali
- ISTAT per codici e denominazioni dei comuni

Ricerca utenti

- Thread di r/ItaliaPersonalFinance, r/techcompenso e r/Italia sui calcolatori RAL → netto, circa 130 commenti letti

---

*Prototipo a scopo dimostrativo. Non sostituisce il cedolino né il parere di un professionista.*
