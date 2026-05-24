# Fonts Self-Hosted

Per la compliance GDPR è necessario scaricare i font da Google Fonts
e ospitarli localmente. Procedura:

## 1. Spectral
- Vai su: https://fonts.google.com/specimen/Spectral
- Pesi necessari: 300, 400, 500, 600, italics 300, 400, 500
- Clicca "Download family"
- Estrai i file .ttf

## 2. Inter
- Vai su: https://fonts.google.com/specimen/Inter
- Pesi necessari: 300, 400, 500, 600
- Clicca "Download family"
- Estrai i file .ttf

## 3. JetBrains Mono
- Vai su: https://fonts.google.com/specimen/JetBrains+Mono
- Pesi necessari: 400, 500
- Clicca "Download family"
- Estrai i file .ttf

## 4. Conversione in formato web (consigliato)

Per ottimizzare il caricamento, converti i file .ttf in .woff2 usando:
- Online: https://transfonter.org/
- O CLI: fonttools (Python)

## 5. Posizionamento

Metti tutti i file .woff2 (o .ttf) in questa cartella `/assets/fonts/`

Il CSS del sito è già configurato per usarli come file locali.

## Alternativa rapida — usare CDN privacy-friendly

Se non vuoi ospitare i font, puoi usare Bunny Fonts (mirror GDPR-compliant di Google Fonts):
- https://fonts.bunny.net/
- Sostituisci nel CSS `fonts.googleapis.com` con `fonts.bunny.net`
