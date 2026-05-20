# Supplier ESG Intelligence

Solução analítica funcional para classificação de risco, maturidade e criticidade ESG de fornecedores da Edenred Brasil.

## Informações do Projeto

| | |
|---|---|
| **Disciplina** | Machine Learning I — Projeto 3 |
| **Instituição** | CESAR School |
| **Google Sites** | https://sites.google.com/cesar.school/nome-do-clube/solu%C3%A7%C3%A3o |

## Membros

| Nome | GitHub |
|---|---|
| [Davi Alves de Melo] | [@DaviMelooficial](https://github.com/DaviMelooficial) |
| [Henrique Bouwman] | [@henriquebouwman](https://github.com/henriquebouwman) |
| [João Pedro Lins] | [@jplacet1](https://github.com/jplacet1) |
| [Alana Cecília Costa] | [@alana](https://github.com/alana) |


## Stack

| Camada | Tecnologia |
|---|---|
| Frontend | Next.js 14 + Tailwind CSS + TypeScript |
| Backend | Python 3.11 + FastAPI + SQLAlchemy |
| Banco de Dados | SQLite (persistido em volume Docker) |
| Orquestração | Docker Compose |

## Pré-requisitos

- Docker e Docker Compose instalados
- Portas 3000 e 8000 disponíveis

## Como rodar

```bash
# Clone o projeto
cd supplier-esg-intelligence

# Subir toda a stack
docker compose up --build

# Acesse:
# Frontend: http://localhost:3000
# Backend API: http://localhost:8000
# Docs da API: http://localhost:8000/docs
```

## Funcionalidades do MVP

### 1. Dashboard Executivo
- KPIs de topo: total de fornecedores, média de maturidade, % críticos, confiança
- Matriz de criticidade 2×2 (Risco × Maturidade)
- Distribuição por cluster de risco
- Top setores em risco

### 2. Listagem de Fornecedores
- Filtros por quadrante, setor e porte
- Busca por nome
- Paginação
- Colunas: maturidade, risco, criticidade, quadrante

### 3. Scorecard Individual
- Scores por dimensão (Ambiental, Social, Governança, Cadeia)
- Detalhamento de práticas (✓/✗)
- Plano de ação recomendado baseado no quadrante

### 4. Importação de Dados
- Import via JSON
- Seeder de dados sintéticos para demonstração

## Motor de Scoring

```
Score de Maturidade = soma das práticas ESG com pesos por categoria (0–100)
Score de Risco      = 100 - maturidade + amplificadores de risco (sanções, multas, etc.)
Score de Confiança  = % de campos preenchidos com consistência
Score de Criticidade = (Risco × 0.5) + (max(0, 60 - Maturidade) × 0.5)
```

### Quadrantes da Matriz

| Quadrante | Condição | Ação |
|---|---|---|
| 🔴 Atenção Crítica | Risco ≥ 60 e Maturidade < 50 | Auditoria imediata |
| 🟠 Mitigação | Risco ≥ 60 e Maturidade ≥ 50 | Plano corretivo 90 dias |
| 🟡 Desenvolvimento | Risco < 60 e Maturidade < 50 | Capacitação e reforço documental |
| 🟢 Monitoramento | Risco < 60 e Maturidade ≥ 50 | Acompanhamento semestral |

## Estrutura do Projeto

```
supplier-esg-intelligence/
├── backend/
│   ├── app/
│   │   ├── main.py          # FastAPI app
│   │   ├── models.py        # ORM models
│   │   ├── schemas.py       # Pydantic schemas
│   │   ├── scoring.py       # ESG scoring engine
│   │   ├── seed.py          # Dados sintéticos
│   │   └── routers/         # Endpoints
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── app/                 # Next.js App Router
│   ├── components/          # UI components
│   ├── lib/api.ts           # API client
│   └── Dockerfile
└── docker-compose.yml
```

## API Endpoints

| Método | Endpoint | Descrição |
|---|---|---|
| GET | /api/v1/dashboard/kpis | KPIs agregados |
| GET | /api/v1/dashboard/matrix | Dados para matriz |
| GET | /api/v1/suppliers | Lista com filtros |
| GET | /api/v1/suppliers/{id} | Detalhes do fornecedor |
| GET | /api/v1/suppliers/{id}/action-plan | Plano de ação |
| POST | /api/v1/import | Importar dados |
| POST | /api/v1/seed | Carregar dados sintéticos |
| GET | /docs | Swagger UI |
