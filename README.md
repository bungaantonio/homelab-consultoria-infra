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
## Serviços Implementados (v0.1)
* **Routing e Gateway:** Acesso à internet gerido por um único ponto de falha controlado (pfSense).
* **Serviços de Domínio:** Floresta e Domínio `lan.homelab.ao` criados.
* **Handoff de DNS:** O DHCP do pfSense entrega o IP estático do DC (`10.0.10.10`) como único DNS para os clientes.
* **DNS Forwarding:** O Domain Controller resolve domínios internos e encaminha tráfego externo para o pfSense.
* **Integração Endpoint:** Estação Windows 10 associada ao domínio e a autenticar utilizadores da rede.

## Roadmap de Implementação

**Fase 1: Fundação (Concluída - v0.1)**
* [x] Configuração base de Rede (pfSense)
* [x] Instalação do Windows Server e promoção a Domain Controller
* [x] Configuração da estrutura lógica do AD (OUs, Grupos, 5 Utilizadores)
* [x] Associação do Windows 10 ao Domínio

**Fase 2: Gestão e Segurança (GPOs e Hardening)**
* [ ] GPO 1: Bloquear Pendrives e Armazenamento USB (Segurança)
* [ ] GPO 2: Definir Wallpaper Corporativo Padronizado (Identidade)
* [ ] GPO 3: Políticas Extras de Restrições e Hardening (Bloqueio de CMD e Painel de Controlo)

**Fase 3: Serviços Corporativos**
* [ ] Integração do TrueNAS no AD
* [ ] Partilhas SMB departamentais e mapeamento automático via GPO
* [ ] Instalação e Integração do PaperCut NG (Gestão de Impressão)

**Fase 4: Nível Enterprise (Competências Avançadas)**
* [ ] Ativação da Lixeira do Active Directory (AD Recycle Bin)
* [ ] Delegação de Controle (Helpdesk com acesso restrito a Reset de Passwords)
* [ ] Windows LAPS (Local Administrator Password Solution)
* [ ] Diretivas de Senha Refinadas (Fine-Grained Password Policies - FGPP)
* [ ] Autenticação do pfSense no AD (Acesso VPN / Admin via conta de rede)
* [ ] Configurar o AD CS (Serviços de Certificados do Active Directory) e LDAPS

## Licença
Documentação desenvolvida para fins de estudo, laboratório e portfólio de arquitetura de TI.
