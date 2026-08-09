# Infraestrutura Corporativa: HomeLab Consultoria & Contabilidade

Este repositório documenta um laboratório de estudo para simular uma infraestrutura de TI corporativa pequena, com foco em identidade, segurança, partilha de ficheiros, gestão administrativa e suporte a uma organização que presta serviços de consultoria e contabilidade.

## O que é este projeto?

É um projeto de documentação e aprendizagem que descreve a construção de um ambiente corporativo fictício para a empresa **HomeLab Consultoria & Contabilidade**, com necessidades reais de:

- rede isolada e controlada;
- domínio Active Directory;
- políticas de segurança e GPO;
- partilhas de ficheiros com controlo de acesso;
- uma lógica de autorização baseada em grupos;
- suporte a funções de consultoria, contabilidade, fiscalidade e gestão.

O objetivo principal é demonstrar como uma infraestrutura empresarial pode ser organizada e administrada, mesmo num ambiente de laboratório.

## Como foi pensado e construído

A estrutura do projeto foi pensada em camadas:

1. Fundação da rede
2. Criação do domínio e da identidade
3. Definição de grupos e permissões
4. Implementação de recursos como ficheiros e partilhas
5. Aplicação de políticas e hardening
6. Evolução para funcionalidades mais avançadas

A filosofia central é separar claramente:

- quem é o utilizador;
- onde esse utilizador pertence;
- quais permissões esse utilizador tem sobre um recurso.

## Como funciona o laboratório

O ambiente opera na sub-rede `10.0.10.0/24` e utiliza os seguintes componentes principais:

- **pfSense** como gateway, firewall e DHCP
- **Windows Server / Active Directory** como serviço de identidade e DNS
- **TrueNAS SCALE** como servidor de ficheiros com integração AD
- **GPOs** para aplicar políticas de configuração e segurança

A lógica de autorização segue o modelo simplificado:

```text
Utilizador → Grupo Global (GG) → Grupo Domain Local (DL) → Recurso
```

## Contexto de negócio e organização

O laboratório foi desenhado para refletir uma organização composta por áreas funcionais distintas:

- **Consultoria**: apoio a clientes, documentos de projeto e operação diária.
- **Contabilidade e Fiscalidade**: dados financeiros sensíveis, relatórios e processos internos.
- **Direção**: necessidade de visão centralizada e controlo operacional.
- **TI**: administração de identidade, segurança e serviços de infraestrutura.

Esta separação é refletida na estrutura lógica do domínio e nos grupos de acesso.

## Estrutura do domínio

O domínio principal é `lan.homelab.ao` e a organização lógica foi pensada para suportar departamentos e políticas por função.

```text
lan.homelab.ao
├── 00_Admin
├── 01_Users
├── 02_Computers
├── 03_Servers
└── 04_Service_Accounts
```

## Gestão de rede e endereçamento

Para detalhes sobre atribuições e gamas de rede, consulte o [Plano IPAM](docs/03-network/ipam.md).

## Como navegar este repositório

- [docs/00-project-guide.md](docs/00-project-guide.md) — guia principal de leitura do projeto
- [docs/01-project-overview.md](docs/01-project-overview.md) — contexto e objetivo
- [docs/02-architecture.md](docs/02-architecture.md) — visão geral da arquitetura
- [docs/03-network.md](docs/03-network.md) — rede e topologia
- [docs/03-network/ipam.md](docs/03-network/ipam.md) — plano de endereçamento e atribuições
- [docs/04-active-directory.md](docs/04-active-directory.md) — Active Directory e organização lógica
- [docs/08-file-services-design.md](docs/08-file-services-design.md) — desenho dos serviços de ficheiros
- [docs/14-truenas-scale-preparacao-integracao-ad.md](docs/14-truenas-scale-preparacao-integracao-ad.md) — implementação do TrueNAS
- [docs/13-roadmap.md](docs/13-roadmap.md) — roadmap de evolução do laboratório

## Estado atual

O projeto já mostra uma base sólida de:

- rede e domínios;
- identidade e estrutura lógica no AD;
- integração de ficheiros com controlo de acesso;
- políticas e GPOs base.

O que ainda está por evoluir inclui segurança avançada, automação, impressão e monitorização.

## Licença

Documentação desenvolvida para fins de estudo, laboratório e portfólio de arquitetura de TI.
