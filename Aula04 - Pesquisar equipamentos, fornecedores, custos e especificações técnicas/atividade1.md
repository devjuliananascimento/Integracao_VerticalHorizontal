# Aula 04 — Atividade 1: Análise da Aplicação de SDCD no Projeto Integrador (ESP32, MQTT, Servidor e Dashboard)

**Disciplina:** Integração Vertical e Horizontal  
**Aluna:** Maria Eduarda Claro  
**Data:** Junho de 2026

---

## 1. Introdução ao SDCD (Sistema Digital de Controle Distribuído)

O **SDCD** — também conhecido pela sigla inglesa **DCS (Distributed Control System)** — é uma arquitetura de controle industrial em que a lógica de controle é distribuída por múltiplos controladores ao longo da planta, ao contrário dos sistemas centralizados onde um único controlador processa todo o processo.

### Características Fundamentais do SDCD

| Característica | Descrição |
|---|---|
| Distribuição | A inteligência de controle está distribuída em controladores locais (field controllers) |
| Redundância | Controladores, fontes e links de comunicação são duplicados |
| Integração | Todos os controladores comunicam-se por um barramento de dados comum |
| Supervisão centralizada | Operadores monitoram e intervêm via estações de trabalho centralizadas |
| Histórico | Dados de processo são armazenados em servidores de historiador |

### SDCD vs. CLP (PLC): Diferenças Principais

| Aspecto | SDCD/DCS | CLP/PLC |
|---|---|---|
| Foco | Controle contínuo de processos | Controle discreto de máquinas |
| Aplicação típica | Refinarias, petroquímica, alimentos | Linhas de montagem, robótica |
| Configuração | Baseada em função de blocos | Baseada em ladder/ST/FBD |
| Escalabilidade | Alta (milhares de tags) | Moderada |
| Custo | Alto | Moderado a baixo |

---

## 2. Arquitetura do Projeto Integrador (PII3)

O Projeto Integrador PII3 implementa um **SDCD de baixo custo** utilizando tecnologias abertas e acessíveis — uma abordagem alinhada à Indústria 4.0 que democratiza o acesso às funcionalidades de sistemas SCADA/DCS tradicionais.

### 2.1 Visão Geral da Arquitetura

```
┌────────────────────────────────────────────────────────────────┐
│                    CAMADA DE CAMPO                              │
│                                                                  │
│   [ESP32 Node 1]    [ESP32 Node 2]    [ESP32 Node 3]           │
│   Temperatura       Umidade           Pressão / Fluxo          │
│   Sensor DHT22      Sensor SHT31      Sensor BMP280            │
│   Atuador Relé      Atuador Servo     Atuador Bomba             │
└──────────────────────────────┬─────────────────────────────────┘
                               │  Wi-Fi / MQTT
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                    CAMADA DE COMUNICAÇÃO                       │
│                                                                │
│              [MQTT Broker — Mosquitto]                         │
│               Tópicos: sensors/+/data                         │
│                        actuators/+/cmd                        │
│                        alerts/+/status                        │
└──────────────────────────────┬─────────────────────────────────┘
                               │  MQTT / TCP
                               ▼
┌──────────────────────────────────────────────────────────────┐
│               CAMADA DE SERVIDOR / PROCESSAMENTO              │
│                                                                │
│   [Node-RED]          [InfluxDB]          [Servidor Python]   │
│   Regras de negócio   Banco de dados      Lógica de controle  │
│   Integração          time-series         Alarmes e setpoints  │
└──────────────────────────────┬─────────────────────────────────┘
                               │  HTTP / WebSocket
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                    CAMADA DE VISUALIZAÇÃO                      │
│                                                                │
│               [Grafana Dashboard]                              │
│               Gráficos em tempo real                          │
│               Alarmes e notificações                           │
│               Relatórios históricos                            │
└──────────────────────────────────────────────────────────────┘
```

---

## 3. Componentes do Sistema

