
# Guida al Deployment - SalesOS

Questa applicazione è una **Static Single Page Application (SPA)** costruita con React e Vite.

Ecco le guide per distribuirla su **Vercel** (Consigliato) e **Google Cloud Run** (Containerizzato).

---

## Opzione 1: Vercel (Consigliato)
Vercel è ottimizzato per applicazioni frontend come questa. È gratuito per hobby projects, veloce e gestisce automaticamente la configurazione di routing.

### Prerequisiti
*   Account [Vercel](https://vercel.com).
*   Account GitHub con il codice pushato in un repository.

### Passaggi
1.  **Connetti GitHub a Vercel**:
    *   Vai sulla Dashboard di Vercel.
    *   Clicca su **"Add New..."** -> **"Project"**.
    *   Importa il repository di `SalesOS` da GitHub.

2.  **Configurazione Build**:
    *   Framework Preset: Vercel dovrebbe rilevare automaticamente **Vite**.
    *   Build Command: `npm run build`
    *   Output Directory: `dist`

3.  **Variabili d'Ambiente (Fondamentale)**:
    *   Nella sezione **Environment Variables**, aggiungi le chiavi del tuo progetto Supabase:
        *   `VITE_SUPABASE_URL`: La tua URL Supabase (es. `https://xyz.supabase.co`).
        *   `VITE_SUPABASE_ANON_KEY`: La tua chiave pubblica `anon` key.

4.  **Deploy**:
    *   Clicca **Deploy**.
    *   Attendi circa 1 minuto. Vercel fornirà un dominio pubblico (es. `sales-os.vercel.app`).

---

## Opzione 2: Google Cloud Run

Cloud Run esegue container stateless. Poiché l'app è statica, dobbiamo creare un container Docker che usa **Nginx** per servire i file generati da Vite.

### Prerequisiti
*   Progetto su **Google Cloud Platform (GCP)** con billing attivo.
*   **gcloud CLI** installata sul tuo computer.
*   **Docker** installato e in esecuzione.

### File di Configurazione (Già inclusi nel progetto)
*   `Dockerfile`: Configurato per build multi-stage (Node -> Nginx).
*   `nginx.conf`: Configurato per gestire il routing SPA (reindirizza tutto a index.html) e ascoltare sulla porta 8080.

### Passaggi per il Deployment

#### 1. Login e Configurazione GCP
Apri il terminale nella cartella del progetto:
```bash
# Login a Google Cloud
gcloud auth login

# Imposta il progetto ID
gcloud config set project [TUO_PROJECT_ID]

# Abilita i servizi necessari (se non già attivi)
gcloud services enable run.googleapis.com artifactregistry.googleapis.com
```

#### 2. Creazione Repository Artifact Registry
Se non hai già un repository Docker su GCP:
```bash
gcloud artifacts repositories create sales-os-repo \
    --repository-format=docker \
    --location=europe-west1 \
    --description="Repository per SalesOS"
```

#### 3. Build e Push dell'Immagine Docker
⚠️ **Nota Importante**: Le variabili `VITE_` vengono incorporate nel codice JavaScript **durante la build**. Non basta impostarle nella console di Cloud Run a runtime. Devono essere passate come `build-args`.

Sostituisci i valori con le tue vere credenziali Supabase:

```bash
# Build dell'immagine in locale (o usa Cloud Build)
gcloud builds submit --tag europe-west1-docker.pkg.dev/[TUO_PROJECT_ID]/sales-os-repo/sales-os:latest \
    --build-arg VITE_SUPABASE_URL="[LA_TUA_URL_SUPABASE]" \
    --build-arg VITE_SUPABASE_ANON_KEY="[LA_TUA_CHIAVE_ANON]" .
```

#### 4. Deploy su Cloud Run
Una volta caricata l'immagine, distribuiscila:

```bash
gcloud run deploy sales-os \
    --image europe-west1-docker.pkg.dev/[TUO_PROJECT_ID]/sales-os-repo/sales-os:latest \
    --platform managed \
    --region europe-west1 \
    --allow-unauthenticated \
    --port 8080
```

### Aggiornamenti Futuri su Cloud Run
Ogni volta che modifichi il codice:
1.  Devi rieseguire il comando `gcloud builds submit`.
2.  Devi rieseguire il comando `gcloud run deploy`.

---

## Confronto
| Caratteristica | Vercel | Google Cloud Run |
| :--- | :--- | :--- |
| **Costo** | Gratuito (Hobby) | A consumo (Free tier disponibile ma limitato) |
| **Complessità** | Molto Bassa | Media (Richiede Docker e gestione container) |
| **Performance** | Ottima (Edge Network CDN) | Buona (Container Cold Starts possibili) |
| **CI/CD** | Automatica (Git Push) | Va configurata (Cloud Build triggers) |
| **Caso d'uso** | Frontend Statico (React/Vite) | Backend complessi o Container custom |
