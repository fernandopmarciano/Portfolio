# NLP Sentiment Analyzer

![Python](https://img.shields.io/badge/Python-3.9+-blue?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-orange?logo=scikit-learn&logoColor=white)
![NLTK](https://img.shields.io/badge/NLTK-3.8+-green)
![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow?logo=huggingface&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green)

> Análise de sentimento multi-abordagem: do TF-IDF clássico ao BERT fine-tuned.

---

## Objetivo

Construir um pipeline completo de **análise de sentimento** comparando técnicas clássicas de NLP com modelos baseados em Transformers, demonstrando a evolução das abordagens e seus trade-offs em performance, custo computacional e interpretabilidade.

---

## Pipeline

```
Dados brutos (reviews/tweets)
  ↓
Pré-processamento (tokenização, limpeza, normalização)
  ↓
Feature Engineering
  ├─ Bag of Words / TF-IDF (sparse, interpretável)
  ├─ Word2Vec / GloVe (embeddings densos)
  └─ BERT Tokenizer (subword, contextual)
  ↓
Modelagem
  ├─ Logistic Regression + TF-IDF (baseline)
  ├─ SVM + TF-IDF
  ├─ Random Forest + Word2Vec
  └─ BERT fine-tuned (state-of-the-art)
  ↓
Avaliação (Accuracy, F1-macro, ROC-AUC, Confusion Matrix)
  ↓
Análise de Erros + Interpretabilidade (LIME/SHAP)
```

---

## Abordagens Comparadas

| Modelo | Representação | Vantagem | Limitação |
|--------|--------------|----------|-----------|
| Logistic Regression | TF-IDF | Rápido, interpretável | Ignora ordem das palavras |
| SVM (RBF) | TF-IDF | Boa generalização | Lento em datasets grandes |
| Random Forest | Word2Vec médio | Captura semântica | Perde contexto de frase |
| **BERT fine-tuned** | **Contextual** | **State-of-the-art** | **Requer GPU, ~500MB** |

---

## Técnicas Utilizadas

- **Pré-processamento:** remoção de stopwords, stemming/lemmatização, regex cleaning
- **Vetorização:** TF-IDF (n-grams), Word2Vec (Gensim), BERT tokenizer
- **Validação:** Stratified K-Fold (5 folds) com métricas por fold
- **Interpretabilidade:** LIME para explicação local de predições
- **Tratamento de desbalanceamento:** class_weight, oversampling (se necessário)

---

## Estrutura do Projeto

```
NLP_Sentiment/
├── src/
│   ├── data/          # Download, limpeza e cache de datasets
│   ├── features/      # Vetorização (TF-IDF, Word2Vec, BERT tokenizer)
│   ├── models/        # Treinamento e avaliação dos classificadores
│   └── utils/         # Helpers (timer, métricas, visualização)
├── tests/             # Testes unitários e de integração
├── notebooks/         # Análise exploratória e experimentação
├── data/              # Datasets (gitignored)
└── pyproject.toml     # Dependências e configuração
```

---

## Stack Tecnológica

| Categoria | Tecnologias |
|-----------|-------------|
| Linguagem | Python 3.9+ |
| NLP Clássico | NLTK, scikit-learn (TF-IDF, CountVectorizer) |
| Embeddings | Gensim (Word2Vec), GloVe |
| Transformers | HuggingFace Transformers, PyTorch |
| Dados | pandas, NumPy, datasets (HuggingFace) |
| Visualização | Matplotlib, Seaborn, WordCloud |
| Qualidade | pytest, black, isort, flake8, pre-commit |
| CI/CD | GitHub Actions (testes, lint, cobertura) |

---

## Status

🚧 **Em desenvolvimento** — Estrutura modular criada, implementação em andamento.

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

## Licença

MIT
