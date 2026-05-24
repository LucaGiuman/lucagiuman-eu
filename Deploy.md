DEPLOY.md — Storia tecnica del deploy di lucagiuman.eu
Documentazione interna del processo di deploy. Non destinata al pubblico, ma utile come memoria operativa per eventuali manutenzioni future, debug, o ricostruzione del setup.


Sito: https://lucagiuman.eu Data deploy iniziale: maggio 2026 Stack: GitHub + Cloudflare Pages + Cloudflare DNS + OVH (registrar)


________________


1. Architettura
┌─────────────────────────────────────────────────────────┐


│ Visitatore (browser)                                     │


└─────────────────────────────────────────────────────────┘


                          ↓ HTTPS


┌─────────────────────────────────────────────────────────┐


│ Cloudflare CDN (edge node geograficamente più vicino)    │


│ - SSL/TLS Let's Encrypt                                  │


│ - DDoS protection                                        │


│ - Caching                                                │


│ - Web Analytics                                          │


└─────────────────────────────────────────────────────────┘


                          ↓


┌─────────────────────────────────────────────────────────┐


│ Cloudflare Pages                                         │


│ - Sito statico servito direttamente                      │


│ - Deploy automatico da GitHub                            │


└─────────────────────────────────────────────────────────┘


                          ↑


┌─────────────────────────────────────────────────────────┐


│ GitHub repository (lucagiuman-eu)                        │


│ - Sorgente del sito                                      │


│ - Branch main = produzione                               │


└─────────────────────────────────────────────────────────┘
DNS
Registry .eu (autorità superiore)


        ↓


Nameserver Cloudflare:


  - martin.ns.cloudflare.com


  - sunny.ns.cloudflare.com


        ↓


Zona DNS gestita su Cloudflare:


  - lucagiuman.eu → CNAME → lucagiuman-eu.pages.dev


  - www.lucagiuman.eu → CNAME → lucagiuman-eu.pages.dev


________________


2. Cronologia operativa
Fase 1 — Concept & Design
1. Sito statico single-page, 3 sezioni: Opere + Diario + Chi sono
2. Estetica cinematografica/atmosferica
3. Palette: nero #0A0C10, ciano #7DD3D8, viola #A78BD9, moonlight #F5F3ED
4. Tipografia: Spectral (serif) + Inter (sans) + JetBrains Mono
5. Niente cookie, niente Google Analytics, niente tracking invasivo
Fase 2 — Contenuti
1. 49 articoli LinkedIn mappati dal foglio Drive su sheet 1m9d6OGgUzFlrULML95hJEf4U2MfHb8h_L8PU4Yjpliw
2. Tag finali (10):
   * governance (16) · riflessioni (15) · ambiente-cognitivo (10)
   * lavoro (9) · tecnica (8) · responsabilità (7)
   * educazione (5) · creatività (4) · libertà (4) · coscienza-relazione (3)
3. 2 opere PDF:
   * Quando la tecnologia smette di essere uno strumento (2024)
   * Forza, Ambiente, Libertà (2025)
4. Intervista La Piazza Web (PDF)
Fase 3 — Versioni iterative del codice
Versione
	Caratteristica principale
	v1-v3
	Esplorazione design
	v4
	Estetica cinematografica consolidata, 49 articoli, 3 sezioni
	v5
	Filtri espandibili, sort cronologico, estratti articoli
	v6
	Foto profilo, Privacy Policy, Note legali, link footer legal
	v7
	SEO Priority 1+2, schema.org JSON-LD, pagine legal separate, asset SEO
	Fase 4 — SEO implementata (v7)
