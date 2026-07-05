# Rocket Landing RL

> Pouso autonomo de foguete em navio-drone (ASDS) com fisica 6DOF e neuroevolucao — o agente inclina o corpo para voar ate a barca e endireita para pousar, como um booster real.

[![Python](https://img.shields.io/badge/python-3.12-blue)](https://www.python.org/)
[![Version](https://img.shields.io/badge/version-0.8.0-informational)](#)
[![Tests](https://img.shields.io/badge/tests-412%20passing-brightgreen)](#)
[![License: All Rights Reserved](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)](#licenca)

---

## Sobre o Projeto

Simulacao fisica completa de um foguete (inspirado na recuperacao do booster Falcon 9) com jogo interativo em pygame e pipeline de treinamento por **neuroevolucao** (algoritmo genetico): o agente aprende a pousar na barca vindo de ate 160 m de distancia, controlando empuxo e rotacao.

O diferencial da versao atual e o **controle realista rotate-and-thrust**: as acoes de inclinacao viram um comando de taxa de rotacao fly-by-wire (±40°/s) — para se deslocar lateralmente o agente precisa *inclinar o corpo e empurrar*, exatamente como um foguete de verdade. Nada de deslizar de lado "em pe".

![Pouso autonomo rotate-and-thrust no navio-drone](assets/realistic_landing.gif)

> O agente inclina o corpo na aproximacao, freia, endireita e toca o deck abaixo de 1° de inclinacao a ~1,5 m/s — telemetria ao vivo (altitude, velocidades, inclinacao, throttle, combustivel). A chama do motor aparece apenas na frenagem ativa.

![Fly-in de longo alcance: ~150 m de downrange](assets/realistic_longrange.gif)

> Aproximacao completa a partir de **~150 m de distancia**: cruzeiro inclinado na direcao da barca, flip de frenagem e pouso vertical.

---

## Resultados

Modelo oficial treinado sob **criterios de pouso estritos**: um toque so conta
como pouso abaixo de 5 m/s vertical e 10° de inclinacao — mar, contato do
corpo e impactos rapidos sao crash. (Um booster real toca a < 2 m/s.)

| Fase de aproximacao (downrange) | Taxa de pouso valido (n=40, seeds held-out) |
|---------------------------------|----------------------------------------------|
| Fase 1 (25–45 m)   | **100%** |
| Fase 6 (140–160 m) | **82%** — zero crashes (falhas restantes: paira sem bater) |

Metricas de realismo comportamental (protocolo proprio, alem da taxa de pouso):

| Metrica | Valor |
|---------|-------|
| Velocidade vertical no toque | **1,47 m/s** em media (max 1,71) — territorio de booster real |
| Inclinacao maxima em voo | ate ~55° nas aproximacoes longas (inclina de verdade para se deslocar) |
| Inclinacao no toque | **0,2–0,9°** (endireita para pousar) |
| Uso do comando de rotacao | 100% dos passos |

---

## Destaques Tecnicos

- **Fisica 6DOF realista**: gravidade, arrasto, gimbal TVC, RCS, depleicao de massa, vento, CG baixo (instabilidade de pendulo)
- **Curriculo de aproximacao (fly-in)**: 6 fases progressivas, nascendo cada vez mais longe (25 → 160 m) com velocidade de entrada
- **Reward anti-hover**: timeout punido pior que crash e bonus de descida pago *por metro* (nao por passo) — eliminou o otimo local de "pairar" que travou 4 runs anteriores
- **Validacao com seeds held-out + early stopping**: o melhor modelo por validacao vence o melhor por fitness de treino no protocolo real (confirmado em duas runs)
- **Jogo interativo** (pygame): voo manual, corrida de 8 IAs, espectador navegando geracoes e replay acelerado do treinamento
- **Treino headless paralelo** com snapshots por geracao e checkpoint completo da populacao
- **412 testes** automatizados (fisica, env, reward, GA, modos de jogo em SDL headless)

---

## Arquitetura

```
Fisica 6DOF (dt=0.02s) → Env Gymnasium (obs 7D, MultiDiscrete [5 throttle × 3 rotacao])
    → Neuroevolucao GA (pop 200, rede [7, 64, 32, 8] = 1.480 parametros)
    → Validacao held-out por geracao → best-val checkpoint → jogo/GIFs
```

Baseline PPO (Stable-Baselines3) incluido para comparacao com o GA.

---

## Tecnologias

Python, NumPy, Gymnasium, pygame, multiprocessing (treino paralelo), Stable-Baselines3 (baseline PPO), pytest.

---

## Autor

**Fernando Marciano** — [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/)

---

## Licenca

**All Rights Reserved** — codigo-fonte em repositorio privado.

> Interessado no codigo ou em uma demonstracao? Entre em contato pelo [LinkedIn](https://www.linkedin.com/in/fernandopmarciano/).
