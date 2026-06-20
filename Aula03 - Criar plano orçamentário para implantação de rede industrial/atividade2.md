# Aula 03 — Atividade 2: Pesquisa de Equipamentos, Fornecedores, Custos e Especificações Técnicas

**Disciplina:** Integração Vertical e Horizontal  
**Aluna:** Maria Eduarda Claro  
**Data:** Junho de 2026

---

## Introdução

Esta atividade apresenta uma pesquisa detalhada sobre os principais equipamentos utilizados em redes industriais, incluindo especificações técnicas, fornecedores referência no mercado nacional e internacional, e faixas de preço praticadas. O levantamento tem como base os requisitos de uma planta industrial de médio porte com comunicação em tempo real entre campo, controle e supervisão.

---

## 1. Switches Industriais

Os switches industriais diferenciam-se dos switches corporativos por suportar temperaturas extremas, vibração, umidade elevada e tensões de alimentação em 24 Vcc — condições comuns no ambiente de chão de fábrica.

### 1.1 Switches Não Gerenciáveis (Nível de Campo)

| Fabricante | Modelo | Portas | Velocidade | Alimentação | Temperatura | Preço Aprox. (R$) |
|---|---|---|---|---|---|---|
| Phoenix Contact | FL SWITCH 1000 | 8x RJ45 | 10/100 Mbps | 24 Vcc | -20°C a 60°C | 1.200,00 |
| Moxa | EDS-208A | 8x RJ45 | 10/100 Mbps | 12-48 Vcc | -10°C a 60°C | 1.450,00 |
| Wago | 852-1816 | 8x RJ45 | 10/100 Mbps | 24 Vcc | -20°C a 55°C | 980,00 |

### 1.2 Switches Gerenciáveis L2 (Nível de Controle)

| Fabricante | Modelo | Portas | Recursos | Redundância | Preço Aprox. (R$) |
|---|---|---|---|---|---|
| Siemens | SCALANCE X208 | 8x RJ45 + 2 fibra | VLAN, RSTP, SNMP | MRP / RSTP | 4.800,00 |
| Hirschmann | MACH1040 | 24x RJ45 + 4 SFP | VLAN, QoS, IGMP | HIPER-Ring | 6.200,00 |
| Rockwell | Stratix 5400 | 20x RJ45 + 4 SFP | CIP sync, PROFINET | REP / RSTP | 8.400,00 |
| Cisco | IE-3400H | 24x RJ45 | VLAN, QoS, 802.1AS | ERPS / REP | 9.100,00 |

### 1.3 Switches Core L3 (Backbone Industrial)

| Fabricante | Modelo | Portas | Roteamento | Redundância | Preço Aprox. (R$) |
|---|---|---|---|---|---|
| Siemens | SCALANCE X308-2M | 8x RJ45 + 2 fibra MM/SM | OSPF, BGP | MRP, VRRP | 12.400,00 |
| Moxa | PT-7528 | 28 portas Giga | OSPF, RIPv2 | Turbo Ring v2 | 14.800,00 |

---

## 2. Roteadores e Firewalls Industriais

Equipamentos responsáveis pela separação das redes de TI (corporativa) e OT (operacional), com funções de NAT, VPN, filtragem de pacotes e inspeção de protocolo industrial.

| Fabricante | Modelo | VPN | Firewall | Protocolos Ind. | DIN-rail | Preço Aprox. (R$) |
|---|---|---|---|---|---|---|
| Moxa | EDR-810 | IPsec, OpenVPN | SPI, ACL | Modbus, DNP3 | Sim | 8.600,00 |
| Phoenix Contact | mGuard RS4000 | IPsec | Stateful | IEC 61850 | Sim | 10.200,00 |
| Cisco | IR1101 | IPsec, DMVPN | Zone-based | SCADA protocols | Sim | 11.500,00 |
| Fortinet | FortiGate Rugged 60F | SSL-VPN | NGFW | OT protocols | Não | 18.000,00 |

---

## 3. CLPs / PLCs (Controladores Lógicos Programáveis)

Equipamentos responsáveis pelo controle dos processos industriais no nível 1-2 do modelo de Purdue.

### 3.1 CLPs de Médio Porte

| Fabricante | Modelo | CPU | Memória | Comunicação | Preço Aprox. (R$) |
|---|---|---|---|---|---|
| Siemens | S7-1500 CPU 1515-2 PN | 300 ns/instrução | 3 MB trabalho | PROFINET, MPI, DP | 12.800,00 |
| Rockwell | ControlLogix 5580 | Dual-core | 40 MB | EtherNet/IP, DH+ | 18.600,00 |
| Schneider | Modicon M580 | RISC 800 MHz | 8 MB | PROFIBUS, Modbus TCP | 9.400,00 |
| Mitsubishi | MELSEC iQ-R R08CPU | 1,79 ns/instrução | 64 KB passos | CC-Link IE, Modbus | 8.200,00 |

### 3.2 CLPs Compactos (para Máquinas)

| Fabricante | Modelo | E/S integradas | Comunicação | Preço Aprox. (R$) |
|---|---|---|---|---|
| Siemens | S7-1200 CPU 1214C | 14DI / 10DO / 2AI | PROFINET, Modbus TCP | 2.400,00 |
| Omron | CP2E-N60 | 36DI / 24DO | EtherNet/IP, RS-232 | 3.100,00 |
| Schneider | Modicon M221 | 24DI / 16DO | Ethernet, Serial | 1.800,00 |

