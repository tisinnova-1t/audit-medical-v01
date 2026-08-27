# Regole Operative MLQA — R8–R36

Base di conoscenza estratta da 50 casi ISP con confronto CTC Atena.
Ogni regola include: trigger, fasi impattate, severità, descrizione operativa.

---

## R8 — PDF immagine (OCR)
**Trigger:** documento PDF non selezionabile (scansione).
**Azione:** usare PyMuPDF (fitz) via bash per estrarre testo. Se fallisce, usare OCR manuale o segnalare.

---

## R9 — Patologia neurologica/sistemica preesistente
**Trigger:** periziando con patologia neurologica cronica o sistemica significativa (es. leucoencefalopatia vascolare, diabete, osteoporosi).
**Fasi:** B, D.
**In RCA:** penalizzazione degradata da GRAVE a MODERATA, salvo IP evidentemente errata per omissione.
**In P.INF:** piena penalizzazione se substrato non valutato.

---

## R10 — EO articolare senza misure goniometriche
**Trigger:** esame obiettivo descrive limitazioni articolari in termini qualitativi (es. "circa 1/3", "pochi gradi", "limitata nei gradi estremi") senza misure in gradi.
**Fasi:** C, (cascata D).
**Severità:**
- **LIEVE:** se il quadro EO include ALTRE misure quantitative (perimetria in cm, ipomiotrofia in cm, asimmetrie misurate) → la mancanza di gradi goniometrici è parzialmente compensata.
- **MODERATA:** se IP > 0% e nessuna misura quantitativa di alcun tipo.
- **GRAVE:** se vaghezza manifesta incompatibile con IP proposta (es. "circa 1/3 di limitazione" → IP 8% non verificabile → cascata D).
**In RCA (R18):** EO qualitativo più tollerato; standard MODERATA solo se IP evidentemente non supportata.

---

## R11 — IP su lesione evolutiva senza imaging
**Trigger:** IP assegnata su lesione che richiede imaging (frattura, lesione tendinea, meniscale) senza che il VML abbia visto o citato alcun referto radiologico.
**Fasi:** B, D.
**Severità:** GRAVE in entrambe le fasi.

---

## R12 — Perizia di diniego
**Trigger:** caso in cui ISP ha già rifiutato il sinistro e la perizia VML supporta il diniego.
**Fasi:** B.
**Nota:** il VML in una perizia di diniego deve argomentare il nesso negativo. Penalizzare B solo per omissioni medico-legali, non per il fatto che la conclusione sia negativa.

---

## R13 — Codice fiscale mancante o errato
**Trigger:** CF del periziando assente o non corrispondente ai dati anagrafici.
**Fasi:** A.
**Severità:** LIEVE (omissione formale).

---

## R14 — Allegazione documenti
**Trigger:** documenti citati nel testo della perizia non allegati al fascicolo, o viceversa.
**Fasi:** A.
**Severità:** LIEVE–MODERATA a seconda dell'impatto sul contenuto.

---

