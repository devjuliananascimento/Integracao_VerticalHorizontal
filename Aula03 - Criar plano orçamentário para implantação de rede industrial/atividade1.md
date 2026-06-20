# Aula 03 — Atividade 1: Plano Orçamentário para Implantação de Rede Industrial

**Disciplina:** Integração Vertical e Horizontal  
**Aluna:** Maria Eduarda Claro  
**Data:** Junho de 2026

---

## Contexto do Projeto

Este plano orçamentário foi desenvolvido para a implantação de uma rede industrial em uma planta de manufatura de médio porte, com o objetivo de integrar os níveis de chão de fábrica (campo), controle e supervisão conforme o modelo hierárquico da ISA-95 / Modelo de Purdue.

**Escopo:** Fábrica com 3 linhas de produção, 45 CLPs/PLCs instalados, área total de 8.000 m², com necessidade de comunicação em tempo real entre sensores, controladores, servidores de supervisão e sistemas corporativos.

---

## 1. Estrutura da Rede Industrial Proposta

A rede será implantada em 3 níveis hierárquicos:

| Nível | Descrição | Tecnologia |
|---|---|---|
| Nível 0-1 (Campo) | Sensores, atuadores e instrumentos de campo | PROFIBUS PA, IO-Link |
| Nível 1-2 (Controle) | CLPs, drives e controladores de processo | PROFINET, EtherNet/IP |
| Nível 2-3 (Supervisão) | SCADA, HMIs e servidores de dados | Ethernet Industrial (IEEE 802.3) |
| Nível 3-4 (Corporativo) | ERP, MES e sistemas de gestão | TCP/IP Corporativo |

---

## 2. Plano Orçamentário Detalhado

### 2.1 Infraestrutura de Rede Passiva

| Item | Especificação | Qtd | Valor Unit. (R$) | Total (R$) |
|---|---|---|---|---|
| Cabo STP Cat6A Industrial | 305m/bobina, blindado, 600V | 12 bobinas | 890,00 | 10.680,00 |
| Cabo Profibus DP | 2 fios, 22 AWG blindado | 500m | 18,50/m | 9.250,00 |
| Eletroduto conduit metálico | 3/4" EMT, galvanizado | 800m | 12,80/m | 10.240,00 |
| Calha eletrocalha perfurada | 100x50mm, aço galvanizado | 200m | 34,00/m | 6.800,00 |
| Patch panel 24 portas Cat6A | Industrial, DIN-rail | 8 un | 420,00 | 3.360,00 |
| Conectores RJ45 Cat6A industrial | IP67, blindado | 200 un | 28,00 | 5.600,00 |
| **Subtotal Infraestrutura Passiva** | | | | **45.930,00** |

### 2.2 Equipamentos de Rede Ativa

| Item | Fabricante/Modelo | Qtd | Valor Unit. (R$) | Total (R$) |
|---|---|---|---|---|
| Switch gerenciável industrial L2 | Siemens SCALANCE X208 | 6 un | 4.800,00 | 28.800,00 |
| Switch gerenciável industrial L3 (core) | Siemens SCALANCE X308-2M | 2 un | 12.400,00 | 24.800,00 |
| Roteador industrial firewall | Moxa EDR-810 | 2 un | 8.600,00 | 17.200,00 |
| Access point industrial Wi-Fi 6 | Hirschmann BAT-C2 | 4 un | 3.200,00 | 12.800,00 |
| Conversor de mídia fibra óptica | Phoenix Contact FL MC | 12 un | 980,00 | 11.760,00 |
| **Subtotal Equipamentos Ativos** | | | | **95.360,00** |

### 2.3 Servidores e Infraestrutura de TI Industrial

