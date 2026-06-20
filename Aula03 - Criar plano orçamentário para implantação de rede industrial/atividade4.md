# Aula 03 — Atividade 4: Diagrama da Arquitetura da Rede Industrial

**Disciplina:** Integração Vertical e Horizontal  
**Aluna:** Maria Eduarda Claro  
**Data:** Junho de 2026

---

## Introdução

Este documento apresenta o diagrama de arquitetura da rede industrial proposta, baseado no **Modelo de Purdue** (ISA-95 / IEC 62264), que organiza os sistemas industriais em níveis hierárquicos bem definidos, desde o campo de instrumentação até os sistemas corporativos de gestão.

O modelo de Purdue é o padrão de referência para arquitetura de redes industriais e é fundamental para implementar a separação entre redes de TI (Tecnologia da Informação) e OT (Tecnologia Operacional).

---

## 1. Modelo de Purdue — Visão Geral

```
╔══════════════════════════════════════════════════════════════════════╗
║                    NÍVEL 4 — REDE CORPORATIVA (TI)                  ║
║         ERP (SAP) │ BI / Analytics │ E-mail │ Active Directory       ║
╠══════════════════════════════════════════════════════════════════════╣
║                         ▲  FIREWALL IT/OT  ▲                        ║
║                         │   DMZ Industrial  │                        ║
╠══════════════════════════════════════════════════════════════════════╣
║                    NÍVEL 3 — SUPERVISÃO (MES/SCADA)                 ║
║       Servidor SCADA │ Servidor MES │ Historiador PI │ Engenharia    ║
╠══════════════════════════════════════════════════════════════════════╣
║                         ▲  SWITCH CORE L3  ▲                        ║
╠══════════════════════════════════════════════════════════════════════╣
║                    NÍVEL 2 — CONTROLE DE ÁREA                       ║
║         HMI Linha 1 │ HMI Linha 2 │ HMI Linha 3 │ Estação Op.      ║
╠══════════════════════════════════════════════════════════════════════╣
║               ▲  SWITCH L2 ANEL REDUNDANTE (MRP)  ▲                 ║
╠══════════════════════════════════════════════════════════════════════╣
║                    NÍVEL 1 — CONTROLE DE PROCESSO                   ║
║     CLP Linha 1 │ CLP Linha 2 │ CLP Linha 3 │ Drives │ Robôs       ║
╠══════════════════════════════════════════════════════════════════════╣
║                    NÍVEL 0 — CAMPO                                  ║
║  Sensores │ Atuadores │ Instrumentos │ Válvulas │ Motores            ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 2. Diagrama Detalhado da Arquitetura

```
                         INTERNET / WAN
                               │
                         [Roteador BGP]
                               │
┌──────────────────────────────┴──────────────────────────────────────┐
│                     NÍVEL 4 — REDE CORPORATIVA                      │
│                                                                      │
│  [Servidor ERP]──[Switch Core TI]──[Servidor BI]──[File Server]     │
│       │                │                                            │
│  [Active Dir.]    [Estações Admin]                                  │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                    ╔══════════╧══════════╗
                    ║  FIREWALL IT/OT     ║  ← Moxa EDR-810
                    ║  (DMZ Industrial)   ║     IPsec VPN
                    ╚══════════╤══════════╝     IDS/IPS
                               │
┌──────────────────────────────┴──────────────────────────────────────┐
│                     NÍVEL 3 — SUPERVISÃO                            │
│                                                                      │
│  [Servidor SCADA]──[Servidor MES]──[Historiador PI]──[Jump Server]  │
│        │                │                │                          │
│   WinCC SCADA      Prometheus MES    OSIsoft PI        Engenharia   │
│                                                                      │
│  [Switch Core L3 Anel A]────────────[Switch Core L3 Anel B]        │
│   Siemens SCALANCE X308            Siemens SCALANCE X308            │
│   (Ring Primary)                   (Ring Secondary)                 │
└──────────┬──────────────────────────────────┬───────────────────────┘
           │                                  │
    ┌──────┴──────┐                    ┌──────┴──────┐
    │  LINHA 1    │                    │  LINHA 2    │              etc.
    │             │                    │             │
    │[Switch L2]  │                    │[Switch L2]  │
    │ SCALANCE    │                    │ SCALANCE    │
    │   X208      │                    │   X208      │
    │             │                    │             │
    │[HMI KTP900] │                    │[HMI KTP900] │
    │             │                    │             │
    │[CLP S7-1500]│                    │[CLP S7-1500]│
    │             │                    │             │
    │ PROFINET    │                    │ PROFINET    │
    │    Ring     │                    │    Ring     │
    └──────┬──────┘                    └──────┬──────┘
           │                                  │
   ┌───────┼───────┐                 ┌────────┼──────┐
   │       │       │                 │        │      │
[Drive] [Drive] [Drive]          [Drive] [Drive] [Drive]
 VFD     VFD     VFD               VFD     VFD    VFD
   │       │       │                 │        │      │
[Motor] [Motor] [Motor]          [Motor] [Motor] [Motor]

NÍVEL 0 — CAMPO (PROFIBUS PA / IO-Link)
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  [Sensor Temp.]  [Sensor Press.]  [Sensor Flow]  [Válvula Controle] │
│  Endress+Hauser  Rosemount 3051   Yokogawa       Fisher Controls    │
│                                                                      │
│  [Sensor Nível]  [Analisador]     [Encoder]      [Célula de Carga]  │
│  Siemens SITRANS ABB Analytical   Heidenhain     Hottinger HBM      │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 3. Diagrama de Segmentação de Rede (VLANs)

