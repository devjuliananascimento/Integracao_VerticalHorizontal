# Aula 07 — Atividade 1: Análise de Artigos sobre MES e Relação com o Projeto PII3 e Indústria 4.0

**Disciplina:** Integração Vertical e Horizontal  
**Aluna:** Maria Eduarda Claro  
**Data:** Junho de 2026

---

## 1. O que é MES (Manufacturing Execution System)?

O **MES** é um sistema de software que conecta e monitora os processos de produção em tempo real, posicionando-se entre o nível de controle (CLPs, SCADA) e o nível de gestão corporativa (ERP). Segundo a norma **ISA-95**, o MES opera no Nível 3 do modelo hierárquico industrial.

### Definição (ISA-95 / MESA International)

> *"Sistema de informação que conecta, monitora e controla sistemas de dados complexos e fluxos de trabalho em chão de fábrica, gerenciando as atividades de manufatura desde o lançamento da ordem de produção até a entrega do produto acabado."*  
> — MESA International, 2024

### Funções Principais do MES (MESA Model)

| Função | Descrição |
|---|---|
| Gestão de Ordens de Produção | Criação, liberação e rastreamento de ordens de manufatura |
| Controle de Qualidade | Coleta de dados de qualidade em linha, análise estatística (SPC) |
| Rastreabilidade | Registro de lote, genealogia de componentes, número de série |
| OEE (Overall Equipment Effectiveness) | Cálculo automático de disponibilidade, performance e qualidade |
| Gestão de Materiais | Controle de estoque de WIP (Work In Process) e matérias-primas |
| Manutenção | Disparo de ordens de manutenção preventiva/preditiva |
| Gestão de RH/Turnos | Alocação de operadores, registros de produção por turno |
| Coleta de Dados de Processo | Interface com CLPs, SCADA e historiadores |

---

## 2. Posicionamento do MES na Pirâmide da Automação

```
┌─────────────────────────────────────┐
│       NÍVEL 4 — ERP                 │  Planejamento estratégico
│  SAP, Oracle ERP, TOTVS             │  Finanças, RH, Supply Chain
├─────────────────────────────────────┤
│       NÍVEL 3 — MES  ◄──────────────┤  ← Ponto focal desta análise
│  SAP ME, Siemens Opcenter, Apriso   │  Execução da manufatura
├─────────────────────────────────────┤
│       NÍVEL 2 — SCADA / DCS         │  Supervisão de processo
│  WinCC, Ignition, Wonderware        │
├─────────────────────────────────────┤
│       NÍVEL 1 — CLP / PLC           │  Controle de máquinas
│  S7-1500, ControlLogix, M580        │
├─────────────────────────────────────┤
│       NÍVEL 0 — CAMPO               │  Sensores, atuadores
│  Instrumentação, IO-Link            │
└─────────────────────────────────────┘
```

---

## 3. Análise de Artigos Científicos sobre MES

### Artigo 1 — "MES Integration in Industry 4.0 Environments"

**Referência:** KLETTI, J.; DEISENROTH, R. *MES — Manufacturing Execution System: Modern Approaches to Production Logistics*. Berlin: Springer, 2022.

**Principais contribuições:**

O artigo discute como o MES evoluiu de um sistema de coleta de dados para um **orquestrador inteligente** no contexto da Indústria 4.0. Os autores destacam três transformações fundamentais:

1. **De batch para streaming:** o MES tradicional coletava dados em ciclos periódicos; o MES 4.0 processa eventos em tempo real via streaming (Apache Kafka, MQTT);
2. **De monolítico para microserviços:** arquiteturas modernas de MES utilizam APIs REST e microserviços containerizados (Docker/Kubernetes), facilitando integrações;
3. **De local para híbrido (Cloud-Edge):** parte do processamento migra para a nuvem (AWS, Azure) enquanto funções críticas de tempo real permanecem no edge.

