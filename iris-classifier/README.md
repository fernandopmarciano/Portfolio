# Iris Flower Classifier

> Classificacao de especies de flores Iris usando multiplos algoritmos de Machine Learning com validacao cruzada e conjunto de validacao independente.

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](#licenca)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)](https://scikit-learn.org/)

---

## Sobre o Projeto

Projeto classico de Machine Learning utilizando o dataset Iris (Fisher, 1936) para classificar tres especies de flores: **Setosa**, **Versicolor** e **Virginica** com base em medidas de sepalas e petalas.

Foco em demonstrar o pipeline completo de Data Science: analise exploratoria, pre-processamento, comparacao de modelos, otimizacao de hiperparametros e validacao rigorosa.

---

## Resultados

### Comparacao de Modelos (10-Fold Cross-Validation)

| Modelo                       | Acuracia Media | Desvio Padrao |
|------------------------------|----------------|---------------|
| Logistic Regression          | 86.1%          | 0.153         |
| Linear Discriminant Analysis | 95.4%          | 0.038         |
| KNN (k=2, GridSearchCV)      | 95.4%          | 0.051         |
| Decision Tree                | 93.1%          | 0.064         |
| **Naive Bayes (GaussianNB)** | **96.2%**      | **0.062**     |
| **SVM**                      | **96.2%**      | **0.052**     |
| K-Means (n=3)                | 21.5%          | 0.315         |

### Predicao no Conjunto de Teste (3 melhores modelos)

| Modelo     | Acuracia | Precision | Recall | F1-Score |
|------------|----------|-----------|--------|----------|
| GaussianNB | **100%** | 1.00      | 1.00   | 1.00     |
| SVM        | **100%** | 1.00      | 1.00   | 1.00     |
| KNN (k=2)  | **100%** | 1.00      | 1.00   | 1.00     |

### Validacao Independente

SVM atingiu **100% de acuracia** no conjunto de validacao (6 amostras reservadas antes do treino — 2 de cada especie).

---

## Metodologia

### Pipeline

```
Dataset Iris (150 amostras, 4 features)
    |
    v
Preprocessing
    |-- Target mapping (species → 0, 1, 2)
    |-- Separacao do Validation Set (6 amostras)
    |-- Normalizacao (sklearn normalize)
    |
    v
Analise Exploratoria
    |-- Correlacao Pearson & Spearman
    |-- Parallel Coordinates, PairPlot, Violin
    |-- Box Plot, Radviz, KDE
    |
    v
Otimizacao de Hiperparametros
    |-- GridSearchCV (KNN → k=2)
    |-- Elbow Method + Silhouette (K-Means → k=3)
    |
    v
Treinamento & Avaliacao
    |-- 10-Fold Cross-Validation
    |-- Comparacao de 7 modelos
    |-- Confusion Matrix por modelo
    |
    v
Validacao Final
    |-- Conjunto independente (nunca visto no treino/teste)
    |-- Decision Tree Plot
```

---

## Visualizacoes Realizadas

- Heatmap de correlacao (Pearson & Spearman)
- Parallel Coordinates por especie
- JointPlots (scatter, KDE, histogram, regression)
- FacetGrid Scatter e Histogramas
- Box Plot e Violin Plot por especie
- PairPlot com distribuicoes
- Radviz (visualizacao radial)
- K-Means clustering e centroids
- Decision Tree Plot

---

## Stack Tecnologica

| Categoria        | Tecnologias                                  |
|------------------|----------------------------------------------|
| **Linguagem**    | Python 3.9+                                  |
| **ML**           | scikit-learn (SVM, KNN, NB, LDA, CART)       |
| **Deep Learning**| TensorFlow / Keras                           |
| **Clustering**   | K-Means (com Elbow e Silhouette)             |
| **Visualizacao** | Matplotlib, Seaborn                          |
| **Dados**        | pandas, NumPy                                |

---

## Dataset

- **Nome:** Iris Dataset (UCI ML Repository)
- **Amostras:** 150 (50 por especie)
- **Features:** 4 (sepal_length, sepal_width, petal_length, petal_width)
- **Classes:** 3 (Setosa, Versicolor, Virginica)
- **Referencia:** Fisher, R.A. (1936). *The Use of Multiple Measurements in Taxonomic Problems*

---

## Autor

**Fernando Marciano**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fernando_Marciano-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/fernandomarciano/)

---

## Licenca

Distribuido sob a licenca MIT.

---

> **Interessado no codigo-fonte ou em uma demonstracao?** Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandomarciano/).