### 3.1 ESP32 — Controlador de Campo

O **ESP32** (Espressif Systems) é o "controlador distribuído" do projeto. Cada nó ESP32 é responsável por:

- Leitura de sensores locais (temperatura, umidade, pressão, fluxo);
- Acionamento de atuadores (relés, servos, bombas, válvulas solenoides);
- Publicação de dados no broker MQTT via Wi-Fi;
- Recebimento de comandos do servidor para controle dos atuadores;
- Execução de lógica de controle local (ex.: PID embarcado simples).

**Especificações relevantes do ESP32:**

| Parâmetro | Valor |
|---|---|
| Processador | Xtensa LX6 dual-core 240 MHz |
| Memória RAM | 520 KB SRAM |
| Flash | 4 MB (expansível) |
| Conectividade | Wi-Fi 802.11 b/g/n + Bluetooth 4.2 |
| GPIOs | 34 pinos programáveis |
| ADC | 18 canais, 12 bits |
| Custo aprox. | R$ 35–55 (módulo DevKit) |

**Código de exemplo — publicação MQTT (ESP32 / Arduino):**

```cpp
#include <WiFi.h>
#include <PubSubClient.h>
#include <DHT.h>

#define DHTPIN 4
#define DHTTYPE DHT22
#define MQTT_TOPIC "sensors/node1/data"

DHT dht(DHTPIN, DHTTYPE);
WiFiClient espClient;
PubSubClient client(espClient);

void publishSensorData() {
    float temp = dht.readTemperature();
    float hum  = dht.readHumidity();
    
    String payload = "{\"temperature\":" + String(temp) +
                     ",\"humidity\":"    + String(hum)  +
                     ",\"node\":\"node1\"}";
    
    client.publish(MQTT_TOPIC, payload.c_str());
}
```

---

### 3.2 MQTT — Protocolo de Comunicação

O **MQTT (Message Queuing Telemetry Transport)** é o protocolo de comunicação central do sistema, escolhido por suas características ideais para IoT industrial:

| Característica | Valor |
|---|---|
| Modelo | Publish/Subscribe (assíncrono) |
| Overhead de protocolo | Mínimo (2 bytes de cabeçalho fixo) |
| QoS disponível | 0 (no máximo 1 vez), 1 (pelo menos 1 vez), 2 (exatamente 1 vez) |
| Porta padrão | 1883 (TCP) / 8883 (TLS) |
| Broker utilizado | Eclipse Mosquitto (open source) |

**Estrutura de tópicos do projeto:**

```
pii3/
├── sensors/
│   ├── node1/
│   │   ├── temperature    → {"value": 23.5, "unit": "°C", "ts": 1718000000}
│   │   ├── humidity       → {"value": 65.2, "unit": "%", "ts": 1718000000}
│   │   └── status         → {"online": true, "rssi": -68}
│   ├── node2/
│   └── node3/
├── actuators/
│   ├── node1/
│   │   ├── relay1/cmd     → {"state": "ON"}
│   │   └── relay1/status  → {"state": "ON", "ts": 1718000000}
└── alerts/
    └── node1/             → {"level": "WARNING", "msg": "Temp > 30°C"}
```

---

### 3.3 Servidor — Node-RED + InfluxDB

**Node-RED** funciona como a camada de integração e regras de negócio:
- Subscreve tópicos MQTT dos sensores;
- Aplica regras de alarme e setpoints configuráveis;
- Persiste dados no InfluxDB (banco de dados time-series);
- Publica comandos nos tópicos de atuadores.

**InfluxDB** armazena as séries temporais de dados do processo, permitindo:
- Consultas históricas com alta performance;
- Retenção configurável de dados;
- Integração nativa com Grafana.

---

### 3.4 Dashboard — Grafana

