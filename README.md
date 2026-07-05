# Infraestrutura Corporativa: HomeLab Consultoria & Contabilidade

## Objetivo

Este projeto documenta a conceção, implementação e administração da infraestrutura de TI da **HomeLab Consultoria & Contabilidade**, uma empresa fictícia de prestação de serviços fiscais e auditoria financeira para PMEs.

O objetivo do laboratório é aplicar as melhores práticas de administração de sistemas em ambiente corporativo, garantindo segurança, centralização de identidades e auditoria num ambiente com 5 utilizadores que lidam com dados altamente sensíveis.

## Tecnologias Utilizadas

* **Firewall / Gateway:** pfSense
* **Identidade & DNS:** Windows Server 2025 (Active Directory Domain Services)
* **Estações de Trabalho:** Windows 10 Pro / Enterprise
* **Armazenamento Central:** TrueNAS Core / Scale *(Em Roadmap)*
* **Gestão de Impressão:** PaperCut NG *(Em Roadmap)*

## Arquitetura e Topologia

O ambiente opera de forma fechada e segura na sub-rede `10.0.10.0/24`.

| Equipamento    | Endereço IP   | Função / Serviço                              |
| -------------- | ------------- | --------------------------------------------- |
| **pfSense**    | `10.0.10.1`   | Gateway, NAT, Firewall e Servidor DHCP        |
| **LAB-DC01**   | `10.0.10.10`  | Domain Controller, AD DS, DNS Autoritativo    |
| **PC-Geral01** | DHCP Dinâmico | Estação de trabalho do utilizador (Win 10)    |
| **TrueNAS**    | `10.0.10.20`  | Servidor de Ficheiros (SMB/ACLs) *(Pendente)* |

## Estrutura do Domínio (`lan.homelab.ao`)

A infraestrutura lógica foi desenhada para facilitar a aplicação de Políticas de Grupo (GPOs) baseadas em departamentos e o princípio do menor privilégio.

```text
lan.homelab.ao
│
├── 00_Admin
│   ├── Users
│   └── Groups
│       ├── GG-Financeiro
│       ├── GG-Comercial
│       ├── GG-Direcao
│       └── GG-IT
│
├── 01_Users
│   ├── Financeiro
│   ├── Comercial
│   └── Direcao
│
├── 02_Computers
│   ├── Workstations
│   └── Test
│
├── 03_Servers
│   ├── Domain Controllers
│   ├── Infrastructure
│   ├── Applications
│   ├── File Servers
│   └── Print Servers
│
├── 04_Service_Accounts
│   ├── Applications
│   ├── Infrastructure
│   └── Automation
│
└── 05_Infrastructure_Services
```