## R15 — Diabete + lesione tendinea
**Trigger:** periziando diabetico con lesione tendinea (cuffia, tendine d'Achille, ecc.).
**Fasi:** B, D.
**Azione:** il VML deve valutare esplicitamente il contributo del diabete alla fragilità tendinea. Mancata menzione = MODERATA in B.

---

## R16 — Preesistenze senza commento
**Trigger:** imaging acquisito dal VML riporta patologia degenerativa/pregressa E il VML non la menziona in anamnesi remota né la commenta nel giudizio.
**Fasi:** B, (A).
**Severità base:** MODERATA in B.
**Affinamento RCA (da C50):** se IP è già al valore minimo tabellare, il CTC considera le preesistenze implicitamente pesate nella quantificazione → severità degradata a LIEVE in B (non MODERATA). La MODERATA si applica quando l'IP è sovrastimata nonostante preesistenza evidente.
**Cascata D:** se la preesistenza avrebbe dovuto ridurre l'IP ma non l'ha ridotta → INSUFFICIENTE in D.

---

## R17 — Incompatibilità dinamica/lesioni grave
**Trigger:** le lesioni refertate sono incompatibili con la dinamica descritta (es. trauma a bassa energia → lesioni pluriframmentarie).
**Fasi:** B.
**Severità:** GRAVE. Il VML deve commentare o segnalare nelle note riservate.

---

## R18 — Standard differenziati per perizie RCA
**Trigger:** tipo gestione RCA (CARD o non-CARD), qualunque prodotto.
**Principio:** in RCA le perizie seguono standard meno stringenti rispetto a P.INF.
**Calibrazioni specifiche:**
- EO qualitativo più tollerato (vedi R10 affinamento).
- B: motivazione sintetica del nesso accettabile per micro-permanente (<5% IP).
- D baseline ≥80 per perizie INTERMEDIE (non definitive).
- Preesistenze degenerative in soggetto anziano con IP al minimo: gestite implicitamente (vedi R16 affinamento).
- Note riservate: sempre pertinenti in RCA definitiva per elementi che potrebbero impattare liquidazione (R19).

---

## R19 — Note riservate pertinenti in RCA definitiva
**Trigger:** perizia RCA definitiva con elementi clinici o di bonarietà significativi.
**Fasi:** R.
**Elementi che richiedono note riservate:**
- Oscillazioni Romberg che regrediscono con manovre distraenti (componente funzionale).
- Accesso autonomo in PS (senza 118) per trauma grave dichiarato.
- Abbandono PS contro parere medico.
- Preesistenza rilevante quando IP è superiore al minimo.
- Utilizzo effettivo del casco non verificato (RCA moto).
**Affinamento RCA (da C50):** Romberg distraibile NON è una lacuna critica delle note riservate se l'IP è già al minimo tabellare — il danno non potrebbe essere ridotto ulteriormente.
**Severità:** MODERATA per elemento non segnalato; LIEVE se IP già al minimo e l'elemento non avrebbe cambiato la quantificazione.

---

## R20 — Coerenza interna B→D: nesso controverso
**Trigger:** nesso causale evento→lesioni gravemente insufficiente (B=GI) per assenza strutturale di supporto (no PS, latenza >48h, imaging degenerativo, no TC, nessun meccanismo traumatico tipico).
**Fasi:** D.
**Regola:** D non può superare 40 quando B è GI per nesso causale controverso. La quantificazione del danno è inficiata se il nesso non è dimostrato.

---

## R21 — Arto dominante obbligatorio
**Trigger:** lesione che interessa arto superiore o inferiore (polso, gomito, spalla, mano, dita, ginocchio, caviglia, anca).
**Fasi:** C, B.
**Azione:** il VML DEVE indicare quale arto è dominante. L'arto dominante impatta la voce tabellare INAIL e il valore IP.
**Severità:** MODERATA in C + B se mancante.

---

## R22 — Struttura temporanea coerente con condizioni di polizza
**Trigger:** IT biologica strutturata come ITP 75%/50%/25% in una polizza che prevede solo ITT + ITP 50% (o altra struttura specifica).
**Fasi:** D.
**Severità:** MODERATA — il CTC penalizza come "insufficiente" la voce quantificazione anche se l'impatto liquidativo è nullo.
**Nota:** verificare SEMPRE le condizioni di polizza per la struttura IT prevista prima di giudicare.

---

## R23 — Politrauma: orari PS
**Trigger:** periziando con lesioni a 3+ distretti che ha effettuato accesso PS.
**Fasi:** B, (R).
**Azione:** verificare orari accesso/uscita PS dagli allegati scansionati. Permanenza <30 min con politrauma multiplo + presidio = red flag per nesso causale.
**Severità:** GRAVE in B se non commentato; MODERATA in R se non in note riservate.

---

## R24 — Lesioni complesse: visualizzazione immagini RMN
**Trigger:** lesioni complesse (menischi, legamenti, tendini, fratture parcellari) valutate da VML.
**Fasi:** B, D.
**Azione:** il VML deve indicare esplicitamente se ha visionato direttamente le immagini RMN o solo i referti testuali. Mancata visualizzazione diretta = MODERATA in B+D.

---

## R25 — Atto quotidiano + RMN degenerativa
**Trigger:** dinamica è un atto lavorativo/quotidiano senza meccanismo traumatico tipico (rialzarsi, torcersi, flettersi) E RMN mostra quadro prevalentemente degenerativo senza lesioni acute strutturali.
**Fasi:** B, D.
**Azione:** il VML deve: (a) dimostrare i requisiti della "causa violenta, fortuita ed esterna"; (b) analizzare contributo degenerativo; (c) segnalare nelle note riservate il rischio di diniego.
**Severità:** GRAVE in B + cascata D se nesso dichiarato "compatibile" senza motivazione specifica.

---

## R26 — E: analisi perizia di parte
**Trigger:** fase E con perizia di parte presente.
**Regola:** mera registrazione dei dati della perizia di parte (nominativo medico, IP proposta, struttura IT) senza analisi critica = SUFF/DISC in E, NON INSUFF/GRAVE.
INSUFF si applica solo se dati perizia di parte assenti o non riportati.
BUONO/OTTIMO richiede confutazione argomentata voce per voce.

---

## R27 — CPI/MALATTIA: principio malattia unica
**Trigger:** polizza CPI tipo MALATTIA (es. Proteggimutuo).
**Regola:** la copertura è limitata alle "conseguenze dirette ed esclusive della singola malattia denunciata". Il VML deve:
(a) identificare UNA sola malattia come evento principale assicurato;
(b) qualificare le altre patologie come preesistenze/comorbidità NON contributive;
(c) valorizzare ITT/ITP come ASSENZA LAVORATIVA (non biologica);
(d) IP riguarda sole conseguenze della malattia denunciata.
Mancata identificazione malattia unica = MODERATA in B. ITT come biologica invece di lavorativa = MODERATA in D.
E spesso NON PERTINENTE (nessuna perizia di parte in CPI).

---

## R28 — Preesistenza esplicita nell'imaging valutata come danno attuale
**Trigger:** referto imaging acquisito dal VML classifica esplicitamente una lesione come preesistente ("esiti di pregressa distrazione", "processo cronico", "calcificazione pregressa") E il VML la valuta come danno dell'evento attuale senza commento sulla natura preesistente.
**Fasi:** B, D.
**Severità:** INSUFFICIENTE in B (nesso lesioni-menomazioni non dimostrato) + cascata D.
**Distinzione da R25:** R28 si applica anche quando l'evento è genuino (triaggettivazione presente) ma le specifiche lesioni valutate sono preesistenti nell'imaging.

---

## R29 — Tendini/cuffia >50 anni: substrato degenerativo
**Trigger:** periziando >50 anni con lesione tendinea o legamentosa (cuffia rotatori, tendine d'Achille, legamenti del ginocchio).
**Fasi:** B, D.
**Azione:** il VML deve: (a) valutare esplicitamente il potenziale substrato degenerativo anche senza anamnesi dichiarata; (b) richiedere rilettura immagini RMN da radiologo fiduciario; (c) segnalare nelle note riservate il sospetto di non piena indennizzabilità.
Età + lavoro manuale/fisico + lesione bi-tendinea = profilo ad altissimo rischio degenerativo.
**Severità:** GRAVE in B + INSUFF in D se non valutato.
**Distinzione da R28:** R29 si applica anche senza referto esplicito di preesistenza, per sola valutazione anagrafico-clinica.

---

## R30 — Cicatrice al volto con Tabella INAIL 1965
**Trigger:** polizza P.INF con sola Tabella INAIL 1965 + esito cicatriziale facciale.
**Regola:** INAIL 1965 non attribuisce IP per cicatrici facciali in assenza di deficit funzionale dimostrabile (mimica, vista, masticazione). Disestesia locale e visibilità non sono sufficienti. IP estetica per cicatrice è prerogativa di tabelle biologiche/civili (ANIA, Gelli-Bianco).
**Azione:** cicatrice "non interferente sulla mimica" = 0% INAIL (non 1% o più).
Verificare sempre la tabella richiesta dalla polizza prima di attribuire IP per lesioni estetiche.

---

## R31 — Identificazione documento da auditare
**Trigger:** fascicolo con più perizie o perizie atipiche.
**Regola:** la perizia fiduciaria ISP è SEMPRE di studio ISP-panel (MEDEXPERT/ProntoMedital/Assiservices). Se il fascicolo contiene una perizia di un medico non appartenente al panel ISP (CTU tribunale, perito privato, specialista esterno), quella perizia è la perizia di parte → Fase E, NON la perizia da auditare.
**Procedura:** prima di qualsiasi valutazione, verificare intestazione e formato. Se non trovata subito, leggere TUTTI i merged PDF. Se nome file contiene "ok": verificare contenuto — potrebbe essere RESTITUZIONE NEGATIVA (visita non effettuata, incarico restituito). In tal caso il "ok" non indica perizia completata.

---

## R32 — RCA CARD: campo dinamica vuoto
**Trigger:** perizia RCA CARD con campo "Modalità accadimento" o "Descrizione circostanze e modalità dell'evento" non compilato.
**Fasi:** B (GRAVE), A (MODERATA).
**Calibrazione:** campo vuoto + incoerenza interna nel documento (es. cita casco E cinture per stesso veicolo) → B scende a 50 (INSUFF). Solo campo vuoto senza incoerenza → B scende a 55–58 (QS).

---

## R33 — Garanzia Fratture: lesione fratturativa non trattata
**Trigger:** polizza P.INF con garanzia "Indennizzo Fratture" (Ombrello2018, XME Protezione) + documentazione medica riporta lesione di natura fratturativa (anche parziale: infrazione, microfrattura, frattura intraspongiotica).
**Fasi:** D.
**Regola:** il VML deve obbligatoriamente: (a) verificare se la lesione rientra nell'elenco fratture della polizza; (b) valorizzare l'importo tabellare se inclusa; (c) motivare esplicitamente l'esclusione se non inclusa.
Mancata trattazione = INSUFF in D.
Calibrazione A: campi modulo non compilati senza impatto sul contenuto sostanziale = al più DISC in A (non INSUFF).

---

## R34 — RCA CARD diniego tecnico: separazione perizia medica/tecnica
**Trigger:** perizia RCA CARD con reiezione per motivi tecnico-legali (incompatibilità danni veicolo, contestazione fatto storico, mancata collaborazione parti).
**Regola:** la perizia VML medica NON è penalizzata in B per non aver affrontato la contestazione tecnica del veicolo. La voce B valuta competenza medico-legale sul nesso causale medico (evento→lesioni), autonoma dalla decisione liquidativa e dalla perizia tecnica veicolare.
Penalizzare B per omessa contestazione perizia tecnica = category error.
**Fasi B:** la bonarietà dell'evento (accesso autonomo PS senza 118, abbandono PS contro parere medico, casco non verificato) è una criticità di R (note riservate), NON di B.

---

## R35 — Infortunio lavorativo: verifica INAIL + timing ustioni
**Trigger:** polizza P.INF con evento occorso durante attività lavorativa (autonomo o dipendente, inclusi lavoratori domestici dipendenti come badanti e colf).
**Fasi:** B, D.
**Azione INAIL:** il VML deve: (a) verificare se l'evento è stato denunciato come infortunio lavorativo all'INAIL; (b) richiedere la valutazione INAIL (IT + IP tabella INAIL); (c) commentare il rapporto tra valutazione INAIL e indennizzo di polizza.
Mancata verifica/richiesta = INSUFF in B + INSUFF in D.
**Timing cicatrici/ustioni:** visita a <12 mesi dall'evento per esiti cicatriziali = GRAVE in B (metodologia) + GRAVE in D (quantificazione sovrastimata). Gli esiti cicatriziali richiedono almeno 12 mesi per stabilizzarsi.
**Corollario Danni Estetici:** la garanzia specifica richiede lettura obbligatoria CG polizza per determinare se il danno estetico specifico è in tabella. Non è sufficiente che la garanzia esista.

---

## R36 — Frattura polso arto dominante con osteosintesi
**Trigger:** frattura epifisi distale radio arto dominante + trattamento chirurgico (riduzione e sintesi con placca e viti in situ) + residua limitazione funzionale documentata all'EO.
**Fasi:** D.
**Regola:** IP ANIA minima attesa = 7–8%. IP ≤ 5% ANIA = "estremamente restrittiva" → INSUFF in D.
**Corollario:** la conversione ANIA→INAIL non è meccanicamente +1 punto percentuale. Dipende dalla voce specifica della tabella INAIL 1965 per il distretto articolare. Il VML deve verificare la voce INAIL corrispondente.

---

## Ricalibrazione B in P.INF post-C48

In P.INF, quando la motivazione del nesso è sintetica MA:
- la conclusione è corretta (nesso affermato appropriatamente), E
- confermata da CMC o MEDEXPERT in revisione

→ voto B = DISCRETO (non INSUFFICIENTE). La INSUFF in B si riserva a nesso sbagliato, assente, o contraddittorio.

## Ricalibrazione D in P.INF post-C48

IP confermata da CMC o MEDEXPERT → D tende a BUONO (non SUFF o DISC).

## Bias sistematico MLQA in RCA CARD (da C28–C50)

MLQA tende strutturalmente a sopravvalutare le criticità in RCA CARD rispetto al CTC:
- B sovrapenalizzato su preesistenze non menzionate quando IP al minimo
- R sovrapenalizzato su elementi di bonarietà quando IP al minimo
- D tendenzialmente più severo del CTC in RCA micro-permanente

Applicare sempre R18 con calibrazione specifica per RCA prima di assegnare il voto B.
