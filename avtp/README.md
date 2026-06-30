# AVTP — Assistente Virtual Totalmente Privado

> Assistente pessoal com IA **100% local**: LLM, RAG, memória, voz e 30+ ferramentas rodando no próprio hardware. Zero dependência de cloud — privacidade total por design.

[![Platform](https://img.shields.io/badge/platform-Windows%2011-0078D6)](#)
[![Runtime](https://img.shields.io/badge/runtime-Node.js%2020-339933?logo=node.js&logoColor=white)](#)
[![LLM](https://img.shields.io/badge/LLM-Qwen2.5--7B%20(llama.cpp%20Vulkan)-orange)](#)
[![Tests](https://img.shields.io/badge/tests-~975%20passing-brightgreen)](#)
[![License: All Rights Reserved](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)](#licenca)

---

## Sobre o Projeto

O AVTP é um assistente de IA que roda **inteiramente offline**, no hardware do usuário. Nenhum texto, documento ou áudio sai da máquina — o modelo de linguagem, a busca semântica (RAG), a memória de longo prazo, a síntese e o reconhecimento de voz executam localmente.

O desafio central é de **engenharia de sistemas**: entregar a experiência de um assistente moderno (streaming, ferramentas, voz, agentes autônomos) com a latência e o custo de um LLM de 7B parâmetros numa GPU de consumo (RX 6600, 8 GB), mantendo privacidade e segurança de nível profissional.

---

## Resultados de Engenharia

Por ser um sistema (e não um modelo), os resultados são de **desempenho e robustez**:

| Métrica | Valor | Como |
|---------|-------|------|
| **TTFT** (time to first token) | **~32–140 ms** | Prompt caching + warm requests + definições de tool enxutas |
| **Throughput** | **~37 tok/s** | Backend Vulkan na RX 6600 8 GB |
| **Ganho com speculative decoding** | **+40,9%** | Modelo rascunho 0.5B + verificação 7B |
| **Suíte de testes** | **~975 testes** | Inclui 656 do *spider-test* adversarial (507 ataques) |
| **Defesa contra prompt injection** | **33 padrões + sanitização zero-width** | Validada por 507 testes adversariais |
| **Custo de operação** | **R$ 0 / mês** | 100% local, sem API de cloud |

---

## Capacidades

- **30+ ferramentas** — PowerShell sandboxed, arquivos, memória, busca semântica (BM25 + stemming PT-BR), clipboard, PDF/DOCX/Excel, OCR, TTS/STT, SymPy (matemática simbólica), screenshot, code sandbox, análise de YouTube, gerenciador de skills, consulta de jurisprudência (DataJud/CNJ) e classificação de certidões.
- **RAG híbrido** — recuperação combinando BM25 (lexical) + embeddings `bge-m3` (semântico), com reordenação.
- **Sistema de skills** — receitas reutilizáveis com auto-ativação, CRUD e sincronização via GitHub.
- **Sistema de agentes** — modo autônomo multi-passo, templates builtin e agentes customizados com *whitelist* de ferramentas e perfis de hardware automáticos.
- **Voz PT-BR** — síntese via Piper TTS (CPU, sub-100 ms) e transcrição via Whisper (STT).
- **Memória e contexto** — memória de longo prazo + auto-sumarização de conversas antigas (via modelo 0.5B) em vez de descarte.
- **Analytics local** — 9 KPIs e 7 gráficos (tokens/s, latência, uso, complexidade).
- **UI completa** — multi-sessão, busca Ctrl+K, render de markdown, exportação, anexos e entrada por voz.

---

## Arquitetura

```
                         UI (multi-sessão, voz, Ctrl+K)
                                    |
                                    v
                          Orquestrador de chat
              (streaming SSE, prompt caching, tool retry)
                                    |
        +---------------------------+---------------------------+
        |                           |                           |
        v                           v                           v
   LLM local                  RAG híbrido                 Camada de tools
   Qwen2.5-7B                 BM25 + bge-m3               30+ ferramentas
   llama.cpp Vulkan          (lexical + semântico)       (sandbox + whitelist)
   speculative decoding              |                           |
        |                           v                           v
        |                     Memória / contexto          Defesa de segurança
        |                     (auto-sumarização 0.5B)     (33 padrões anti-injeção
        |                                                  + sanitização zero-width)
        +---------------------------+---------------------------+
                                    |
                                    v
                       Voz: Piper TTS (CPU) / Whisper STT
```

---

## Decisões de Engenharia que Importaram

### 1. Latência de um 7B em GPU de consumo
TTFT de 32–140 ms num modelo local de 7B foi alcançado combinando **prompt caching** (reaproveitar o KV-cache do system prompt), **warm requests** (manter o modelo carregado) e **definições de tool enxutas** (reduzir tokens do contexto de ferramentas). O **speculative decoding** (rascunho 0.5B verificado pelo 7B) elevou o throughput em **+40,9%**.

### 2. Segurança como requisito de primeira classe
Um assistente que executa PowerShell e lê arquivos é uma superfície de ataque séria. A defesa contra **prompt injection** combina 33 padrões de detecção e sanitização de caracteres invisíveis (zero-width), validada por um **spider-test adversarial** com 507 ataques. Ferramentas rodam com *whitelist* e sandbox.

### 3. Recuperação confiável sem cloud
Busca puramente semântica erra em termos exatos (nomes, códigos); busca puramente lexical erra em paráfrases. O **RAG híbrido** (BM25 + `bge-m3`) cobre os dois regimes, com stemming PT-BR para o português.

### 4. Robustez do loop de ferramentas
LLMs locais alucinam chamadas de ferramenta. Um mecanismo de **tool retry** com 4 padrões de regex detecta alucinações e re-promove o modelo com correção, em vez de falhar.

---

## Tecnologias

| Categoria | Tecnologias |
|-----------|-------------|
| LLM / Inferência | Qwen2.5-7B (Q4_K_M), llama.cpp, backend Vulkan, speculative decoding |
| RAG | BM25, embeddings bge-m3, stemming PT-BR |
| Voz | Piper TTS (CPU), Whisper (STT), Tesseract 5 (OCR) |
| Runtime | Node.js 20, JavaScript, Windows 11 |
| Segurança | Defesa anti-prompt-injection, sanitização zero-width, sandbox, tool whitelist |
| Qualidade | ~975 testes (656 spider-test adversarial), CI/CD |

---

## Privacidade e Segurança

- **100% local** — nenhum dado trafega para serviços externos.
- **Defesa adversarial** — 507 testes de prompt injection.
- **Execução isolada** — ferramentas com sandbox e lista de permissões.
- **Custo zero e auditável** — sem dependência de provedores de IA.

---

## Autor

**Fernando Marciano** — Mestre em Engenharia Elétrica (Inteligência Computacional e Machine Learning) — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

## Licenca

Todos os direitos reservados (All Rights Reserved). Projeto privado por design (assistente totalmente privado).

---

> Este é um estudo de caso. O código-fonte é privado. Interessado em uma demonstração ou nos detalhes de arquitetura? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
