# 07 - Group Policy

## 1. Visão Geral

As Group Policy Objects (GPOs) são o mecanismo de configuração e automação do ambiente **HomeLab Consultoria & Contabilidade**.

Elas servem para padronizar a experiência do utilizador, aplicar hardening e automatizar tarefas de entrega. Não servem para conceder permissões a ficheiros, partilhas ou impressoras.

---

## 2. Objetivo

As GPOs têm como objetivo:

- Garantir consistência de configuração entre dispositivos
- Aplicar políticas de segurança centralizadas
- Reduzir intervenção manual em endpoints
- Automatizar entrega de recursos aos utilizadores
- Hardenizar servidores e estações de trabalho

---

## 3. Arquitetura

As GPOs são aplicadas com base em:

- Estrutura de OUs
- Escopo de objetos do domínio
- Filtragem por grupos de segurança quando necessário

Regras importantes:

- GPOs de configuração alteram o estado do sistema
- GPOs de automação distribuem recursos como drives e impressoras
- Permissões de recursos continuam a ser tratadas por GG, DL e ACLs

---

## 4. Pré-requisitos

- OUs criadas e validadas
- Utilizadores e computadores colocados nas OUs corretas
- GG e DL existentes quando houver dependência de recursos
- File Server funcional para qualquer GPO de drive mapping
- Partilhas e permissões já configuradas antes da automação

---

## 5. Implementação

### 5.1 GPOs de configuração

Estas GPOs controlam postura de segurança e configuração do endpoint.

Exemplos:

- `GPO_Desktop_Restrictions`
- `GPO_Windows_Defender_Policy`
- `GPO_Windows_Firewall_Policy`
- `GPO_Local_Administrators_Management`
- `GPO_Remote_Desktop_Policy`
- `GPO_BitLocker`
- `GPO_LAPS_Policy`

### 5.2 GPOs de automação

Estas GPOs entregam recursos aos utilizadores sem alterar permissões de base.

Exemplos:

- `GPO_Drive_Mapping`
- `GPO_Printer_Deployment`

### 5.3 Modelo de aplicação

O fluxo recomendado é:

1. Definir a OU alvo
2. Confirmar os grupos GG e DL
3. Validar permissões do recurso
4. Aplicar a GPO para automatizar a entrega

---

## 6. Ordem de Aplicação

A hierarquia segue o comportamento padrão do Active Directory:

1. Local policy
2. Site
3. Domain
4. OU
5. GPOs linkadas a OUs

---

## 7. Segurança e Boas Práticas

- Evitar GPOs excessivamente globais
- Preferir GPOs específicas por OU
- Utilizar grupos de segurança para filtragem
- Não usar GPO para substituir permissões de share ou NTFS
- Testar alterações na OU `Test` antes de produção

---

## 8. Limitações Atuais

- Sem GPO central de baseline unificada
- Sem AGPM
- Sem versionamento avançado de políticas
- Parte da automação ainda depende da conclusão dos File Services

---

## 9. Evolução Futura

- Criação de baseline de segurança corporativa
- Implementação de políticas de LAPS via GPO
- Hardening avançado de endpoints
- Integração com auditoria centralizada
- Segmentação de GPO por compliance e risco

---

## 10. Rastreamento de GPOs

| ID | GPO | Tipo | Escopo | Objetivo | Status | Fase |
|----|-----|------|--------|----------|--------|------|
| 1 | GPO_Baseline_Domain_Policy | Configuração | Domínio | Políticas globais de segurança | Concluído | Fase 1 |
| 2 | GPO_Workstation_Baseline | Configuração | `02_Computers > Workstations` | Padronização de estações | Concluído | Fase 1 |
| 3 | GPO_USB_Storage_Restriction | Configuração | Workstations | Bloqueio de USB | Concluído | Fase 1 |
| 4 | GPO_Corporate_Wallpaper | Configuração | Utilizadores | Wallpaper institucional | Concluído | Fase 1 |
| 5 | GPO_Desktop_Restrictions | Configuração | Utilizadores | Bloqueio de CMD, PowerShell e Painel de Controlo | Concluído | Fase 1 |
| 6 | GPO_Windows_Defender_Policy | Configuração | Workstations | Proteção em tempo real e cloud protection | Concluído | Fase 2 |
| 7 | GPO_Windows_Firewall_Policy | Configuração | Workstations e Servers | Firewall ativa e perfis corretos | Concluído | Fase 2 |
| 8 | GPO_Audit_Policy | Configuração | Domain Controllers | Auditoria de eventos críticos | Concluído | Fase 2 |
| 9 | GPO_Drive_Mapping | Automação | Utilizadores | Mapeamento automático de drives por GG | Concluído | Fase 3 |
| 10 | GPO_Printer_Deployment | Automação | Utilizadores | Distribuição automática de impressoras | Pendente | Fase 3 |
| 11 | GPO_Local_Administrators_Management | Configuração | Workstations | Controlo de administradores locais | Pendente | Fase 4 |
| 12 | GPO_Remote_Desktop_Policy | Configuração | Servers | Regras de RDP e NLA | Pendente | Fase 4 |
| 13 | GPO_BitLocker | Configuração | Workstations | Criptografia de disco | Pendente | Fase 4 |
| 14 | GPO_LAPS_Policy | Configuração | Workstations | Password única por máquina | Pendente | Fase 4 |