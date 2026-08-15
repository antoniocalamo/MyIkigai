# My Ikigai — contesto per Claude Code

## Cos'è
App personale di gestione vita quotidiana ("My Ikigai" / "MyIkigai"), single-file
HTML/CSS/JS, backend Supabase, pubblicata su GitHub Pages.

- File vero e unico: `index.html` (nella radice del repository) — HTML + `<style>`
  + `<script>` tutto nello stesso file. Nessun bundler, nessun build step, nessuna
  dipendenza npm installata: le uniche librerie esterne (es. `xlsx` per
  import/export Excel) sono caricate on-demand via `<script>` dinamico, solo
  quando servono.
- URL pubblico: https://antoniocalamo.github.io/MyIkigai/
- Backend Supabase con 2 utenti reali: io (admin, via `isAdmin()`) e mia moglie
  (utente base). Attenzione quando tocchi permessi/segregazione dati.
- Non sono uno sviluppatore: usa sempre un linguaggio chiaro e semplice.
  Ogni termine tecnico (git, pull request, branch, commit, deploy, ecc.) va
  spiegato con parole semplici/analogie la prima volta che lo usi in una
  conversazione, non solo "se utile" — per me non è mai scontato.

## Struttura dell'app
Schede principali: Home (to-do settimanale + backlog + widget/KPI + meteo +
Google Calendar), Finanze (patrimonio netto, investimenti con P&L, flussi di
cassa, piani di ammortamento mutui), Orto (garden planner), Viaggi,
Manutenzione (Checklist/Tabella), Progetti (Dashboard KPI + Tabella + Gantt
timeline), Eventi, più schede custom (es. Ricette) e private (Idee regalo,
Visite mediche).

4 temi selezionabili (Ikigai, Milano, Pirati, Ninja) via variabili CSS
definite nel blocco `:root` base e sovrascritte per tema — un colore "fisso"
va sempre definito **solo** nel blocco base, mai ripetuto uguale nei blocchi
tema, altrimenti perde il senso di "colore fisso cross-tema".

Pattern di codice ricorrenti:
- Costruzione DOM tramite un helper `el(tag, attrs, children)` fatto in casa
  (non JSX, non framework).
- Drag & drop delle card/liste basato su Pointer Events (non HTML5 drag&drop
  nativo, che risponde male su mobile/touch).
- Layout mobile gestito quasi sempre via media query CSS (breakpoint
  `max-width:880px`), non con rendering JS condizionale separato.

## Regole di lavoro
- **Prima di ogni modifica**, scarica sempre l'ultima versione pubblicata su
  GitHub (`git fetch`) e riparti da quella, anche a metà di una conversazione
  già avviata — non fidarti della copia locale se è passato tempo dall'ultimo
  controllo. Serve perché più sessioni di Claude Code possono lavorare in
  parallelo su parti diverse dell'app (es. una su Orto, una su Finanze): senza
  questo controllo si rischia di sovrascrivere per errore modifiche che
  un'altra sessione ha già pubblicato nel frattempo.
- **Non alzare mai `APP_VERSION`** a meno che non venga chiesto esplicitamente.
- Dopo ogni modifica, verifica la sintassi JS prima di finire (es. estrarre il
  contenuto tra i tag `<script>` e lanciare `node --check`, oppure eseguire
  l'app in locale se possibile).
- Mostra un mockup o una descrizione prima di modificare solo se lo chiedo io
  esplicitamente in anticipo ("fammi vedere prima come verrebbe..."). Se non lo
  chiedo, procedi direttamente.
- Per bug di CSS/layout che non si risolvono al primo tentativo, crea un file
  di test isolato con dati finti invece di continuare a tentare sull'app
  intera.
- Se un bug riguarda un controllo nativo del browser (`<input type="date">`,
  `<input type="file">`, ecc.), considera da subito che potrebbe avere limiti
  di stile/comportamento non aggirabili via CSS.
- Occhio ai punti "fragili" noti:
  - Nella vista Timeline di Progetti (Gantt), l'altezza di ogni riga è un
    valore fisso (`GANTT_ROW_H`) usato per calcolare la posizione assoluta
    delle barre. Qualsiasi contenuto che possa "crescere" oltre quell'altezza
    (testo che va a capo, ecc.) disallinea le barre dalle etichette — va
    sempre vincolato con overflow nascosto o gestito aumentando la costante,
    mai lasciato libero di espandersi.
  - Le date vengono sempre interpretate come `gg/mm/aaaa`. Se importi da
    Excel/CSV, non fidarti delle librerie che "indovinano" il formato data:
    per i `.csv` leggi il testo grezzo e interpretalo tu stesso; per gli
    `.xlsx` gestisci sia le celle-data vere (numero seriale) sia il testo.
- Ho accesso a GitHub direttamente da qui (a differenza della chat normale di
  Claude, dove lavoro da un file scaricato e poi te lo ridò da caricare a
  mano) — puoi quindi leggere, modificare e fare commit/push direttamente sul
  repo quando è chiaro cosa fare. Per modifiche rischiose o ambigue, chiedi
  prima di procedere.
- **Pubblica sempre subito** (commit, merge sul branch principale e push) non
  appena una modifica è pronta e verificata, senza chiedermi conferma ogni
  volta — a meno che non sia io a chiederti esplicitamente in anticipo un
  mockup o un test preventivo prima di renderla definitiva (vedi punto sopra).

## Come mi piace lavorare
- Spiegazioni chiare e dirette, senza fronzoli.
- Se una richiesta è ambigua o rischiosa da interpretare, chiedi piuttosto
  che tentare alla cieca.
