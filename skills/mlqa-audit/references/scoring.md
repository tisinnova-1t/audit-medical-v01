# Algoritmo di Scoring MLQA

## Pesi standard (6 fasi)

| Fase | Descrizione         | Peso  |
|------|---------------------|-------|
| A    | Completezza         |  5%   |
| B    | Metodologia         | 35%   |
| C    | Prestazione         | 15%   |
| D    | Danno biologico     | 35%   |
| E    | Analisi parte       |  5%   |
| R    | Robustezza          |  5%   |

## Redistribuzione con E = N/P

Quando non esiste perizia di parte (E = NP), i pesi delle 5 fasi rimanenti vengono riscalati dividendo ciascuno per 0.95 (che corrisponde a 1 − 5%):

| Fase | Peso ridistribuito |
|------|--------------------|
| A    | 5/95  = 5.263%     |
| B    | 35/95 = 36.842%    |
| C    | 15/95 = 15.789%    |
| D    | 35/95 = 36.842%    |
| R    | 5/95  = 5.263%     |

**Formula voto finale:** Σ (voto_fase × peso_fase_ridistribuito / 100)

## Scala fasce (CTC/Atena — da C43 in poi)

| Punteggio | Fascia                  | Sigla |
|-----------|-------------------------|-------|
| 90–100    | OTTIMO                  | OTT   |
| 80–89     | BUONO                   | BUONO |
| 70–79     | DISCRETO                | DISC  |
| 66–69     | PIÙ CHE SUFFICIENTE     | PCS   |
| 60–65     | SUFFICIENTE             | SUFF  |
| 56–59     | QUASI SUFFICIENTE       | QS    |
| 40–55     | INSUFFICIENTE           | INSUFF|
| 0–39      | GRAVEMENTE INSUFFICIENTE| GI    |

> Nota: la scala è stata corretta il 26/08/2026. Per i casi precedenti a C43, usare la scala vecchia (GI 0–49, INSUFF 50–59, QS 60–69, SUFF 70–79, ecc.) per mantenere coerenza storica del tracker.

## Lettura scheda CTC

Il voto CTC è espresso in formato x.xx/10 (es. 7.61).
Conversione in scala 0–100: moltiplicare per 10 (es. 7.61 → 76.1).

Il giudizio del controllore può divergere dal voto software:
- Controllore più indulgente del software → fascia più alta del voto numerico.
- Usare il voto software per il confronto numerico Δ.
- Annotare la divergenza tra controllore e software nel tracker.

## Linee guida di scoring per fase

### A — Completezza (0–100)
- 80–100: tutti i campi compilati, documentazione completa, spese con fatture e giudizio congruità, privacy presente.
- 60–79: qualche campo non compilato senza impatto sostanziale (DISC).
- 40–59: sezione spese completamente vuota con fatture in fascicolo; anamnesi remota errata o mancante.
- <40: documento gravemente incompleto, dinamica assente, nessun documento clinico elencato.

### B — Metodologia (0–100)
- 80–100: nesso causale argomentato con riferimenti a letteratura/criteri medico-legali; preesistenze analizzate; conclusione ben motivata.
- 66–79: motivazione sintetica ma corretta; in P.INF con conclusione confermata da CMC = DISCRETO.
- 56–65: motivazione molto sintetica o con lacune parziali; nesso affermato ma non del tutto argomentato.
- 40–55: nesso causale dichiarato senza motivazione specifica; preesistenze ignorate nonostante evidenza imaging; R25/R28 non applicati.
- <40: nesso causale assente, sbagliato, o contraddittorio con la documentazione.

### C — Prestazione (0–100)
- 80–100: visita diretta, EO completo con misure quantitative (gradi goniometrici, cm perimetria), arto dominante indicato.
- 66–79: EO con alcune misure qualitative ma con altre quantitative; in RCA accettabile per R18.
- 56–65: EO prevalentemente qualitativo senza alcuna misura quantitativa.
- <56: EO assente o gravemente incompleto; distretto principale non esaminato.

### D — Danno biologico (0–100)
- 80–100: IT biologicamente congruente; IP corretta con tabella polizza; preesistenza pesata; garanzie specifiche valorizzate; in RCA intermedia baseline ≥80.
- 66–79: IP nella norma con piccole imprecisioni; IT accettabile.
- 56–65: IP ai limiti del range accettabile; una garanzia specifica non valorizzata.
- 40–55: IP sistematicamente errata (sovrastimata o sottostimata); struttura IT incompatibile con polizza; R36/R30/R33 violate.
- <40: IP abnorme; R20 attiva (nesso GI → D non può superare 40).

### E — Analisi perizia di parte (0–100, solo se pertinente)
- 80–100: confutazione argomentata voce per voce della perizia di parte.
- 60–79: analisi parziale; dati riportati con commento parziale.
- 40–59: mera registrazione dati (nominativo, IP, IT) senza analisi critica — SUFF/DISC, non INSUFF.
- <40: dati perizia di parte assenti o gravemente incompleti.

### R — Robustezza (0–100)
- 80–100: note riservate complete e pertinenti; elementi di bonarietà segnalati; documento internamente coerente.
- 60–79: note riservate presenti ma incomplete; qualche elemento di bonarietà non segnalato.
- 40–59: note riservate assenti in caso con elementi rilevanti; Romberg distraibile non segnalato quando IP superiore al minimo.
- <40: documento internamente incoerente; elementi di bonarietà sistematicamente ignorati.
