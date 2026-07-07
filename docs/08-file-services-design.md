# 08 - File Services Design

## 1. Visão Geral

Os File Services são o serviço de armazenamento partilhado do ambiente **HomeLab Consultoria & Contabilidade**. O objetivo é centralizar ficheiros de departamento, aplicar permissões coerentes e reduzir a dependência de dados dispersos em estações de trabalho.

No desenho atual, o serviço principal de ficheiros é o **TrueNAS SCALE 25.10.4**, integrado ao Active Directory via SMB/ACLs. O Windows Server continua válido como alternativa funcional para cenários que exijam uma implementação nativa Microsoft, mas não é a escolha principal do design.

---

## 2. Objetivo

Este serviço deve:

- Fornecer partilhas centralizadas por departamento
- Garantir controlo de acesso através de grupos e ACLs
- Separar leitura e escrita de forma explícita
- Servir de base ao mapeamento automático de unidades via GPO
- Suportar auditoria e recuperação de dados

---

## 3. Arquitetura

### 3.1 Papel do File Server

O File Server é o repositório central de ficheiros do domínio. Ele não decide quem é o utilizador nem a que departamento pertence; essa informação vem do Active Directory e dos grupos globais.

### 3.2 Opções possíveis

#### TrueNAS / Samba

- Servidor de ficheiros principal no desenho atual
- Integração via SMB/CIFS com o domínio `lan.homelab.ao`
- Boa separação entre storage e camada de identidade
- Exige disciplina no modelo de ACLs e na convenção de grupos

#### Windows Server File Server

- Integração nativa com Active Directory
- Gestão direta de permissões NTFS e SMB
- Boa compatibilidade com GPO, auditoria e ACLs
- Mantido como alternativa compatível para cenários Microsoft puros

### 3.3 Decisão arquitetural

A decisão recomendada para o ambiente é usar **TrueNAS SCALE 25.10.4** como implementação principal de File Services.

Razões:

- Alinhamento com o diagrama arquitetural atual
- Separação clara entre storage e serviços de identidade
- Base adequada para SMB, snapshots e recuperação
- Mantém o Windows Server como opção compatível sem mudar o modelo mental de autorização

Windows Server File Server continua válido como alternativa futura para cenários em que a implementação nativa Microsoft seja desejada.

---

## 4. Estrutura das Partilhas

### 4.1 Estrutura prevista

```text
\\truenas
└── Shares
  ├── Financeiro
  ├── Comercial
  ├── Direcao
  └── Publico
```

### 4.2 Organização dos dados

- Cada partilha representa uma área funcional
- Ficheiros sensíveis ficam separados por departamento
- O acesso público deve ser limitado ao mínimo necessário
- O armazenamento de administração não deve misturar-se com dados de utilizador

---

## 5. Convenção de Nomes

### 5.1 Servidor

- `truenas` para o servidor de ficheiros principal

### 5.2 Partilhas

- `Financeiro`
- `Comercial`
- `Direcao`
- `Publico`

### 5.3 Grupos de autorização

- `GG-` para grupos globais de função
- `DL-FS-` para grupos Domain Local de autorização de ficheiros

---

## 6. Modelo de Autorização

O ambiente segue o modelo AGDLP:

```text
Accounts
↓
Global Groups
↓
Domain Local Groups
↓
Permissions
```

### 6.1 Significado das camadas

- Accounts: utilizadores reais do domínio
- Global Groups: função ou departamento do utilizador
- Domain Local Groups: autorização sobre um recurso específico
- Permissions: ACLs e Share Permissions no File Server

### 6.2 Regra operacional

- Não atribuir permissões diretamente a utilizadores
- Não usar GG para permissões de recursos
- Não misturar identidade com autorização

### 6.3 Exemplo prático

```text
Ana Silva
  ↓
GG-Comercial
  ↓
DL-FS-Comercial-RW
  ↓
Permissão NTFS
  ↓
\\truenas\Comercial
```

---

## 7. Permissões

### 7.1 ACLs

As ACLs devem ser usadas para o controlo fino de acesso no TrueNAS:

- Read
- Write
- Modify
- Full Control apenas para administração

### 7.2 Share Permissions

Permissões SMB/Share devem complementar as ACLs, não substituí-las.

Regras práticas:

- Usar permissões simples no share
- Aplicar o detalhe na camada de ACLs
- Evitar permissões diretas por utilizador

### 7.3 Leitura e escrita

- `RO` para leitura apenas
- `RW` para leitura e escrita
- `Full` apenas para suporte técnico ou administração quando necessário

---

## 8. Pré-requisitos

- Active Directory funcional
- OUs e grupos globais já criados
- Domain Local Groups definidos para cada recurso
- TrueNAS preparado para integração ao domínio via SMB/CIFS
- Plano de backup e restauração definido

---

## 9. Implementação

A implementação deve seguir a separação entre:

- Identidade no AD
- Autorização em DL Groups
- Permissões finais em ACLs e SMB
- Automatização do acesso via GPO

---

## 10. Validação / Testes

Validar:

- A resolução do nome `truenas`
- A autenticação no domínio
- A herança de permissões nas partilhas
- O acesso correto por departamento
- A diferença entre leitura e escrita

---

## 11. Boas Práticas

- Usar a mesma lógica de nomes em todo o ambiente
- Separar partilhas por contexto de negócio
- Evitar permissões locais em pastas de departamento
- Usar GG para identidade e DL para autorização
- Testar o modelo com utilizadores de validação antes de produção

---

## 12. Limitações Atuais

- File Server operacional e integrado ao domínio
- Sem política de quotas implementada
- Sem auditoria avançada aplicada a todas as partilhas

---

## 13. Evolução Futura

- Implementar quotas por departamento
- Adicionar auditoria detalhada de acessos
- Expandir para home folders, se necessário
- Integrar snapshots e restore automatizado
- Avaliar storage alternativo com TrueNAS/Samba