# Market Forecast

> Sistema de previsao de direcao de preco (alta/baixa) em indices financeiros globais usando Machine Learning com Walk-Forward Validation.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![Version](https://img.shields.io/badge/version-3.1.0-informational)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](#licenca)

---

## Sobre o Projeto

Market Forecast e um sistema completo de **classificacao binaria** que preve se o preco de fechamento de indices financeiros vai **subir ou descer em relacao a abertura do mesmo dia**.

O projeto cobre todo o pipeline de data science: coleta de dados, feature engineering, treinamento de multiplos modelos, validacao temporal realista e backtest com custos operacionais.

---

## Resultados (IBOVESPA 2017-2024)

| Modelo                    | Acuracia | Retorno Acumulado | Sharpe Ratio |
|---------------------------|----------|-------------------|--------------|
| SARIMA (baseline)         | ~50%     | --                | --           |
| RNN / GRU                 | ~51%     | --                | --           |
| LSTM                      | 51.4%    | --                | --           |
| **XGBoost (WFV)**         | **57.6%**| **+354%**         | **1.56**     |
| **XGBoost + Optuna HPO**  | **59.0%**| **+547%**         | **1.80**     |
| Threshold >= 65%          | --       | +485%             | 3.23         |
| **Threshold >= 70%**      | --       | +330%             | **3.89**     |

> **Nota:** Resultados sao de backtest educacional com Walk-Forward Validation. Nao constituem recomendacao de investimento.

---

## Indices Analisados

| Indice       | Pais   | Ticker   | Periodo     |
|--------------|--------|----------|-------------|
| IBOVESPA     | Brasil | `^BVSP`  | 2016-2024   |
| BSE SENSEX   | India  | `^BSESN` | 2016-2024   |
| NASDAQ       | EUA    | `^IXIC`  | 2016-2024   |
| Dow Jones    | EUA    | `^DJI`   | 2016-2024   |

---

## Arquitetura

```
Dados (yfinance, Stooq, CNN Fear & Greed)
    |
    v
Loader (5 fontes de fallback + cache CSV)
    |
    v
Feature Engineering (20 features)
    |-- Tecnicas: RSI, MACD, Bollinger Bands, MAs, Momentum, Gap
    |-- Macro: VIX, Dollar Index (DXY)
    |-- Sentimento: CNN Fear & Greed Index
    |
    v
Modelos
    |-- XGBoost + Walk-Forward Validation (principal)
    |-- ARIMA / SARIMA (baseline)
    |-- Prophet (Meta)
    |-- Deep Learning: RNN, LSTM, GRU
    |
    v
Backtest & Analise
    |-- Simulacao day trading com custos (0.10%/operacao)
    |-- Threshold Analysis (filtragem por confianca)
    |-- Metricas: Acuracia, Sharpe, Max Drawdown, Win Rate
```

---

## Metodologia

### Walk-Forward Validation (WFV)

Diferente de um split treino/teste fixo, o WFV simula o uso real do modelo:

- Para cada dia `t`: treina com **todo o historico** `[0, t-1]`, preve apenas o dia `t`
- Retreina a cada 5 dias uteis (semanal)
- **Elimina data leakage por design** — o modelo nunca ve dados futuros

### Otimizacao de Hiperparametros

- **Optuna** com pruning automatico (MedianPruner)
- Busca bayesiana sobre 8 hiperparametros do XGBoost
- Validacao cruzada temporal (TimeSeriesSplit)

### Threshold Analysis

- Filtragem de operacoes por nivel de confianca do modelo
- Threshold >= 70% alcanca **Sharpe 3.89** (troca volume por precisao)

---

## Stack Tecnologica

| Categoria          | Tecnologias                                    |
|--------------------|------------------------------------------------|
| **Linguagem**      | Python 3.9+                                    |
| **ML / Modelos**   | XGBoost, Optuna, scikit-learn                  |
| **Deep Learning**  | TensorFlow / Keras (RNN, LSTM, GRU)            |
| **Series Temporais** | statsmodels (ARIMA), Prophet (Meta)          |
| **Dados**          | yfinance, pandas-datareader, pandas, NumPy     |
| **Visualizacao**   | Matplotlib, Seaborn                            |
| **CI/CD**          | GitHub Actions (pytest + flake8 + black)       |
| **Testes**         | pytest, pytest-cov, Codecov                    |
| **Qualidade**      | black, isort, flake8, pre-commit               |

---

## Features de Entrada (20 total)

### Tecnicas (14)
RSI (14d), MACD + Histograma, Bollinger Band Width, Volatilidade Realizada (30d), Medias Moveis (7/21/50d), Momentum (3d/5d), Gap de Abertura, Retorno D-1, Amplitude D-1, Variacao de Volume.

### Macro (3)
VIX Level, VIX Change, Dollar Index (DXY) Change.

### Sentimento (3)
CNN Fear & Greed Score, Zona Categorica, Momentum F&G (5d).

---

## Estrutura do Projeto

```
Market_Forecast/
├── notebooks/           # Notebook principal (120+ celulas)
├── src/
│   ├── config.py        # Constantes centralizadas
│   ├── data/            # Loader com 5 fontes de fallback
│   ├── features/        # Feature engineering (20 features)
│   ├── models/          # XGBoost, ARIMA, Prophet, Deep Learning
│   └── utils/           # Helpers
├── tests/               # Testes unitarios + integracao (pytest)
├── scripts/             # Scripts auxiliares
├── .github/workflows/   # CI/CD (testes + lint)
├── pyproject.toml       # Configuracao do projeto
├── Makefile             # Comandos padronizados
└── Dockerfile           # Container para reproducibilidade
```

---

## Autor

**Fernando Marciano**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fernando_Marciano-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/fernandomarciano/)

---

## Licenca

Distribuido sob a licenca MIT.

---

> **Interessado no codigo-fonte ou em uma demonstracao?** Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandomarciano/).
