# Stock Price Forecast (v1)

> Previsao de precos de acoes usando series temporais e Deep Learning, com analise de decomposicao, estacionariedade e cross-validation temporal.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](#licenca)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-Deep_Learning-orange)](https://www.tensorflow.org/)

---

## Sobre o Projeto

Primeira versao do sistema de previsao de mercado financeiro, focada em **regressao de precos** (prever o valor de fechamento) usando series temporais classicas e redes neurais.

Este projeto evoluiu para o [Market Forecast v3](https://github.com/fernandopmarciano/Market-Forecast-Overview) — que migrou de regressao para **classificacao binaria** (alta/baixa) com XGBoost e Walk-Forward Validation.

---

## Indices Analisados

| Indice       | Pais   | Ticker   | Periodo     |
|--------------|--------|----------|-------------|
| IBOVESPA     | Brasil | `^BVSP`  | 2016-2022   |
| BSE SENSEX   | India  | `^BSESN` | 2016-2022   |
| NASDAQ       | EUA    | `^IXIC`  | 2016-2022   |
| Dow Jones    | EUA    | `^DJI`   | 2016-2022   |

**Referencia principal:** IBOVESPA (~1.733 dias de pregao)

---

## Metodologia

### Pipeline

```
Download de Dados (yfinance)
    |-- 4 indices globais (2016-2022)
    |
    v
Preprocessing
    |-- Renomeacao de colunas
    |-- Rolling Mean & Std (janela 10 dias)
    |-- Analise descritiva
    |
    v
Analise Exploratoria
    |-- Correlacao Pearson & Spearman entre indices
    |-- Candlestick charts (Plotly)
    |-- Rolling Mean/Std plots
    |
    v
Testes de Estacionariedade
    |-- Augmented Dickey-Fuller Test (ADF)
    |-- Decomposicao de series temporais
    |
    v
Modelagem
    |-- Split simples treino/teste
    |-- Baseline estatistico
    |-- Time Series Cross-Validation
    |-- Deep Learning (RNN, LSTM, GRU)
    |
    v
Avaliacao
    |-- Comparacao de modelos
    |-- Metricas de regressao
```

### Teste ADF (Estacionariedade)

| Metrica                    | Valor     |
|----------------------------|-----------|
| Test Statistic             | -2.306    |
| p-value                    | 0.170     |
| Critical Value (1%)        | -3.434    |
| Critical Value (5%)        | -2.863    |
| Observacoes                | 1,721     |

**Conclusao:** Serie nao-estacionaria (p > 0.05), necessitando diferenciacao para modelos ARIMA.

---

## Visualizacoes

- Correlacao Pearson & Spearman entre indices globais
- Grafico de velas (Candlestick) interativo com Plotly
- Precos de fechamento ajustados dos 4 indices
- Rolling Mean e Std (janela bi-semanal)
- Decomposicao de series temporais
- Resultados do teste ADF

---

## Modelos Utilizados

| Categoria           | Modelos                        |
|---------------------|--------------------------------|
| **Baseline**        | Estatistico                    |
| **Series Temporais**| ARIMA, decomposicao sazonal    |
| **Deep Learning**   | RNN, LSTM, GRU (TensorFlow)   |

---

## Stack Tecnologica

| Categoria        | Tecnologias                                    |
|------------------|------------------------------------------------|
| **Linguagem**    | Python 3.9+                                    |
| **Deep Learning**| TensorFlow / Keras (RNN, LSTM, GRU)            |
| **Series Temp.** | statsmodels (ADF test, decomposicao)           |
| **Dados**        | yfinance, pandas-datareader, pandas, NumPy     |
| **Visualizacao** | Matplotlib, Seaborn, Plotly                    |
| **Preprocessing**| scikit-learn (normalizacao)                    |

---

## Evolucao do Projeto

Este projeto (v1) serviu como base de estudo e evoluiu significativamente:

| Aspecto      | v1 (este projeto)          | v3 (Market Forecast)                    |
|--------------|----------------------------|-----------------------------------------|
| Objetivo     | Regressao de preco         | Classificacao binaria (alta/baixa)      |
| Validacao    | Split simples              | Walk-Forward Validation                 |
| Modelo       | LSTM/GRU                   | XGBoost + Optuna HPO                    |
| Features     | Preco bruto                | 20 features (tecnicas + macro + sentimento) |
| Backtest     | Nao                        | Sim, com custos operacionais            |
| Resultado    | Estudo exploratório        | 59% acuracia, Sharpe 1.80, +547% retorno |

---

## Autor

**Fernando Marciano**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fernando_Marciano-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/fernandomarciano/)

---

## Licenca

Distribuido sob a licenca MIT.

---

> **Interessado no codigo-fonte ou em uma demonstracao?** Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandomarciano/).
