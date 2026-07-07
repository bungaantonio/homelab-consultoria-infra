# Infraestrutura Corporativa: HomeLab Consultoria & Contabilidade

## Objetivo

Este projeto documenta a conceção, implementação e administração da infraestrutura de TI da **HomeLab Consultoria & Contabilidade**, uma empresa fictícia de prestação de serviços fiscais e auditoria financeira para PMEs.

O objetivo do laboratório é aplicar as melhores práticas de administração de sistemas em ambiente corporativo, garantindo segurança, centralização de identidades e auditoria num ambiente com 5 utilizadores que lidam com dados altamente sensíveis.

## Tecnologias Utilizadas

* **Firewall / Gateway:** pfSense
* **Identidade & DNS:** Windows Server 2025 (Active Directory Domain Services)
* **Estações de Trabalho:** Windows 10 Pro / Enterprise
* **Serviços de Ficheiros:** TrueNAS SCALE 25.10.4 *(Principal e operacional)*
* **Armazenamento Alternativo:** Windows Server File Server *(Alternativa futura)*
* **Gestão de Impressão:** PaperCut NG *(Próxima fase)*

## Arquitetura e Topologia

O ambiente opera de forma fechada e segura na sub-rede `10.0.10.0/24`.

![Diagrama da arquitetura HomeLab](/HomeLab_Diagram.png)

| Equipamento    | Endereço IP   | Função / Serviço                              |
| -------------- | ------------- | --------------------------------------------- |
| **pfSense**    | `10.0.10.1`   | Gateway, NAT, Firewall e Servidor DHCP        |
| **LAB-DC01**   | `10.0.10.10`  | Domain Controller, AD DS, DNS Autoritativo    |
| **LAB-WS-001** | DHCP Dinâmico | Estação de trabalho do utilizador (Win 10)    |
| **TrueNAS**    | `10.0.10.20`  | Servidor de Ficheiros principal (SMB/ACLs)    |

## Estrutura do Domínio (`lan.homelab.ao`)

A infraestrutura lógica foi desenhada para facilitar a aplicação de Políticas de Grupo (GPOs) baseadas em departamentos e o princípio do menor privilégio.

```text
lan.homelab.ao
│
├── 00_Admin
│   ├── Users
│   └── Groups
│       ├── Global
│       │   ├── GG-Comercial
│       │   ├── GG-Financeiro
│       │   ├── GG-Direcao
│       │   ├── GG-IT
│       │   └── GG-Domain-Admins
│       └── DomainLocal
│           ├── DL-FS-Comercial-RW
│           ├── DL-FS-Comercial-RO
│           ├── DL-FS-Financeiro-RW
│           └── DL-FS-Financeiro-RO
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
└── 04_Service_Accounts
    ├── Applications
    ├── Infrastructure
    └── Automation

```

## Documentação Técnica

- [04 - Active Directory](docs/04-active-directory.md)
- [07 - Group Policy](docs/07-group-policy.md)
- [08 - File Services Design](docs/08-file-services-design.md)
- [09 - File Services Implementation](docs/09-file-services-implementation.md)
- [14 - TrueNAS SCALE Preparation and AD Integration](docs/14-truenas-scale-preparacao-integracao-ad.md)
- [10 - GPO_Drive_Mapping](docs/10-gpo-drive-mapping.md)
- [09 - Print Services](docs/09-print-services.md)
- [10 - Backup](docs/10-backup.md)
## Serviços Implementados (v0.1)
* **Routing e Gateway:** Acesso à internet gerido por um único ponto de falha controlado (pfSense).
* **Serviços de Domínio:** Floresta e Domínio `lan.homelab.ao` criados.
* **Handoff de DNS:** O DHCP do pfSense entrega o IP estático do DC (`10.0.10.10`) como único DNS para os clientes.
* **DNS Forwarding:** O Domain Controller resolve domínios internos e encaminha tráfego externo para o pfSense.
* **Integração Endpoint:** Estação Windows 10 associada ao domínio e a autenticar utilizadores da rede.
* **Storage Principal:** TrueNAS SCALE 25.10.4 integrado ao AD, com ZFS, SMB, ACLs NFSv4 e drive mapping operacional.

## Roadmap de Implementação

**Fase 1: Fundação (Concluída - v0.1)**
* [x] Configuração base de Rede (pfSense)
* [x] Instalação do Windows Server e promoção a Domain Controller
* [x] Configuração da estrutura lógica do AD (OUs, Grupos, 5 Utilizadores)
* [x] Associação do Windows 10 ao Domínio

**Fase 2: Gestão e Segurança (GPOs e Hardening)**
* [x] GPO_Desktop_Restrictions (CMD, PowerShell e Painel de Controlo)
* [x] GPO_Workstation_Baseline (Firewall, Defender, Windows Update, AutoRun)
* [x] GPO_USB_Storage_Restriction
* [x] GPO_Corporate_Wallpaper
* [x] GPO_Audit_Policy
* [ ] GPO_LAPS_Policy
* [ ] GPO_BitLocker
* [ ] GPO_Local_Administrators_Management
* [ ] GPO_Remote_Desktop_Policy

**Fase 3: Serviços Corporativos**
* [x] File Services Design alinhado com AGDLP
* [x] TrueNAS SCALE 25.10.4 preparado e validado para integração ao AD
* [x] Join do TrueNAS ao domínio `lan.homelab.ao`
* [x] Criação de partilhas departamentais
* [x] Criação dos DL para autorização de recursos
* [x] Configuração final de SMB e ACLs no TrueNAS
* [x] Mapeamento automático de drives via GPO
* [ ] Implementação e integração do PaperCut NG (Gestão de Impressão)

**Fase 4: Segurança Avançada e Enterprise**
* [ ] Ativação da Lixeira do Active Directory (AD Recycle Bin)
* [ ] Delegação de Controle (Helpdesk com acesso restrito a Reset de Passwords)
* [ ] Diretivas de Senha Refinadas (Fine-Grained Password Policies - FGPP)
* [ ] Autenticação do pfSense no AD (Acesso VPN / Admin via conta de rede)
* [ ] Configurar o AD CS (Serviços de Certificados do Active Directory) e LDAPS

**Fase 5: Automação e Observabilidade**
* [ ] Automatização de tarefas administrativas
* [ ] Scripts de provisionamento de utilizadores
* [ ] Monitorização em tempo real
* [ ] Alertas automáticos de falhas

## Licença
Documentação desenvolvida para fins de estudo, laboratório e portfólio de arquitetura de TI.
