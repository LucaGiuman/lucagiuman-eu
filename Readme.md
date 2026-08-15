# lucagiuman.eu

Sito personale di **Luca Giuman** — uno spazio di divulgazione del pensiero sull'intelligenza artificiale come *ambiente cognitivo*.

🌐 **Live**: [https://lucagiuman.eu](https://lucagiuman.eu)

---

## Descrizione

Sito statico personale che raccoglie:

- **2 opere editoriali** in formato PDF
- **55 articoli** pubblicati su LinkedIn (indice ragionato con filtri tematici)
- **Profilo professionale** dell'autore

Non è un blog. È un *diario di ricerca pubblico* — uno snodo che rimanda ai contenuti che vivono altrove (LinkedIn, PDF scaricabili).

## Tecnologie

- **HTML/CSS/JS statici** — nessun framework, nessuna build chain
- **Cloudflare Pages** — hosting (deploy automatico da GitHub)
- **Cloudflare DNS** — gestione DNS
- **Cloudflare Web Analytics** — statistiche privacy-first (no cookie, no banner)
- **Google Fonts** — Spectral, Inter, JetBrains Mono
- **Schema.org JSON-LD** — structured data per SEO

## Struttura

```
.
├── index.html              # Homepage (Opere + Diario + Chi sono)
├── privacy/index.html      # Privacy Policy
├── note-legali/index.html  # Note legali
├── robots.txt              # Regole crawler
├── sitemap.xml             # Mappa URL per motori di ricerca
├── manifest.json           # PWA manifest
├── _headers                # Cloudflare security headers
├── _redirects              # Cloudflare redirect rules
├── assets/
│   ├── sfondo.jpg          # Immagine di sfondo
│   ├── luca-giuman.png     # Foto profilo
│   ├── og-image.png        # Anteprima social 1200×630
│   ├── favicon.ico         # Favicon multi-size
│   ├── favicon-*.png       # Favicon variants
│   └── apple-touch-icon.png
└── pdf/
    ├── libro-versione-1.pdf
    ├── forza-ambiente-liberta.pdf
    └── intervista-la-piazza-web.pdf
```

## Come aggiornare i contenuti

### Aggiungere un articolo al Diario

1. Apri `index.html` su GitHub (matita / Edit)
2. Trova la sezione `const articles = [`
3. Aggiungi un nuovo elemento all'array:

```javascript
{
  data: "2026-MM-DD",
  titolo: "Titolo articolo",
  estratto: "Breve descrizione di 2-3 frasi.",
  tag: ["governance", "responsabilità"],
  url: "https://www.linkedin.com/pulse/..."
}
```

4. Aggiorna il counter nel footer e nei diary-stats se cambia il totale
5. Commit → deploy automatico su Cloudflare Pages

### Tag disponibili (10 totali)

`governance` · `riflessioni` · `ambiente-cognitivo` · `lavoro` · `tecnica` · `responsabilità` · `educazione` · `creatività` · `libertà` · `coscienza-relazione`

### Aggiungere un PDF

1. Carica il file nella cartella `pdf/` su GitHub
2. Linkalo dal codice HTML dove necessario (`<a href="/pdf/nome-file.pdf">`)
3. Commit → deploy automatico

## Privacy & Compliance

- **No cookie** — sito completamente cookieless
- **No tracking** — niente Google Analytics, niente Facebook Pixel
- **Cloudflare Web Analytics** — anonimo, GDPR-compliant
- **Privacy Policy** e **Note legali** pubblicate

Per esercitare i diritti GDPR: **gmnlcu.supervisor@gmail.com**

## SEO

Implementati:
- Open Graph + Twitter Cards
- Schema.org Person + WebSite + Book ×2 + BreadcrumbList
- Canonical URLs
- robots.txt + sitemap.xml
- Favicon multi-formato
- OG image dedicata 1200×630
- Manifest PWA
- Skip link + aria-label per accessibilità

## Deploy

Il deploy avviene **automaticamente** ad ogni push sulla branch `main`:

1. Modifica un file su GitHub (o via Git locale)
2. Commit
3. Cloudflare Pages rileva il push entro 30 secondi
4. Build e deploy automatici
5. Sito aggiornato live in 1-2 minuti

URL di produzione: `https://lucagiuman.eu`
URL temporaneo (per preview deploy): `https://lucagiuman-eu.pages.dev`

## Contatti

**Luca Giuman**
Padova, Veneto, Italia
- LinkedIn: [linkedin.com/in/lucagiuman](https://it.linkedin.com/in/lucagiuman)
- X/Twitter: [@GiumanLuca](https://twitter.com/GiumanLuca)
- Email (GDPR): gmnlcu.supervisor@gmail.com

## Licenza

I contenuti del sito (testi, opere, articoli) sono protetti dal diritto d'autore.
Vedere [Note legali](https://lucagiuman.eu/note-legali/) per dettagli.

Il codice del sito è personale e non è distribuito sotto licenza open source.

---

© MMXXVI — Luca Giuman
