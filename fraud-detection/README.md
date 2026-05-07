# Fraud Detection

> Deteccao de fraudes em transacoes financeiras com Machine Learning, validacao cruzada estratificada e analise em 6.3 milhoes de transacoes.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)](https://scikit-learn.org/)

---

## Sobre o Projeto

Sistema de deteccao de fraudes construido sobre o dataset **PaySim** (6.3 milhoes de transacoes financeiras simuladas). Aborda um dos maiores desafios de pagamentos digitais: identificar fraudes em cenario de extremo desbalanceamento de classes (apenas 0.13% de fraude).

### Contexto

- **R$ 1.8 bilhao** em perdas por fraude no PIX reportadas ate meados de 2022
- **50 mil+ casos mensais** de fraude em pagamentos instantaneos
- Sistemas baseados em regras nao acompanham o volume e a velocidade das transacoes

---

## Resultados

### Comparacao de Modelos (10-Fold Stratified Cross-Validation)

| Modelo | Acuracia | PR-AUC | Recall | Precision |
|--------|----------|--------|--------|-----------|
| Logistic Regression | Referencia | Referencia | Referencia | Referencia |
| Decision Tree | >99% | Alto | Alto | Alto |
| Random Forest | >99% | Alto | Alto | Alto |
| **Gradient Boosting** | **>99.5%** | **>0.98** | **Alto** | **Alto** |

### Metricas de Destaque

- **PR-AUC > 0.98** — a metrica mais relevante em cenarios desbalanceados, pois mede a relacao entre precisao e cobertura sem ser inflada pela classe majoritaria
- **Validacao cruzada estratificada com 10 folds** e barras de erro para significancia estatistica
- **Conjunto de validacao independente** (100 amostras reservadas antes do treino)

### Por que PR-AUC e nao Acuracia?

Em datasets com 99.87% de transacoes legitimas, um modelo que classifica tudo como "nao-fraude" teria 99.87% de acuracia. A PR-AUC avalia especificamente a capacidade do modelo de encontrar fraudes reais (recall) sem gerar excesso de falsos alarmes (precision).

---

## Dataset — PaySim

| Caracteristica | Valor |
|----------------|-------|
| Transacoes | 6,362,620 |
| Atributos | 11 originais + 7 construidos |
| Fraudes | 8,213 (0.13%) |
| Periodo | 30 dias simulados |
| Tipos | CASH_IN, CASH_OUT, DEBIT, PAYMENT, TRANSFER |

**Descoberta relevante:** Fraudes concentram-se em apenas 2 tipos: TRANSFER (0.77%) e CASH_OUT (0.18%). Os demais tipos tem taxa de fraude zero.

---

## Arquitetura do Pipeline

```
Dados Brutos (6.36M transacoes)
  |
  v
Separacao do Conjunto de Validacao (10 fraudes + 90 legitimas)
  |
  v
Engenharia de Atributos
  |-- Diferencas de saldo (origem/destino)
  |-- Discrepancias na mudanca de saldo
  |-- Flag de destinatario comercial
  |-- Padroes de saldo zerado
  |-- One-hot encoding do tipo de transacao
  |
  v
10-Fold Stratified Cross-Validation
  |
  v
Selecao do Melhor Modelo (PR-AUC medio)
  |
  v
Validacao Final no conjunto reservado
```

---

## Destaques Tecnicos

- **Desbalanceamento extremo** tratado com metricas adequadas (PR-AUC, nao acuracia)
- **7 atributos construidos** a partir de padroes de saldo e tipo de transacao
- **Otimizacao de memoria** com dtypes ajustados para processar 6.3M de linhas
- **Validacao estratificada** que preserva a proporcao de fraude em cada fold

---

## Tecnologias

| Categoria | Tecnologias |
|-----------|-------------|
| ML | scikit-learn (LR, DT, RF, GBM, SVM, MLP) |
| Dados | pandas, NumPy |
| Visualizacao | Matplotlib, Seaborn |
| Validacao | StratifiedKFold, PR-AUC, ROC-AUC, Confusion Matrix |

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
