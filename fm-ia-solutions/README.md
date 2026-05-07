# FM IA Solutions

> Plataforma comercial de produtos e servicos de Inteligencia Artificial para profissionais e empresas brasileiras.

![Next.js](https://img.shields.io/badge/Next.js-16-000?logo=next.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)

---

## Sobre o Projeto

Site comercial completo com catalogo de 6 produtos de IA, chatbot inteligente com modelo de linguagem, sintese de voz, painel administrativo e documentacao legal (LGPD). Desenvolvido como vitrine e canal de venda da FM IA Solutions.

---

## Funcionalidades

### Para o Usuario Final

- **Chatbot com IA** — conversa com Llama 3.3 70B via Groq, respostas em tempo real (streaming SSE) e sintese de voz em portugues (Google Cloud WaveNet)
- **Catalogo de 6 produtos** — paginas individuais com descricao, demonstracao e CTA
- **Kit de Prompts** — 924+ prompts profissionais para 28 areas de atuacao, entregue em HTML com protecao DRM
- **Assistente de Laudos ORL** — transcricao de laudos medicos em tempo real com exportacao TXT/PDF

### Para o Administrador

- **Painel com 5 sub-paginas:** visao geral, consumo de tokens, usuarios, financeiro e historico de conversas
- **Analytics** — geolocalizacao por headers Vercel (custo zero)
- **Gestao de creditos** — controle de uso por usuario

### Seguranca e Conformidade

- Autenticacao com Supabase Auth (senha forte, hold-to-reveal)
- CSP configurado, CORS restrito, rate limiting progressivo
- Sanitizacao de input contra injecao de prompt
- LGPD: politica de privacidade, termos de uso

---

## Arquitetura

```
Landing Page + Catalogo
  |
  v
API Routes (Next.js App Router)
  |-- /api/chat      -> Groq (Llama 3.3 70B, streaming SSE)
  |-- /api/tts       -> Google Cloud TTS (WaveNet pt-BR)
  |-- /api/auth      -> Supabase Auth
  |-- /api/credits   -> Controle de consumo
  |-- /api/analytics -> Geolocalizacao
  |
  v
Banco de Dados (Supabase PostgreSQL com RLS)
  |
  v
Painel Admin (5 sub-paginas)
```

---

## Tecnologias

| Camada | Tecnologias |
|--------|-------------|
| Framework | Next.js 16 (App Router, Turbopack) |
| Interface | React 19, Tailwind CSS 4, shadcn/ui, Lucide Icons |
| Chatbot | Groq Cloud (Llama 3.3 70B), streaming SSE |
| Voz | Google Cloud Text-to-Speech (WaveNet pt-BR) |
| Autenticacao e BD | Supabase (Auth, PostgreSQL, Row Level Security) |
| Linguagem | TypeScript 5 |
| Deploy | Vercel |

---

## Numeros do Projeto

| Metrica | Valor |
|---------|-------|
| Produtos no catalogo | 6 |
| Prompts profissionais | 924+ |
| Profissoes cobertas | 28 |
| Sub-paginas do admin | 5 |
| Secoes da landing page | 7 |

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