---

## 4. HMIs (Interface Homem-Máquina)

Painéis de operação instalados nas máquinas e células de produção para visualização e comando local.

| Fabricante | Modelo | Tela | Resolução | IP | Protocolos | Preço Aprox. (R$) |
|---|---|---|---|---|---|---|
| Siemens | KTP900 Basic | 9" TFT touch | 800x480 | IP65 | PROFINET | 3.800,00 |
| Weintek | MT8102iE | 10" TFT touch | 1024x600 | IP65 | Modbus, EtherNet/IP | 2.600,00 |
| Advantech | TPC-1782H | 17" touch industrial | 1280x1024 | IP65 | Ethernet, Serial | 7.200,00 |
| Rockwell | PanelView 5510 | 10" TFT touch | 1024x768 | IP65 | EtherNet/IP | 8.400,00 |

---

## 5. Servidores SCADA e Historiadores de Dados

| Fabricante | Produto | Função | Tags incluídas | Preço Aprox. (R$) |
|---|---|---|---|---|
| Siemens | WinCC SCADA V8.0 | Supervisório/SCADA | 500 tags | 28.000,00 |
| AVEVA | System Platform | SCADA + MES lite | 2.000 tags | 95.000,00 |
| Inductive Automation | Ignition 8.x | SCADA + Historian | Ilimitado | 48.000,00 |
| OSIsoft (AVEVA) | PI System | Historiador industrial | 1.000 tags | 85.000,00 |
| Elipse | E3 SCADA | Supervisório nacional | 1.000 tags | 22.000,00 |

---

## 6. Instrumentos de Campo

### 6.1 Sensores de Temperatura

| Fabricante | Modelo | Tipo | Saída | Faixa | Preço Aprox. (R$) |
|---|---|---|---|---|---|
| Endress+Hauser | iTEMP TMT82 | RTD/Termopar | HART, PA | -200°C a 850°C | 1.800,00 |
| Yokogawa | YTA510 | Termopar | HART 5.x | -200°C a 1200°C | 1.600,00 |
| Siemens | SITRANS TH400 | RTD/TC | HART, PA | -50°C a 850°C | 1.400,00 |

### 6.2 Transmissores de Pressão

| Fabricante | Modelo | Faixa | Saída | Preço Aprox. (R$) |
|---|---|---|---|---|
| Emerson | Rosemount 3051 | 0 a 250 bar | HART, 4-20 mA | 3.200,00 |
| Endress+Hauser | Cerabar PMC71 | 0 a 400 bar | HART, PROFIBUS PA | 2.800,00 |
| ABB | 266 Series | 0 a 700 bar | HART 7.x, FF | 2.600,00 |

---

## 7. Principais Fornecedores no Brasil

| Empresa | Segmento | Representação de | Presença |
|---|---|---|---|
| AXXON (São Paulo) | Automação e redes industriais | Moxa, Hirschmann | SP, RJ, MG |
| Kalatec Automação | Siemens, Phoenix Contact | Siemens | Nacional |
| Rockwell Automation Brasil | Automação industrial completa | Allen-Bradley, Rockwell | Nacional |
| Elipse Software | SCADA nacional | Elipse | Nacional |
| Pepperl+Fuchs | Instrumentação de campo | P+F, TURCK | Nacional |
| Direct Industry | Marketplace industrial B2B | Multi-fabricante | Online/Nacional |
| Automação Industrial (loja) | Componentes e redes | Multi-marca | SP e Online |

---

## 8. Comparativo Técnico: PROFINET vs EtherNet/IP vs Modbus TCP

| Critério | PROFINET | EtherNet/IP | Modbus TCP |
|---|---|---|---|
| Organização | PI (Profibus & Profinet Int.) | ODVA | Modbus Organization |
| Velocidade | 100 Mbps / 1 Gbps | 100 Mbps / 1 Gbps | 100 Mbps |
| Tempo de ciclo | < 1 ms (IRT) | < 1 ms (CIP Sync) | > 5 ms |
| Topologia | Estrela, anel, linha | Estrela, anel | Qualquer |
| Diagnóstico | Avançado (alarmes, manutenção) | Avançado | Básico |
| Principal mercado | Europa, Brasil (Siemens) | América do Norte | Legado/Universal |
| Custo de implementação | Médio-Alto | Médio-Alto | Baixo |

---

## Conclusão

A pesquisa evidencia a ampla oferta de equipamentos de redes industriais disponíveis no mercado brasileiro, com fabricantes de renome internacional como Siemens, Rockwell, Moxa e Phoenix Contact competindo em diferentes faixas de preço e aplicações. A escolha dos equipamentos deve considerar não apenas o custo de aquisição, mas também a compatibilidade com os protocolos já existentes na planta, a disponibilidade de suporte técnico local e o custo total de propriedade (TCO), que inclui manutenção, atualizações e treinamento da equipe.

---

## Referências

- Catálogos técnicos Siemens IA&DT 2026.
- Moxa Industrial Networking Product Guide 2025.
- Phoenix Contact Network & Connectivity Catalog 2025.
- PROFIBUS & PROFINET International — *Technology Overview*. 2024.
- Rockwell Automation — *EtherNet/IP Design and Implementation Guide*. 2024.
