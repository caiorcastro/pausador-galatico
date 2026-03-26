# Pausador Galático 🌌

Scripts e documentação para gerenciar custos de infraestrutura GCP — pausando instâncias Cloud SQL e jobs Cloud Scheduler no fim do mês e reativando tudo no dia 1.

---

## Por que isso existe?

Cloud SQL cobra por hora, **mesmo quando ninguém está usando**. Se você tem vários projetos GCP com instâncias Postgres rodando 24/7, o custo acumula ao longo do mês mesmo sem tráfego.

Este repo centraliza os scripts de pausa/retomada e o inventário completo da infraestrutura.

---

## O que está aqui

| Arquivo | Conteúdo |
|---|---|
| `GCP-REPORT.md` | Inventário completo: projetos, serviços, métricas de uso e estimativas de custo |

---

## Uso rápido

### Pré-requisitos

```bash
# gcloud CLI instalado e autenticado
gcloud auth login
gcloud config set account SEU_EMAIL
```

### Pausar tudo no fim do mês

```bash
# Parar Cloud SQL (o maior custo fixo)
gcloud sql instances patch art-agent-db-v1 --project=art-agent-app --activation-policy=NEVER --quiet &
gcloud sql instances patch art-auto-db     --project=art-auto-app   --activation-policy=NEVER --quiet &
gcloud sql instances patch art-telecom-db  --project=art-telecom-app --activation-policy=NEVER --quiet &
gcloud sql instances patch art-coffee-db   --project=mari-v4         --activation-policy=NEVER --quiet &
gcloud sql instances patch d4u-social-db   --project=social-listener-490020 --activation-policy=NEVER --quiet &
gcloud sql instances patch art-finance-db  --project=art-finance-app --activation-policy=NEVER --quiet &
wait && echo "SQL pausado"

# Pausar Cloud Scheduler (impede que jobs acordem os serviços)
for JOB in art-task-enrich art-task-scraper art-report-tech art-task-rss art-task-cleanup art-report-weekly; do
  gcloud scheduler jobs pause $JOB --project=art-agent-app --location=us-central1 --quiet
done
for JOB in art-auto-weekly art-auto-db-cleanup art-auto-scraper-pesado art-auto-rss-coletor art-auto-ia-enriquecimento art-auto-monitor; do
  gcloud scheduler jobs pause $JOB --project=art-auto-app --location=us-central1 --quiet
done
for JOB in art-telecom-ia-analyzer art-telecom-rss art-telecom-system-monitor art-telecom-weekly-report; do
  gcloud scheduler jobs pause $JOB --project=art-telecom-app --location=southamerica-east1 --quiet
done
gcloud scheduler jobs pause d4u-social-market-sync-hourly \
  --project=social-listener-490020 --location=southamerica-east1 --quiet

echo "Tudo pausado!"
```

### Reativar no dia 1

```bash
# Reativar Cloud SQL
gcloud sql instances patch art-agent-db-v1 --project=art-agent-app --activation-policy=ALWAYS --quiet &
gcloud sql instances patch art-auto-db     --project=art-auto-app   --activation-policy=ALWAYS --quiet &
gcloud sql instances patch art-telecom-db  --project=art-telecom-app --activation-policy=ALWAYS --quiet &
gcloud sql instances patch art-coffee-db   --project=mari-v4         --activation-policy=ALWAYS --quiet &
gcloud sql instances patch d4u-social-db   --project=social-listener-490020 --activation-policy=ALWAYS --quiet &
gcloud sql instances patch art-finance-db  --project=art-finance-app --activation-policy=ALWAYS --quiet &
wait && echo "SQL reativado"

# Retomar Cloud Scheduler
for JOB in art-task-enrich art-task-scraper art-report-tech art-task-rss art-task-cleanup art-report-weekly; do
  gcloud scheduler jobs resume $JOB --project=art-agent-app --location=us-central1 --quiet
done
for JOB in art-auto-weekly art-auto-db-cleanup art-auto-scraper-pesado art-auto-rss-coletor art-auto-ia-enriquecimento art-auto-monitor; do
  gcloud scheduler jobs resume $JOB --project=art-auto-app --location=us-central1 --quiet
done
for JOB in art-telecom-ia-analyzer art-telecom-rss art-telecom-system-monitor art-telecom-weekly-report; do
  gcloud scheduler jobs resume $JOB --project=art-telecom-app --location=southamerica-east1 --quiet
done
gcloud scheduler jobs resume d4u-social-market-sync-hourly \
  --project=social-listener-490020 --location=southamerica-east1 --quiet

echo "Tudo reativado!"
```

