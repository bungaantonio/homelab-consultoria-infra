# 04 - Active Directory

## 1. Visão Geral

O Active Directory é o serviço de diretório central do ambiente **HomeLab Consultoria & Contabilidade** e o ponto de controlo para identidades, organização lógica e delegação administrativa.

O domínio corporativo é:

- `lan.homelab.ao`

Neste modelo, o AD gere identidades e a organização dos objetos. As permissões de recursos ficam fora do AD e são aplicadas através de grupos e ACLs nos serviços corretos.

---

## 2. Objetivo

O Active Directory existe para:

- Autenticar utilizadores, computadores e contas de serviço
- Organizar objetos em OUs com intenção administrativa clara
- Suportar aplicação de GPOs por escopo e função
- Separar identidade, organização lógica e autorização
- Permitir delegação segura de administração

---

## 3. Arquitetura

O desenho lógico segue uma abordagem por camadas:

- Identidade: utilizadores, grupos globais e contas de serviço
- Organização lógica: OUs por função administrativa
- Autorização: grupos Domain Local ligados a recursos
- Recursos: file servers, impressoras e outros serviços
- Automação: GPOs e scripts de apoio operacional

```text
lan.homelab.ao
│
├── 00_Admin
│   ├── Users
│   └── Groups
│       ├── Global
│       └── DomainLocal
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
```

---

## 4. Organização por OUs

### 4.1 `00_Admin`

Contém contas administrativas e grupos de gestão.

Objetivo:

- Separar identidades privilegiadas do utilizador comum
- Aplicar políticas mais restritivas a administradores
- Guardar grupos globais e Domain Local de forma organizada

### 4.2 `01_Users`

Contém utilizadores finais organizados por departamento.

Objetivo:

- Aplicar GPOs de utilizador por função
- Facilitar filtragem por departamento
- Manter a estrutura ligada à organização real da empresa

### 4.3 `02_Computers`

Contém estações de trabalho e máquinas de teste.

Objetivo:

- Aplicar hardening e baseline por tipo de equipamento
- Separar produção de validação

### 4.4 `03_Servers`

Contém servidores da infraestrutura.

Objetivo:

- Aplicar políticas específicas para servidores
- Reduzir superfície de ataque
- Facilitar administração e auditoria

### 4.5 `04_Service_Accounts`

Contém contas não interativas usadas por serviços e automações.

Objetivo:

- Isolar identidades técnicas de contas humanas
- Evitar reutilização de contas administrativas para serviços

---

## 5. Modelo de Identidade e Autorização

### 5.1 Identidade

O AD identifica quem é o utilizador com:

- Contas de utilizador
- Contas de serviço
- Grupos globais que representam função ou departamento

### 5.2 Organização lógica

As OUs não concedem permissões de recurso. Elas servem para:

- Aplicação de GPOs
- Delegação administrativa
- Separação operacional de objetos

### 5.3 Autorização

As permissões de acesso são atribuídas através de grupos Domain Local e ACLs dos recursos.

O padrão é:

- GG = quem é o utilizador
- DL = que recurso ou permissão será atribuída

---

## 6. Integração com DNS

O Active Directory integra-se com o DNS interno para resolver:

- Nome do domínio `lan.homelab.ao`
- Localização de controladores de domínio
- Descoberta de serviços internos

---

## 7. GPO e Recursos

As GPOs são usadas para configuração e automação.

Elas não substituem:

- Permissões NTFS
- Permissões SMB/Share
- ACLs de impressoras

O AD define identidade e pertença a grupos. O recurso aplica a autorização final.

---

## 8. Modelo de Segurança

Princípios aplicados:

- Separação de contas administrativas
- Não usar utilizadores diretamente em permissões de recursos
- Delegar por grupos e OUs
- Manter o menor privilégio possível
- Validar alterações em OU de teste antes de produção

---

## 9. Limitações Atuais

- Single Domain Controller, sem redundância
- Sem Sites & Services configurado
- Sem FGPP implementado
- Sem LAPS totalmente operacional

---

## 10. Evolução Futura

- Ativação do AD Recycle Bin
- Implementação de LAPS
- Delegação administrativa granular para helpdesk
- Fine-Grained Password Policies
- Integração com AD CS
