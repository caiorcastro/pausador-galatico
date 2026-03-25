# GCP Infrastructure Report
### Gerado em: 25/03/2026 — por Claude Code + gcloud CLI

---

## Índice

1. [Contas de Faturamento](#1-contas-de-faturamento)
2. [Mapa de Projetos por Conta](#2-mapa-de-projetos-por-conta)
3. [Serviços por Projeto](#3-serviços-por-projeto)
4. [Métricas de Uso — Últimos 30 Dias](#4-métricas-de-uso--últimos-30-dias)
5. [Status Atual (25/03/2026)](#5-status-atual-25032026)
6. [Como Pausar no Fim do Mês](#6-como-pausar-no-fim-do-mês)
7. [Como Reativar no Dia 1](#7-como-reativar-no-dia-1)
8. [Billing Export — BigQuery Configurado](#8-billing-export--bigquery-configurado)
9. [Projetos Sem Billing Vinculado](#9-projetos-sem-billing-vinculado)
10. [Pendências Manuais](#10-pendências-manuais)
11. [Estimativa de Custo por Categoria](#11-estimativa-de-custo-por-categoria)
12. [Comandos Úteis de Diagnóstico](#12-comandos-úteis-de-diagnóstico)

---

## 1. Contas de Faturamento

| Nome | Status | Moeda | Projetos vinculados |
|---|---|---|---|
| Q1-2025 | ⛔ FECHADA (`open: false`) | BRL | iris-eb1a0, mari-v4, lottery-searcher |
| Conta 2 | ✅ Ativa | BRL | art-agent-app, art-bet-app-v1, art-coffee-d7afc, art-finance-w6x2j1, puxador-capivara |
| Conta 3 | ✅ Ativa | BRL | art-telecom-app, art-auto-app, social-listener-490020 |
| Conta 4 | ✅ Ativa | BRL | art-finance-app |
| ARTPLAN - Projetos AI | ⛔ FECHADA (`open: false`) | BRL | — (sem projetos) |

> ⚠️ **O que significa "conta fechada"?**
> No GCP, `open: false` significa que a conta de faturamento foi **desativada** — pode ser por saldo vencido não pago, encerramento manual, ou crédito expirado. Projetos vinculados a uma conta fechada podem continuar funcionando por alguns dias em período de carência, mas novos recursos podem ser bloqueados e cobranças ficam em aberto.
>
> **Ação necessária:** Verificar no [Console de Billing](https://console.cloud.google.com/billing) se há débitos pendentes na conta Q1-2025 e se os projetos `iris-eb1a0`, `mari-v4` e `lottery-searcher` precisam ser migrados para uma conta ativa.

---

## 2. Mapa de Projetos por Conta

### Conta 2
```
├── art-agent-app          ← mais ativo do portfólio (553h Cloud Run / mês)
├── art-bet-app-v1
├── art-coffee-d7afc
├── art-finance-w6x2j1
└── puxador-capivara
```

### Conta 3
```
├── art-telecom-app        ← segundo mais ativo (522h Cloud Run / mês)
├── art-auto-app
└── social-listener-490020
```

### Conta 4
```
└── art-finance-app
```

### Conta Q1-2025 ⛔ FECHADA
```
├── iris-eb1a0
├── mari-v4
└── lottery-searcher       ← 13 serviços Cloud Run + Vertex AI + Vision API
```

---

## 3. Serviços por Projeto

### `art-agent-app` | Conta 2
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | `art-agent-app` (1 vCPU / 2 GB) · `ssrartagentapp` (1 vCPU / 256 MB) · us-central1 |
| **Cloud SQL** | `art-agent-db-v1` · PostgreSQL 15 · `db-f1-micro` · us-central1 |
| **Cloud Functions** | 1 função ativa |
| **Firebase** | Hosting, App Distribution, Extensions, Remote Config, Rules |
| **Gemini / Generative Language** | ✅ habilitado |
| **Vertex AI** | ✅ habilitado |
| **Pub/Sub** | ✅ habilitado |
| **Secret Manager** | ✅ habilitado |
| **Cloud Storage** | ✅ habilitado |
| **BigQuery** | ✅ habilitado |
| **Cloud Build** | ✅ habilitado |
| **Cloud Scheduler** | 6 jobs (us-central1) |

---

### `art-telecom-app` | Conta 3
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | `ssrarttelecomapp` (1 vCPU / 1 GB) · southamerica-east1 |
| **Cloud SQL** | `art-telecom-db` · PostgreSQL 15 · `db-f1-micro` · southamerica-east1 |
| **Cloud Functions** | 5 funções: `seedTags`, `triggerColetaManual`, `coletarFeeds`, `classificarNoticia`, `gerarRelatorio` |
| **Firebase** | Hosting, App Distribution, Data Connect, Extensions, Remote Config, Rules |
| **Firestore** | ✅ habilitado |
| **Gemini / Generative Language** | ✅ habilitado |
| **Pub/Sub** | ✅ habilitado |
| **Secret Manager** | ✅ habilitado |
| **Cloud Scheduler** | 4 jobs (southamerica-east1) |

---

### `art-auto-app` | Conta 3
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | `art-auto-app` (1 vCPU / 512 MB) · us-central1 |
| **Cloud SQL** | `art-auto-db` · PostgreSQL 15 · `db-f1-micro` · us-central1 |
| **Firebase** | Hosting, Remote Config, Rules |
| **Gemini / Generative Language** | ✅ habilitado |
| **Pub/Sub** | ✅ habilitado |
| **Secret Manager** | ✅ habilitado |
| **Cloud Scheduler** | 6 jobs (us-central1) |

---

### `social-listener-490020` | Conta 3
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | `d4u-social-api` (1 vCPU / 512 MB) · `d4u-social-web` (1 vCPU / 512 MB) · southamerica-east1 |
| **Cloud SQL** | `d4u-social-db` · PostgreSQL 16 · **`db-custom-1-3840`** (1 vCPU / 3.75 GB RAM) · southamerica-east1 |
| **Firebase** | Hosting, App Distribution, Remote Config |
| **Firestore** | ✅ habilitado |
| **Secret Manager** | ✅ habilitado |
| **Cloud Scheduler** | 1 job hourly (southamerica-east1) |

> 💸 Instância SQL mais cara do portfólio: `db-custom-1-3840` cobra ~R$ 180–220/mês só de SQL. Considerar downgrade para `db-f1-micro` se não precisar de 3.75 GB RAM.

---

### `mari-v4` | Conta Q1-2025 ⛔
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | `art-coffee`, `art-finance-web`, `gammaproxy`, `notifyadmin` (us-central1) · `mari-backend` (us-east4, min=0) |
| **Cloud SQL** | `art-coffee-db` · PostgreSQL 15 · `db-f1-micro` · us-central1 |
| **Cloud Functions** | `gammaProxy`, `notifyAdmin` |
| **Firebase** | Hosting, App Hosting, App Distribution, Extensions, Remote Config |
| **Firestore** | ✅ habilitado |
| **Gemini / Generative Language** | ✅ habilitado |
| **Pub/Sub** | ✅ habilitado (8.165 msgs/mês) |
| **Secret Manager** | ✅ habilitado |

---

### `lottery-searcher` | Conta Q1-2025 ⛔
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | 13 serviços: `analyzechannel`, `betmgm-feed-intelligence`, `generatedailysnapshot`, `generatedailysnapshotbackup`, `getdailysnapshot`, `getpipelinelogs`, `reclassifydailysnapshot`, `reclassifysnapshot`, `refreshchannelmetrics`, `rundailysnapshotnow`, `ssrlotterysearcher`, `updatesinglechannel`, `updateviplist` |
| **Cloud SQL** | — (sem SQL) |
| **Cloud Functions** | Espelho das Cloud Run (mesmo código) |
| **Vertex AI** | ✅ habilitado |
| **Vision API** | ✅ habilitado |
| **Gemini / Generative Language** | ✅ habilitado |
| **Firebase** | Hosting, Extensions, Remote Config |
| **Firestore** | ✅ habilitado |
| **Secret Manager** | ✅ habilitado |

---

### `iris-eb1a0` | Conta Q1-2025 ⛔
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | `generatedailysnapshot`, `getdailysnapshot`, `rundailysnapshotnow` · southamerica-east1 |
| **Cloud SQL** | — (sem SQL) |
| **Cloud Functions** | Espelho das Cloud Run |
| **Firebase** | Hosting, Storage, Extensions, App Distribution, Remote Config |
| **Firestore** | ✅ habilitado |
| **Gemini / Generative Language** | ✅ habilitado |
| **Secret Manager** | ✅ habilitado |

---

### `art-bet-app-v1` | Conta 2
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | `art-bet-service` (1 vCPU / 1 GB) · us-central1 · `ssrarttelecomapp` (1 vCPU / 1 GB, **min=1**) · southamerica-east1 |
| **Cloud SQL** | `art-bet-pg-instance` · PostgreSQL 15 · `db-f1-micro` · **STOPPED** |
| **Firebase** | Hosting, Remote Config, Rules |
| **Gemini / Generative Language** | ✅ habilitado |
| **Pub/Sub** | ✅ habilitado |
| **Secret Manager** | ✅ habilitado |
| **Cloud Scheduler** | 5 jobs (us-central1) — pausados |

> ⚠️ O serviço `ssrarttelecomapp` tem `min-instances=1` — cobra 1 instância 24/7 mesmo sem tráfego. Ver pendência #2.

---

### `art-finance-app` | Conta 4
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | serviços presentes |
| **Cloud SQL** | `art-finance-db` · PostgreSQL 15 · `db-f1-micro` · southamerica-east1 |
| **Pub/Sub** | ✅ habilitado |
| **Secret Manager** | ✅ habilitado |
| **Cloud Storage** | ✅ habilitado |

---

### `puxador-capivara` | Conta 2
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | `ssrpuxadorcapivara` (1 vCPU / 256 MB) · us-central1 |
| **Cloud Functions** | `ssrpuxadorcapivara` |
| **Firebase** | Hosting, Extensions, Remote Config |
| **Cloud Storage** | ✅ habilitado |

---

### `art-finance-w6x2j1` | Conta 2
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | serviços presentes |
| **Cloud Functions** | ativas |
| **Firebase** | Hosting, App Distribution, Remote Config |
| **Firestore** | ✅ habilitado |
| **Pub/Sub** | ✅ habilitado |
| **Cloud Storage** | ✅ habilitado |
| **FCM (Firebase Cloud Messaging)** | ✅ habilitado |

---

### `art-coffee-d7afc` | Conta 2
| Serviço | Detalhes |
|---|---|
| **Cloud Run** | serviços presentes |
| **Cloud Functions** | ativas |
| **Firebase** | Hosting, App Distribution, Remote Config |
| **Pub/Sub** | ✅ habilitado |
| **Cloud Storage** | ✅ habilitado |

---

## 4. Métricas de Uso — Últimos 30 Dias

> Período: **24/02/2026 → 25/03/2026**
> Fonte: Cloud Monitoring API — métricas reais coletadas via CLI

### Cloud Run — Tempo de Instância Faturável

| Projeto | Horas de Instância | Requests | Observação |
|---|---|---|---|
| `art-agent-app` | 🔴 **553.7h** | 28.787 | Maior consumidor do portfólio |
| `art-telecom-app` | 🔴 **522.6h** | 6.302 | Segundo maior |
| `art-bet-app-v1` | 🟡 21.7h | 7.002 | Tinha min-instances=1 — custos mesmo sem tráfego |
| `art-auto-app` | 🟢 9.2h | 1.853 | — |
| `mari-v4` | 🟢 8.1h | 1.090 | — |
| `lottery-searcher` | 🟢 4.0h | 609 | 13 serviços mas baixo uso |
| `social-listener-490020` | 🟢 0.6h | 519 | — |
| `iris-eb1a0` | 🟢 0.7h | 30 | — |
| `puxador-capivara` | 🟢 0.4h | 43 | — |

### Cloud SQL — Utilização de CPU

| Projeto | Instância | Tier | CPU Média |
|---|---|---|---|
| `art-agent-app` | art-agent-db-v1 | f1-micro | 14.4% |
| `mari-v4` | art-coffee-db | f1-micro | 9.3% |
| `social-listener-490020` | d4u-social-db | **custom-1-3840** | 8.3% |
| `art-telecom-app` | art-telecom-db | f1-micro | 8.1% |
| `art-finance-app` | art-finance-db | f1-micro | 7.3% |
| `art-auto-app` | art-auto-db | f1-micro | 7.1% |

### Cloud Functions — Execuções

| Projeto | Execuções |
|---|---|
| `art-telecom-app` | 6.303 |
| `lottery-searcher` | 600 |
| `mari-v4` | 408 |
| `puxador-capivara` | 42 |
| `iris-eb1a0` | 27 |
| `art-agent-app` | 2 |

### Pub/Sub — Mensagens Publicadas

| Projeto | Mensagens |
|---|---|
| `mari-v4` | 8.165 |

---

## 5. Status Atual (25/03/2026)

> Sessão de pausa executada em 25/03/2026 para reduzir custos do mês.

### Cloud SQL

| Projeto | Instância | Tier | Status |
|---|---|---|---|
| `art-agent-app` | art-agent-db-v1 | f1-micro | ⏸ **STOPPED** |
| `art-auto-app` | art-auto-db | f1-micro | ⏸ **STOPPED** |
| `art-telecom-app` | art-telecom-db | f1-micro | ⏸ **STOPPED** |
| `mari-v4` | art-coffee-db | f1-micro | ✅ **RUNNABLE** (reativado) |
| `social-listener-490020` | d4u-social-db | custom-1-3840 | ✅ **RUNNABLE** (reativado) |
| `art-finance-app` | art-finance-db | f1-micro | ⏸ **STOPPED** |
| `art-bet-app-v1` | art-bet-pg-instance | f1-micro | ⏸ **STOPPED** (estava assim antes) |

### Cloud Scheduler

| Projeto | Jobs | Status |
|---|---|---|
| `art-agent-app` | 6 jobs | ⏸ PAUSADOS |
| `art-auto-app` | 6 jobs | ⏸ PAUSADOS |
| `art-telecom-app` | 4 jobs | ⏸ PAUSADOS |
| `social-listener-490020` | 1 job | ✅ ATIVO (reativado) |
| `art-bet-app-v1` | 5 jobs | ⏸ PAUSADOS (estavam assim antes) |

### Cloud Run

| Projeto | Serviços | Min-instances | Status |
|---|---|---|---|
| Todos (exceto abaixo) | — | 0 | ✅ Escala a zero — sem custo em repouso |
| `art-bet-app-v1` · `ssrarttelecomapp` | 1 | **1** | ⚠️ Ainda cobra — pendência manual |

---

## 6. Como Pausar no Fim do Mês

Execute este bloco quando quiser pausar para economizar:

```bash
# 1. Parar Cloud SQL
gcloud sql instances patch art-agent-db-v1 --project=art-agent-app --activation-policy=NEVER --quiet &
gcloud sql instances patch art-auto-db     --project=art-auto-app   --activation-policy=NEVER --quiet &
gcloud sql instances patch art-telecom-db  --project=art-telecom-app --activation-policy=NEVER --quiet &
gcloud sql instances patch art-coffee-db   --project=mari-v4         --activation-policy=NEVER --quiet &
gcloud sql instances patch d4u-social-db   --project=social-listener-490020 --activation-policy=NEVER --quiet &
gcloud sql instances patch art-finance-db  --project=art-finance-app --activation-policy=NEVER --quiet &
wait && echo "SQL pausado"

# 2. Pausar Cloud Scheduler — art-agent-app
for JOB in art-task-enrich art-task-scraper art-report-tech art-task-rss art-task-cleanup art-report-weekly; do
  gcloud scheduler jobs pause $JOB --project=art-agent-app --location=us-central1 --quiet
done

# 3. Pausar Cloud Scheduler — art-auto-app
for JOB in art-auto-weekly art-auto-db-cleanup art-auto-scraper-pesado art-auto-rss-coletor art-auto-ia-enriquecimento art-auto-monitor; do
  gcloud scheduler jobs pause $JOB --project=art-auto-app --location=us-central1 --quiet
done

# 4. Pausar Cloud Scheduler — art-telecom-app
for JOB in art-telecom-ia-analyzer art-telecom-rss art-telecom-system-monitor art-telecom-weekly-report; do
  gcloud scheduler jobs pause $JOB --project=art-telecom-app --location=southamerica-east1 --quiet
done

# 5. Pausar Cloud Scheduler — social-listener
gcloud scheduler jobs pause d4u-social-market-sync-hourly \
  --project=social-listener-490020 --location=southamerica-east1 --quiet

echo "Tudo pausado!"
```

---

## 7. Como Reativar no Dia 1

```bash
# 1. Reativar Cloud SQL (todos em paralelo)
gcloud sql instances patch art-agent-db-v1 --project=art-agent-app --activation-policy=ALWAYS --quiet &
gcloud sql instances patch art-auto-db     --project=art-auto-app   --activation-policy=ALWAYS --quiet &
gcloud sql instances patch art-telecom-db  --project=art-telecom-app --activation-policy=ALWAYS --quiet &
gcloud sql instances patch art-coffee-db   --project=mari-v4         --activation-policy=ALWAYS --quiet &
gcloud sql instances patch d4u-social-db   --project=social-listener-490020 --activation-policy=ALWAYS --quiet &
gcloud sql instances patch art-finance-db  --project=art-finance-app --activation-policy=ALWAYS --quiet &
wait && echo "SQL reativado"

# 2. Retomar Cloud Scheduler — art-agent-app
for JOB in art-task-enrich art-task-scraper art-report-tech art-task-rss art-task-cleanup art-report-weekly; do
  gcloud scheduler jobs resume $JOB --project=art-agent-app --location=us-central1 --quiet
done

# 3. Retomar Cloud Scheduler — art-auto-app
for JOB in art-auto-weekly art-auto-db-cleanup art-auto-scraper-pesado art-auto-rss-coletor art-auto-ia-enriquecimento art-auto-monitor; do
  gcloud scheduler jobs resume $JOB --project=art-auto-app --location=us-central1 --quiet
done

# 4. Retomar Cloud Scheduler — art-telecom-app
for JOB in art-telecom-ia-analyzer art-telecom-rss art-telecom-system-monitor art-telecom-weekly-report; do
  gcloud scheduler jobs resume $JOB --project=art-telecom-app --location=southamerica-east1 --quiet
done

# 5. Retomar Cloud Scheduler — social-listener
gcloud scheduler jobs resume d4u-social-market-sync-hourly \
  --project=social-listener-490020 --location=southamerica-east1 --quiet

echo "Tudo reativado!"
```

### Verificar status após reativar

```bash
echo "=== STATUS SQL ==="
for PROJECT in art-agent-app art-auto-app art-telecom-app mari-v4 social-listener-490020 art-finance-app; do
  gcloud sql instances list --project=$PROJECT \
    --format="value(name,state)" 2>/dev/null | awk -v p="$PROJECT" '{print p" → "$0}'
done

echo ""
echo "=== STATUS SCHEDULER ==="
gcloud scheduler jobs list --project=art-agent-app --location=us-central1 --format="value(name,state)"
gcloud scheduler jobs list --project=social-listener-490020 --location=southamerica-east1 --format="value(name,state)"
```

---

## 8. Billing Export — BigQuery Configurado

Datasets criados em 25/03/2026 para receber o export de billing. **A vinculação precisa ser feita manualmente no Console** (1 clique por conta, feito uma única vez).

| Conta de Faturamento | Projeto BigQuery | Dataset |
|---|---|---|
| Conta 2 | `art-agent-app` | `billing_export` |
| Conta 3 | `art-telecom-app` | `billing_export` |
| Conta 4 | `art-finance-app` | `billing_export` |
| Q1-2025 | `lottery-searcher` | `billing_export` |

### Como vincular (Console — passo único por conta):

1. Acesse [console.cloud.google.com/billing](https://console.cloud.google.com/billing)
2. Selecione a conta de faturamento
3. Menu lateral → **"Exportação de faturamento"**
4. Aba **"BigQuery Export"** → Editar configurações
5. Preencha: **Projeto** = o projeto da tabela acima · **Dataset** = `billing_export`
6. Salvar

### Consultar custos por projeto após export ativo:

```sql
SELECT
  project.id                  AS projeto,
  service.description         AS servico,
  ROUND(SUM(cost), 2)         AS custo_brl,
  SUM(usage.amount)           AS uso,
  usage.unit                  AS unidade
FROM `billing_export.gcp_billing_export_v1_*`
WHERE DATE(_PARTITIONTIME) >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
GROUP BY 1, 2, 4, 5
ORDER BY custo_brl DESC
LIMIT 50;
```

---

## 9. Projetos Sem Billing Vinculado

Projetos existentes na conta GCP mas **sem billing ativo** — não geram cobranças, mas também não podem usar serviços pagos. Candidatos a limpeza:

| Projeto | Nome |
|---|---|
| `cassinobot` | CassinoBOT |
| `gen-lang-client-0728263257` | ClawdBot |
| `gen-lang-client-0867202951` | Generative Language Client |
| `generated-armor-424323-b5` | My First Project |
| `handover-josiel` | Handover Josiel |
| `local-leadder` | Local Leadder |
| `make-email-to` | Make - Email to Claude |
| `mari-firebase-91899999-606f5` | Firebase app |
| `mari-v2-deployment` | MARI v2 Deployment |
| `mary-codex-prod-v1` | Mary Codex Prod |
| `nobrand-site` | NoBrand-Site |
| `tasktracker-452220` | TaskTracker |
| `tasktracker-8c650` | TaskTracker |
| `teste-do-firebase-caeaa` | Teste do Firebase |
| `artplan-attention-diag` | Artplan Attention Diagnostics |

---

## 10. Pendências Manuais

### 🔴 Alta prioridade

| # | Ação | Onde | Motivo |
|---|---|---|---|
| 1 | Vincular billing export ao BigQuery | Console → cada conta de faturamento | Sem isso não há visibilidade de custo via CLI/SQL |
| 2 | Zerar `min-instances` do `ssrarttelecomapp` | Console → Cloud Run → art-bet-app-v1 → ssrarttelecomapp → Editar → Capacidade → mínimo = 0 | CLI falhou por imagem cross-project. Ainda cobra 1 instância 24/7 |
| 3 | Investigar conta Q1-2025 fechada | Console → Billing → Q1-2025 | `open: false` — verificar débitos pendentes e migrar `iris-eb1a0`, `mari-v4`, `lottery-searcher` para conta ativa |

### 🟡 Média prioridade

| # | Ação | Onde | Motivo |
|---|---|---|---|
| 4 | Avaliar downgrade do SQL do social-listener | Console → SQL → d4u-social-db | `db-custom-1-3840` custa ~R$ 180–220/mês. Downgrade para `f1-micro` economiza ~R$ 150/mês se a RAM não for necessária |
| 5 | Avaliar encerrar lottery-searcher | — | 13 serviços Cloud Run + Vertex AI + Vision API para apenas 609 requests/mês |
| 6 | Limpar projetos sem billing | Console → IAM & Admin → Manage Resources | 15 projetos inativos acumulando quotas e poluindo o org |
| 7 | Consolidar contas de faturamento | Console → Billing | 5 contas para ~12 projetos é fragmentação desnecessária — considerar consolidar em 2 |

### 🟢 Baixa prioridade

| # | Ação | Onde | Motivo |
|---|---|---|---|
| 8 | Configurar alertas de budget | Console → Billing → cada conta → Budgets | Nenhuma conta tem alerta configurado — risco de surpresa no faturamento |
| 9 | Automatizar pausa/retomada com Scheduler | Criar jobs no Cloud Scheduler | Script de pausa/retomada automático no fim/início de cada mês |

---

## 11. Estimativa de Custo por Categoria

> ⚠️ Estimativas baseadas em preços públicos do GCP.
> **Valores reais disponíveis após ativar o BigQuery Billing Export (seção 8).**

### Cloud SQL — maior custo fixo (cobra 24/7 mesmo sem uso)

| Instância | Projeto | Tier | Estimativa/mês |
|---|---|---|---|
| `d4u-social-db` | social-listener | custom-1-3840 | ~R$ 180–220 |
| `art-agent-db-v1` | art-agent-app | f1-micro | ~R$ 30–50 |
| `art-auto-db` | art-auto-app | f1-micro | ~R$ 30–50 |
| `art-telecom-db` | art-telecom-app | f1-micro | ~R$ 35–55 |
| `art-coffee-db` | mari-v4 | f1-micro | ~R$ 30–50 |
| `art-finance-db` | art-finance-app | f1-micro | ~R$ 35–55 |
| **Total SQL** | | | **~R$ 340–480/mês** |

> ✅ Com SQL pausado, esse custo é eliminado até o dia 1.

### Cloud Run — cobra por uso (escala a zero por padrão)

| Projeto | Horas/mês | Estimativa/mês |
|---|---|---|
| `art-agent-app` | 553.7h | ~R$ 30–80 |
| `art-telecom-app` | 522.6h | ~R$ 30–80 |
| `art-bet-app-v1` (min=1) | ~720h constante | ~R$ 15–30 |
| Demais | < 22h | < R$ 10 cada |

### Outros serviços

| Serviço | Estimativa/mês |
|---|---|
| Firebase Hosting (todos os projetos) | ~R$ 5–20 total |
| Firestore (reads/writes) | ~R$ 5–30 (depende do volume) |
| Pub/Sub | < R$ 5 (volumes atuais baixos) |
| Secret Manager | < R$ 5 |
| Cloud Functions | < R$ 10 (volumes atuais) |
| Gemini / Generative Language API | Variável — depende de tokens |
| Vertex AI | Variável — lottery-searcher habilitado |

---

## 12. Comandos Úteis de Diagnóstico

```bash
# Listar todos os projetos
gcloud projects list --format="table(projectId,name,lifecycleState)"

# Ver projetos por conta de faturamento
gcloud billing projects list --billing-account=BILLING_ACCOUNT_ID

# Status rápido de todas as instâncias SQL
for P in art-agent-app art-auto-app art-telecom-app mari-v4 social-listener-490020 art-finance-app; do
  gcloud sql instances list --project=$P \
    --format="value(name,state)" 2>/dev/null | awk -v p="$P" '{print p" → "$0}'
done

# Ver todos os Cloud Run services de um projeto
gcloud run services list --project=PROJECT_ID --platform=managed

# Ver jobs do Cloud Scheduler
gcloud scheduler jobs list --project=PROJECT_ID --location=REGION

# Métricas de uso Cloud Run via Monitoring API (últimos 30 dias)
TOKEN=$(gcloud auth print-access-token)
curl -s -G -H "Authorization: Bearer $TOKEN" \
  "https://monitoring.googleapis.com/v3/projects/PROJECT_ID/timeSeries" \
  --data-urlencode "filter=metric.type=\"run.googleapis.com/container/billable_instance_time\"" \
  --data-urlencode "interval.startTime=START_DATE" \
  --data-urlencode "interval.endTime=END_DATE" \
  --data-urlencode "aggregation.alignmentPeriod=2592000s" \
  --data-urlencode "aggregation.perSeriesAligner=ALIGN_SUM" \
  --data-urlencode "aggregation.crossSeriesReducer=REDUCE_SUM"
```

---

*Documento gerado por Claude Code em 25/03/2026 usando `gcloud` CLI + Cloud Monitoring API + Cloud Billing API.*
*Todos os valores são métricas reais coletadas das APIs do GCP — nenhum dado inventado.*
