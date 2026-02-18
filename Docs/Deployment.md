
# Guida al Deployment - SalesOS

Questa applicazione è una **Static Single Page Application (SPA)** costruita con React e Vite.

Ecco le guide per distribuirla su **Vercel** (Consigliato) e **Google Cloud Run** (Containerizzato).

---

## Opzione 1: Vercel (Consigliato)
Vercel è ottimizzato per applicazioni frontend come questa. È gratuito per hobby projects, veloce e gestisce automaticamente la configurazione di routing.

### Prerequisiti
*   Account [Vercel](https://vercel.com).
*   Account GitHub con il codice pushato in un repository.

### Configurazione Automatica (vercel.json)
Il progetto include un file `vercel.json` che pre-configura:
*   **Framework**: Vite
*   **Build Command**: `npm run build`
*   **Output**: `dist`
*   **Rewrites**: Gestione SPA (reindirizzamento a index.html per evitare 404 al refresh)

### Passaggi per il Deploy
1.  **Connetti GitHub a Vercel**:
    *   Vai sulla Dashboard di Vercel.
    *   Clicca su **"Add New..."** -> **"Project"**.
    *   Importa il repository di `SalesOS` da GitHub.

2.  **Impostazioni di Build**:
    *   Grazie al file `vercel.json`, le impostazioni dovrebbero essere già corrette. Se richiesto, conferma `Vite` come framework preset.

3.  **Variabili d'Ambiente (CRUCIALE)**:
    *   Nella schermata di importazione progetto, espandi la sezione **Environment Variables**.
    *   Copia e incolla le seguenti coppie (i valori sono prelevati dal tuo progetto attuale):

    | Key | Value |
    | :--- | :--- |
    | `VITE_SUPABASE_URL` | `https://ewqhndyjsrplgsjrwyvl.supabase.co` |
    | `VITE_SUPABASE_ANON_KEY` | `sb_publishable_JqoQpKprStqz27WeCzfJkA_mTdLCY_Q` |

    *(Nota: Assicurati che non ci siano spazi vuoti prima o dopo i valori quando li incolli).*

4.  **Deploy**:
    *   Clicca **Deploy**.
    *   Attendi il completamento (circa 1 minuto). Vercel fornirà un dominio pubblico (es. `sales-os.vercel.app`).

---

## Opzione 2: Google Cloud Run

Cloud Run esegue container stateless. L'app viene containerizzata con Docker e servita tramite Nginx.

### Prerequisiti
*   Progetto GCP con billing attivo.
*   gcloud CLI e Docker installati.

### Passaggi
1.  **Build dell'immagine**:
    Le variabili d'ambiente devono essere inserite nell'immagine durante la build per Vite.

    ```bash
    gcloud builds submit --tag europe-west1-docker.pkg.dev/[PROJECT_ID]/repo/sales-os:latest \
        --build-arg VITE_SUPABASE_URL="https://ewqhndyjsrplgsjrwyvl.supabase.co" \
        --build-arg VITE_SUPABASE_ANON_KEY="sb_publishable_JqoQpKprStqz27WeCzfJkA_mTdLCY_Q" .
    ```

2.  **Deploy**:
    ```bash
    gcloud run deploy sales-os \
        --image europe-west1-docker.pkg.dev/[PROJECT_ID]/repo/sales-os:latest \
        --platform managed \
        --region europe-west1 \
        --allow-unauthenticated \
        --port 8080
    ```
