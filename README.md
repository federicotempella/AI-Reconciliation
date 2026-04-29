# Hotel ReconciliationDesk v2.0

Portale web di riconciliazione contabile per strutture alberghiere. Abbina automaticamente i movimenti elettronici Nexi con le posizioni aperte del PMS tramite motore AI con audit trail trasparente.

## Demo

Aprire `AI-Reconciliation.html` direttamente nel browser. Nessun server richiesto, nessuna dipendenza da installare.

Credenziali demo: qualsiasi username e password.

## Stack tecnico

- React 18.2 (UMD, via CDN Cloudflare)
- Babel Standalone 7.23.5 (transpiling JSX in-browser)
- Vanilla CSS con CSS custom properties (tema chiaro/scuro)
- Zero dipendenze npm
- Zero build step

## Struttura del file

Il portale è un singolo file HTML (`AI-Reconciliation.html`) con tutto il codice JSX inline nel tag `<script type="text/babel">`. La struttura interna è:
AI-Reconciliation.html
├── Head (CDN scripts: React, ReactDOM, Babel)
├── Script type="text/babel"
│   ├── Costanti e utility (H, EUR, now, timeAgo, hooks)
│   ├── Dati sintetici (CONTI, MOV, RIC, POS, DOCS, CLIENTI_DATA, ANO_DATA, UTENTI_DATA, ESPORT_DATA, TRL)
│   ├── Style helpers (C object)
│   ├── Micro components (StatoBadge, RepTag, ScenTag, ConfBar, Fld, AiTrail)
│   ├── Layout (Sidebar, Topbar, NotifBell, UserMenu)
│   ├── Sezioni operative (Dashboard, IncassiList, IncassiDetail, PosizioniList, DocumentiList, AnomalieSection)
│   ├── Sezioni anagrafiche (ClientiList, EsportazioneList)
│   ├── Sezioni sistema (UtentiSection, RegoleSection, UploadSection)
│   ├── Profilo (Profilo, CambioPassword, FAQ)
│   ├── Auth (Login)
│   └── App root + ReactDOM.createRoot
## Funzionalità implementate

### Riconciliazione
- Matching AI multi-reparto: Camere, Bar, Parcheggio
- Scenari: B2C Diretto, B2C con Conferma, B2B Agenzia, B2B Corporate
- Pattern N:1 (caparra + saldo) e 1:N (bonifico per N prenotazioni)
- Gestione cutoff mezzanotte con finestra tolleranza ±24h
- Gestione pagante ≠ intestatario con penalità confidenza
- Audit trail AI immutabile per ogni abbinamento (step-by-step)
- Confidenza: ≥85% abbinato automatico, 60-84% probabile, <60% non abbinato

### Dettaglio incasso
- Header fisso con importo distribuito / incasso / rimanenza live
- 8 tab: Distribuzione, Dati Movimento, Posizioni Aperte, Documento, Note, Clienti, Allegati, Storico
- Editing inline della distribuzione (importo, tipo doc, riferimento, conto)
- Delete All, aggiunta da posizioni aperte, PDF viewer split-screen simulato
- Azioni: Invia al Gestionale, Salva, Nota, Prenotazione, Archivia, Dissocia Doc., Download XML

### Anomalie
- Bidirezionale: Nexi senza PMS + PMS senza Nexi
- Workflow gestione: selezione causa, note libere, chiusura tracciata

### Sistema
- Gestione utenti con ruoli (Admin, Operatore, Controller, Viewer) e gruppi
- Cronologia accessi
- Regole matching configurabili (soglie, finestre, pesi per reparto)
- Upload EC / Posizioni / Documenti con drag zone e feedback
- Esportazione SAP con chiavi contabili 05/15/40/50 e Download XML
- Dark mode
- Notifiche in-app
- Badge contatori sidebar dinamici

## Dati sintetici inclusi

Il portale include dati di esempio che coprono tutti gli scenari:

| Entità | N | Scenari coperti |
|---|---|---|
| Movimenti Nexi | 8 | B2C Diretto, B2C con Conferma, B2B Agenzia, B2B Corporate |
| Riconciliazioni | 7 | 1:1 Abbinato, N:1 Caparra+Saldo, 1:N B2B, Probabile nome, Probabile cutoff, Non abbinato |
| Posizioni PMS | 12 | Camere, Parcheggio, Bar, gruppo agenzia |
| Documenti | 3 | Associato, Da elaborare OCR, Da associare |
| Clienti | 3 | Agenzia B2B, Corporate x2 |
| Anomalie | 2 | Camera senza Nexi, Parcheggio senza Nexi |
| Utenti | 4 | Admin, Operatore, Controller, DTAdmin |
| Esportazioni SAP | 2 | Completata, Fallita |

## Limitazioni note

- Babel transpiling in-browser: non adatto per file molto grandi (>2000 righe JSX). Per versioni più estese usare Vite + React.
- `import`/`export` non supportati con Babel UMD: usare `const { useState, ... } = React` e `ReactDOM.createRoot` direttamente.
- Nessuna persistenza: i dati sono in memoria e si resettano al ricaricamento della pagina.
- PDF viewer simulato: mostra dati strutturati, non PDF reali.

## Come estendere

Per aggiungere una nuova sezione:

1. Aggiungere la voce al nav array in `Sidebar`
2. Creare il componente funzione
3. Aggiungere il case in `renderPage` dentro `App`
4. Se la sezione usa dati nuovi, aggiungerli nella sezione dati prima degli style helpers

Per passare a produzione (con backend reale):

```bash
npm create vite@latest hotel-recon -- --template react
cd hotel-recon
# copiare i componenti in src/
npm install
npm run dev
```

## Versioning

Vedere `CHANGES.md` per il log completo delle modifiche per versione.

## Licenza

Uso interno — Digital Technologies S.r.l.
