---
description: Avvia audit MLQA su un fascicolo sinistro ISP caricato
allowed-tools: Read, Write, Edit, Bash, Grep, Glob
argument-hint: [numero-caso]
---

Esegui un audit MLQA (Medical-Legal Quality Assurance) completo sul fascicolo sinistro ISP caricato in questa conversazione, riferito al caso $1.

Segui integralmente la procedura descritta in `${CLAUDE_PLUGIN_ROOT}/skills/mlqa-audit/SKILL.md`:

1. Leggi prima le tre reference obbligatorie: `references/regole.md` (regole R8-R36), `references/scoring.md` (algoritmo pesi e scala CTC), `references/prodotti.md` (calibrazioni per tipo polizza).
2. Applica R31 per identificare la perizia ISP-panel da auditare (MEDEXPERT/ProntoMedital/Assiservices) tra i documenti caricati.
3. Estrai i dati del fascicolo (sinistro, periziando, dinamica, iter clinico, EO, quantificazione, preesistenze).
4. Assegna il punteggio alle 6 fasi (A-Completezza, B-Metodologia, C-Prestazione, D-Danno biologico, E-Analisi parte, R-Robustezza), applicando le regole pertinenti e, se E = N/P, ridistribuendo i pesi secondo `references/scoring.md`.
5. Elenca le criticità rilevate con fase, livello (GRAVE/MODERATA/LIEVE) e motivazione.
6. Genera il report .docx (max 2 pagine) seguendo `references/report-template.md`.
7. Se nella conversazione è presente anche una scheda CTC per questo caso, esegui il confronto (Δ, discrepanze, learnings) e aggiorna il report.
8. Se richiesto, aggiorna il tracker del progetto secondo la Fase 7 della skill.

Non introdurre valutazioni sull'operato del liquidatore: l'audit riguarda esclusivamente la qualità della perizia medico-legale.
