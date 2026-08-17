# Caratteri tipografici self-hosted

Aggiornato: 17 agosto 2026. Sostituisce la procedura manuale precedente, che
non è più necessaria: i file sono già in questa cartella.

## Stato

Le tre pagine del sito (`/`, `/privacy/`, `/note-legali/`) caricano i font da
questo dominio tramite `fonts.css`. **Nessuna chiamata a `fonts.googleapis.com`,
`fonts.gstatic.com` o `fonts.bunny.net`**: nessun indirizzo IP del visitatore
raggiunge un fornitore terzo per il caricamento dei caratteri. È la condizione
che rende vera la dichiarazione già pubblicata nella privacy policy
(*"I caratteri tipografici utilizzati sono ospitati direttamente sul sito,
senza chiamate a CDN esterne"*).

`leggi.html` non è coinvolto: usa i font di sistema (`Inter, system-ui`).

## Contenuto

| File | Cosa è |
|---|---|
| `fonts.css` | 26 blocchi `@font-face`. È l'unico punto in cui i font vengono dichiarati: le tre pagine lo linkano, non duplicano le regole. |
| `spectral-*.woff2` | Spectral 300/400/500/600 + corsivi 300/400/500 |
| `inter-*.woff2` | Inter 300/400/500/600 |
| `jetbrains-mono-*.woff2` | JetBrains Mono 400/500 |
| `*-ext.woff2` | variante `latin-ext` di ogni peso |
| `OFL-*.txt` | licenze delle tre famiglie |

## Come sono stati generati

1. TTF sorgente dal repo upstream `github.com/google/fonts` (`ofl/spectral`,
   `ofl/inter`, `ofl/jetbrainsmono`).
2. Inter e JetBrains Mono sono variabili: istanziati sui pesi richiesti con
   `fonttools varLib.instancer` (per Inter l'asse `opsz` è fissato a 14, il suo
   default per il testo). Spectral è già statico.
3. Subset con `pyftsubset` sui **medesimi intervalli Unicode** che Google Fonts
   e Bunny servivano prima (`latin` e `latin-ext`), così la resa non cambia.
4. Compressione con `woff2_compress`.

I due sottoinsiemi sono separati e distinti da `unicode-range`: i file `-ext`
si scaricano **solo** se la pagina contiene caratteri di quell'intervallo. Per
un testo italiano non vengono richiesti — costano zero e restano come garanzia
per nomi stranieri in futuri articoli del Diario.

## Peso

Ogni file `latin` sta fra 19 e 22 KB; i `-ext` fra 6 e 31 KB. Cartella completa
632 KB, ma il browser scarica solo i pesi effettivamente usati dalla pagina.
`_headers` serve `/assets/*` con `Cache-Control: max-age=31536000, immutable`:
dopo la prima visita nessun font viene più richiesto.

## Se un peso va aggiunto o cambiato

Non modificare i `.woff2` a mano. Rigenerare dai TTF upstream con la procedura
sopra e aggiungere il blocco `@font-face` corrispondente in `fonts.css` — due
blocchi, uno `latin` e uno `latin-ext`. Le pagine HTML non vanno toccate.

## Licenza

Spectral, Inter e JetBrains Mono sono rilasciati sotto SIL Open Font License
1.1. Il testo integrale è nei file `OFL-*.txt` di questa cartella, come la
licenza richiede quando i font vengono redistribuiti.
