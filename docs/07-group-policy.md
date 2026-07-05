# 07 - Group Policy

## 1. Visão Geral

As Group Policy Objects (GPOs) são o principal mecanismo de controlo de configuração e segurança no ambiente **HomeLab Consultoria & Contabilidade**.

São aplicadas através do domínio gerido por Active Directory, permitindo padronização de configurações, hardening e gestão centralizada de utilizadores e computadores.

---

## 2. Objetivo das GPOs

As GPOs têm como objetivo:

- Garantir consistência de configuração entre dispositivos
- Aplicar políticas de segurança centralizadas
- Reduzir intervenção manual em endpoints
- Controlar ambiente de utilizador por departamento
- Hardenizar servidores e estações de trabalho

---

## 3. Modelo de Aplicação

As GPOs são aplicadas com base em:

- Estrutura de OUs
- Grupos de segurança (AGDLP model)
- Escopo de objetos do domínio

---

## 4. Tipos de GPO no Ambiente

### 4.1 GPO de Utilizador (User Configuration)

Aplicadas na OU `01_Users`.

Exemplos:
- Configuração de desktop
- Restrição de acesso a ferramentas do sistema
- Mapeamento de drives de rede
- Configuração de impressoras

---

### 4.2 GPO de Computador (Computer Configuration)

Aplicadas na OU `02_Computers`.

Exemplos:
- Firewall ativado
- Windows Defender configurado
- Windows Update automático
- Hardening de sistema operativo

---

### 4.3 GPO de Servidores

Aplicadas na OU `03_Servers`.

Exemplos:
- Desativação de serviços desnecessários
- Auditoria avançada
- Restrições de acesso remoto
- Configuração de segurança de rede (SMB, RDP)

---

### 4.4 GPO Administrativa

Aplicadas na OU `00_Admin`.

Exemplos:
- Restrição de login em estações comuns
- Políticas de autenticação reforçada
- Auditoria de ações privilegiadas
- Controlo de credenciais administrativas

---

## 5. Ordem de Aplicação

A aplicação segue a hierarquia padrão:

1. Local policy
2. Site (não implementado neste ambiente)
3. Domain
4. OU
5. GPOs linkadas a OUs

---

## 6. Segurança e Boas Práticas

- Evitar GPOs excessivamente globais
- Preferir GPOs específicas por OU
- Utilizar grupos de segurança para filtragem
- Minimizar conflitos entre políticas
- Testar políticas na OU `Test` antes de produção

---

## 7. Limitações Atuais

- Sem GPO central de baseline unificada
- Sem Advanced Group Policy Management (AGPM)
- Sem versionamento avançado de GPOs
- Estrutura ainda em fase de implementação

---

## 8. Evolução Prevista

- Criação de baseline de segurança corporativa
- Implementação de políticas de LAPS via GPO
- Hardening avançado de endpoints (AppLocker / WDAC)
- Integração com auditoria centralizada
- Segmentação de GPO por compliance e risco

---

## 9. Rastreamento de GPOs

| ID | GPO | Escopo | Objetivo | Status | Fase |
|----|-----|--------|----------|--------|------|
| 1 | GPO_Baseline_Domain_Policy | Domínio | Políticas globais de segurança (palavras-passe, bloqueio de conta, auditoria) | Pendente | Fase 1 |
| 2 | GPO_Workstation_Baseline | `02_Computers > Workstations` | Padronização de estações (Firewall, Defender, Windows Update, AutoRun, sincronização de hora) | Pendente | Fase 1 |
| 3 | GPO_USB_Storage_Restriction | Workstations | Bloqueio de dispositivos de armazenamento USB | Pendente | Fase 1 |
| 4 | GPO_Corporate_Wallpaper | Utilizadores | Aplicação de wallpaper institucional | Pendente | Fase 1 |
| 5 | GPO_Desktop_Restrictions | Utilizadores | Bloqueio de Painel de Controlo, CMD, PowerShell, alteração de wallpaper | Pendente | Fase 1 |
| 6 | GPO_Windows_Defender_Policy | Workstations | Proteção em tempo real, Cloud Protection, atualizações automáticas, scan periódico | Pendente | Fase 2 |
| 7 | GPO_Windows_Firewall_Policy | Workstations e Servers | Firewall ativa, perfis Domain/Public/Private, bloqueio inbound por defeito | Pendente | Fase 2 |
| 8 | GPO_Audit_Policy | Domain Controllers | Auditoria de logon, logoff, alteração de utilizadores/grupos, criação/eliminação de objetos | Pendente | Fase 2 |
| 9 | GPO_Drive_Mapping | Utilizadores | Mapeamento automático de drives por departamento (Financeiro F:, Comercial C:, Direção D:) | Pendente | Fase 3 |
| 10 | GPO_Printer_Deployment | Utilizadores | Distribuição automática de impressoras (via PaperCut) | Pendente | Fase 3 |
| 11 | GPO_Local_Administrators_Management | Workstations | Controlo do grupo local Administrators para evitar administradores não autorizados | Pendente | Fase 4 |
| 12 | GPO_Remote_Desktop_Policy | Servers | Configuração de acesso RDP, NLA obrigatório, firewall | Pendente | Fase 4 |
| 13 | GPO_BitLocker | Workstations (opcional) | Criptografia de disco, recuperação de chaves, integração com AD (requer TPM) | Pendente | Fase 4 |
| 14 | GPO_LAPS_Policy | Workstations | Password única por máquina, rotação automática, armazenamento no AD | Pendente | Fase 4 |

### Legenda de Status
- Pendente
- Em Progresso
- Concluído
- Bloqueado/Requer Dependências