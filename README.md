# Freeway — Jogo Multiplayer em Rede

<img src="freeway/assets/car_red_small_3.png" height="40"> <img src="freeway/assets/chicken.png" height="40"> <img src="freeway/assets/car_blue_small_2.png" height="40">

Jogo inspirado no clássico Freeway do Atari, em que cada jogador controla um carro que
precisa atravessar a estrada desviando das galinhas. Desenvolvido para a disciplina de
**Sistemas Paralelos e Distribuídos (ECOi23 — UNIFEI Campus Itabira)**, com o mesmo jogo
implementado sobre **três tecnologias de comunicação diferentes** para comparar os modelos.
O estado do jogo é representado por uma matriz e desenhado com Java 2D.

Suporta até **2 jogadores** em rede, inclusive em máquinas diferentes.

## 🏗️ Arquitetura

O servidor é a fonte de verdade: mantém o estado do jogo (posições, obstáculos e placar),
recebe as ações dos clientes e transmite o estado atualizado para todos. A lógica do jogo e a
interface gráfica ficam em um módulo comum, reutilizado pelas três versões.

```
freeway/
├── freeway-common/    lógica do jogo (GameEngine, GameState) e interface gráfica (GamePanel)
├── freeway-sockets/   versão 1 — TCP Sockets
├── freeway-grpc/      versão 2 — gRPC com Protocol Buffers
└── freeway-mqtt/      versão 3 — publish/subscribe com MQTT
```

| Versão | Modelo de comunicação | Destaque |
|---|---|---|
| **Sockets** | Cliente-servidor sobre TCP | Uma thread por cliente (`ClientHandler`) e protocolo próprio |
| **gRPC** | RPC sobre HTTP/2 | Contrato em `.proto` e *server streaming* do estado do jogo |
| **MQTT** | Publish/subscribe via broker | Desacoplamento total: cliente e servidor só conhecem os tópicos |

## 🛠️ Tecnologias

![Java](https://img.shields.io/badge/Java_17-ED8B00?style=flat&logo=openjdk&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=flat&logo=apachemaven&logoColor=white)
![gRPC](https://img.shields.io/badge/gRPC-244c5a?style=flat&logo=grpc&logoColor=white)
![MQTT](https://img.shields.io/badge/MQTT-660066?style=flat&logo=mqtt&logoColor=white)

Java 17 · Maven (multi-módulo) · Swing · gRPC + Protocol Buffers · MQTT (broker HiveMQ)

## 🎮 Como jogar

- **Setas do teclado** — mover o carro
- Chegue ao topo da estrada para marcar pontos
- Evite as galinhas

## ▶️ Como executar

**Pré-requisitos:** Java 17+ e Maven (o IntelliJ já inclui o Maven em
`plugins/maven/lib/maven3/bin/mvn.cmd`). A versão MQTT precisa de internet.

Os scripts ficam na pasta `freeway/`. Em cada versão, abra um terminal para o servidor e
outro para cada jogador:

| Versão | Servidor | Cliente |
|---|---|---|
| Sockets | `rodar-sockets.bat` → `1` | `rodar-sockets.bat` → `2` → host `localhost` |
| gRPC (porta 5001) | `rodar-grpc.bat` → `1` | `rodar-grpc.bat` → `2` → host `localhost` |
| MQTT (`broker.hivemq.com:1883`) | `rodar-mqtt.bat` → `1` | `rodar-mqtt.bat` → `2` (sem host) |

Para **2 jogadores**, abra mais um terminal e repita o passo do cliente.

**Máquinas diferentes:** nas versões Sockets e gRPC, informe no cliente o IP da máquina do
servidor em vez de `localhost`. A versão MQTT não precisa de configuração extra, pois usa um
broker em nuvem.

## 📄 Enunciado

A especificação do trabalho está em [`Projeto1_-_Freeway.pdf`](Projeto1_-_Freeway.pdf).
