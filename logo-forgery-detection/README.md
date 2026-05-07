# Logo Forgery Detection

> Deteccao de logos falsificados com Redes Siamesas, EfficientNetV2-S e busca por similaridade visual via FAISS.

![Python](https://img.shields.io/badge/Python-3.9+-blue?logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c?logo=pytorch&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-Similarity_Search-blue)

---

## Sobre o Projeto

Sistema que verifica a autenticidade de logos comparando imagens suspeitas contra um banco de referencia. Uma rede siamesa aprende representacoes visuais (embeddings) discriminativas, e a busca por vizinho mais proximo via FAISS decide se o logo e genuino ou falsificado.

### Aplicacoes

- Combate a pirataria e falsificacao de marcas
- Verificacao automatizada de autenticidade em marketplaces
- Analise forense de propriedade intelectual

---

## Resultados

### Classificacao (Conjunto de Validacao)

| Metrica | Valor |
|---------|-------|
| AUC-ROC | 0.9665 |
| Average Precision | 0.9770 |
| F1-score | 0.9330 |
| Acuracia | 93.7% |
| Limiar otimo | 0.7010 |

### Busca por Similaridade (FAISS)

| Metrica | Valor |
|---------|-------|
| Recall@1 | 0.9469 |
| Recall@5 | 0.9469 |
| Recall@10 | 0.9469 |

Recall@1 igual a Recall@5 e @10 significa que quando o modelo acerta, a correspondencia correta e sempre o vizinho mais proximo — alta confianca na busca.

### Treino

| Parametro | Valor |
|-----------|-------|
| Epochs | 30 (parada antecipada na epoch 23) |
| Melhor loss de teste | 0.1105 |
| GPU | Tesla T4 (Kaggle) |
| Tempo total | ~37 minutos |

---

## Arquitetura

```
                 ┌────────────────────┐
                 │  EfficientNetV2-S  │
Imagem A ──────> │  (backbone 1280-d) │──> Projecao ──> Embedding A (128-d)
                 │  pesos             │    512 -> 128    L2-normalizado
                 │  compartilhados    │
Imagem B ──────> │  (backbone 1280-d) │──> Projecao ──> Embedding B (128-d)
                 └────────────────────┘    512 -> 128    L2-normalizado
                                                              |
                                                         Distancia L2
                                                              |
                                                    dist < limiar?
                                                    Sim: Genuino
                                                    Nao: Falsificado
```

### Decisoes de Projeto

| Decisao | Motivacao |
|---------|-----------|
| EfficientNetV2-S como backbone | Equilibrio entre acuracia e velocidade de inferencia |
| Embedding 128-d com norma L2 | Dimensao compacta para busca FAISS; normalizacao torna distancia L2 equivalente a similaridade cosseno |
| Contrastive Loss (margem 1.5) | Margem calibrada para embeddings normalizados (distancia maxima = 2.0) |
| Treino em 2 fases | Fase 1: backbone congelado (lr=1e-3) estabiliza a projecao; Fase 2: fine-tuning (lr=1e-4) sem destruir atributos pre-treinados |
| Mixed Precision (AMP) | ~2x de aceleracao em GPUs com Tensor Cores |
| Divisao por imagem | Zero vazamento de dados — imagens de treino nunca aparecem em teste |

---

## Pipeline

```
FlickrLogos-27 (~4.500 recortes de logos, 27 marcas)
  |
  v
Divisao por imagem (70% treino, 15% teste, 15% validacao)
  |
  v
Geracao de pares (50% genuinos, 50% impostores) — regenerados a cada epoch
  |
  v
Aumento de dados (crop, rotacao, perspectiva, blur, cor, flip)
  |
  v
Treino Siames (Contrastive Loss, 2 fases)
  |
  v
Avaliacao (limiar otimo via F1, AUC-ROC, Average Precision)
  |
  v
Inferencia FAISS (embedding da consulta -> vizinho mais proximo -> decisao)
```

---

## Tecnologias

| Categoria | Tecnologias |
|-----------|-------------|
| Deep Learning | PyTorch, timm (EfficientNetV2-S) |
| Busca por similaridade | FAISS (IndexFlatL2) |
| Visualizacao | matplotlib, seaborn, t-SNE |
| API | FastAPI + uvicorn |
| Dados | FlickrLogos-27 (Kalantidis et al., 2011) |

---

## Referencias

1. Koch, G., Zemel, R., Salakhutdinov, R. (2015). *Siamese Neural Networks for One-shot Image Recognition*
2. Hadsell, R., Chopra, S., LeCun, Y. (2006). *Dimensionality Reduction by Learning an Invariant Mapping*
3. Schroff, F., Kalenichenko, D., Philbin, J. (2015). *FaceNet: A Unified Embedding for Face Recognition and Clustering*

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
