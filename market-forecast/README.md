# Market Forecast

> Previsao de direcao de preco (alta/baixa) em indices financeiros usando Machine Learning com validacao temporal realista.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![Version](https://img.shields.io/badge/version-3.2.0-informational)](#)
[![Tests](https://img.shields.io/badge/tests-267%20passing-brightgreen)](#)
[![License: All Rights Reserved](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)](#licenca)

---

## Sobre o Projeto

Sistema de classificacao binaria que preve se o preco de fechamento de indices financeiros vai subir ou descer em relacao a abertura do mesmo dia.

Cobre o pipeline completo: coleta de dados de multiplas fontes, engenharia de 20 atributos (tecnicos, macroeconomicos e de sentimento), treinamento com validacao temporal e backtest com custos operacionais reais.

---

## Resultados (IBOVESPA, 2017-2024)

| Modelo | Directional Accuracy | Retorno Acumulado | Indice de Sharpe |
|--------|----------|-------------------|------------------|
| SARIMA (referencia) | ~50% | -- | -- |
| RNN / GRU | ~51% | -- | -- |
| LSTM Classificador | 48.6% | -21.9% | -1.62 |
| **XGBoost (WFV)** | **57.6%** | **+354%** | **1.56** |
| XGBoost + Macro (VIX+DXY) | 57.5% | +364% | 1.59 |
| **XGBoost + Optuna** | **59.0%** | **+547%** | **1.80** |
| Limiar >= 65% | -- | +485% | 3.23 |
| **Limiar >= 70%** | -- | +330% | **3.89** |

**Nota:** Resultados de backtest educacional com Walk-Forward Validation. Nao constitui recomendacao de investimento.

### Interpretacao dos Resultados

- Em mercados financeiros, acertos acima de 55% ja sao relevantes. O modelo alcanca 59% com otimizacao bayesiana.
- O Indice de Sharpe de 3.89 (com filtro de confianca >= 70%) indica retorno ajustado ao risco muito acima do usual. Isso ocorre porque o modelo opera menos, mas com maior precisao.
- Modelos de series temporais classicos (ARIMA) e redes recorrentes (LSTM, GRU) nao superaram o acaso neste cenario, indicando que a engenharia de atributos foi mais determinante que a arquitetura do modelo.

### Comparacao Multi-Index (XGBoost + Macro)

| Indice       | Acuracia | vs Baseline | Retorno | Sharpe | Max Drawdown |
|--------------|----------|-------------|---------|--------|--------------|
| **IBOVESPA** | **57.5%**| **+5.6pp**  |**+364%**| **1.59**| -22.0%      |
| NASDAQ       | 53.4%    | -1.2pp      | -64.0%  | -1.13  | -66.4%       |
| DOW JONES    | 53.6%    | -0.4pp      | -23.9%  | -0.33  | -29.9%       |
| BSE SENSEX   | 52.5%    | +5.5pp      | -67.7%  | -2.90  | -67.9%       |

> IBOVESPA e o unico mercado consistentemente lucrativo — mercados desenvolvidos (NASDAQ, DJI) sao mais eficientes e dificeis de prever.

---

## Indices Analisados

| Indice       | Pais   | Ticker   | Periodo     | Dias  |
|--------------|--------|----------|-------------|-------|
| IBOVESPA     | Brasil | `^BVSP`  | 2016-2024   | 2,177 |
| BSE SENSEX   | India  | `^BSESN` | 2016-2024   | ~2,200|
| NASDAQ       | EUA    | `^IXIC`  | 2016-2024   | ~2,200|
| Dow Jones    | EUA    | `^DJI`   | 2016-2024   | ~2,200|

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
    |-- LightGBM / CatBoost (gradient boosting alternativo)
    |-- ARIMA / SARIMA (baseline estatistico)
    |-- Prophet (Meta)
    |-- Deep Learning: RNN, LSTM, GRU
    |-- Stacking Ensemble (meta-learner)
    |
    v
Avaliacao & Backtest
    |-- Simulacao day trading com custos (0.10%/operacao)
    |-- Threshold Analysis (filtragem por confianca)
    |-- Blocked Time-Series CV (mean +/- std por blocos temporais)
    |-- SHAP Feature Importance
    |-- Metricas: Acuracia, Sharpe, Max Drawdown, Win Rate
```

---

## Metodologia

### Walk-Forward Validation

Diferente de uma divisao fixa treino/teste, a Walk-Forward Validation simula o uso real:

- Para cada dia `t`, o modelo treina com todo o historico `[0, t-1]` e preve apenas o dia `t`
- Retreina a cada 5 dias uteis (semanalmente)
- Elimina vazamento de dados por construcao — o modelo nunca ve dados futuros

### Otimizacao de Hiperparametros

- **Optuna** com poda automatica (MedianPruner)
- Busca bayesiana sobre 8 hiperparametros do XGBoost
- Validacao cruzada temporal (TimeSeriesSplit)

### Analise de Limiar de Confianca

O modelo filtra operacoes por nivel de confianca: com limiar >= 70%, o Indice de Sharpe salta de 1.80 para 3.89, trocando volume por precisao.

### Validacao Estatistica

- Permutation test: p=0.001 para XGBoost Optuna (resultado estatisticamente significativo)
- Blocked time-series CV para metricas com mean +/- std sem quebrar ordem temporal
- Bootstrap confidence intervals

---

## Atributos de Entrada (20)

**Tecnicos (14):** RSI, MACD + Histograma, Bollinger Bands, Volatilidade Realizada, Medias Moveis (7/21/50 dias), Momentum, Gap de Abertura, Retorno e Amplitude do dia anterior, Variacao de Volume.

**Macroeconomicos (3):** VIX (nivel e variacao), Dollar Index (DXY).

**Sentimento (3):** CNN Fear & Greed (score, zona categorica, momentum 5 dias).

> SHAP feature selection validou 14/14 features tecnicas como relevantes.

---

## Tecnologias

| Categoria | Tecnologias |
|-----------|-------------|
| ML / Modelos | XGBoost, LightGBM, CatBoost, Optuna, scikit-learn |
| Deep Learning | TensorFlow / Keras (RNN, LSTM, GRU) |
| Ensemble | Stacking meta-learner (LogisticRegression/XGBoost) |
| Series Temporais | statsmodels (ARIMA), Prophet |
| Interpretabilidade | SHAP (feature importance + selection) |
| Dados | yfinance, pandas-datareader, pandas, NumPy |
| Qualidade | pytest (267 testes), black, isort, flake8, GitHub Actions |

---

## Estrutura do Projeto

```
Market_Forecast/
├── notebooks/
│   ├── 01-eda.ipynb        # Analise exploratoria e visualizacao
│   ├── 02-models.ipynb     # Treinamento de todos os modelos
│   └── 03-backtest.ipynb   # Backtest, multi-index, validacao OOT
├── src/
│   ├── config.py           # Constantes centralizadas
│   ├── data/               # Loader com 5 fontes de fallback
│   ├── features/           # Feature engineering (20 features)
│   ├── models/             # XGBoost, LightGBM, CatBoost, LSTM, Ensemble
│   ├── evaluation/         # Backtest, threshold analysis, metricas
│   └── utils/              # Helpers, statistics (blocked CV, bootstrap CI)
├── tests/                  # 267 testes unitarios + integracao (pytest)
├── experiments/            # Configs YAML por experimento
├── scripts/                # Scripts auxiliares
├── .github/workflows/      # CI/CD (testes + lint)
├── pyproject.toml          # Configuracao do projeto
├── Makefile                # Comandos padronizados
└── Dockerfile              # Container para reproducibilidade
```

---

## Contexto Academico

- Acuracia direcional de 55-59% com WFV e resultado genuinamente bom na literatura (Lo & MacKinlay, 1999; Gu et al., 2020)
- XGBoost/LightGBM dominam LSTM em features tabulares para previsao financeira
- 97% dos day traders perdem dinheiro a longo prazo (Barber & Odean, 2000)
- O modelo supera consistentemente o Buy & Hold no IBOVESPA com Sharpe > 1.5

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

## Licenca

Todos os direitos reservados (All Rights Reserved).

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
