# 🕵️‍♂️ Mr. White Backend

Backend ufficiale per il gioco interattivo "Mr. White", sviluppato in **.NET 10.0** e **SignalR** per offrire un'esperienza multiplayer in tempo reale.

## 🏗️ Architettura e Overview

Il server gestisce lo stato delle partite, l'assegnazione dei ruoli (Civile o Mr. White) e la comunicazione bidirezionale tra i dispositivi dei giocatori. Le parole segrete, gli indizi e le categorie vengono caricate da un file JSON locale (`words.json`). Tutte le chiamate al backend (API e WebSocket) sono servite dietro il prefisso `/mr-white-api/`.

## ⚙️ Tech Stack

| Tecnologia               | Descrizione                                                               |
| ------------------------ | ------------------------------------------------------------------------- |
| **.NET 10.0**            | Framework principale per lo sviluppo del server                           |
| **ASP.NET Core Web API** | Fornisce le API REST e configura il server web                            |
| **SignalR**              | Gestione delle connessioni WebSocket in tempo reale                       |
| **Docker**               | Containerizzazione tramite build multi-stage (`mcr.microsoft.com/dotnet`) |
| **OpenAPI / Swagger**    | Documentazione autogenerata per le API                                    |

## 📦 Prerequisiti

- [.NET 10.0 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
- [Docker](https://www.docker.com/) (opzionale, per il deployment)

## 🚀 Setup e Avvio

### Esecuzione Locale (Sviluppo)

1. Apri il terminale nella cartella del progetto:
   ```bash
   cd MrWhite.Backend
   ```
2. Ripristina le dipendenze:
   ```bash
   dotnet restore
   ```
3. Avvia il server:
   ```bash
   dotnet run
   ```

### Esecuzione con Docker

Puoi creare e avviare il backend in un container grazie al `Dockerfile` incluso:

1. Build dell'immagine Docker:
   ```bash
   cd MrWhite.Backend
   docker build -t mr-white-backend .
   ```
2. Avvio del container (esponendo sulla porta 8080):
   ```bash
   docker run -d -p 8080:80 mr-white-backend
   ```

## 🎮 Funzionalità del Gioco e Logica

- **Gestione Stanze (Rooms)**: Creazione della stanza da parte di un host e ingresso per gli altri giocatori tramite `RoomCode`.
- **Ruoli e Regole**: Assegnazione casuale ai giocatori del ruolo `Civilian` o `MrWhite`. I civili conoscono la parola segreta, mentre Mr. White no.
- **Fasi di Gioco**: Transizione dello stato gestita dal server (es. _Lobby_, partita in corso, _Voting_).
- **Sistema di Aiuto**: Possibilità di attivare un sistema di indizi (`HintEnabled`) associati alle categorie scelte, memorizzati nello stato della `GameRoom`.
- **Sicurezza in Concorrenza**: Uso di `ConcurrentDictionary` per tracciare giocatori attivi, votazioni e stanze in esecuzione (`GameService`).

## 📡 API Reference e WebSocket

Tutti gli endpoint rispondono al base path `/mr-white-api`.

### REST API

- `GET /mr-white-api/words/categories`  
  Restituisce un array JSON contenente l'elenco delle categorie giocabili.

### WebSocket (SignalR)

- **Endpoint**: `/mr-white-api/gamehub`  
  Hub principale che gestisce la connessione iniziale (`Connected`), il join alle stanze (`JoinRoom`), il passaggio dell'host (`HostChanged`) e le disconnessioni impreviste (`UserLeft`).

## 📁 Dati e Parole

Il database delle parole è un semplice file `words.json` strutturato in categorie (es. _frutta_). Ogni categoria contiene l'array `words` e un array `hints`, utilizzati durante il gioco per le meccaniche avanzate.

---

## 🔗 Progetti Correlati

- [Mr. White Frontend](https://github.com/roberto-ingenito-home-lab/mr-white-frontend) — Interfaccia utente Next.js
- [Homelab Infrastructure](https://github.com/roberto-ingenito-home-lab/server-raspberry-pi) — Infrastruttura server e deployment Docker
