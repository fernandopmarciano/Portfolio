# Atendente IA para Telegram

> Chatbot inteligente que atende clientes, responde duvidas e agenda consultas via Telegram — 24 horas por dia, 7 dias por semana.

![Node.js](https://img.shields.io/badge/Node.js-20+-339933?logo=node.js&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)

---

## Sobre o Projeto

Bot de atendimento automatizado para Telegram que utiliza IA generativa para entender mensagens em linguagem natural, responder perguntas frequentes, qualificar leads e agendar compromissos diretamente no Google Calendar — sem intervencao humana.

Projetado para pequenas e medias empresas que precisam de atendimento fora do horario comercial ou que recebem volume alto de mensagens repetitivas.

---

## Funcionalidades

- **Atendimento 24/7** — responde mensagens automaticamente com IA
- **Agendamento automatico** — cria, reagenda e cancela compromissos no Google Calendar
- **Qualificacao de leads** — identifica servico desejado e urgencia
- **Lembretes** — envia notificacoes de compromissos agendados
- **Transferencia para humano** — detecta quando o cliente precisa de atendimento pessoal
- **Painel administrativo** — metricas, historico de conversas e configuracoes

---

## Arquitetura

```
Telegram → Bot API → Webhook → Agente IA → Resposta
                                   |
                             Google Calendar
                                   |
                               SQLite DB
                                   |
                             Painel Admin
```

---

## Como Funciona (Exemplo)

```
Cliente: "Boa tarde, gostaria de agendar uma consulta para sexta"
Bot:     "Sexta-feira tenho horarios as 09:00, 14:00 e 16:00.
          Qual prefere?"
Cliente: "14h por favor"
Bot:     "Consulta agendada para sexta, 09/05, as 14:00.
          Vou enviar um lembrete na vespera."
```

---

## Tecnologias

| Camada | Tecnologias |
|--------|-------------|
| Runtime | Node.js 20+ |
| IA | Google Gemini |
| Mensageria | Telegram Bot API |
| Agenda | Google Calendar API |
| Banco de dados | SQLite |
| Infraestrutura | Docker + Docker Compose |
| Painel | Express + EJS |

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

> Interessado no codigo-fonte ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
