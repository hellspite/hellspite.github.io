# Hellspite — memoria del progetto

Sito ufficiale di **Hellspite** (progetto musicale: "scrivo e suono canzoni tristi per essere meno triste").
Questo file serve da memoria tra una sessione e l'altra: cosa c'è, come funziona, cosa è stato fatto e cosa resta da fare.
**Aggiornalo alla fine di ogni lavoro significativo** (sezioni "Storico" e "Da fare").

## Stack e struttura

- **Hugo** (extended, v0.147.0 in CI), nessun tema esterno: tutto in `layouts/` + `static/css/style.css`.
- Hosting: **GitHub Pages**, dominio custom `hellspite.com` (`static/CNAME`), `baseURL` fisso in `hugo.toml` con https.
- Bilingue **IT (default, senza prefisso) / EN (`/en/`)**: contenuti in `content/it` e `content/en`, etichette UI in `i18n/it.toml` e `i18n/en.toml`.
- Sito one-page: `layouts/index.html` contiene tutte le sezioni nell'ordine **News → Live → Musica → Video → Foto → Contatti**.

| Cosa | Dove |
|---|---|
| Config, link piattaforme, email, Instagram, ID album Bandcamp | `hugo.toml` (`[params]`) |
| Date live | `data/live.yaml` (campi: date, venue, city, address, time, link, sold_out) |
| Video YouTube | `data/videos.yaml` (solo ID) |
| Foto Instagram (generato in automatico, non editare a mano) | `data/instagram.json` + `assets/img/instagram/` |
| News | `content/{it,en}/news/*.md` (stesso nome file = traduzione collegata) |
| Template | `layouts/index.html`, `layouts/_default/baseof.html`, `layouts/news/`, `layouts/partials/` |
| Immagini hero / copertina | `assets/img/hero.jpg`, `assets/img/album-cover.jpg` (convertite in WebP da Hugo) |

## Automazioni (GitHub Actions)

- `deploy.yml` — build + deploy su push a `main`, a mano, **ogni giorno alle 04:25 UTC** (così le date live passate spariscono anche senza push) e dopo il workflow Instagram.
  - Non passare `--baseURL` al build: `configure-pages` restituisce `http://` col dominio custom.
- `instagram.yml` — ogni giorno alle 06:00 UTC scarica gli ultimi 9 post via Graph API (secrets `INSTAGRAM_TOKEN`, `INSTAGRAM_IG_USER_ID`) e committa su `main`.
- `keepalive.yml` — ogni lunedì riabilita i workflow programmati (GitHub li spegne dopo 60 giorni senza commit).

## Convenzioni

- Messaggi di commit in italiano, formato `Area: cosa cambia` (es. `Live: aggiunge data ...`).
- Date: formato internazionale con nomi di mese/giorno tradotti in base alla lingua della pagina.
- Per una nuova news: creare il file sia in `content/it/news/` sia in `content/en/news/` con lo stesso nome.
- Per una nuova data live: aggiungere una voce in `data/live.yaml`; le date passate finiscono da sole in "Date passate".

## Storico (cosa è stato fatto)

- **giu 2026** — Setup iniziale Hugo; sezioni Video, News, Foto; nav con hamburger su mobile; icone SVG nei contatti; hero panoramica con logo integrato; copertina album cliccabile; workflow Instagram automatico con immagini locali; SEO (Open Graph, Twitter Card, canonical, favicon, robots.txt); CNAME per `hellspite.com`; email `hellspite@hellspite.com`; date live con indirizzo e orario; prima news "Il sito è online".
- **estate–autunno 2026** — Riordino sezioni; data live 11/09/2026 a Latina; scorrimento fluido e ottimizzazioni mobile; nome band e prossima data nell'hero; player Bandcamp embed; lightbox foto; immagini WebP; i18n completo IT/EN con switcher, hreflang e lingua ricordata; date tradotte; news leggibili in home con archivio separato; fix `baseURL` (usciva http); archivio automatico delle date passate; rebuild giornaliero; keepalive settimanale dei workflow (ultimo commit: 2026-11-07).

## Da fare / idee aperte

- [ ] **Link Spotify e Deezer**: in `hugo.toml` sono vuoti (`spotify = ""`, `deezer = ""`), quindi i pulsanti non compaiono.
- [ ] **Date live**: tutte le date in `data/live.yaml` sono passate (l'ultima è l'11/09/2026) → aggiungere le prossime quando ci sono.
- [ ] **News**: c'è solo "Il sito è online" (15/06/2026) → pubblicare aggiornamenti.
- [ ] **Pulizia**: `data/photos.yaml` non è più usato dai template (sostituito da `instagram.json`) → si può eliminare.
- [ ] **Token Instagram**: i token Graph API scadono; se le foto smettono di aggiornarsi, rigenerare il secret `INSTAGRAM_TOKEN`. Il workflow usa l'API `v18.0`, valutare di aggiornarla.
- [ ] **Foto Instagram vecchie**: il workflow scarica le nuove immagini ma non cancella quelle che escono dagli ultimi 9 → `assets/img/instagram/` cresce nel tempo.

<!-- Aggiungi qui sotto nuovi obiettivi man mano che emergono -->
