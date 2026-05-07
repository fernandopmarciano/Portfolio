# Financeiro IA — Controle de Gastos no Telegram

> Agente de IA que registra gastos por conversa natural no Telegram, categoriza automaticamente e envia relatorio analitico mensal.

![Node.js](https://img.shields.io/badge/Node.js-20+-339933?logo=node.js&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)

---

## Sobre o Projeto

Bot para Telegram que funciona como assistente financeiro pessoal. O usuario registra gastos enviando mensagens em linguagem natural ("gastei 50 no mercado") e a IA entende, categoriza e armazena. No inicio de cada mes, envia um relatorio analitico completo com totais, categorias e comparacao com o mes anterior.

---

## Como Funciona

**1. Registro** — envie gastos de forma natural:
```
"gastei 50 no mercado"
"uber 23,50"
"netflix 39,90"
"almoco 35 + cafe 8"
```

**2. Categorizacao automatica** — a IA classifica em 16 categorias (alimentacao, transporte, moradia, saude, lazer, etc.)

**3. Consultas** — pergunte a qualquer momento:
```
"quanto gastei hoje?"
"resumo da semana"
"quanto gastei no mes?"
```

**4. Relatorio mensal** — todo dia 1, recebe automaticamente:
- Total gasto vs. orcamento
- Gastos por categoria
- Media diaria
- Comparacao com mes anterior
- Maior gasto do mes
- Alertas de orcamento (80% e 100%)

---

## Funcionalidades

- Registro por texto natural (multiplos gastos na mesma mensagem)
- 16 categorias automaticas
- Orcamento mensal com alertas
- Resumo por dia, semana ou mes
- Relatorio analitico mensal automatico
- Comparacao com mes anterior
- Painel administrativo

---

## Arquitetura

```
Telegram → Bot API → Webhook → Agente IA → Parser de Gastos
                                               |
                                           SQLite DB
                                               |
                                    Relatorio Mensal (cron)
                                               |
                                       Telegram (resposta)
```

---

## Tecnologias

| Camada | Tecnologias |
|--------|-------------|
| Runtime | Node.js 20+ |
| IA | Google Gemini |
| Mensageria | Telegram Bot API |
| Banco de dados | SQLite |
| Infraestrutura | Docker + Docker Compose |
| Painel | Express + EJS |

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
