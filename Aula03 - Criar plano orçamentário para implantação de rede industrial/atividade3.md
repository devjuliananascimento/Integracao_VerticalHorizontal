# Aula 03 — Atividade 3: Análise de Viabilidade Técnica e Econômica

**Disciplina:** Integração Vertical e Horizontal  
**Aluna:** Maria Eduarda Claro  
**Data:** Junho de 2026

---

## Contexto da Análise

Este documento apresenta a análise de viabilidade técnica e econômica para a implantação da rede industrial descrita nas Atividades 1 e 2, considerando os ganhos operacionais esperados em contrapartida ao investimento total de R$ 525.459,00.

**Empresa de referência:** Planta industrial de médio porte — 3 linhas de produção contínua  
**Produção atual:** 18.000 unidades/mês  
**Faturamento anual:** R$ 38,4 milhões

---

## 1. Viabilidade Técnica

### 1.1 Diagnóstico da Infraestrutura Atual

| Aspecto Avaliado | Situação Atual | Situação Proposta |
|---|---|---|
| Protocolo de comunicação | Modbus RTU (serial) | PROFINET + EtherNet/IP |
| Velocidade de rede | 9.600 bps (serial) | 100 Mbps (Ethernet industrial) |
| Topologia | Barramento linear | Estrela com anel redundante |
| Redundância de rede | Inexistente | MRP (< 200 ms de recuperação) |
| Monitoramento remoto | Não disponível | SCADA centralizado |
| Segmentação OT/TI | Inexistente | DMZ industrial com firewall |
| Tempo médio de diagnóstico de falha | 4,2 horas | < 15 minutos (alarme automático) |

### 1.2 Requisitos Técnicos Atendidos

- [x] Comunicação em tempo real entre CLPs e SCADA (< 10 ms de latência)
- [x] Redundância de rede com failover automático (IEC 62439-2 / MRP)
- [x] Segregação da rede industrial da rede corporativa (ANSI/ISA-99)
- [x] Capacidade de expansão para até 200 nós adicionais
- [x] Suporte a protocolos legados via gateways (Modbus RTU → Modbus TCP)
- [x] Conformidade com normas de segurança IEC 62443

### 1.3 Riscos Técnicos Identificados

| Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|
| Incompatibilidade de equipamentos legados | Média | Alto | Uso de conversores de protocolo (gateways) |
| Interferência eletromagnética (EMI) no chão de fábrica | Alta | Médio | Cabos blindados STP e aterramento adequado |
| Instabilidade durante a migração | Média | Alto | Migração em janelas de manutenção programada |
| Falta de pessoal técnico qualificado | Média | Médio | Treinamento prévio e suporte pós-implantação |

---

## 2. Viabilidade Econômica

### 2.1 Investimento Total (CAPEX)

| Item | Valor (R$) |
|---|---|
| Infraestrutura passiva | 45.930,00 |
| Equipamentos de rede ativos | 95.360,00 |
| Servidores e TI industrial | 117.800,00 |
| Software e licenças | 134.600,00 |
| Serviços de implantação | 84.000,00 |
| Contingência (10%) | 47.769,00 |
| **CAPEX Total** | **525.459,00** |

### 2.2 Custos Operacionais Anuais (OPEX)

| Item | Valor Anual (R$) |
|---|---|
| Manutenção de equipamentos (contrato) | 28.000,00 |
| Atualização de licenças de software | 18.000,00 |
| Suporte técnico especializado | 24.000,00 |
| Energia elétrica (servidores + ativos de rede) | 8.400,00 |
| **OPEX Total Anual** | **78.400,00** |

### 2.3 Benefícios Econômicos Esperados

Os ganhos foram estimados com base em benchmarks de projetos similares no setor industrial brasileiro (ABIMAQ, 2024):

| Benefício | Base de Cálculo | Ganho Anual Estimado (R$) |
|---|---|---|
| Redução de paradas não planejadas | De 12 paradas/ano (4h avg) para 3 paradas/ano | 186.000,00 |
| Ganho de produtividade (OEE) | Aumento de 71% para 83% de OEE | 648.000,00 |
| Redução de desperdício/refugo | De 3,8% para 1,5% de taxa de refugo | 221.760,00 |
| Redução de consumo de energia | Monitoramento e otimização via SCADA | 54.000,00 |
| Redução de horas de manutenção corretiva | De 320h/ano para 80h/ano | 84.000,00 |
| Redução de custos de conformidade | Rastreabilidade automática (auditorias) | 36.000,00 |
| **Total de Benefícios Anuais** | | **1.229.760,00** |

