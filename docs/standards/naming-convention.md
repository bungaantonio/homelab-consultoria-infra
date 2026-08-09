# Padrão de Nomenclatura da Infraestrutura

## 1. Objetivo

Este documento define a convenção de nomes para os ativos da infraestrutura **HomeLab Consultoria & Contabilidade**, de forma a manter coerência operacional, clareza e facilidade de identificação.

## 2. Princípios gerais

- Os nomes devem ser claros e fáceis de reconhecer.
- O padrão deve refletir a função do ativo, o ambiente e a localização lógica.
- O uso de prefixos por função evita ambiguidades.
- Nomes de hosts, VLANs, contas de serviço e grupos devem seguir a mesma lógica de estrutura.

## 3. Convenções por tipo de ativo

### 3.1 Hosts e servidores

- **Prefixo por função**: `HL-LDA-P-` para infraestrutura principal do laboratório.
- **Tipo de serviço**: `FW` (firewall), `DC` (domain controller), `FS` (file server), `MON` (monitorização), `BKP` (backup).
- **Exemplos**:
  - `HL-LDA-P-FW01`
  - `HL-LDA-P-DC01`
  - `HL-LDA-P-FS01`

### 3.2 VLANs

- **Formato**: `VLAN <id> - <nome>`
- **Exemplos**:
  - `VLAN 10 - MGMT`
  - `VLAN 20 - SERVERS`
  - `VLAN 30 - CLIENTS`
  - `VLAN 40 - STORAGE`
  - `VLAN 50 - GUEST`

### 3.3 Domínio e identidade

- **Domínio principal**: `corp.homelab.ao`
- **Nome NetBIOS**: `CORP`
- **Grupos Globais**: `GG-[DEPARTAMENTO]-[FUNCAO]`
- **Grupos Domain Local**: `DL-[RECURSO]-[PERMISSAO]`

### 3.4 Contas de serviço

- **Prefixo**: `s-`
- **Exemplos**:
  - `s-truenas`
  - `s-backup`
  - `s-monitoring`

## 4. Regras de manutenção

- Qualquer novo ativo deve ser registado na documentação correspondente.
- A nomenclatura deve ser mantida estável durante a vida útil do laboratório.
- Mudanças de nome devem ser refletidas em todos os documentos relevantes, incluindo rede, identidade e inventário.
