# Logo Forgery Detection

![Python](https://img.shields.io/badge/Python-3.9+-blue?logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c?logo=pytorch&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-Similarity_Search-blue)
![License: MIT](https://img.shields.io/badge/License-MIT-green)

> Detecção de logos falsificados com Siamese Networks e busca por similaridade visual.

---

## Objetivo

Verificar a **autenticidade de logos** usando uma rede siamesa que aprende embeddings visuais discriminativos. O sistema compara logos suspeitos contra um banco de referência usando busca eficiente via FAISS, decidindo se um logo é genuíno ou falsificado.

---

## Arquitetura

```
                    ┌─────────────────────┐
                    │   EfficientNetV2-S  │
    Imagem A ──────►│   (backbone 1280-d) │──► Projection ──► Embedding A (128-d)
                    │   pesos             │    512 → 128       L2-normalizado
                    │   compartilhados    │                         │
                    │                     │                         │
    Imagem B ──────►│   (backbone 1280-d) │──► Projection ──► Embedding B (128-d)
                    └─────────────────────┘    512 → 128       L2-normalizado
                                                                    │
                                                               ┌────▼────┐
                                                               │ ||A-B|| │
                                                               │ Dist L2 │
                                                               └────┬────┘
                                                                    │
                                                          ┌─────────▼─────────┐
                                                          │ dist < threshold? │
                                                          │  ✓ Genuíno       │
                                                          │  ✗ Falsificado   │
                                                          └───────────────────┘
```

---

## Decisões Técnicas

| Decisão | Motivação |
|---------|-----------|
| **EfficientNetV2-S** como backbone | Trade-off ideal entre accuracy e velocidade de inferência |
| **Embedding 128-d L2-normalizado** | Dimensão compacta para busca FAISS eficiente; normalização torna distância L2 equivalente a similaridade cosseno |
| **Contrastive Loss (margin=1.5)** | Margem calibrada para embeddings normalizados (max dist=2.0), equilibrando separação e convergência |
| **Treino em 2 fases** | Fase 1 (backbone frozen, lr=1e-3) estabiliza projection head; Fase 2 (unfrozen, lr=1e-4) faz fine-tuning sem destruir features ImageNet |
| **Mixed Precision (AMP)** | ~2x speedup em GPUs com Tensor Cores, essencial para viabilizar treino no Kaggle |
| **Split por imagem** | Zero data leakage — imagens de treino nunca aparecem em teste/validação |
| **FAISS IndexFlatL2** | Busca exata de vizinho mais próximo em tempo sub-linear |

---

## Pipeline

```
FlickrLogos-27 (~4,500 logo crops, 27 marcas)
  ↓
Split por imagem (70% train · 15% test · 15% validation)
  ↓
Geração de pares (50% genuínos · 50% impostores)
  ↓ regenerados a cada epoch
Data Augmentation
  ├─ RandomResizedCrop (escala 0.7-1.0)
  ├─ Rotação (±15°), Perspectiva, GaussianBlur
  └─ ColorJitter, HorizontalFlip
  ↓
Treino Siamês (Contrastive Loss)
  ├─ Fase 1: backbone frozen, lr=1e-3 (5 epochs)
  └─ Fase 2: fine-tuning, lr=1e-4 + CosineAnnealing
  ↓
Avaliação
  ├─ Threshold ótimo via F1 no test set
  ├─ AUC-ROC, Average Precision, F1
  └─ t-SNE para visualização de clusters
  ↓
Inferência FAISS
  └─ Embedding query → nearest neighbor → genuíno/falsificado
```

---

## Resultados

| Métrica | Valor |
|---------|-------|
| AUC-ROC | — |
| Average Precision | — |
| F1-score | — |
| Accuracy | — |

*Resultados serão preenchidos após treino completo.*

---

## Stack Técnico

| Categoria | Tecnologias |
|-----------|-------------|
| **Deep Learning** | PyTorch, timm (EfficientNetV2-S) |
| **Busca** | FAISS (IndexFlatL2) |
| **Visualização** | matplotlib, seaborn, t-SNE |
| **API** | FastAPI + uvicorn |
| **Dados** | FlickrLogos-27 (Kalantidis et al., 2011) |

---

## Referências

1. Koch, G., Zemel, R., Salakhutdinov, R. (2015). *Siamese Neural Networks for One-shot Image Recognition*
2. Hadsell, R., Chopra, S., LeCun, Y. (2006). *Dimensionality Reduction by Learning an Invariant Mapping*
3. Schroff, F., Kalenichenko, D., Philbin, J. (2015). *FaceNet: A Unified Embedding for Face Recognition and Clustering*

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)
