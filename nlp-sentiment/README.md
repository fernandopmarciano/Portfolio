# NLP Sentiment Analyzer

> Analise de sentimento multi-abordagem: do TF-IDF classico ao BERT fine-tuned, com comparacao sistematica de 4 tecnicas.

![Python](https://img.shields.io/badge/Python-3.9+-blue?logo=python&logoColor=white)
![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow?logo=huggingface&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-orange?logo=scikit-learn&logoColor=white)

---

## Sobre o Projeto

Pipeline completo de analise de sentimento que compara tecnicas classicas de NLP com modelos baseados em Transformers. O objetivo e demonstrar a evolucao das abordagens e seus compromissos entre desempenho, custo computacional e interpretabilidade.

---

## Abordagens Comparadas

| Modelo | Representacao | Vantagem | Limitacao |
|--------|--------------|----------|-----------|
| Logistic Regression | TF-IDF | Rapido, interpretavel | Ignora ordem das palavras |
| SVM (RBF) | TF-IDF | Boa generalizacao | Lento em grandes volumes |
| Random Forest | Word2Vec medio | Captura semantica | Perde contexto da frase |
| **BERT fine-tuned** | **Contextual** | **Estado da arte** | **Requer GPU, ~500MB** |

---

## Pipeline

```
Dados brutos (reviews/tweets)
  |
  v
Pre-processamento (tokenizacao, limpeza, normalizacao)
  |
  v
Engenharia de Atributos
  |-- Bag of Words / TF-IDF (esparso, interpretavel)
  |-- Word2Vec / GloVe (embeddings densos)
  |-- BERT Tokenizer (subword, contextual)
  |
  v
Modelagem
  |-- Logistic Regression + TF-IDF (referencia)
  |-- SVM + TF-IDF
  |-- Random Forest + Word2Vec
  |-- BERT fine-tuned
  |
  v
Avaliacao (Acuracia, F1-macro, ROC-AUC, Matriz de Confusao)
  |
  v
Analise de Erros + Interpretabilidade (LIME)
```

---

## Tecnicas Utilizadas

- **Pre-processamento:** remocao de stopwords, lematizacao, limpeza com regex
- **Vetorizacao:** TF-IDF (n-grams), Word2Vec (Gensim), BERT tokenizer
- **Validacao:** Stratified K-Fold (5 folds) com metricas por fold
- **Interpretabilidade:** LIME para explicacao local de predicoes
- **Desbalanceamento:** pesos por classe (class_weight)

---

## Tecnologias

| Categoria | Tecnologias |
|-----------|-------------|
| NLP Classico | NLTK, scikit-learn (TF-IDF, CountVectorizer) |
| Embeddings | Gensim (Word2Vec), GloVe |
| Transformers | HuggingFace Transformers, PyTorch |
| Visualizacao | Matplotlib, Seaborn, WordCloud |
| Qualidade | pytest, black, isort, flake8, GitHub Actions |

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