O **Grafana** é a interface de visualização (equivalente à HMI/SCADA do projeto), oferecendo:
- Painéis com gráficos em tempo real (atualização a cada 5s);
- Indicadores de status dos nós (online/offline);
- Histórico de leituras com zoom e anotações;
- Configuração de alertas por e-mail/Telegram;
- Acesso via browser (desktop e mobile).

---

## 4. Paralelo com SDCD Industrial Tradicional

| Componente SDCD Tradicional | Equivalente no Projeto PII3 |
|---|---|
| Field Controllers (Yokogawa Centum VP) | ESP32 com sensores e atuadores |
| Barramento de dados (Foundation Fieldbus) | MQTT over Wi-Fi |
| Historiador de dados (OSIsoft PI) | InfluxDB |
| Engineering Station | Node-RED (flow programming) |
| Operator Station (HMI) | Grafana Dashboard |
| Servidor de alarmes | Node-RED (regras de alarme) + Telegram Bot |

---

## 5. Análise da Integração Vertical no Projeto PII3

O projeto PII3 realiza **integração vertical** ao conectar dados do nível de campo (sensores físicos) até o nível de supervisão (dashboard Grafana), passando pelas camadas intermediárias de comunicação e processamento:

```
Nível 0 — Sensor físico (DHT22, BMP280)
    ↓ GPIO / I²C / SPI
Nível 1 — ESP32 (controlador de campo distribuído)
    ↓ MQTT / Wi-Fi
Nível 2 — Broker Mosquitto (middleware de comunicação)
    ↓ Subscribe / Publish
Nível 3 — Node-RED + InfluxDB (processamento e histórico)
    ↓ HTTP / WebSocket
Nível 4 — Grafana (supervisão e visualização)
```

Essa cadeia de integração permite que um operador no dashboard observe em tempo real o que está acontecendo fisicamente no ambiente monitorado — exatamente o princípio fundamental de qualquer SDCD.

---

## 6. Vantagens e Limitações da Abordagem

### Vantagens
- **Custo reduzido:** ESP32 (R$ 45) vs. field controller industrial (R$ 8.000+)
- **Flexibilidade:** programação em C++/MicroPython, open-source
- **Escalabilidade horizontal:** novos nós adicionados sem reconfiguração central
- **Integração com nuvem:** MQTT funciona com AWS IoT, Azure IoT Hub, Google Cloud IoT

### Limitações
- **Tempo real:** Wi-Fi não garante determinismo (latência variável vs. PROFINET IRT < 1ms)
- **Confiabilidade:** ESP32 não possui certificações industriais (IP, ATEX)
- **Segurança:** MQTT sem TLS é vulnerável a interceptação
- **Redundância:** sem mecanismo nativo de failover como em DCS industriais

---

## 7. Conclusão

O Projeto PII3 demonstra de forma prática como os conceitos de SDCD podem ser implementados com tecnologias acessíveis e abertas, mantendo a essência arquitetural dos sistemas industriais de controle distribuído: **dados coletados no campo → comunicados por protocolo padronizado → processados e armazenados → visualizados em dashboard centralizado**.

Essa abordagem é representativa da Indústria 4.0, onde o IoT industrial (IIoT) democratiza o acesso às funcionalidades antes restritas a sistemas DCS de alto custo. A evolução natural do projeto seria a adoção de protocolos com maior determinismo (OPC-UA, MQTT-SN via LoRaWAN) e a implementação de segurança por camadas (TLS, autenticação x.509).

---

## Referências

- Espressif Systems. *ESP32 Technical Reference Manual*. 2024.
- OASIS MQTT Technical Committee. *MQTT Version 5.0 Specification*. 2019.
- Eclipse Foundation. *Eclipse Mosquitto Documentation*. 2024.
- InfluxData. *InfluxDB 2.0 Documentation*. 2024.
- Grafana Labs. *Grafana Documentation*. 2024.
- YOKOGAWA. *What is DCS?* — Application Notes. 2023.
- IEC 61784-1 — *Communication profiles for industrial networks*.
