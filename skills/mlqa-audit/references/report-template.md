# Template Generazione Report .docx

Report generato via Node.js con il pacchetto `docx` (npm).
Eseguire via bash: `cd /tmp && npm install docx 2>/dev/null; node - << 'JSEOF' ... JSEOF`

## Regole sintassi OBBLIGATORIE (bash heredoc)

- Usare `var` e `function(){}` — NO arrow functions (`=>`)
- NO template literals con backtick `` ` `` — usare concatenazione con `+`
- NO `const`/`let` se causa problemi nel contesto heredoc (usare `var`)
- String delimiter heredoc: `'JSEOF'` (quoted = no interpolazione bash)

## Costanti standard

```javascript
var GRAY_HDR  = { type: ShadingType.CLEAR, color: 'auto', fill: 'BFBFBF' };
var GRAY_LITE = { type: ShadingType.CLEAR, color: 'auto', fill: 'D9D9D9' };
var GRAVE     = { type: ShadingType.CLEAR, color: 'auto', fill: 'FF6666' };
var MOD       = { type: ShadingType.CLEAR, color: 'auto', fill: 'FFD966' };
var LIEVE_SH  = { type: ShadingType.CLEAR, color: 'auto', fill: 'BDD7EE' };
var GREEN_SH  = { type: ShadingType.CLEAR, color: 'auto', fill: 'C6EFCE' };
var TW = 9360; // larghezza totale tabella in DXA (twips)
```

## Colori intestazione

- Header principale (titolo scheda): fill `1F3864`, testo bianco
- Header sezioni (scoring, criticità): fill `2E4057`, testo bianco
- Header confronto CTC: fill `7B2D00` (arancio-bruno), testo bianco

## Funzioni helper standard

```javascript
function hdr(txt, sz, color, fill) {
  sz = sz || 11; color = color || 'FFFFFF'; fill = fill || '2E4057';
  return new Paragraph({
    alignment: AlignmentType.CENTER,
    shading: { type: ShadingType.CLEAR, color: 'auto', fill: fill },
    children: [new TextRun({ text: txt, bold: true, size: sz * 2, color: color })]
  });
}

function cell(txt, opts) {
  opts = opts || {};
  return new TableCell({
    width: opts.width ? { size: opts.width, type: WidthType.DXA } : undefined,
    shading: opts.shading || undefined,
    verticalAlign: VerticalAlign.CENTER,
    columnSpan: opts.span || 1,
    children: [new Paragraph({
      alignment: opts.align || AlignmentType.LEFT,
      children: [new TextRun({
        text: txt || '',
        bold: !!opts.bold,
        size: (opts.sz || 9) * 2,
        color: opts.color || '000000'
      })]
    })]
  });
}

