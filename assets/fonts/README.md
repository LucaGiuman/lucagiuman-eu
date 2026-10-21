# Caratteri tipografici self-hosted

Aggiornato: 21 ottobre 2026. Sostituisce la dotazione precedente (Spectral,
Inter, JetBrains Mono) dopo l'adozione del sistema visivo D · Soglia.

## Stato

Le pagine del sito (`/`, `/privacy/`, `/note-legali/`, `/404.html`) caricano i
font da questo dominio tramite `fonts.css`. **Nessuna chiamata a
`fonts.googleapis.com`, `fonts.gstatic.com` o `fonts.bunny.net`**: nessun
indirizzo IP del visitatore raggiunge un fornitore terzo per il caricamento dei
caratteri. È la condizione che rende vera la dichiarazione già pubblicata nella
privacy policy (*"I caratteri tipografici utilizzati sono ospitati direttamente
sul sito, senza chiamate a CDN esterne"*).

`leggi.html` non è coinvolto: usa i font di sistema per scelta.

## Contenuto

| File | Cosa è |
|---|---|
| `fonts.css` | 8 blocchi `@font-face`. È l'unico punto in cui i font vengono dichiarati: le pagine lo linkano, non duplicano le regole. |
| `bricolage-grotesque-var.woff2` | Bricolage Grotesque, variabile sul peso 400–700. Titoli, numeri, nomi. |
| `instrument-sans-var.woff2` | Instrument Sans, variabile sul peso 400–600. Testo corrente, bottoni. |
| `jetbrains-mono-400.woff2`, `jetbrains-mono-500.woff2` | JetBrains Mono. Date, etichette, contatori. |
| `*-ext.woff2` | variante `latin-ext` di ciascuno |
| `OFL-*.txt` | licenze delle tre famiglie |

## Come sono stati generati

1. TTF sorgente dal repo upstream `github.com/google/fonts`
   (`ofl/bricolagegrotesque`, `ofl/instrumentsans`, `ofl/jetbrainsmono`).
2. Bricolage Grotesque e Instrument Sans sono variabili su più assi. Istanziati
   con `fonttools varLib.instancer` tenendo **variabile il solo asse del peso**:
   per Bricolage l'asse ottico è fissato a `opsz=28` e la larghezza a
   `wdth=100`, per Instrument Sans la sola larghezza. Fissare l'asse ottico non
   è cosmetico: lasciarlo libero portava il file `latin` da 35 a 86 KB, oltre il
   margine disponibile su LCP.
3. Subset con `pyftsubset` sui **medesimi intervalli Unicode** usati in
   precedenza (`latin` e `latin-ext`).
4. Compressione con `woff2_compress`.

I due sottoinsiemi sono separati e distinti da `unicode-range`: i file `-ext` si
scaricano **solo** se la pagina contiene caratteri di quell'intervallo. Per un
testo italiano non vengono richiesti.

## Peso

La home richiede quattro file: `bricolage-grotesque-var` (35 KB),
`instrument-sans-var` (37 KB), `jetbrains-mono-400` e `-500` (20 KB ciascuno),
per un totale di circa **113 KB** — contro i 124 KB in sei file della dotazione
precedente. `_headers` serve `/assets/*` con
`Cache-Control: max-age=31536000, immutable`: dopo la prima visita nessun font
viene più richiesto.

Un font variabile copre tutti i pesi di una famiglia con un file solo: per
questo i blocchi `@font-face` sono scesi da 26 a 8.

## Se un peso va aggiunto o cambiato

Non modificare i `.woff2` a mano. Con i font variabili un peso nuovo dentro
l'intervallo dichiarato non richiede alcun file: basta usarlo. Serve
rigenerare solo per **allargare** l'intervallo (per esempio Bricolage oltre
700) o per aggiungere un corsivo, che è un file a sé. In quel caso si
rigenera dai TTF upstream con la procedura sopra e si aggiorna il
`font-weight` nei due blocchi `@font-face` della famiglia. Le pagine HTML non
vanno toccate.

## Licenza

Bricolage Grotesque, Instrument Sans e JetBrains Mono sono rilasciati sotto SIL
Open Font License 1.1. Il testo integrale è nei file `OFL-*.txt` di questa
cartella, come la licenza richiede quando i font vengono redistribuiti.