### Verificar status atual

```bash
echo "=== Cloud SQL ==="
for P in art-agent-app art-auto-app art-telecom-app mari-v4 social-listener-490020 art-finance-app; do
  gcloud sql instances list --project=$P \
    --format="value(name,state)" 2>/dev/null | awk -v p="$P" '{print p" → "$0}'
done

echo ""
echo "=== Cloud Scheduler ==="
gcloud scheduler jobs list --project=art-agent-app --location=us-central1 --format="value(name,state)"
gcloud scheduler jobs list --project=art-auto-app  --location=us-central1 --format="value(name,state)"
gcloud scheduler jobs list --project=art-telecom-app --location=southamerica-east1 --format="value(name,state)"
gcloud scheduler jobs list --project=social-listener-490020 --location=southamerica-east1 --format="value(name,state)"
```

---

## Portfólio de projetos

| Projeto | Conta | Cloud Run | Cloud SQL | Scheduler |
|---|---|---|---|---|
| `art-agent-app` | Conta 2 | 2 serviços | f1-micro PG15 | 6 jobs |
| `art-telecom-app` | Conta 3 | 1 serviço | f1-micro PG15 | 4 jobs |
| `art-auto-app` | Conta 3 | 1 serviço | f1-micro PG15 | 6 jobs |
| `social-listener-490020` | Conta 3 | 2 serviços | custom-1-3840 PG16 | 1 job |
| `mari-v4` | Q1-2025 | 5 serviços | f1-micro PG15 | — |
| `lottery-searcher` | Q1-2025 | 13 serviços | — | — |
| `iris-eb1a0` | Q1-2025 | 3 serviços | — | — |
| `art-finance-app` | Conta 4 | — | f1-micro PG15 | — |
| `art-bet-app-v1` | Conta 2 | 2 serviços | f1-micro PG15 (STOPPED) | 5 jobs |
| `puxador-capivara` | Conta 2 | 1 serviço | — | — |
| `art-finance-w6x2j1` | Conta 2 | — | — | — |
| `art-coffee-d7afc` | Conta 2 | — | — | — |

Ver detalhes completos em [GCP-REPORT.md](GCP-REPORT.md).

---

## Economia estimada com a pausa

| O que para | Custo evitado/mês |
|---|---|
| 6 instâncias Cloud SQL pausadas | ~R$ 340–480 |
| 17 Cloud Scheduler jobs pausados | < R$ 5 |
| **Total** | **~R$ 340–480** |

> Cloud Run já escala a zero por padrão — não precisa pausar.

---

## Pendências abertas

- [ ] Vincular billing export ao BigQuery em cada conta (ver seção 8 do GCP-REPORT.md)
- [ ] Zerar `min-instances` do `ssrarttelecomapp` no projeto `art-bet-app-v1` via Console
- [ ] Investigar conta Q1-2025 (`open: false`) — verificar débitos e migrar projetos
- [ ] Avaliar downgrade do SQL do `social-listener-490020` (custom → f1-micro)
- [ ] Configurar alertas de budget em todas as contas ativas

---

*Gerado com [Claude Code](https://claude.ai/code) em 25/03/2026.*
