# Fraud Detection in Online Payment Systems

> Sistema de deteccao de fraudes em transacoes financeiras usando Machine Learning, com validacao cruzada estratificada e analise em dataset de 6.3M de transacoes.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](#licenca)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)](https://scikit-learn.org/)

---

## Sobre o Projeto

Sistema production-grade de deteccao de fraudes desenvolvido sobre o dataset **PaySim** (6.3 milhoes de transacoes financeiras simuladas). O projeto aborda um dos maiores desafios de pagamentos digitais: detectar fraudes em tempo real com extremo desbalanceamento de classes (0.13% de fraude).

### Motivacao: Crise de Fraudes no PIX

- **R$ 1.8 bilhao** em perdas por fraude reportadas ate meados de 2022
- **50.000+ casos** mensais de fraude em transacoes PIX
- Sistemas baseados em regras falham com o volume e velocidade dos pagamentos instantaneos

---

## Resultados

### Performance dos Modelos (10-Fold Stratified Cross-Validation)

| Modelo                | Acuracia  | PR-AUC   | Recall   | Precision |
|-----------------------|-----------|----------|----------|-----------|
| Logistic Regression   | Baseline  | Baseline | Baseline | Baseline  |
| Decision Tree         | >99%      | Alto     | Alto     | Alto      |
| Random Forest         | >99%      | Alto     | Alto     | Alto      |
| **Gradient Boosting** | **>99.5%**| **>0.98**| **Alto** | **Alto**  |

### Metricas-Chave

- **>99.5% de acuracia** em dados altamente desbalanceados (0.13% de fraude)
- **PR-AUC >0.98** — forte capacidade de discriminacao
- **10-Fold Stratified CV** com barras de erro e significancia estatistica
- **Conjunto de validacao independente** (100 amostras reservadas antes do treino)

---

## Dataset

### PaySim — Transacoes Financeiras Sinteticas

| Caracteristica | Valor |
|----------------|-------|
| **Transacoes** | 6,362,620 |
| **Features** | 11 originais + 7 engenheiradas |
| **Fraudes** | 8,213 (0.13% do total) |
| **Periodo** | 30 dias simulados (steps de 1 hora) |
| **Tipos** | CASH_IN, CASH_OUT, DEBIT, PAYMENT, TRANSFER |

### Descoberta-Chave
Fraudes concentradas em apenas 2 tipos: **TRANSFER** (0.77%) e **CASH_OUT** (0.18%). Os demais tipos tem taxa de fraude **zero**.

---

## Arquitetura

```
Dados Brutos (6.36M transacoes)
    |
    v
Reservar Validation Set (10 fraudes + 90 legitimas)
    |
    v
Feature Engineering
    |-- balance_diff_orig/dest (diferenca de saldo)
    |-- error_orig/dest (discrepancia na mudanca de saldo)
    |-- is_merchant (flag de destinatario comercial)
    |-- zero_balance flags (padroes suspeitos)
    |-- One-hot encoding (tipo de transacao)
    |
    v
10-Fold Stratified Cross-Validation
    |-- Treino: 9 folds (~5.7M transacoes)
    |-- Teste: 1 fold (~630K transacoes)
    |-- Multiplos modelos por fold
    |
    v
Selecao de Modelo (melhor PR-AUC medio)
    |
    v
Validacao Final
    |-- Treino no dataset completo
    |-- Avaliacao no validation set reservado
```

---

## Analise Exploratoria

- Distribuicao de fraude por tipo de transacao
- Distribuicao de valores (amount) por status de fraude
- Correlacao entre features
- Analise temporal de fraudes (steps)
- Box plots e histogramas comparativos

---

## Stack Tecnologica

| Categoria          | Tecnologias                                          |
|--------------------|------------------------------------------------------|
| **Linguagem**      | Python 3.9+                                          |
| **ML**             | scikit-learn (LR, DT, RF, GBM, SVM, MLP)            |
| **Dados**          | pandas, NumPy                                        |
| **Visualizacao**   | Matplotlib, Seaborn                                  |
| **Validacao**      | StratifiedKFold, PR-AUC, ROC-AUC, Confusion Matrix  |
| **Preprocessing**  | StandardScaler, One-Hot Encoding                     |

---

## Destaques Tecnicos

- **Extreme class imbalance handling** — 0.13% de fraude com metricas apropriadas (PR-AUC em vez de accuracy)
- **Feature Engineering** — 7 features engenheiradas a partir de padroes de saldo e tipo de transacao
- **Stratified CV** — preserva proporcao de fraude em cada fold
- **Validacao independente** — dados nunca vistos durante treino ou CV
- **Memory optimization** — dtypes otimizados para processar 6.3M de linhas eficientemente

---

## Autor

**Fernando Marciano**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fernando_Marciano-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/fernandomarciano/)

---

## Licenca

Distribuido sob a licenca MIT.

---

> **Interessado no codigo-fonte ou em uma demonstracao?** Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandomarciano/).