| Item | Especificação | Qtd | Valor Unit. (R$) | Total (R$) |
|---|---|---|---|---|
| Servidor de SCADA/MES | Dell PowerEdge R550, 32GB RAM, 2TB NVMe | 2 un | 18.500,00 | 37.000,00 |
| Servidor de dados históricos | OSIsoft PI Data Archive | 1 un | 42.000,00 | 42.000,00 |
| No-break (UPS) industrial | APC Smart-UPS 3000VA Industrial | 4 un | 5.800,00 | 23.200,00 |
| Rack industrial 19" 24U | Engebras RI-2400-C, IP55 | 3 un | 3.600,00 | 10.800,00 |
| Switch KVM industrial | Raritan Dominion KX | 2 un | 2.400,00 | 4.800,00 |
| **Subtotal Servidores e TI** | | | | **117.800,00** |

### 2.4 Software e Licenças

| Item | Fornecedor | Qtd/Tipo | Valor (R$) |
|---|---|---|---|
| SCADA Supervisório (WinCC SCADA) | Siemens | 500 tags | 28.000,00 |
| Software Historiador (OSIsoft PI) | AVEVA | 1.000 tags/5 anos | 85.000,00 |
| Antivírus industrial (Trend Micro ICS) | Trend Micro | 20 licenças | 12.000,00 |
| Sistema operacional Windows Server 2025 | Microsoft | 3 licenças | 9.600,00 |
| **Subtotal Software e Licenças** | | | | **134.600,00** |

### 2.5 Serviços de Implantação

| Item | Descrição | Valor (R$) |
|---|---|---|
| Projeto de engenharia de redes | Especificação, diagramas, documentação técnica | 18.000,00 |
| Instalação física de cabeamento | Passagem de cabos, eletrodutos, organização | 24.000,00 |
| Configuração de switches e roteadores | Parametrização, VLANs, QoS, segurança | 16.000,00 |
| Comissionamento e testes | FAT, SAT, testes de redundância | 12.000,00 |
| Treinamento da equipe (20h) | Operação e manutenção da rede | 8.000,00 |
| Documentação final (As-Built) | Diagramas revisados, manual de operação | 6.000,00 |
| **Subtotal Serviços** | | | | **84.000,00** |

---

## 3. Resumo Geral do Orçamento

| Categoria | Valor (R$) | % do Total |
|---|---|---|
| Infraestrutura Passiva | 45.930,00 | 9,6% |
| Equipamentos de Rede Ativos | 95.360,00 | 20,0% |
| Servidores e TI Industrial | 117.800,00 | 24,7% |
| Software e Licenças | 134.600,00 | 28,2% |
| Serviços de Implantação | 84.000,00 | 17,6% |
| **TOTAL** | **477.690,00** | **100%** |

**Contingência (10%):** R$ 47.769,00  
**TOTAL COM CONTINGÊNCIA:** R$ **525.459,00**

---

## 4. Cronograma de Desembolso

| Mês | Evento | Desembolso Previsto (R$) |
|---|---|---|
| Mês 1 | Aprovação do projeto + contratação de serviços de engenharia | 50.000,00 |
| Mês 2 | Aquisição de equipamentos passivos e ativos | 141.290,00 |
| Mês 3 | Aquisição de servidores e UPS | 117.800,00 |
| Mês 4 | Instalação física de infraestrutura | 40.000,00 |
| Mês 5 | Compra de software e configuração de equipamentos | 150.600,00 |
| Mês 6 | Comissionamento, testes e treinamento | 26.000,00 |
| **Total** | | **525.690,00** |

---

## 5. Considerações Finais

Este plano orçamentário foi elaborado com base em preços de mercado pesquisados em junho de 2026, com cotações de distribuidores autorizados dos fabricantes citados. Todos os valores são referenciais e devem ser validados por meio de processo formal de cotação (RFQ) antes da aprovação do investimento.

A priorização dos itens deve considerar o cronograma de paradas da planta, de modo a minimizar impactos na produção durante a fase de instalação.

---

## Referências

- Catálogo Siemens SCALANCE 2026. Disponível em: https://www.siemens.com/br
- Catálogo Moxa Industrial Networking 2025.
- Phoenix Contact — Produtos para Redes Industriais.
- PROFIBUS & PROFINET International (PI). *Guia de Aplicação PROFINET*. 2024.
