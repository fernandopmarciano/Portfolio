# Market Forecast

> Previsao de direcao de preco (alta/baixa) em indices financeiros usando ensemble de Gradient Boosting com Walk-Forward Validation.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![Version](https://img.shields.io/badge/version-3.2.0-informational)](#)
[![Tests](https://img.shields.io/badge/tests-274%20passing-brightgreen)](#)
[![License: All Rights Reserved](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)](#licenca)

---

## Sobre o Projeto

Sistema de classificacao binaria que preve se o preco de fechamento de indices financeiros vai subir ou descer em relacao a abertura do mesmo dia.

Pipeline completo: coleta de dados de multiplas fontes, engenharia de 20 atributos (tecnicos, macroeconomicos e de sentimento), treinamento com validacao temporal, ensemble de 3 modelos gradient boosting e backtest com custos operacionais reais.

---

## Resultados (IBOVESPA, 2017-2024)

### Blocked Time-Series CV (5 blocos temporais)

| Modelo | Acc BlockedCV | Sharpe BlockedCV | Retorno |
|--------|---------------|------------------|---------|
| XGBoost Base (WFV) | 57.0% +/- 2.2% | 1.318 +/- 0.321 | +375.9% |
| XGBoost + Macro (VIX+DXY) | 57.5% +/- 2.6% | 1.306 +/- 0.257 | +364.0% |
| XGBoost + Optuna HPO | 59.4% +/- 0.6% | 1.640 +/- 0.440 | +633.2% |
| LightGBM + Optuna | 59.8% +/- 1.9% | 1.865 +/- 0.493 | +787.7% |
| CatBoost + Optuna | 59.5% +/- 1.2% | 1.869 +/- 0.390 | +897.5% |
| **Stacking Ensemble** | **60.2% +/- 1.4%** | **2.315 +/- 0.408** | **+1367.9%** |
| LSTM Classificador | 52.8% +/- 2.3% | -0.506 +/- 1.520 | -9.1% |

**Nota:** Resultados de backtest educacional com Walk-Forward Validation. Nao constitui recomendacao de investimento.

### Threshold Analysis (XGBoost Optuna)

| Limiar | Operacoes | Acc | Retorno | Sharpe | Max Drawdown |
|--------|-----------|-----|---------|--------|--------------|
| >= 50% | 1,185 (61%) | 59.2% | +633.2% | 1.906 | -23.4% |
| >= 55% | 879 (46%) | 62.7% | +1227.7% | 3.322 | -18.6% |
| >= 60% | 612 (32%) | 63.6% | +924.5% | 4.327 | -17.2% |
| >= 65% | 305 (16%) | 62.6% | +309.6% | 4.626 | -8.3% |
| >= 70% | 79 (4%) | 68.4% | +46.1% | 5.886 | -3.4% |

### Baselines de Regressao

| Modelo | Metrica | Valor |
|--------|---------|-------|
| ARIMA | RMSE | 21,261 |
| Prophet | RMSE | 17,617 |
| RNN | MAE / R2 | 3,858 / 0.614 |
| LSTM (regressao) | MAE / R2 | 6,557 / 0.060 |
| GRU | MAE / R2 | 8,591 / -0.417 |

### Interpretacao dos Resultados

- Em mercados financeiros, acertos acima de 55% ja sao relevantes. O ensemble alcanca 60.2% com blocked CV estavel (+/- 1.4%).
- O Sharpe de 2.31 do Stacking indica retorno ajustado ao risco consistente. Com threshold >= 60%, o Sharpe salta para 4.33.
- Gradient boosting (XGBoost, LightGBM, CatBoost) domina LSTM em features tabulares — a engenharia de atributos foi mais determinante que a arquitetura do modelo.
- **Validacao estatistica:** Permutation test p=0.0005 confirma que os resultados sao significativos.

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
Modelos (com persistencia em disco)
    |-- XGBoost + Walk-Forward Validation (principal)
    |-- LightGBM + Optuna HPO
    |-- CatBoost + Optuna HPO
    |-- Stacking Ensemble (meta-learner sobre os 3 acima)
    |-- ARIMA / SARIMA (baseline estatistico)
    |-- Prophet (Meta)
    |-- Deep Learning: RNN, LSTM, GRU
    |
    v
Avaliacao & Backtest
    |-- Simulacao day trading com custos (0.10%/operacao)
    |-- Threshold Analysis (filtragem por confianca)
    |-- Blocked Time-Series CV (mean +/- std por blocos temporais)
    |-- Bootstrap CI + Permutation Test
    |-- SHAP Feature Importance
```

---

## Metodologia

### Walk-Forward Validation

Diferente de uma divisao fixa treino/teste, a Walk-Forward Validation simula o uso real:

- Para cada dia `t`, o modelo treina com todo o historico `[0, t-1]` e preve apenas o dia `t`
- Retreina a cada 5 dias uteis (semanalmente)
- Elimina vazamento de dados por construcao — o modelo nunca ve dados futuros

### Otimizacao de Hiperparametros

- **Optuna** com busca bayesiana (TPE sampler)
- 5-fold TimeSeriesSplit para XGBoost, LightGBM e CatBoost
- Cada modelo otimizado independentemente (20-30 trials)

### Stacking Ensemble

- Meta-learner (LogisticRegression) sobre probabilidades calibradas
- Combina XGBoost Optuna + LightGBM + CatBoost
- Melhor resultado geral: 60.2% acc, Sharpe 2.31

### Validacao Estatistica

- **Blocked Time-Series CV:** 5 blocos cronologicos, metricas com mean +/- std
- **Permutation test:** p=0.0005 (XGBoost Optuna) — estatisticamente significativo
- **Bootstrap CI:** intervalos de confianca a 95%

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
| Ensemble | Stacking meta-learner (LogisticRegression) |
| Series Temporais | statsmodels (ARIMA), Prophet |
| Interpretabilidade | SHAP (feature importance + selection) |
| Dados | yfinance, pandas-datareader, pandas, NumPy |
| Qualidade | pytest (274 testes), black, isort, flake8, GitHub Actions |

---

## Estrutura do Projeto

```
Market_Forecast/
├── notebooks/
│   ├── 01-eda.ipynb              # Analise exploratoria e visualizacao
│   ├── 02-train-xgboost.ipynb    # XGBoost Base + Macro + Optuna HPO
│   ├── 03-train-boosting.ipynb   # LightGBM, CatBoost, Stacking Ensemble
│   ├── 04-train-deep.ipynb       # ARIMA, Prophet, RNN/LSTM/GRU, LSTM Clf
│   ├── 05-analysis.ipynb         # Backtest, blocked CV, bootstrap, threshold
│   └── legacy/                   # Notebooks historicos
├── src/
│   ├── config.py                 # Constantes centralizadas
│   ├── data/                     # Loader com 5 fontes de fallback
│   ├── features/                 # Feature engineering (20 features)
│   ├── models/                   # XGBoost, LightGBM, CatBoost, LSTM, persistence
│   ├── evaluation/               # Backtest, threshold analysis
│   └── utils/                    # Helpers, statistics (blocked CV, bootstrap)
├── models/saved/                 # Modelos treinados + resultados persistidos
├── tests/                        # 274 testes unitarios + integracao
├── .github/workflows/            # CI/CD (testes + lint)
├── pyproject.toml                # Configuracao do projeto
└── LICENSE
```

---

## Contexto Academico

- Acuracia direcional de 55-60% com WFV e resultado genuinamente bom na literatura (Lo & MacKinlay, 1999; Gu et al., 2020)
- XGBoost/LightGBM dominam LSTM em features tabulares para previsao financeira
- 97% dos day traders perdem dinheiro a longo prazo (Barber & Odean, 2000)
- O ensemble supera consistentemente o Buy & Hold no IBOVESPA com Sharpe > 2.0

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

## Licenca

Todos os direitos reservados (All Rights Reserved).

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
