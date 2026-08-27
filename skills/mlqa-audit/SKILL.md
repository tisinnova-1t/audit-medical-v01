---
name: mlqa-audit
description: >
  This skill should be used when the user uploads fascicolo documents for an ISP insurance claim
  and asks for "audit MLQA", "caso MLQA", "audit perizia", "valuta la perizia VML", "C [number]"
  referring to an MLQA training case, or presents a CTC scheda for comparison after a previously
  completed MLQA audit. Also triggers on "perizia ISP", "valutazione VML", "scoring MLQA".
metadata:
  version: "0.1.0"
  author: "TIS Company"
  training_basis: "50 casi ISP (C1-C50), confronti CTC Atena"
---

# MLQA Audit ISP — Procedura Operativa

## Scope e vincolo fondamentale

Auditare ESCLUSIVAMENTE la qualità della perizia medico-legale (VML) ISP-panel.
**Non valutare il lavoro del liquidatore.** Tutte le criticità sono framed come problemi di qualità VML.
Non inserire mai sezioni "NOTE PER IL LIQUIDATORE".

## Pre-requisiti obbligatori

Prima di qualsiasi scoring, leggere con il tool Read:
1. `references/regole.md` — tutte le regole operative R8–R36 con trigger e calibrazioni
2. `references/scoring.md` — algoritmo pesi, redistribuzione E=NP, scala CTC/Atena
3. `references/prodotti.md` — differenziazioni per tipo polizza (RCA, P.INF, CPI)

## Fase 1 — R31: Identificare la perizia da auditare

La perizia da auditare è SEMPRE quella di uno studio ISP-panel: **MEDEXPERT SRL**, **ProntoMedital**, **Assiservices**. In alternativa: una fiduciaria incaricata direttamente da ISP.

**Procedura:**
1. Scansionare tutti i documenti caricati cercando intestazione ISP-panel o campi strutturati (IP INAIL/ANIA, IT giorni, spese, nesso causale).
2. Se un file ha "ok" nel nome: verificarne il contenuto — può essere una RESTITUZIONE NEGATIVA (visita non effettuata), non una perizia completata. In tal caso non è la perizia da auditare.
3. Se esistono più perizie: quella ISP-panel è la perizia da auditare; le altre sono perizie di parte → Fase E.
4. Se nessuna perizia ISP-panel trovata: dichiarare il caso non auditabile.

## Fase 2 — Lettura fascicolo

Estrarre dai documenti caricati:
- **Sinistro:** numero, data, prodotto/polizza, tipo gestione
- **Periziando:** nome, sesso, età, professione, residenza
- **Dinamica:** modalità evento, 118/Polizia Locale/cinture allacciate
- **Iter clinico:** PS (con orari se disponibili), visite specialistiche, imaging (Rx, RMN, eco), terapia, certificato di guarigione
- **EO VML:** dettagli esame obiettivo, Romberg, test specifici
- **Quantificazione:** IT (ITT + ITP per %), IP, spese mediche
- **Preesistenze:** qualsiasi patologia pregressa nell'imaging o anamnesi
- **Perizia di parte:** se presente, annotare per Fase E

Per PDF con testo selezionabile: usare tool Read. Per PDF solo scansione: bash con PyMuPDF (fitz).

## Fase 3 — Scoring fasi A–R

Assegnare voto 0–100 per ogni fase. Applicare le regole da `references/regole.md` e le calibrazioni da `references/prodotti.md`.

### A — Completezza
Verificare: documentazione clinica elencata completamente; anamnesi remota E prossima presenti; privacy/consenso informato; dinamica descritta; presidi di sicurezza (cinture/casco) indicati; spese elencate con importi e congruità espressa. Sezione spese completamente vuota con fatture in fascicolo = MODERATA (non LIEVE).

### B — Metodologia
Verificare: nesso causale evento→lesioni argomentato; nesso lesioni→menomazioni argomentato; preesistenze identificate e commentate (R16); per P.INF con evento lavorativo: verifica INAIL (R35); timing visita adeguato (R35 per ustioni/cicatrici: <12 mesi = GRAVE); coerenza interna del documento; nessuna incoerenza dinamica non commentata.

### C — Prestazione
Verificare: visita diretta effettuata; EO completo per il/i distretto/i; arto dominante indicato per arto superiore/inferiore (R21); qualità misurazioni EO (vedere R10 e affinamenti in regole.md).

### D — Danno biologico
Verificare: IT biologica coerente con decorso clinico; IP coerente con la tabella applicabile (INAIL 1965 o ANIA — verificare quale prevede la polizza); preesistenza riflessa nell'IP; garanzia Fratture valorizzata se applicabile (R33); regole specifiche: R30 (cicatrice INAIL), R36 (polso arto dom. + osteosintesi), R29 (tendini >50aa). Corollario R20: se B è GI per nesso controverso → D non può superare 40.

### E — Analisi perizia di parte
- **Nessuna perizia di parte:** E = NP → redistribuire i pesi (dividere ciascun peso rimanente per 0.95).
- **Perizia di parte presente:** mera registrazione dati = SUFF/DISC (non INSUFF); confutazione argomentata voce per voce = BUONO/OTTIMO; assenza totale di analisi = INSUFF/GI.

### R — Robustezza
Verificare: note riservate presenti e pertinenti (R19); elementi di bonarietà segnalati (accesso autonomo PS, Romberg distraibile, ecc.); coerenza interna; in RCA: nota su preesistenza rilevante se IP superiore al minimo.

## Fase 4 — Criticità

Per ogni criticità rilevata produrre:
- **Fase:** A / B / C / D / E / R
- **Descrizione:** specifica e documentata (citare referto o atto che evidenzia il problema)
- **Livello:** GRAVE / MODERATA / LIEVE
- **Motivazione:** con riferimento alla regola attiva (es. "R16: preesistenza non menzionata")

## Fase 5 — Generazione report .docx

Leggere `references/report-template.md` per il pattern Node.js.

Generare via bash in `/tmp`, poi copiare in `/sessions/gallant-trusting-curie/mnt/outputs/MLQA_C[N]_[Cognome].docx`.

**Struttura report (max 2 pagine):**
- **Pagina 1:** Tabella intestazione (metadati caso) + Tabella scoring (6 fasi + voto finale + fascia) + riga voto finale bold
- **Pagina 2:** Tabella criticità (fase | descrizione | livello con colore | motivazione MLQA) + Tabella confronto CTC (da popolare dopo ricezione scheda)

## Fase 6 — Confronto CTC (quando arriva la scheda)

1. Estrarre dalla scheda PDF: voto software (x.xx/10 → ×10 per scala 0–100), giudizio calcolato, giudizio controllore, voti per voce, note consulente.
2. Calcolare Δ = voto MLQA − voto CTC.
3. Identificare discrepanze fase per fase.
4. Applicare learnings: verificare se errore sistematico (bias RCA, sovrapenalizzazione B, ecc.).
5. Rigenerare il report con sezione confronto CTC popolata.

## Fase 7 — Aggiornamento tracker

Aggiornare `MLQA_Training_Tracker.md` nella cartella outputs:
- Aggiungere/aggiornare riga caso in "Registro Confronti CTC"
- Aggiornare CR1 (streak Δ<5), CR2 (streak no nuove regole)
- Aggiungere eventuali nuove regole con descrizione
- Aggiornare "Copertura Tipologie Polizza" e "Copertura Distretti Anatomici"
- Aggiornare footer con data e sommario caso