Meta tags
* Open Graph completo (og:title, og:description, og:image, og:url, og:locale, og:site_name)
* Twitter Cards (summary_large_image, @GiumanLuca)
* Canonical URL
* Theme-color #0A0C10
* Robots: index, follow
* Author: Luca Giuman
* Keywords specifiche
Schema.org JSON-LD
1. Person — Luca Giuman (con worksFor, alumniOf, knowsAbout, sameAs)
2. WebSite — lucagiuman.eu
3. CreativeWork (Book) ×2 — le due opere
4. BreadcrumbList — navigazione
Asset SEO
* og-image.png 1200×630 dedicata
* favicon.ico + 4 PNG (16, 32, 192, 512)
* apple-touch-icon.png 180×180
* manifest.json PWA-ready
* robots.txt
* sitemap.xml
* _headers (HSTS, X-Frame-Options, Permissions-Policy, cache headers)
* _redirects (HTTPS forzato, dominio canonico senza www)
Accessibilità
* Skip link
* Aria-label su pulsanti decorativi
* Aria-hidden su layer puramente visivi
* HTML semantico
Fase 5 — Privacy & GDPR
Decisioni di compliance
1. Niente cookie — sito completamente cookieless
2. Cloudflare Web Analytics invece di Google Analytics (privacy-first, no cookie, no consenso richiesto)
3. Font ospitati esternamente ma con <link preconnect> (alternativa pianificata: self-host)
4. Privacy Policy e Note legali come pagine vere (URL /privacy/ e /note-legali/)
5. Email contatto GDPR: gmnlcu.supervisor@gmail.com (separata dall'email personale)
6. Foro competente: Padova
Cosa abbiamo deciso di NON fare
* Niente cookie banner (non servono, no cookie)
* Niente Google Analytics (richiederebbe banner GDPR)
* Niente form contatti pubblici (no raccolta dati)
* Niente newsletter (no raccolta email)


________________


3. Setup tecnico — step by step
Step 1 — Repository GitHub
1. Account GitHub: LucaGiuman
2. Repository: lucagiuman-eu (pubblico)
3. Branch: main
4. Upload iniziale via web UI (drag & drop)
5. Caricamento PDF manuale nella cartella /pdf/
6. Modifiche successive via web UI (matita)
Step 2 — Cloudflare Pages
1. Workers & Pages → Pages → Connect to Git
2. Autorizzazione GitHub: Only select repositories → lucagiuman-eu
3. Build settings:
   * Project name: lucagiuman-eu
   * Production branch: main
   * Framework preset: None
   * Build command: vuoto
   * Build output directory: /
4. Deploy automatico → URL temporaneo: https://lucagiuman-eu.pages.dev
Step 3 — Cloudflare Web Analytics
1. Analytics & Logs → Web Analytics → Add a site
2. Hostname: lucagiuman.eu
3. Token ottenuto e copiato
4. Token sostituito a CF_TOKEN_PLACEHOLDER in:
   * index.html
   * privacy/index.html
   * note-legali/index.html
Step 4 — DNS Cloudflare (trasferimento da OVH)
1. Cloudflare richiede trasferimento DNS per dominio root .eu (limitazione CNAME su apex)
2. Add a site su Cloudflare: lucagiuman.eu
3. Selezione piano: Free
4. Cloudflare scansiona DNS attuale OVH
5. Trovati:
   * 1 A: lucagiuman.eu → 213.186.33.5 (Google Sites)
   * 1 CNAME: www → ghs.googlehost.com (Google Sites)
   * 2 MX: email OVH non utilizzata
   * 2 SRV: IMAP/SMTP OVH non utilizzati
   * 2 TXT: verifiche Google Sites
6. Cancellati 6 record (2 MX + 2 SRV + 2 TXT) — email mai usata né futura
7. Mantenuti A e CNAME (verranno aggiornati dopo)
8. Cloudflare assegna nameserver: martin.ns.cloudflare.com + sunny.ns.cloudflare.com
Step 5 — Modifica nameserver su OVH
1. OVH Manager → Domini → lucagiuman.eu → Informazioni generali
2. DNSSEC: disattivato (verifica con Verisign DNSSEC Analyzer)
   * Nota: OVH ha mostrato "Disattivazione in corso" per ore, ma il DS record era già rimosso dalla zona .eu (verifica esterna confermata)
3. Server DNS → Modifica → Utilizzare i tuoi DNS:
   * Nameserver 1: martin.ns.cloudflare.com
   * Nameserver 2: sunny.ns.cloudflare.com
4. NO glue records (campi IP lasciati vuoti, non servono per nameserver esterni)
Step 6 — Propagazione DNS
1. Verifica con whatsmydns.net (NS record)
2. Propagazione globale in ~1 ora
3. Click I updated my nameservers su Cloudflare
4. Cloudflare conferma: "Il tuo dominio è ora protetto da Cloudflare"
Step 7 — Custom domain su Pages
1. Workers e Pages → lucagiuman-eu → Domini personalizzati
2. Configura un dominio personalizzato:
   * lucagiuman.eu → attivo
   * www.lucagiuman.eu → attivo
3. SSL Let's Encrypt emesso automaticamente
4. Cloudflare ha configurato i record CNAME nel DNS (visibili in Cloudflare DNS panel)
Step 8 — Search Console
1. Proprietà sc-domain:lucagiuman.eu già esistente da tempi precedenti (vecchio Google Sites)
2. Sitemap → invio sitemap.xml
   * Inizialmente "Impossibile recuperare" (cache Google)
   * Si risolve in 24-48 ore automaticamente
3. Controllo URL → Richiedi indicizzazione per:
   * https://lucagiuman.eu/
   * https://lucagiuman.eu/privacy/
   * https://lucagiuman.eu/note-legali/


________________


4. Configurazioni di produzione
Cloudflare DNS — Record attivi
Type     Name             Content                       Proxy


CNAME    lucagiuman.eu    lucagiuman-eu.pages.dev      Proxied


CNAME    www              lucagiuman-eu.pages.dev      Proxied
Cloudflare Pages — Custom domains
lucagiuman.eu          → Attivo, SSL abilitato


www.lucagiuman.eu      → Attivo, SSL abilitato


lucagiuman-eu.pages.dev → URL temporaneo (sempre attivo, utile per preview)
OVH — Server DNS
Tipo: Personalizzato (Utilizzare i tuoi DNS)


Server DNS 1: martin.ns.cloudflare.com


Server DNS 2: sunny.ns.cloudflare.com
Cloudflare Web Analytics
Token: [salvato privatamente, presente nei 3 file HTML]


Hostname: lucagiuman.eu


Tipo: Site (no cookie, GDPR-compliant)


________________


5. Workflow di aggiornamento
Aggiungere un articolo al Diario
1. GitHub → index.html → matita
2. Cerca const articles = [
3. Aggiungi oggetto:


{


  data: "2026-MM-DD",


  titolo: "Titolo articolo",


  estratto: "Descrizione 2-3 frasi.",


  tag: ["governance", "responsabilità"],


  url: "https://www.linkedin.com/pulse/..."


}


4. Aggiorna counter footer e diary-stats se conteggio totale cambia
5. Commit changes → push automatico → deploy Cloudflare 30-60s
Modificare la sezione Chi sono
1. GitHub → index.html → matita
2. Cerca <!-- ==================== CHI SONO ==================== -->
3. Modifica contenuto
4. Commit
Aggiungere un nuovo PDF
1. GitHub → cartella /pdf/ → Add file → Upload files
2. Carica il PDF (nome kebab-case, no spazi)
3. Aggiungi link nell'HTML dove serve
4. Commit
Rimuovere/modificare un articolo
1. Stesso processo: edit index.html → modifica/rimuovi oggetto dall'array articles
2. Aggiorna counter se necessario


________________


6. Procedure di emergenza
Sito offline / errore 500
1. Verifica https://www.cloudflarestatus.com (eventuali outage Cloudflare)
2. Verifica deploy log su Cloudflare Pages → Deployments
3. Se ultimo deploy ha errori → rollback al precedente:
   * Pages → Deployments → click deploy precedente → Rollback to this deployment
Sito mostra contenuti vecchi
1. Cloudflare cache → Pages → progetto → Settings → Purge cache
2. Browser cache → Ctrl+Shift+R per hard reload
DNS non risolve
1. Verifica nameserver su OVH (devono essere Cloudflare)
2. Verifica record DNS su Cloudflare (devono esserci CNAME corretti)
3. Se cambio recente: aspetta propagazione (max 24h)
Sostituzione domain root in emergenza
Se servisse cambiare dominio:


1. Cloudflare Pages → Domini personalizzati → rimuovi lucagiuman.eu
2. Aggiungi nuovo dominio
3. Aggiorna DNS (CNAME a lucagiuman-eu.pages.dev)
Backup repository
1. GitHub → repository → Code → Download ZIP
2. Salva localmente con timestamp
3. Periodicità raccomandata: mensile


________________


7. Monitoraggio periodico
Settimanale
* Cloudflare Web Analytics → visite, paesi, pagine più viste
* Search Console → impression, clic, posizione media
Mensile
* Verifica scadenza dominio OVH (rinnovo annuale)
* Verifica certificato SSL (rinnovo automatico, ma controllare)
* Verifica deploy log Cloudflare (errori imprevisti)
* Backup repository GitHub
Annuale
* Aggiornare data "Ultimo aggiornamento" in Privacy/Note legali
* Verificare link esterni (LinkedIn, SerenDPT, EGGON)
* Verificare PDF accessibili
* Verificare presenza nuovi standard SEO (es. nuovi tipi schema.org)


________________


8. Decisioni di design da ricordare
Cosa NON cambiare
1. Single-page — il sito è progettato come uno snodo, non come blog tradizionale
2. 3 sezioni — Opere, Diario, Chi sono. Non aggiungere "blog", "shop", "newsletter"
3. No cookie — qualunque servizio terzo che richiederebbe cookie va valutato con cautela
4. Sfondo cinematografico — è la cifra estetica del sito, va preservata
5. Estratti senza emoji — coerenti col registro editoriale
6. Tag 10 totali — consolidati, non aggiungerne facilmente
Cosa si può evolvere
1. Nuovi articoli (workflow definito)
2. Nuova foto profilo (se cambia)
3. Aggiornamenti bio
4. Nuovi PDF (es. saggi futuri, ulteriori interviste)
5. Eventuali pagine future (es. /contatti/ se decidi di aggiungere form)


________________


9. Riferimenti
Dashboard
Servizio
	URL
	Sito live
	https://lucagiuman.eu
	GitHub
	https://github.com/LucaGiuman/lucagiuman-eu
	Cloudflare Pages
	https://dash.cloudflare.com → Workers e Pages
	Cloudflare DNS
	https://dash.cloudflare.com → lucagiuman.eu
	Cloudflare Analytics
	https://dash.cloudflare.com → Web Analytics
	Google Search Console
	https://search.google.com/search-console
	OVH Manager
	https://www.ovh.com → Manager
	Strumenti diagnostici
* DNS propagation: https://www.whatsmydns.net
* DNSSEC check: https://dnssec-analyzer.verisignlabs.com
* DNS detail: https://dnsviz.net
* Rich Results test: https://search.google.com/test/rich-results
* PageSpeed: https://pagespeed.web.dev
* SSL test: https://www.ssllabs.com/ssltest/
Documentazione
* Cloudflare Pages: https://developers.cloudflare.com/pages/
* Schema.org: https://schema.org
* Open Graph: https://ogp.me
* Sitemaps: https://www.sitemaps.org


________________


10. Stato finale verificato
Item
	Stato
	Sito live https://lucagiuman.eu
	✅
	Sito live https://www.lucagiuman.eu
	✅
	SSL Let's Encrypt
	✅
	SEO Priority 1 (meta, OG, Twitter, canonical)
	✅
	SEO Priority 2 (schema.org, URL reali pagine legal)
	✅
	Sitemap.xml accessibile
	✅
	Robots.txt accessibile
	✅
	Privacy Policy
	✅
	Note legali
	✅
	Cloudflare Web Analytics attivo
	✅
	49 articoli mappati
	✅
	2 PDF opere scaricabili
	✅
	1 PDF intervista scaricabile
	✅
	Search Console proprietà attiva
	✅
	Sitemap inviata a Google
	✅
	Indicizzazione richiesta per 3 URL
	✅
	GDPR compliance
	✅
	No cookie
	✅
	Accessibilità base (skip link, aria)
	✅
	

________________




Documento generato durante il deploy iniziale. Aggiornare in caso di modifiche strutturali significative.
