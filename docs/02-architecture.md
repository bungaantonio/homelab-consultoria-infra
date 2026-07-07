# 02 - Architecture

## 1. Visão Geral da Arquitetura

A infraestrutura do ambiente **HomeLab Consultoria & Contabilidade** foi desenhada como uma rede corporativa isolada, simulando um cenário real de pequena empresa com controlo centralizado de identidade e serviços internos.

O ambiente opera numa única sub-rede:

- `10.0.10.0/24`

Toda a gestão de identidade e autenticação é centralizada no domínio `lan.homelab.ao`, suportado por um controlador de domínio baseado em Windows Server.

---

## 2. Topologia Lógica

A arquitetura é composta por três camadas principais:

### 2.1 Camada de Rede
Responsável pela conectividade e controlo de tráfego.

- Firewall / Gateway: pfSense
- DHCP: pfSense
- NAT: saída para internet
- Segmentação: rede única isolada (flat network)

---

### 2.2 Camada de Identidade
Responsável pela autenticação e controlo de acesso.

- Domínio: `lan.homelab.ao`
- Serviço: Active Directory Domain Services
- Controlador de domínio: LAB-DC01
- DNS integrado ao domínio

Funções:
- autenticação centralizada
- resolução de nomes internos
- aplicação de políticas de grupo (GPO)

---

### 2.3 Camada de Serviços
Responsável pelos serviços internos da organização.

- Estações de trabalho Windows 10 Pro/Enterprise
- Servidor de ficheiros TrueNAS SCALE 25.10.4
- Serviços de impressão (PaperCut NG – em roadmap)
- Serviços futuros: backup, monitorização, PKI e automação

---

## 3. Componentes da Infraestrutura

| Componente | Função |
|------------|--------|
| pfSense | Gateway, firewall, DHCP |
| LAB-DC01 | Domain Controller, DNS, AD DS |
| PC-Geral01 | Endpoint do utilizador |
| TrueNAS SCALE | Storage central e File Server |

---

## 4. Fluxo de Comunicação

### 4.1 Autenticação
1. Utilizador inicia sessão no Windows
2. Estação consulta o Domain Controller
3. Autenticação via Active Directory
4. Aplicação de GPOs conforme OU e grupos

---

### 4.2 Resolução de nomes
1. Cliente envia query DNS
2. DC resolve nomes internos
3. Queries externas são encaminhadas para pfSense

---

### 4.3 Acesso à internet
1. Tráfego sai via gateway pfSense
2. NAT traduz IP interno para IP público
3. Resposta retorna ao endpoint

---

## 5. Princípios de Design

A arquitetura segue os seguintes princípios:

- Centralização de identidade
- Segmentação lógica por função (não por VLANs neste estágio)
- Simplicidade operacional (single subnet)
- Escalabilidade futura para serviços adicionais
- Separação clara entre identidade, rede e serviços

---

## 6. Limitações Atuais

- Rede única sem segmentação VLAN
- Apenas um Domain Controller
- Sem redundância de serviços
- Serviços adicionais ainda em fase de implementação

---

## 7. Evolução Prevista

- Introdução de VLANs (users / servers / management)
- Segundo Domain Controller (redundância)
- Serviços de backup e monitorização
- Hardening avançado de segurança e logging centralizado
- Expansão dos serviços de ficheiros e impressão