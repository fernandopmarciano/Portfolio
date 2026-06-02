# NLP Sentiment Analyzer

> Analise de sentimento financeiro multi-abordagem: do TF-IDF classico ao FinBERT, com comparacao sistematica de 4 niveis de complexidade e exportacao de features para integracao cross-project.

![Python](https://img.shields.io/badge/Python-3.9+-blue?logo=python&logoColor=white)
![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow?logo=huggingface&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-orange?logo=scikit-learn&logoColor=white)

---

## Sobre o Projeto

Pipeline completo de analise de sentimento financeiro que compara tecnicas classicas de NLP com modelos baseados em Transformers. Classifica sentimento em 3 classes (negativo, neutro, positivo) sobre o dataset Financial PhraseBank (4,846 sentencas anotadas por 16 especialistas).

Inclui modulo de exportacao de sentimento diario (`src/export/`) para integracao com o projeto [Market Forecast](../market-forecast/), alimentando features de sentimento NLP no pipeline de previsao.

---

## Abordagens Comparadas

| Modelo | Representacao | Vantagem | Limitacao |
|--------|--------------|----------|-----------|
| Logistic Regression | TF-IDF | Rapido, interpretavel (~72%) | Ignora ordem das palavras |
| SVM (RBF) | TF-IDF | Boa generalizacao (~74%) | Lento em grandes volumes |
| XGBoost + Random Forest | Word2Vec medio | Captura semantica (~71%) | Perde contexto da frase |
| BERT fine-tuned | Contextual | Alta acuracia (~85%) | Requer GPU, ~500MB |
| **FinBERT fine-tuned** | **Domain-specific** | **Estado da arte (~88%)** | **Requer GPU, modelo financeiro** |

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
