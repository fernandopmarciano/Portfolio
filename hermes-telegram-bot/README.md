# Hermes — Assistente Pessoal no Telegram

> Agente de IA no Telegram com controle financeiro, agenda, auditoria de repositorios, monitoramento de dependencias e relatorios automaticos — operando 24/7 em producao.

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-2.5_Flash-4285F4?logo=google&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-Bot_API-26A5E4?logo=telegram&logoColor=white)

---

## Sobre o Projeto

Assistente pessoal que roda como bot no Telegram, integrado com Google Calendar, Gmail, SQLite e GitHub. Opera em um grupo com 6 topicos tematicos, cada um com funcoes e automacoes especificas.

Diferente de chatbots convencionais, o Hermes combina respostas de IA com scripts deterministicos: operacoes criticas (como registro de gastos) sao processadas por regex antes de chegar ao modelo, garantindo confiabilidade mesmo quando o LLM esta indisponivel.

---

## Funcionalidades

### Controle Financeiro (Topic: Budget)

- Registro de gastos por texto natural: "gastei 50 no mercado", "uber 23,50"
- Parser deterministico (regex) que funciona sem depender do modelo de IA
- 8 categorias automaticas com suporte a acentos (alimentacao, transporte, moradia, saude, lazer, educacao, vestuario, servicos)
- Categoria explicita apos virgula: "pokehome 10,90, lazer"
- Orcamento mensal com alerta de limite (R$3.000)
- Relatorio mensal automatico com totais por categoria
- Consultas interativas: "quanto gastei esse mes?", "relatorio"

### Relatorio Diario (Topic: Matinal)

Relatorio automatico gerado as 23:59, enviado direto pela API do Telegram sem passar pelo modelo:
- Agenda do dia (Google Calendar)
- Proximos 7 dias
- Emails nao lidos (Gmail)
- Resumo de orcamento
- Gastos do dia com grafico de barras dos ultimos 7 dias
- Lancamentos completos do mes
- Vencimentos de cartao nos proximos 5 dias

### Auditoria de Repositorios (Topic: Auditoria)

- Varredura automatica a cada 2 horas em todos os repos GitHub (round-robin)
- Relatorio diario consolidado com achados novos, resolvidos e score por repo

### Monitoramento (Topics: Dependencias, GitHub)

- **Dependencias:** verificacao diaria de pacotes desatualizados e CVEs
- **Email Scanner:** resumo de emails classificados por prioridade
- **PRWatcher:** monitoramento de pull requests e issues 3x ao dia

### Resumo Mensal (Topic: Semanal)

- Consolidacao mensal de uso do Hermes, agenda, emails e saude dos crons

---

## Arquitetura

```
Telegram (Grupo AVTP HQ — 6 topicos)
  |
  v
Hermes Gateway (polling, tmux no WSL Ubuntu)
  |
  v
Hook pre_gateway_dispatch
  |-- Budget (thread 6): expense_parser.py (regex, sem LLM)
  |     |-- Gasto reconhecido -> SQLite + confirma
  |     |-- Nao reconhecido -> encaminha ao modelo
  |-- Outros topicos: encaminha direto
  |
  v
Gemini 2.5 Flash (modelo principal)
  |-- Fallback: Groq LLaMA 3.3 70B
  |-- Usa: terminal, skills, Google APIs
  |
  v
Resposta no topic correto (com splitting para msgs longas)
```

### Hook Deterministico (Budget)

O sistema de hooks intercepta mensagens **antes** do modelo processar. No topic de gastos:

1. Mensagem chega ("gastei 50 no mercado")
2. Hook chama `expense_parser.py` (regex puro, sem IA)
3. Se reconhece o padrao: registra no SQLite e reescreve a mensagem para o modelo apenas confirmar
4. Se nao reconhece: libera para o modelo processar normalmente

Isso garante que gastos sao registrados mesmo quando o Gemini retorna erro 503 ou o fallback Groq esta instavel.

---

## Automacoes (Cron)

| Horario | Funcao | Destino |
|---------|--------|---------|
| 23:59 | Relatorio Diario (deterministico) | Matinal |
| 23:50 | Monitor de Dependencias | Dependencias |
| 23:30 | Email Scanner | Dependencias |
| 10h, 16h, 22h | PRWatcher | GitHub |
| A cada 2h | Auditoria de Repos | Auditoria |
| 23:59 | Relatorio de Auditoria | Auditoria |
| Dia 1, 8h | Resumo Mensal | Semanal |
| Dia 1, 8h | Fechamento Financeiro Mensal | Budget |

---

## Tecnologias

| Camada | Tecnologias |
|--------|-------------|
| Agente | Hermes v0.11.0 (Python 3.11) |
| Modelo principal | Gemini 2.5 Flash (Google AI Studio) |
| Fallback | Groq LLaMA 3.3 70B |
| Mensageria | Telegram Bot API |
| Integracao | Google Calendar API, Gmail API |
| Banco de dados | SQLite (gastos, cartoes) |
| Scripts | Python (relatorio diario, parser de gastos, auditoria) |
| Infraestrutura | WSL Ubuntu, tmux, auto-start via Windows Startup |
| Qualidade | 124 testes E2E (parser, hook, relatorio, config) |

---

## Numeros do Projeto

| Metrica | Valor |
|---------|-------|
| Skills ativas | 12 |
| Cron jobs | 8 ativos |
| Topics do Telegram | 6 |
| Testes E2E | 124 (118 passando, 6 falsos positivos) |
| Categorias de gastos | 8 |
| Fontes de dados | Telegram, Google Calendar, Gmail, GitHub, SQLite |

---

## Decisoes de Projeto

| Decisao | Motivacao |
|---------|-----------|
| Parser deterministico para gastos | Independencia do LLM para operacoes criticas. Gemini 503, Groq instavel — gasto registra mesmo assim |
| Hook antes do modelo | Interceptacao sem custo de tokens. Resolve em milissegundos |
| reasoning_effort: none | Campo reasoning_content do Gemini quebra o fallback Groq (erro 400). Hermes nao sanitiza entre providers |
| Relatorio diario sem LLM | Script Python envia direto pela API do Telegram. Zero custo, zero falha por modelo |
| tmux em vez de systemd | systemd no WSL mata o gateway em ~15 segundos. tmux e estavel |
| Splitting de mensagens | Telegram tem limite de 4096 caracteres. Divisao automatica por secao |

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
