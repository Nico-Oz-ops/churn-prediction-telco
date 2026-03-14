# Churn Prediction — Telco Customer Dataset

Progetto di Machine Learning per la previsione dell'abbandono clienti (churn) in un'azienda di telecomunicazioni, sviluppato nell'ambito del percorso **Professional Data Analyst** presso **ITS ICTAccademy** (Roma).

---

## Obiettivo

Costruire un modello di classificazione binaria in grado di prevedere se un cliente abbandonerà o meno il servizio (`Churn = Yes / No`), con l'obiettivo di supportare strategie di **retention proattiva**.

---

## Struttura della Repository

```
Churn-Prediction-Telco/
│
├── README.md
├── flow/                  # Export del Flow Dataiku
└── presentation/          # Presentazione del progetto (Gamma)
```

---

## Dataset

| Caratteristica | Dettaglio |
|---|---|
| Fonte | Telco Customer Churn Dataset |
| Righe | 7.043 clienti |
| Colonne | 22 features + 1 target (Churn) |
| Distribuzione target | 73.5% No — 26.5% Yes |
| Sbilanciamento | Lieve (rapporto ~3:1) — non richiede tecniche di bilanciamento |

### Features principali

- **Tipo di contratto** — Month-to-month, One year, Two year
- **Tenure** — Fascia di anzianità del cliente (0-12, 13-24, 25-48, >48 mesi)
- **Servizi attivi** — Internet service, Online Security, Phone Service
- **Dati economici** — MonthlyCharges, TotalCharges

---

## Tool e Tecnologie

| Tool | Utilizzo |
|---|---|
| **Dataiku DSS** | Pipeline ML end-to-end, Feature Engineering, Modello, Scoring, Dashboard e Reportistica |

---

## Pipeline del Progetto

```
Telco_raw
    ↓ [Prepare Recipe]
Telco_prepared
    ↓ [Prepare Recipe]
Telco_features
    ↓ [Visual ML — Random Forest]
Predict Churn (binary)
    ↓                                       ↓
[Score Recipe — Trofeo 1]       [Score Recipe — Trofeo 2]
    ↓                                       ↓
Telco_test_scored                  telco_predictions
(valutazione modello)            (50 nuovi clienti futuri)
```

---

## Modello

| Parametro | Valore |
|---|---|
| Algoritmo | Random Forest |
| Soglia (threshold) | 0.400 |
| Ottimizzazione soglia | Privilegiare Recall (problema di business) |

### Metriche di Performance

| Metrica | Valore |
|---|---|
| **AUC-ROC** | 0.854 |
| **Recall** | 89% |
| **Precision** | 46% |
| **Accuracy** | 70% |
| **F1-Score** | 61% |

---

## Scelta della Soglia — Ragionamento di Business

La soglia è stata abbassata da **0.6 (ottimale per F1)** a **0.4**, privilegiando la **Recall** rispetto alla Precision.

**Motivazione:**
- Il costo di un **falso negativo** (cliente che abbandona senza essere intercettato) è molto più alto del costo di un **falso positivo** (offerta di retention a un cliente che sarebbe rimasto)
- Con soglia 0.4 → Recall **89%** — il modello intercetta quasi 9 clienti a rischio su 10

---

## Insight Principali

I valori SHAP mostrano che le feature più influenti sono:

1. **Contract - Month-to-month** → alto valore = alto rischio churn (42.71% churn rate)
2. **Contract - Two year** → alto valore = basso rischio churn (2.83% churn rate)
3. **IS - Fiber optic** → associato a maggiore probabilità di abbandono
4. **OS - No** → assenza di Online Security aumenta il rischio
5. **TG - 0-12 mesi** → clienti nuovi sono i più vulnerabili

### Profilo cliente ad alto rischio
> Contratto mensile + Fibra ottica + Nessuna sicurezza online + Tenure 0-12 mesi

### Profilo cliente fedele
> Contratto biennale + Tenure >48 mesi

---

## Previsioni su Nuovi Clienti

Il modello è stato applicato a un dataset di **50 nuovi clienti** (`telco_future_customers`):

| Previsione | Clienti | % |
|---|---|---|
| Churn = No | 35 | 70% |
| Churn = Yes | 15 | 30% |

---

## Presentazione del Progetto

[Visualizza la presentazione su Gamma](https://gamma.app/docs/Analisi-del-Churn-in-unazienda-Telco-g57evcsu8u0m7pb)

---


## Autore

**Nicolás Valenzuela**
Studente Professional Data Analyst — ITS ICTAccademy, Roma
[GitHub](https://github.com/Nico-Oz-ops)
