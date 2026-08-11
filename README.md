# Calcolatore RAL → netto · 2026

**→ [Provalo qui](https://lorenzorusso1995-jpg.github.io/jethr-calcolatore-netto/)**

Prototipo realizzato per la selezione **Product Builder @ Jet HR**. Data una RAL, calcola il netto annuo e mensile con il dettaglio di ogni trattenuta, il calcolo passo per passo con i numeri di chi lo usa e la fonte normativa di ogni voce. Funziona anche al contrario: dal netto desiderato alla RAL da chiedere.

Tutto sta in un unico file HTML, senza dipendenze e senza build.

---

## Cosa fa

- **Dalla RAL al netto** annuo e mensile, distinguendo il mese ordinario dalla tredicesima (sulle mensilità aggiuntive non si applicano le detrazioni, quindi la 13ª netta è più bassa: mostrarle insieme sarebbe stato un errore).
- **Dal netto alla RAL**, il caso d'uso vero della negoziazione: *«che RAL devo chiedere per portare a casa 1.800 € netti?»*
- **Dettaglio riga per riga**, come un cedolino: ogni trattenuta con la sua base imponibile, la sua aliquota, il calcolo svolto e il riferimento normativo.
- **Addizionale comunale reale per tutti i 7.904 comuni italiani**, importata dagli elenchi ufficiali del MEF: aliquota unica o a scaglioni, con la relativa soglia di esenzione.
- **Addizionali regionali** per tutte le 21 tabelle (19 regioni più le province autonome di Trento e Bolzano, che deliberano separatamente).
- **Opzioni avanzate**: fondo pensione complementare, carichi familiari, agevolazioni. Nessuna è applicata di nascosto — sono tutte visibili e disattivabili.
- **Aliquota marginale effettiva**: «su 1.000 € di aumento, quanto ti resta davvero».
- Il calcolo si condivide come link: lo stato dei campi viaggia nell'URL.

---

## Come ci sono arrivato

### 1. Una busta paga vera, per capire cosa c'è davvero dentro

Prima di cercare aliquote, sono partito da una busta paga reale (marzo 2025, CCNL Commercio e Terziario, impiegato livello 3), usata come **mappa delle voci**: quali trattenute esistono, con che nomi, su quale base si calcolano. Nei calcolatori online alcune di queste voci non compaiono proprio — e sono esattamente quelle che fanno sballare il risultato di qualche decina di euro al mese.

È servita anche come banco di prova: il modello contributivo riproduce **al centesimo** il totale dei contributi di quella busta. L'agevolazione presente sul cedolino (regime impatriati) l'ho riconosciuta ed **esclusa** dal caso standard, rendendola un'opzione attivabile a parte.

### 2. Le fonti primarie, non i riassunti

Per ogni voce sono partito dalla norma: Legge di bilancio 2026 (L. 199/2025) per gli scaglioni IRPEF, artt. 12 e 13 TUIR per le detrazioni, L. 207/2024 per il taglio del cuneo, circolare INPS n. 6 del 30 gennaio 2026 per minimali, massimali e prima fascia pensionabile. Gli articoli dei siti fiscali sono serviti solo per orientarmi: quando un riassunto e la norma dicevano cose diverse, ha vinto la norma.

La regola che mi sono dato: **un numero entra nel modello solo se ha accanto il suo riferimento normativo.** È il motivo per cui ogni riga del dettaglio, nell'app, cita la propria fonte.

Per le addizionali comunali la fonte è il MEF, che pubblica gli **elenchi generali in CSV** di tutti i comuni, aggiornati quotidianamente: è lo stesso dato che usano i software paghe.

### 3. Reddit, per capire perché i calcolatori esistenti vengono bocciati

Prima di disegnare l'interfaccia ho letto i thread di r/ItaliaPersonalFinance, r/techcompenso e r/Italia in cui gli autori di due calcolatori concorrenti hanno chiesto feedback pubblico — circa 130 commenti in tutto, tra critiche, bug segnalati e richieste.

Ne è uscito un quadro molto netto:

- **Il metro di giudizio è uno solo: il confronto con il proprio cedolino.** *«Usabilità ottima, accuratezza bassa»* è il commento a un tool che sbagliava di 120 € al mese. Un calcolatore bello che sbaglia viene bocciato, uno spartano che azzecca viene promosso.
- **Tre bug ricorrenti**, ammessi dagli autori stessi: massimale INPS ignorato sulle RAL alte, le due direzioni (lordo→netto e netto→lordo) che non tornano, bonus applicati d'ufficio a chi non ne ha diritto.
- **L'addizionale comunale fissata a 0,80% è il difetto più citato**: *«ci sono comuni allo 0% e altri sopra l'1%»*.
- **Diffidenza verso i tool «fatti con l'AI»**: interfacce tutte uguali, emoji usate come icone, link rotti nel footer.
- Richieste ricorrenti: fondo pensione deducibile, numero di mensilità, campo digitabile invece dello slider (*«ho dovuto cliccare 19 volte +1k per salire da 15k a 34k»*), **dark mode**, e la possibilità di vedere la formula sotto il risultato.

### 4. I competitor, per fissare lo standard e controllare i numeri

Ho confrontato il modello con i calcolatori esistenti — **stipendee.it**, indicato dalla community come il riferimento, **calcolastipendionetto.it**, **tuttocalcolato.it**, **calcolonetto.it** — e con i calcolatori pubblicati da **Jet HR**. Su due fronti: verificare che i miei numeri fossero in linea, e osservare come presentano il risultato, cosa mostrano e cosa nascondono. Da lì ho preso quello che è ormai uno standard atteso (le card di sintesi in alto, la ripartizione del lordo, le mensilità) e ho scelto dove provare a fare meglio: la trasparenza del calcolo e la precisione del dato comunale.

---

## Le decisioni che ne sono uscite

| Cosa ho osservato | Cosa ho deciso |
|---|---|
| L'accuratezza è l'unico metro di giudizio | Prima il motore, poi la trasparenza, l'interfaccia per ultima. Nessuna scorciatoia sulle voci minori (FIS, COVELCO, EBINTER, EST): sono quelle che spostano le decine di euro |
| L'aliquota comunale fissa è il difetto più citato | Importate le aliquote **reali di tutti i 7.904 comuni** dagli elenchi MEF, con soglie di esenzione e scaglioni progressivi, invece del solito 0,80% medio |
| «Vorrei vedere la formula sottostante» | Ogni riga del risultato si apre sul **calcolo passo per passo con i numeri dell'utente** e cita la norma. Non è una spiegazione scritta a parte: la genera lo stesso motore che calcola, quindi non può divergere |
| Le due direzioni che non tornano | Il netto→RAL **non è una seconda formula**: è una bisezione sulla stessa funzione del RAL→netto. Le due direzioni coincidono per costruzione |
| Massimale INPS ignorato | Modellati sia la prima fascia pensionabile (56.224 €, oltre la quale scatta l'1% aggiuntivo) sia il massimale (122.295 €) |
| Bonus applicati d'ufficio | Trattamento integrativo e taglio del cuneo sono **opzioni visibili e disattivabili**, ognuna con le condizioni di spettanza calcolate e spiegate |
| Dark mode richiesta più volte | Implementata, con il tema che si ricorda tra una visita e l'altra |
| Slider al posto del campo numerico | Campo digitabile, sempre |
| Diffidenza verso i tool «AI slop» | Nessuna emoji usata come icona, nessuna sezione decorativa, nessun link rotto. Il design riprende il sistema visivo di Jet HR (palette, tipografia, raggi, elevazione) invece di inventarne uno generico |
| «Quello che calcoli oggi, fra 6 mesi non vale più» | Anno fiscale e data di aggiornamento dichiarati in testa alla pagina, provenienza del dato dichiarata comune per comune |

---

## Come funziona il calcolo

La catena, nell'ordine:

```
RAL
 − contributi INPS a carico del dipendente   (IVS 9,19% + 1% oltre la prima fascia,
                                              CIGS, FIS, enti bilaterali; con massimale)
 − fondo pensione complementare              (deducibile fino a 5.164,57 €)
 = imponibile IRPEF
 − IRPEF netta                               (scaglioni 23% / 33% / 43%, meno le
                                              detrazioni da lavoro, cuneo, familiari;
                                              mai negativa)
 − addizionale regionale e comunale          (sull'imponibile, non sull'IRPEF;
                                              azzerate in caso di incapienza)
 + trattamento integrativo e somma esente    (sono crediti: si sommano, non si sottraggono)
 = NETTO ANNUO
```

Il cuore è una funzione pura: dato l'input restituisce **sia i totali sia la spiegazione di come ci è arrivata**, come un'unica struttura. Ogni riga del dettaglio nell'app è quella struttura renderizzata — non esistono due versioni del calcolo che possono andare fuori sincrono.

Alcuni casi limite sono modellati esplicitamente, perché sono quelli in cui gli altri sbagliano:

- **Incapienza**: se le detrazioni azzerano l'IRPEF, le addizionali non sono dovute — e l'app lo spiega, invece di mostrare zero in silenzio.
- **Soglia secca contro franchigia**: a Milano l'esenzione fino a 23.000 € è una soglia, non una franchigia. A 22.999 € non si paga nulla, a 23.001 € si paga lo 0,80% sull'**intero** imponibile.
- **Scaglioni comunali progressivi**: nei comuni multialiquota ogni aliquota colpisce solo la sua fascia di reddito, come nell'IRPEF.
- **Tredicesima e quattordicesima**: non cambiano il netto annuo, cambiano come viene distribuito.

---

## Come l'ho verificato

- **Contro la busta paga reale**: il totale dei contributi torna al centesimo.
- **I bug dei concorrenti trasformati in casi di test**: massimale INPS, coerenza tra le due direzioni, bonus non dovuti.
- **Tre batterie di test** eseguite sulla pagina vera nel browser, con controlli di regressione dopo ogni modifica importante: Milano, Roma, Napoli, Forlì, Cagliari, comuni di recente fusione, RAL a 200.000 €, campo vuoto, round-trip netto→RAL→netto.
- **Controlli automatici sull'import dei dati**: il dato MEF di Milano deve coincidere con quello che avevo verificato a mano sulla delibera — e coincide.

Un esempio del tipo di controllo che ha pagato: il parser restituiva Napoli all'1,00% e Genova fino all'1,2%, sopra il tetto ordinario dello 0,8%. Sembrava un bug. Verificando la riga grezza del CSV e la delibera, erano le maggiorazioni consentite ai comuni in squilibrio finanziario: il numero strano era il dato giusto. Diffidare dei propri numeri e ricontrollarli alla fonte fa parte del lavoro.

---

## Limiti, dichiarati

Preferisco dirli qui, e infatti l'app li dice all'utente:

1. **Esenzioni per categoria non modellate.** Circa 600 delibere comunali prevedono esenzioni legate a categorie di reddito o platee specifiche (pensionati, fasce ISEE): sono testo libero, non modellabili. Il calcolatore le segnala comune per comune e calcola il caso generale.
2. **Comuni senza delibera 2026.** Dove manca, resta in vigore l'ultima delibera utile — così prevede la norma e così assume il calcolatore, dichiarandolo. Milano è in questa situazione.
3. **15 comuni nati da fusioni recenti** non hanno ancora una riga negli elenchi MEF: ricevono lo 0,80% indicativo, con avviso dedicato.
4. **Caso standard**: lavoro dipendente a tempo indeterminato, anno intero. Niente conguagli di fine anno, premi di risultato detassati o welfare aziendale.
5. **TFR e buoni pasto restano fuori dal netto**, perché non sono liquidità corrente in busta.
6. **Non è stato rivisto da un consulente del lavoro.** È un prototipo, e il footer dell'app lo dice.

---

## Note tecniche

**Un solo file, zero dipendenze, zero build.** Stile, markup, dati e logica vivono tutti in `index.html`: si apre con un doppio click, si pubblica copiando un file, si legge dall'inizio alla fine. Per un prototipo il cui scopo è dimostrare di avere il controllo delle logiche, ogni strato di tooling in mezzo sarebbe stato un costo senza beneficio.

Niente librerie nemmeno dove sarebbe stato comodo: il grafico a ciambella e il diagramma di flusso sono SVG generati a mano, e la ricerca dei comuni è una combobox costruita sul pattern ARIA, con filtro insensibile agli accenti (scrivendo «forli» si trova Forlì) e navigazione da tastiera.

I dati dei 7.904 comuni sono compressi in un micro-formato testuale (~200 KB) incorporato nella pagina, così il file resta uno solo e funziona anche senza rete. L'import dagli elenchi MEF è uno script riproducibile, non un lavoro manuale: si rilancia quando il MEF aggiorna.

---

## Fonti principali

**Normativa**
- Legge di bilancio 2026 (L. 199/2025) — scaglioni e aliquote IRPEF
- artt. 12 e 13 TUIR — detrazioni per carichi di famiglia e per lavoro dipendente
- L. 207/2024 — ulteriore detrazione e trattamento integrativo
- Circolare INPS n. 6 del 30 gennaio 2026 — minimali, massimali, prima fascia pensionabile
- CCNL Terziario, Distribuzione e Servizi (Confcommercio) — enti bilaterali e fondo sanitario

**Dati locali**
- [MEF — elenchi generali delle addizionali comunali](https://www1.finanze.gov.it/finanze2/dipartimentopolitichefiscali/fiscalitalocale/addirpef_newDF/download/tabella.htm) (CSV ufficiali, aggiornamento quotidiano)
- Portali regionali per le addizionali regionali
- ISTAT — codici e denominazioni dei comuni

**Ricerca utenti**
- Thread di r/ItaliaPersonalFinance, r/techcompenso e r/Italia sui calcolatori RAL→netto (~130 commenti analizzati)

---

*Prototipo a scopo dimostrativo: non sostituisce il cedolino né la consulenza di un professionista.*
