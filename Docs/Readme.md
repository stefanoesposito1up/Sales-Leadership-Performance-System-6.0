# SalesOS - Sales & Leadership Performance System 5.0

**SalesOS** è una Web App Progressive (PWA) progettata per team di vendita ad alte prestazioni. Permette il tracciamento granulare delle attività giornaliere, la pianificazione strategica degli obiettivi (Reverse Funnel Engineering) e la gestione gerarchica dei team con funzionalità di coaching assistito da AI.

## 🚀 Funzionalità Principali

### Per i Sales Representative
*   **Daily Logging Rapido**: Inserimento veloce di chiamate, appuntamenti fissati, svolti e vinti.
*   **Monitoraggio Stato Mentale**: Tracciamento di Energia, Focus e Confidenza giornalieri.
*   **Dashboard Personale**: KPI in tempo reale, proiezioni di chiusura giornata e analisi dei trend.
*   **Pianificazione Strategica**: Calcolatore "Reverse Funnel" per determinare l'attività necessaria per raggiungere i target economici.
*   **AI Coach**: Analisi automatica delle performance con diagnosi e piani d'azione suggeriti.

### Per Leader e Coach
*   **Team View**: Panoramica "Traffic Light" (Semaforo) dello stato di salute del team.
*   **Matrice Performance**: Analisi Scatter Plot (Volume vs Risultati) per segmentare il team (Leaders, Snipers, Grinders, Risk).
*   **Interventi Mirati**: Identificazione automatica dei "Top 5 da aiutare oggi".

### Per Admin
*   **Gestione Utenti**: Generazione codici invito, gestione ruoli e struttura gerarchica (Sponsor/Downline).
*   **Analytics Globali**: Visione completa su tutto il network di vendita.

## 🛠 Tech Stack

*   **Frontend**: React 19, TypeScript, Vite.
*   **UI/UX**: TailwindCSS, Lucide React (Icone), Recharts (Grafici).
*   **State Management**: React Context API (`AuthProvider`, `PlanProvider`).
*   **Backend & Database**: Supabase (PostgreSQL).
*   **Auth**: Supabase Auth con RLS (Row Level Security).
*   **PWA**: Manifest JSON e ottimizzazioni mobile-first.

## 📦 Installazione e Setup

### Prerequisiti
*   Node.js (v18+)
*   Account Supabase

### 1. Clona il repository
```bash
git clone [repo-url]
cd sales-os
npm install
```

### 2. Configurazione Ambiente
Crea un file `.env` nella root del progetto:
```env
VITE_SUPABASE_URL=latua_url_supabase
VITE_SUPABASE_ANON_KEY=latua_chiave_anon_supabase
```

### 3. Setup Database
Esegui lo script SQL fornito (`db_production_setup.sql`) nella dashboard di Supabase (SQL Editor) per creare tabelle, funzioni e policy di sicurezza.

### 4. Avvio Sviluppo
```bash
npm run dev
```

### 5. Build Produzione
```bash
npm run build
```

## 📱 Utilizzo come App Mobile
L'applicazione è ottimizzata come PWA.
1.  Apri il sito da browser mobile (Safari su iOS, Chrome su Android).
2.  Seleziona "Condividi" -> "Aggiungi a Schermata Home".
3.  L'app apparirà come nativa, senza barra degli indirizzi e a schermo intero.