function row() { return new TableRow({ children: [].slice.call(arguments) }); }
```

## Struttura documento

```javascript
var doc = new Document({ sections: [{ properties: {}, children: [
  // --- PAGINA 1 ---
  hdr('SCHEDA MLQA – C[N] – COGNOME NOME', 13, 'FFFFFF', '1F3864'),
  new Paragraph({ spacing: { after: 80 } }),
  intestTable,       // tabella metadati caso
  new Paragraph({ spacing: { after: 100 } }),
  hdr('SCORING MLQA (...)', 10, 'FFFFFF', '2E4057'),
  new Paragraph({ spacing: { after: 60 } }),
  scoringTable,      // tabella 6 fasi + voto finale
  new Paragraph({ spacing: { after: 80 } }),
  new Paragraph({ children: [
    new TextRun({ text: 'VOTO FINALE: ', bold: true, size: 22 }),
    new TextRun({ text: voto + ' – FASCIA', bold: true, size: 24, color: '1F3864' }),
    new TextRun({ text: '   |   VML: Dr. Nome (Studio) – incarico XXXXX', size: 18 })
  ]}),
  // --- SALTO PAGINA ---
  new Paragraph({ children: [new PageBreak()] }),
  // --- PAGINA 2 ---
  hdr('CRITICITA MLQA – C[N] – COGNOME', 10, 'FFFFFF', '1F3864'),
  new Paragraph({ spacing: { after: 60 } }),
  critTable,         // tabella criticità
  new Paragraph({ spacing: { after: 120 } }),
  hdr('CONFRONTO CTC', 10, 'FFFFFF', '2E4057'),
  new Paragraph({ spacing: { after: 60 } }),
  confTable,         // tabella confronto CTC (pre: "In attesa CTC"; post: dati reali)
  new Paragraph({ spacing: { after: 100 } }),
  // note apprendimento (testo libero)
]}]});
```

## Tabella intestazione (intestTable)

Colonne: `[1400, 1560, 1800, 4600]` (sum = 9360 = TW)
Righe tipiche: Caso | Tipo sinistro | Periziando | Sesso/Età | Polizza | N.Sinistro | VML | Incarico | Dinamica | Diagnosi/Preesistenze | Quantificazione

## Tabella scoring (scoringTable)

Colonne: `[700, 2500, 800, 700, 1000, 660]` (sum = 6360 — NON uguale a TW; aggiustare se necessario o usare `[700, 2960, 800, 700, 1000, 1200]`)
Intestazione: Fase | Descrizione | Peso | Voto | Fascia | Pond.
Riga finale: span 3 celle vuote + Voto (size 20, bold) + Fascia (size 20, bold) + Voto (size 22, bold)
Shading alternato: righe pari GRAY_LITE, righe dispari undefined.

## Tabella criticità (critTable)

Colonne: `[600, 3000, 900, 4860]` (sum = 9360 = TW)
Intestazione: Fase | Criticità | Livello | Motivazione MLQA
Livello shading: GRAVE=FF6666, MODERATA=FFD966, LIEVE=BDD7EE

## Tabella confronto CTC (confTable — pre-CTC)

```javascript
var confTable = new Table({
  width: { size: TW, type: WidthType.DXA },
  columnWidths: [2200, 1500, 1500, 4160],
  rows: [
    row(cell('', { bold: true, shading: GRAY_HDR, sz: 9 }),
        cell('MLQA', { bold: true, align: 'CENTER', shading: GRAY_HDR, sz: 9 }),
        cell('CTC',  { bold: true, align: 'CENTER', shading: GRAY_HDR, sz: 9 }),
        cell('Delta / Esito', { bold: true, align: 'CENTER', shading: GRAY_HDR, sz: 9 })),
    row(cell('In attesa scheda CTC', { shading: GRAY_LITE, sz: 9 }),
        cell(voto + ' FASCIA', { align: 'CENTER', shading: GRAY_LITE, sz: 9 }),
        cell('---', { align: 'CENTER', shading: GRAY_LITE, sz: 9 }),
        cell('---', { shading: GRAY_LITE, sz: 9 }))
  ]
});
```

## Tabella confronto CTC (post-CTC)

Aggiungere righe:
- Riga voto: MLQA xx.x FASCIA | CTC yy.y FASCIA | Δ = ±z.z | Fascia ✅/❌
- Riga voce INSUFF CTC: elenco voci insufficienti
- Riga voci BUONO/DISC: elenco
- Riga Note CTC: testo note consulente (italics)
- Riga Errore MLQA principale: testo esplicativo

## Output

```javascript
Packer.toBuffer(doc).then(function(buf) {
  fs.writeFileSync('/sessions/gallant-trusting-curie/mnt/outputs/MLQA_C[N]_Cognome.docx', buf);
  console.log('OK ' + voto + ' FASCIA');
});
```

## Import completo

```javascript
const { Document, Packer, Paragraph, Table, TableRow, TableCell, TextRun,
        WidthType, AlignmentType, ShadingType, PageBreak, VerticalAlign } = require('docx');
const fs = require('fs');
```

## Calcolo voto con E=NP

```javascript
var phases = [
  { id: 'A', w: 5/95,  score: <voto_A> },
  { id: 'B', w: 35/95, score: <voto_B> },
  { id: 'C', w: 15/95, score: <voto_C> },
  { id: 'D', w: 35/95, score: <voto_D> },
  { id: 'R', w: 5/95,  score: <voto_R> }
];
var total = phases.reduce(function(s, p) { return s + p.score * p.w; }, 0);
var voto = Math.round(total * 10) / 10;
```

Con E presente (non NP): usare pesi standard (5,35,15,35,5,5) / 100.