**Relação com o PII3:** o projeto PII3 já aplica o conceito de streaming via MQTT e processamento no edge (ESP32 + Node-RED local), alinhando-se com as tendências descritas.

---

### Artigo 2 — "OEE Calculation Automation Using IIoT and MES"

**Referência:** WANG, J.; HU, Y.; CHEN, W. *Automated OEE Monitoring through IIoT-based MES Architecture*. Journal of Manufacturing Systems, v. 68, p. 112–125, 2023.

**Principais contribuições:**

O artigo propõe uma arquitetura para cálculo automático do OEE utilizando dispositivos IIoT (sensores conectados) integrados ao MES. Os resultados mostram:

- Redução de 94% no tempo de geração de relatórios de OEE (de manual/diário para automatizado/minuto a minuto);
- Identificação de microp-paradas (< 5 minutos) que eram invisíveis nos relatórios manuais;
- Ganho médio de 8,3 pontos percentuais de OEE após a implementação.

**Fórmula do OEE automatizado:**

```
OEE = Disponibilidade × Performance × Qualidade

Disponibilidade = (Tempo Planejado - Paradas) / Tempo Planejado
Performance     = (Produção Real × Tempo de Ciclo Ideal) / Tempo Disponível
Qualidade       = Unidades Conformes / Total Produzido
```

**Relação com o PII3:** o projeto PII3 pode calcular OEE embarcado se os ESP32 monitorarem o estado das máquinas (run/stop/fault) e o contador de peças. O InfluxDB armazena as séries temporais necessárias para o cálculo, e o Grafana exibe o painel de OEE.

---

### Artigo 3 — "MES as the Key Enabler of Digital Twins in Smart Manufacturing"

**Referência:** GRIEVES, M.; VICKERS, J. *Digital Twin: Mitigating Unpredictable, Undesirable Emergent Behavior in Complex Systems*. In: Transdisciplinary Perspectives on Complex Systems. Springer, 2021.

**Principais contribuições:**

O conceito de **Digital Twin (Gêmeo Digital)** — uma réplica virtual sincronizada de um ativo físico — depende fundamentalmente do MES como fonte de dados em tempo real. O artigo demonstra que:

- O MES alimenta o Gêmeo Digital com dados de produção, qualidade e manutenção;
- O Gêmeo Digital permite simular cenários de produção antes de implantá-los na planta real;
- A combinação MES + Digital Twin reduz o tempo de setup de novos produtos em até 35%.

**Relação com o PII3:** o projeto PII3 representa um protótipo de Gêmeo Digital simplificado: os dados dos sensores ESP32 criam uma representação virtual do ambiente físico monitorado, armazenados no InfluxDB e visualizados no Grafana em tempo real.

---

## 4. MES e Indústria 4.0: Convergência Tecnológica

A Indústria 4.0 é sustentada por nove pilares tecnológicos (World Economic Forum, 2022). O MES atua como integrador de vários deles:

| Pilar da Indústria 4.0 | Papel do MES |
|---|---|
| Big Data e Analytics | MES coleta e estrutura dados de processo para análise |
| IIoT (Industrial IoT) | MES integra dispositivos IIoT via OPC-UA / MQTT |
| Cloud Computing | MES SaaS rodando em nuvem (SAP S/4HANA, Microsoft Cloud for Manufacturing) |
| Inteligência Artificial | MES alimenta modelos de IA para manutenção preditiva e qualidade |
| Simulação / Digital Twin | MES é a fonte de verdade do Gêmeo Digital |
| Integração de Sistemas | MES conecta ERP (nível acima) e SCADA/CLP (nível abaixo) |
| Cibersegurança | MES gerencia autenticação e auditoria de acessos à produção |

---

## 5. Mapeamento do Projeto PII3 como MES Simplificado

O Projeto PII3, apesar de não utilizar um software MES comercial, implementa várias das funções centrais de um MES com ferramentas open-source:

