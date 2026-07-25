# CLAUDE.md — Portfolio (vitrine pública)

## O que é este repositório

Vitrine pública de projetos de Data Science & ML. Contém APENAS READMEs de apresentação
(uma pasta por projeto) — o código real vive em repositórios privados.

## Regras

1. **Nunca** adicionar código-fonte, dados, credenciais ou detalhes internos dos repos
   privados — somente descrição, métricas publicadas, imagens/GIFs e stack.
2. Cada projeto deve ter versão **PT-BR e EN** (público: recrutadores brasileiros e
   internacionais).
3. Métricas citadas devem existir nos repos de origem (nada de números não reproduzidos).
4. Imagens/GIFs otimizados (<10MB) em pasta `assets/` do próprio projeto.

## Fluxo de atualização

Usar a skill global `update-portfolio` (em `~/.claude/skills`): ela sincroniza métricas,
descrições, badges e resultados a partir dos repos de origem. Invocar sempre que um projeto
de origem tiver release/melhoria de métrica.

## Estrutura

Uma pasta por projeto (avtp, market-forecast, nlp-sentiment, logo-forgery-detection,
fraud-detection, iris-classifier, rocket-landing-rl, hermes-telegram-bot, fm-ia-solutions),
cada uma com README.md (PT), README.en.md (EN) e assets.
