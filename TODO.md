# TODO — Scheda AIS Vini

## Da fare, sessione dedicata (non di fretta a fine giornata)

### Modellazione dati denominazioni (oggetto `DN`, campo `tip`)
Il campo `tip` di ogni denominazione in `DN{}` mescola sotto un'unica etichetta
generica cinque categorie normative concettualmente diverse:

1. **Menzioni tradizionali/qualitative** regolamentate dal disciplinare
   (Riserva, Superiore, Classico, Gran Selezione) — comportano requisiti
   specifici (invecchiamento minimo, resa più bassa)
2. **Colori/tipologie di prodotto** (Bianco, Rosso, Rosato)
3. **Metodi di produzione/stile** (Frizzante, Spumante, Liquoroso, Passito,
   Vendemmia tardiva, Da uve stramature)
4. **Nomi di vitigno in etichetta mono-varietale** (es. "Alghero": Torbato,
   Cagnulari, Chardonnay, Sauvignon — mescolati alle voci sopra)
5. **Sottozone geografiche** (es. "Cannonau di Sardegna": Classico Oliena,
   Classico Capo Ferrato, Classico Jerzu)

Da fare: separare in 5 campi distinti, non un array unico. Fonte per la
verifica corretta: disciplinari di produzione nazionali di ogni singola
DOP/DOCG (non la classificazione regionale usata per i vitigni — quella
è un'altra fonte, vedi sotto). Nessuna fonte/data era citata nel codice
originale per l'oggetto `DN` — va tracciata da zero, stesso problema
già trovato per `VIT`.

### Vitigni — fase 2 (rimandata volutamente)
Fase 1 completata: tabella `vitigni_sardegna` su Supabase, 108 varietà,
fonte: Regione Autonoma della Sardegna — Assessorato Agricoltura,
Allegato 1 alla Determinazione n. 6361/186 del 23/04/2020 (discende da
Delib. G.R. 27/19 dell'8/06/2004). Inizialmente 5 voci con nota di
incertezza esplicita di estrazione dal PDF (Aglianico, Ansonica, Garganega,
Marselan, Traminer Aromatico) — vedi colonna `nota_incertezza` nella
tabella.

**Aggiornamento 7/07/2026, seconda parte**: Rob ha scovato che la Regione
pubblica le determinazioni anche su sardegnaambiente.it, con l'elenco
completo delle Determinazioni successive separate dai loro allegati
(`sardegnaambiente.it/index.php?xsl=2425&c=229&s=385220&v=2&tb=13769`).
Questo ha rivelato che mancavano **Viognier** (riconosciuto con Determ.
n. 80/2627 del 17/02/2022, codice 346) e **Ciliegiolo** (Determ. n. 256
prot. 6622 del 05/04/2023, codice 062, sinonimo "Morettone") — entrambi
ora aggiunti. **Totale attuale: 110 varietà**, non più 108.

La fonte di riferimento della tabella è stata spostata alla versione più
recente e cumulativa (Allegato alla Determinazione n. 256/6622 del
05/04/2023: `sardegnaambiente.it/documenti/1_38_20230407091439.pdf`),
che contiene tutte le 110 voci in un solo documento coerente.

Verificando il contenuto su **tre versioni diverse del documento** (2020,
2022, 2023), confermato che Ansonica e Marselan hanno le stesse identiche
anomalie in ognuna — non refusi isolati mai corretti, ma caratteristiche
persistenti della fonte nel tempo:
- **Ansonica (013)**: "A" invece di "I"/"IC" in tutte e tre le versioni
- **Marselan (485)**: nessuna lettera di colore in nessuna delle tre versioni

Traminer Aromatico (238), invece, risulta ora chiaramente leggibile e
confermato ("Rs. I") in due versioni pulite del documento — nota di
incertezza rimossa, non era un problema reale.

Restano 2 voci con nota di incertezza (non più 3): Ansonica, Marselan.

Fase 2 da fare: regole di composizione per ciascuna DOP sarda (percentuale
minima del vitigno principale, soglia dei complementari ammessi) — fonte:
disciplinari nazionali pubblicati da MASAF, NON il documento regionale
usato per la fase 1 (quello dice solo "idoneo alla coltivazione in
Sardegna", non "in che percentuale in questa specifica denominazione").

Fase 3 (non ancora iniziata): collegare l'app all'uso reale di
`vitigni_sardegna` — oggi la tabella esiste su Supabase ma il codice
continua a leggere l'oggetto `VIT{}` hardcoded.

### eAmbrosia (fonte UE) — verificato, non praticabile per ora
API REST esiste (`webgate.ec.europa.eu/eambrosia-api/api/v1/geographical-indications`)
ma timeout ripetuto su ogni tentativo di accesso diretto (3 tentativi,
parametri diversi, tutti falliti, luglio 2026). Anche se accessibile, il
dato non è detto sia strutturato: un lavoro scientifico pubblicato su PMC
(novembre 2021) ha richiesto 8 mesi di estrazione manuale dai documenti
legali per ottenere un inventario strutturato dei disciplinari europei.
Non riprovare senza una ragione concreta per pensare che qualcosa sia
cambiato nel frattempo.

### Refactor identificativi (ramo `refactor-identificativi`)
In corso di test da parte di Rob al momento in cui questo file è stato
scritto — non ancora mergiato su `main`. Vedi i commit del ramo per lo
storico completo dei bug trovati e corretti durante il test dal vivo.