```
┌─────────────────────────────────────────────────────────────────┐
│                   SEGMENTAÇÃO POR VLANs                         │
│                                                                  │
│  VLAN 10 — Rede Corporativa TI        192.168.10.0/24           │
│  VLAN 20 — DMZ Industrial (Firewall)  192.168.20.0/28           │
│  VLAN 30 — Supervisão SCADA/MES       10.10.30.0/24             │
│  VLAN 40 — Controle Linha 1           10.10.40.0/24             │
│  VLAN 50 — Controle Linha 2           10.10.50.0/24             │
│  VLAN 60 — Controle Linha 3           10.10.60.0/24             │
│  VLAN 70 — Manutenção / Engenharia    10.10.70.0/28             │
│  VLAN 99 — Gerência de Rede (SNMP)    10.10.99.0/28             │
│                                                                  │
│  ▸ Tráfego entre VLANs 10 e 30: controlado pelo Firewall IT/OT  │
│  ▸ VLANs 40/50/60 isoladas entre si por ACLs no switch L3       │
│  ▸ VLAN 70 com acesso a todas as VLANs OT (manutenção)          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 4. Diagrama de Redundância de Rede (MRP Ring)

O protocolo **MRP (Media Redundancy Protocol — IEC 62439-2)** garante recuperação de falha em menos de 200 ms, mantendo a disponibilidade das linhas de produção mesmo em caso de falha de um enlace ou switch.

```
                    [Switch Core L3 — A]
                   /                    \
          [Switch L2]                [Switch L2]
          Linha 1 / Linha 2          Linha 3
               |                         |
          [Switch L2]                [Switch L2]
          (Downstream)               (Downstream)
                   \                    /
                    [Switch Core L3 — B]

        ● Anel primário ativo: sentido horário
        ● Em caso de falha de enlace: reconfiguração < 200 ms
        ● MRM (Media Redundancy Manager): Switch Core L3 — A
        ● MRC (Media Redundancy Client): demais switches
```

---

## 5. Diagrama do Fluxo de Dados (Pirâmide da Automação)

```
        ┌──────────────────────────┐
        │   NÍVEL 4: ERP / BI      │  ← Pedidos, planejamento, relatórios
        │   Dados agregados/dia    │     SAP, Power BI
        └────────────┬─────────────┘
                     │  (via Firewall IT/OT)
        ┌────────────┴─────────────┐
        │  NÍVEL 3: SCADA / MES    │  ← Supervisão, ordens de produção
        │  Dados por turno/hora    │     WinCC, PI Historian
        └────────────┬─────────────┘
                     │  (Ethernet Industrial)
        ┌────────────┴─────────────┐
        │  NÍVEL 2: HMI            │  ← Setpoints, alarmes, receitas
        │  Dados por minuto        │     KTP900, WinCC RT
        └────────────┬─────────────┘
                     │  (PROFINET)
        ┌────────────┴─────────────┐
        │  NÍVEL 1: CLP / PLC      │  ← Lógica de controle, interlocks
        │  Dados por ciclo (ms)    │     Siemens S7-1500
        └────────────┬─────────────┘
                     │  (PROFIBUS PA / IO-Link)
        ┌────────────┴─────────────┐
        │  NÍVEL 0: CAMPO          │  ← Medições físicas contínuas
        │  Dados em tempo real     │     Sensores, atuadores
        └──────────────────────────┘
```

---

## 6. Especificações de Segurança da Arquitetura

A arquitetura segue as diretrizes da norma **IEC 62443** (Segurança para Sistemas de Automação e Controle Industrial):

| Requisito IEC 62443 | Implementação na Arquitetura |
|---|---|
| Segmentação de rede (zones & conduits) | VLANs por nível + Firewall IT/OT |
| Autenticação de dispositivos | Certificados X.509 nos switches gerenciáveis |
| Controle de acesso | RBAC configurado no SCADA e switches |
| Criptografia de dados | IPsec VPN entre planta e servidor remoto |
| Auditoria e log | Servidor de syslog centralizado + SIEM |
| Detecção de intrusão | IDS/IPS no Firewall IT/OT |

---

## 7. Conclusão

A arquitetura proposta baseada no Modelo de Purdue oferece uma estrutura robusta, segura e escalável para integração vertical dos sistemas industriais. A clara separação entre os níveis de campo, controle, supervisão e corporativo — com firewalls nas fronteiras críticas — garante que um incidente de segurança na rede corporativa não comprometa a continuidade operacional do chão de fábrica.

A redundância via protocolo MRP e a segmentação por VLANs são os pilares que sustentam os índices de disponibilidade exigidos por operações de manufatura contínua (meta OEE > 83%).

---

## Referências

- ANSI/ISA-95 — *Enterprise-Control System Integration*.
- IEC 62443 — *Security for Industrial Automation and Control Systems*.
- IEC 62439-2 — *Media Redundancy Protocol (MRP)*.
- PROFIBUS & PROFINET International. *PROFINET Architecture Description*. 2024.
- NIST SP 800-82 — *Guide to ICS Security*. Rev. 3, 2023.