> **Detalhamento do ganho de OEE:**  
> Produção atual: 18.000 un/mês × 12 = 216.000 un/ano a R$ 178/un = R$ 38,4 M/ano  
> Com aumento de OEE de 71% → 83%: ganho de 12% na capacidade = +25.920 un/ano  
> Margem de contribuição de R$ 25/un → ganho líquido de R$ 648.000,00/ano

### 2.4 Fluxo de Caixa Projetado (5 anos)

| Ano | CAPEX (R$) | OPEX (R$) | Benefícios (R$) | Fluxo Líquido (R$) | Acumulado (R$) |
|---|---|---|---|---|---|
| 0 | (525.459,00) | — | — | (525.459,00) | (525.459,00) |
| 1 | — | (78.400,00) | 921.720,00* | 843.320,00 | 317.861,00 |
| 2 | — | (78.400,00) | 1.229.760,00 | 1.151.360,00 | 1.469.221,00 |
| 3 | — | (78.400,00) | 1.229.760,00 | 1.151.360,00 | 2.620.581,00 |
| 4 | — | (78.400,00) | 1.229.760,00 | 1.151.360,00 | 3.771.941,00 |
| 5 | — | (78.400,00) | 1.229.760,00 | 1.151.360,00 | 4.923.301,00 |

> \* Ano 1 com 75% dos benefícios realizados (ramp-up de implantação)

### 2.5 Indicadores de Retorno

| Indicador | Valor |
|---|---|
| **Payback Simples** | **7,4 meses** |
| **Payback Descontado (TMA 12% a.a.)** | **8,9 meses** |
| **VPL (5 anos, TMA 12%)** | **R$ 3.628.400,00** |
| **TIR (Taxa Interna de Retorno)** | **219% a.a.** |
| **ROI (5 anos)** | **836%** |

### 2.6 Análise de Sensibilidade

Variação do VPL em função dos benefícios realizados:

| Cenário | % Benefícios Realizados | VPL (5 anos) | Payback |
|---|---|---|---|
| Pessimista | 50% | R$ 1.529.000,00 | 12,8 meses |
| Base | 100% | R$ 3.628.400,00 | 8,9 meses |
| Otimista | 120% | R$ 4.432.000,00 | 7,4 meses |

Mesmo no cenário pessimista, com apenas 50% dos benefícios realizados, o VPL permanece positivo e o payback ocorre em menos de 13 meses — demonstrando a robustez econômica do investimento.

---

## 3. Análise de Break-Even

O ponto de equilíbrio financeiro do projeto ocorre quando o fluxo de caixa acumulado retorna ao zero (recuperação do CAPEX).

```
CAPEX = R$ 525.459,00
Benefício Líquido Mensal (Benefícios - OPEX/12) = R$ 1.229.760 - R$ 78.400 / 12 = R$ 95.947/mês

Break-even = R$ 525.459 / R$ 95.947 ≈ 5,5 meses de operação plena
```

---

## 4. Conclusão da Análise

A análise de viabilidade técnica confirma que a infraestrutura proposta é tecnicamente sólida, atendendo aos requisitos de comunicação industrial em tempo real, redundância, segurança e escalabilidade. Os principais riscos técnicos são gerenciáveis com as mitigações propostas.

Do ponto de vista econômico, o projeto apresenta indicadores excepcionais: **payback inferior a 9 meses**, **TIR de 219%** e **VPL de R$ 3,6 milhões** em 5 anos — resultado da combinação de ganhos de produtividade, redução de paradas e eliminação de desperdícios que a visibilidade em tempo real proporcionada pela rede industrial viabiliza.

A recomendação é pela **aprovação e execução imediata do projeto**, priorizando as fases de infraestrutura passiva e equipamentos ativos, que são os habilitadores de todos os demais benefícios.

---

## Referências

- ABIMAQ — Associação Brasileira da Indústria de Máquinas e Equipamentos. *Benchmarks de Automação Industrial*. 2024.
- LMI Consulting. *Industrial Network ROI Calculator*. 2025.
- IEC 62443 — *Security for Industrial Automation and Control Systems*.
- ANSI/ISA-95 — *Enterprise-Control System Integration*.