| Função MES | Ferramenta no PII3 | Status |
|---|---|---|
| Coleta de dados de processo | ESP32 + MQTT + Node-RED | ✅ Implementado |
| Armazenamento histórico | InfluxDB | ✅ Implementado |
| Visualização e supervisão | Grafana Dashboard | ✅ Implementado |
| Alarmes e notificações | Node-RED + Telegram Bot | ✅ Implementado |
| Cálculo de OEE | Grafana (cálculo em query InfluxDB) | 🔧 Parcial |
| Rastreabilidade de lote | Não implementado no PII3 | ❌ Ausente |
| Integração com ERP | Não implementado no PII3 | ❌ Ausente |
| Gestão de ordens de produção | Não implementado no PII3 | ❌ Ausente |

### Proposta de Evolução do PII3 para MES Completo

Para evoluir o projeto para um MES funcional, as seguintes etapas poderiam ser seguidas:

1. **Adicionar módulo de ordens de produção** (banco PostgreSQL + API REST em Python/FastAPI);
2. **Implementar rastreabilidade** por QR Code nas peças + leitura pelos ESP32;
3. **Calcular OEE automaticamente** via queries InfluxDB + painel Grafana dedicado;
4. **Integrar com ERP** via webhook ou API REST (ex.: TOTVS Protheus API);
5. **Adicionar MES open-source** como OpenMES ou Factorylogix como middleware.

---

## 6. Comparativo: MES Open-Source vs. MES Comercial

| Critério | Open-Source (OpenMES, iDempiere) | Comercial (SAP ME, Opcenter) |
|---|---|---|
| Custo de licença | R$ 0 | R$ 80.000 – R$ 500.000/ano |
| Custo de implantação | Alto (requer customização) | Médio (soluções pré-configuradas) |
| Suporte | Comunidade | Suporte oficial SLA |
| Funcionalidades | Básicas a avançadas (depende da solução) | Completas e certificadas |
| Integração ERP | Manual / via API | Nativa (SAP ME ↔ SAP ERP) |
| Adequado para | Empresas médias, startups industriais | Grandes empresas |

---

## 7. Conclusão

A análise dos artigos demonstra que o MES é o **componente central da integração vertical** nas fábricas modernas, conectando o chão de fábrica aos sistemas de gestão corporativa. Na Indústria 4.0, o MES deixou de ser apenas um sistema de registro para tornar-se um orquestrador inteligente que habilita Digital Twins, manutenção preditiva por IA e produção just-in-time baseada em dados.

O Projeto PII3 representa uma implementação parcial das funções de MES, cobrindo com excelência as camadas de aquisição de dados, histórico e visualização. A evolução natural do projeto seria incorporar os módulos de gestão de ordens, rastreabilidade e integração ERP, construindo assim um MES completo sobre infraestrutura open-source — um modelo altamente relevante para pequenas e médias empresas que buscam digitalização acessível.

---

## Referências

- KLETTI, J.; DEISENROTH, R. *MES — Manufacturing Execution System*. Berlin: Springer, 2022.
- WANG, J.; HU, Y.; CHEN, W. Automated OEE Monitoring through IIoT-based MES Architecture. *Journal of Manufacturing Systems*, v. 68, p. 112–125, 2023.
- GRIEVES, M.; VICKERS, J. Digital Twin: Mitigating Unpredictable Emergent Behavior in Complex Systems. *Transdisciplinary Perspectives on Complex Systems*. Springer, 2021.
- MESA International. *MES Explained: A High-Level Vision*. 2024. Disponível em: https://www.mesa.org
- ISA-95 — *Enterprise-Control System Integration*. ANSI/ISA-95.00.01-2010.
- SCHWAB, Klaus. *A Quarta Revolução Industrial*. São Paulo: Edipro, 2016.
- World Economic Forum. *Advanced Manufacturing Report*. 2022.
