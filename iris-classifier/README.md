# Iris Flower Classifier

> Classificacao de especies de flores Iris com 7 algoritmos e validacao cruzada estratificada.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)](https://scikit-learn.org/)

---

## Sobre o Projeto

Projeto classico de Machine Learning sobre o dataset Iris (Fisher, 1936) para classificar tres especies — Setosa, Versicolor e Virginica — a partir de medidas de sepalas e petalas.

Foco em demonstrar o pipeline completo: analise exploratoria, pre-processamento, comparacao de modelos, otimizacao de hiperparametros e validacao rigorosa com conjunto independente.

---

## Resultados

### Comparacao de Modelos (10-Fold Cross-Validation)

| Modelo | Acuracia Media | Desvio Padrao |
|--------|----------------|---------------|
| Logistic Regression | 86.1% | 0.153 |
| Linear Discriminant Analysis | 95.4% | 0.038 |
| KNN (k=2, GridSearchCV) | 95.4% | 0.051 |
| Decision Tree | 93.1% | 0.064 |
| **Naive Bayes** | **96.2%** | **0.062** |
| **SVM** | **96.2%** | **0.052** |
| K-Means (k=3) | 21.5% | 0.315 |

### Validacao Final (3 melhores modelos)

| Modelo | Acuracia | Precision | Recall | F1 |
|--------|----------|-----------|--------|-----|
| Naive Bayes | **100%** | 1.00 | 1.00 | 1.00 |
| SVM | **100%** | 1.00 | 1.00 | 1.00 |
| KNN (k=2) | **100%** | 1.00 | 1.00 | 1.00 |

SVM atingiu 100% no conjunto de validacao independente (6 amostras reservadas antes do treino, 2 por especie).

### Observacao sobre K-Means

K-Means (21.5%) aparece para fins de comparacao: e um algoritmo de agrupamento nao-supervisionado, incapaz de associar rotulos as classes. A diferenca de desempenho ilustra a importancia de escolher a abordagem adequada ao tipo de problema.

---

## Metodologia

```
Dataset Iris (150 amostras, 4 atributos)
  |
  v
Pre-processamento (mapeamento de classes, normalizacao, separacao de validacao)
  |
  v
Analise Exploratoria (correlacao, PairPlot, Violin, Radviz, KDE)
  |
  v
Otimizacao (GridSearchCV para KNN, Elbow + Silhouette para K-Means)
  |
  v
10-Fold Cross-Validation (7 modelos)
  |
  v
Validacao Final no conjunto independente
```

---

## Dataset

- **Nome:** Iris (UCI ML Repository)
- **Amostras:** 150 (50 por especie)
- **Atributos:** 4 (comprimento e largura de sepala e petala)
- **Classes:** 3 (Setosa, Versicolor, Virginica)
- **Referencia:** Fisher, R.A. (1936). *The Use of Multiple Measurements in Taxonomic Problems*

---

## Tecnologias

| Categoria | Tecnologias |
|-----------|-------------|
| ML | scikit-learn (SVM, KNN, NB, LDA, CART) |
| Clustering | K-Means (Elbow, Silhouette) |
| Visualizacao | Matplotlib, Seaborn |
| Dados | pandas, NumPy |

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
