# Infraestrutura Corporativa: HomeLab Consultoria & Contabilidade

[![Versão](https://img.shields.io/badge/Vers%C3%A3o-v0.2.0-blue.svg)](docs/architecture/index.md)
[![Estado](https://img.shields.io/badge/Estado-Desenho%20%26%20Planeamento-orange.svg)](docs/architecture/index.md)
[![Licença](https://img.shields.io/badge/Licen%C3%A7a-Estudo-green.svg)](LICENSE.md)

Este repositório contém a documentação técnica profissional para a conceção, arquitetura e operação da infraestrutura de TI da **HomeLab Consultoria & Contabilidade**, uma empresa fictícia prestadora de serviços de consultoria, contabilidade geral, fiscalidade e auditoria financeira para Pequenas e Médias Empresas (PMEs).

O objetivo desta infraestrutura (versão **v0.2.0**) é aplicar os padrões mais rigorosos de administração de sistemas corporativos, garantindo a confidencialidade dos dados dos clientes, conformidade regulatória, segmentação lógica de rede e alta disponibilidade operacional.

---

## 1. Perfil Organizacional e Requisitos

*   **Utilizadores:** 5 colaboradores iniciais distribuídos pelos departamentos: **Financeiro**, **Comercial**, **Direção** e **IT/Administração**.
*   **Dados Protegidos:** Relatórios financeiros, dados pessoais de clientes (RGPD), documentos fiscais sob guarda legal e propriedade intelectual interna.
*   **Princípios Fundamentais:**
    *   **Segmentação Rígida:** Redes separadas por VLANs geridas pelo pfSense.
    *   **Identidade Única:** Autenticação centralizada e segurança baseada no modelo **AGDLP** no Active Directory.
    *   **Armazenamento Resiliente:** Pool ZFS redundante (RAIDZ2) no TrueNAS SCALE com controlo granular via ACLs de domínio.
    *   **Auditoria Ativa:** Rastreamento de acessos a pastas sensíveis e logins.

---

## 2. Visão Geral da Arquitetura Lógica

A rede corporativa abandona o modelo de LAN única e adota a segmentação de rede controlada pelo firewall/router virtualizado **pfSense** (`HL-LDA-P-FW01`):

```text
               [ Internet ]
                    │
          [ HL-LDA-P-FW01 (pfSense) ]
                    │
      [ Trunking 802.1Q (Switch Core) ]
         ┌──────────┼──────────┬──────────┐
         │          │          │          │
      [VLAN 10]  [VLAN 20]  [VLAN 30]  [VLAN 40]  [VLAN 50]
        MGMT     SERVERS    CLIENTS    STORAGE     GUEST
     (Gestão)   (Serviços) (Estações)   (Dados) (Visitantes)
```

Para mais detalhes sobre atribuições e gamas de rede, consulte o [Plano IPAM](docs/network/ipam.md).

---

## 3. Índice de Documentação Técnica (v0.2.0)

A documentação está dividida por áreas funcionais para facilitar a consulta pela equipa de infraestrutura:

### Desenho de Arquitetura e Padrões
*   **Visão Geral & Diagrama:** [Arquitetura de Referência](docs/architecture/index.md)
*   **Regras de Nomenclatura:** [Padrão de Nomenclatura (Naming Convention)](docs/standards/naming-convention.md)
*   **Desenho do Domínio:** [Estrutura do Active Directory (OUs, GPOs e AGDLP)](docs/identity/active-directory.md)

### Redes e Segurança
*   **Desenho de Redes:** [Roteamento e Regras de Firewall do pfSense](docs/network/routing-firewall.md)
*   **Tabela de IPAM:** [Plano de Endereçamento IP](docs/network/ipam.md)
*   **Diretivas Lógicas:** [Políticas de Segurança e Segmentação](docs/security/security-policies.md)

### Armazenamento e Serviços
*   **Armazenamento ZFS:** [Arquitetura TrueNAS SCALE, Partilhas e ACLs](docs/storage/storage-design.md)
*   **Serviços Base:** [Infraestrutura de Rede (DNS, DHCP, NTP, PKI)](docs/06-services/infrastructure.md)
*   **Serviços Auxiliares:** [Operação Corporativa (Backup Veeam, Monitorização, Syslog)](docs/06-services/corporate.md)

---

## 4. Procedimentos Operacionais Padrão (SOPs)

Instruções passo a passo para a execução de tarefas diárias de administração:

*   **Gestão de Identidades:**
    *   [Como criar uma conta de utilizador](procedures/domain-management/create-user.md)
    *   [Como criar e aninhar grupos de segurança (AGDLP)](procedures/domain-management/create-group.md)
    *   [Processo de Onboarding de novos colaboradores](procedures/domain-management/onboard-user.md)
    *   [Processo de Offboarding e suspensão de contas](procedures/domain-management/offboard-user.md)
*   **Gestão de Armazenamento:**
    *   [Integrar o servidor TrueNAS SCALE no Active Directory](procedures/storage-mgmt/join-truenas.md)
*   **Gestão de Rede:**
    *   [Realizar cópias de segurança do pfSense](procedures/network-mgmt/backup-pfsense.md)

---

## 5. Inventários Operacionais

Acompanhamento em tempo real dos ativos instalados:
*   **Matriz IPAM Ativa:** [Inventário de IPs em Uso](inventory/ipam.md)
*   **Inventário de Hardware & Software:** [Lista de Ativos e Licenciamento](inventory/assets.md)

---

## 6. Histórico de Versões e Transição (v0.1.0)

A versão **v0.1.0** deste repositório documentou um ambiente de testes anterior baseado numa sub-rede única (`10.0.10.0/24`). Toda essa infraestrutura foi **descontinuada e totalmente removida**.

Para fins de auditoria, registo de lições aprendidas e histórico técnico, a documentação antiga e os seus procedimentos associados estão preservados no diretório:
*   [Arquivo Histórico v0.1.0](versions/v0.1.0/README.md)
