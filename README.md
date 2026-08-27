# audit-medical-v01 — Plugin MLQA per Perizie ISP

Audit strutturato di qualità su perizie medico-legali ISP (Intesa Sanpaolo Assicura).
Sviluppato su 50 casi di training con confronto CTC Atena.

## Cosa fa

- **Identifica la perizia da auditare** nel fascicolo sinistro (R31: MEDEXPERT / ProntoMedital / Assiservices)
- **Scoring in 6 fasi** (A Completezza, B Metodologia, C Prestazione, D Danno biologico, E Analisi parte, R Robustezza)
- **Redistribuzione automatica pesi** quando E = N/P (nessuna perizia di parte)
- **Genera report .docx** (max 2 pagine: scoring + criticità + confronto CTC)
- **Confronto CTC Atena** quando arriva la scheda audit: calcolo Δ, analisi discrepanze, lesson learned
- **Aggiornamento tracker** (MLQA_Training_Tracker.md): CR1/CR2/CR3/CR4, copertura polizze e distretti

## Componenti

| Componente | Percorso                              | Funzione |
|------------|----------------------------------------|----------|
| Skill      | `skills/mlqa-audit/SKILL.md`          | Procedura completa, attivata automaticamente al caricamento di un fascicolo o alla menzione di "audit MLQA" |
| Comando    | `commands/audit-mlqa.md`              | Scorciatoia esplicita `/audit-mlqa [numero-caso]` per forzare l'esecuzione della procedura |
| Reference  | `skills/mlqa-audit/references/*.md`   | Base di conoscenza: regole R8-R36, algoritmo scoring, calibrazioni per prodotto, template report |

## Come si usa

**Automatico:** caricare i documenti del fascicolo sinistro e scrivere il numero del caso (es. "C 51") oppure "audit MLQA" o "valuta la perizia" — la skill si attiva da sola.

**Esplicito:** digitare `/audit-mlqa 51` dopo aver caricato i documenti del fascicolo.

Quando arriva la scheda CTC: caricarla senza altro testo — viene riconosciuta e usata per aggiornare report e tracker.

## Struttura del plugin

```
audit-medical-v01/
├── .claude-plugin/
│   └── plugin.json               Manifest plugin
├── commands/
│   └── audit-mlqa.md             Comando slash esplicito
├── skills/mlqa-audit/
│   ├── SKILL.md                  Procedura operativa (R31, fasi A-R, report, CTC, tracker)
│   └── references/
│       ├── regole.md             Tutte le regole R8-R36 con trigger e calibrazioni
│       ├── scoring.md            Algoritmo pesi, redistribuzione E=NP, scala CTC
│       ├── prodotti.md           Calibrazioni per tipo polizza (RCA, P.INF, CPI, Pastore)
│       └── report-template.md    Pattern Node.js per generazione .docx
├── marketplace.json               Auto-listing per installazione come marketplace interno
└── README.md
```

## Distribuzione interna (marketplace)

Questo repo si auto-registra come marketplace tramite `marketplace.json` alla radice, in modo da poter essere aggiunto direttamente come fonte:

```
/plugin marketplace add <url-repo-git>
/plugin install audit-medical-v01@tis-audit-medical
```

> Nota tecnica: la documentazione ufficiale Claude Code colloca `marketplace.json` dentro `.claude-plugin/marketplace.json`. Questo repo replica invece la convenzione già in uso in altri repo TIS (marketplace.json alla radice). Se l'installazione tramite marketplace non viene riconosciuta, spostare il file in `.claude-plugin/marketplace.json` e aggiornare i comandi di installazione di conseguenza.

## Prodotti coperti

| Tipo    | Prodotti                                              |
|---------|-------------------------------------------------------|
| P.INF   | Ombrello2018, XME Protezione, ISP Infortuni, Microtek |
| P.INF   | Convenzione Antonio Pastore (dirigenti, tabella ANIA) |
| RCA     | Viaggia Con Me, Moto ConMe, Spimimoto (CARD/non-CARD) |
| CPI     | Proteggimutuo (malattia, invalidità)                  |

## Dipendenze runtime

- Node.js + pacchetto `npm install docx` (installato automaticamente in /tmp)
- Python + PyMuPDF (`pip install pymupdf`) per OCR su PDF scansionati
- Cartella outputs accessibile a `/sessions/.../mnt/outputs/`

## Regole attive (sintesi)

| Regola | Trigger principale                                        |
|--------|-------------------------------------------------------------|
| R10    | EO qualitativo senza misure goniometriche                 |
| R16    | Preesistenze non menzionate in anamnesi                   |
| R18    | Standard differenziati per perizie RCA                    |
| R19    | Note riservate pertinenti in RCA definitiva                |
| R21    | Arto dominante obbligatorio                                |
| R22    | Struttura IT incompatibile con polizza                     |
| R25    | Atto quotidiano + RMN degenerativa                          |
| R27    | CPI/malattia: principio malattia unica                      |
| R28    | Preesistenza esplicita nell'imaging valutata come attuale   |
| R29    | Tendini/cuffia in assicurato >50aa                          |
| R30    | Cicatrice facciale con tabella INAIL 1965                   |
| R31    | Identificazione perizia ISP-panel da auditare                |
| R32    | RCA CARD: campo dinamica vuoto = GRAVE in B                 |
| R33    | Garanzia Fratture: lesione fratturativa non trattata        |
| R34    | RCA CARD diniego tecnico: separazione perizia medica/VDV     |
| R35    | Infortunio lavorativo: verifica INAIL + timing ustioni       |
| R36    | Frattura polso arto dominante con osteosintesi               |

Versione completa di tutte le regole: `skills/mlqa-audit/references/regole.md`
