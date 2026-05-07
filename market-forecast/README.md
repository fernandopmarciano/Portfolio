# Market Forecast

> Previsao de direcao de preco (alta/baixa) em indices financeiros usando Machine Learning com validacao temporal realista.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![Version](https://img.shields.io/badge/version-3.1.0-informational)](#)

---

## Sobre o Projeto

Sistema de classificacao binaria que preve se o preco de fechamento de indices financeiros vai subir ou descer em relacao a abertura do mesmo dia.

Cobre o pipeline completo: coleta de dados de multiplas fontes, engenharia de 20 atributos (tecnicos, macroeconomicos e de sentimento), treinamento com validacao temporal e backtest com custos operacionais reais.

---

## Resultados (IBOVESPA, 2017-2024)

| Modelo | Acuracia | Retorno Acumulado | Indice de Sharpe |
|--------|----------|-------------------|------------------|
| SARIMA (referencia) | ~50% | -- | -- |
| RNN / GRU | ~51% | -- | -- |
| LSTM | 51.4% | -- | -- |
| **XGBoost (WFV)** | **57.6%** | **+354%** | **1.56** |
| **XGBoost + Optuna** | **59.0%** | **+547%** | **1.80** |
| Limiar >= 70% | -- | +330% | **3.89** |

**Nota:** Resultados de backtest educacional com Walk-Forward Validation. Nao constitui recomendacao de investimento.

### Interpretacao dos Resultados

- Em mercados financeiros, acertos acima de 55% ja sao relevantes. O modelo alcanca 59% com otimizacao bayesiana.
- O Indice de Sharpe de 3.89 (com filtro de confianca >= 70%) indica retorno ajustado ao risco muito acima do usual. Isso ocorre porque o modelo opera menos, mas com maior precisao.
- Modelos de series temporais classicos (ARIMA) e redes recorrentes (LSTM, GRU) nao superaram o acaso neste cenario, indicando que a engenharia de atributos foi mais determinante que a arquitetura do modelo.

---

## Indices Analisados

| Indice | Pais | Periodo |
|--------|------|---------|
| IBOVESPA | Brasil | 2016-2024 |
| BSE SENSEX | India | 2016-2024 |
| NASDAQ | EUA | 2016-2024 |
| Dow Jones | EUA | 2016-2024 |

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

---

## Atributos de Entrada (20)

**Tecnicos (14):** RSI, MACD + Histograma, Bollinger Bands, Volatilidade Realizada, Medias Moveis (7/21/50 dias), Momentum, Gap de Abertura, Retorno e Amplitude do dia anterior, Variacao de Volume.

**Macroeconomicos (3):** VIX (nivel e variacao), Dollar Index (DXY).

**Sentimento (3):** CNN Fear & Greed (score, zona categorica, momentum 5 dias).

---

## Tecnologias

| Categoria | Tecnologias |
|-----------|-------------|
| ML / Modelos | XGBoost, Optuna, scikit-learn |
| Deep Learning | TensorFlow / Keras (RNN, LSTM, GRU) |
| Series Temporais | statsmodels (ARIMA), Prophet |
| Dados | yfinance, pandas-datareader, pandas, NumPy |
| Qualidade | pytest, black, isort, flake8, GitHub Actions |

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
