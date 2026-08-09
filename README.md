# Infraestrutura Corporativa: HomeLab Consultoria & Contabilidade

Este repositório documenta um laboratório de estudo para simular uma infraestrutura de TI corporativa pequena, com foco em identidade, segurança, partilha de ficheiros, gestão administrativa e suporte a uma organização que presta serviços de consultoria e contabilidade.

## O que é este projeto?

É uma referência documental e de aprendizagem para a empresa fictícia **HomeLab Consultoria & Contabilidade**. O objetivo não é representar um ambiente de produção, mas sim demonstrar como uma organização com necessidades reais de contabilidade, fiscalidade, consultoria e gestão pode ser suportada por uma infraestrutura de TI coerente.

## Como está pensado o laboratório

A estrutura do projeto foi organizada em camadas e separa claramente:

1. rede e conectividade;
2. identidade e domínio Active Directory;
3. autorização baseada em grupos;
4. recursos compartidos e serviços corporativos;
5. políticas de segurança e hardening.

A lógica de autorização segue o modelo simplificado:

```text
Utilizador → Grupo Global (GG) → Grupo Domain Local (DL) → Recurso
```

## Base técnica atual

O laboratório atual funciona com os componentes principais abaixo:

- **pfSense** como gateway, firewall e ponto de entrada da rede;
- **Windows Server / Active Directory** como serviço de identidade e DNS interno;
- **TrueNAS SCALE** como proposta de servidor de ficheiros com integração AD;
- **GPOs** para aplicar políticas de segurança e configuração.

O domínio principal atual é **`corp.homelab.ao`** e a rede base é organizada em VLANs com foco em separação e controlo.

## Estado atual do projeto

O projeto já mostra uma base sólida de:

- rede segmentada e documentada;
- identidade e estrutura lógica no Active Directory;
- desenho de acesso por grupos e permissões;
- políticas de segurança base e documentação operacional.

A evolução futura foca-se em armazenamento, backups, automação e reforço da segurança.

## Mapa da documentação

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

## Licença

Documentação desenvolvida para fins de estudo, laboratório e referência de arquitetura de TI.
