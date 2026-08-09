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

A estrutura do projeto foi pensada em camadas para separar claramente:

1. a fundação da rede;
2. a criação do domínio e da identidade;
3. a definição de grupos e permissões;
4. a implementação de recursos como ficheiros e partilhas;
5. a aplicação de políticas e hardening;
6. a evolução para funcionalidades mais avançadas.

A filosofia central é separar claramente:

- quem é o utilizador;
- onde esse utilizador pertence;
- quais permissões esse utilizador tem sobre um recurso.

## Como funciona o laboratório

O ambiente atual opera com uma base de rede organizada em VLANs e utiliza os seguintes componentes principais:

- **pfSense** como gateway, firewall e ponto de entrada da rede;
- **Windows Server / Active Directory** como serviço de identidade e DNS interno;
- **TrueNAS SCALE** como proposta de servidor de ficheiros com integração AD;
- **GPOs** para aplicar políticas de configuração e segurança.

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

O domínio principal atual é **`corp.homelab.ao`** e a organização lógica foi pensada para suportar departamentos e políticas por função.

```text
corp.homelab.ao
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
- [docs/01-project-overview.md](docs/01-project-overview.md) — contexto de negócio e objetivos
- [docs/02-architecture.md](docs/02-architecture.md) — visão geral da arquitetura
- [docs/03-network.md](docs/03-network.md) — rede e topologia
- [docs/03-network/ipam.md](docs/03-network/ipam.md) — plano IPAM e atribuições
- [docs/04-active-directory.md](docs/04-active-directory.md) — Active Directory e modelo de autorização
- [docs/05-security.md](docs/05-security.md) — políticas e segurança
- [docs/06-services.md](docs/06-services.md) — serviços base e serviços corporativos
- [docs/07-roadmap.md](docs/07-roadmap.md) — evolução prevista do laboratório
- [docs/architecture/index.md](docs/architecture/index.md) — arquitetura detalhada da versão atual
- [docs/identity/active-directory.md](docs/identity/active-directory.md) — desenho técnico do AD DS
- [docs/network/routing-firewall.md](docs/network/routing-firewall.md) — regras e interfaces do pfSense
- [docs/storage/storage-design.md](docs/storage/storage-design.md) — desenho do storage

## Estado atual

O projeto já mostra uma base sólida de:

- rede segmentada e documentada;
- identidade e estrutura lógica no Active Directory;
- desenho de acesso por grupos e permissões;
- políticas de segurança base e documentação operacional.

A evolução futura foca-se em armazenamento, backups, automação e reforço da segurança.

## Licença

Documentação desenvolvida para fins de estudo, laboratório e referência de arquitetura de TI.
