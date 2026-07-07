# OU Design - Active Directory

## Estrutura de Organizational Units

```
lan.homelab.ao
│
├── 00_Admin
│   ├── Users
│   └── Groups
│
├── 01_Users
│   ├── Financeiro
│   ├── Comercial
│   └── Direção
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

## Papel das OUs no Active Directory

Organizational Units são containers que permitem:

- Aplicação de Group Policy Objects (GPOs) em escopo específico
- Delegação de administração granular
- Organização lógica de objetos por função e tipo
- Controlo de herança de políticas

As OUs definem fronteiras administrativas onde as GPOs são aplicadas. A estrutura hierárquica permite herança de políticas de parente para filho, com possibilidade de bloqueio de herança (Block Inheritance) e aplicação forçada (Enforced).

## Relação com Group Policy Objects

O scope de uma GPO é determinado pela OU onde é linkada:

- GPO linkada a OU raiz aplica-se a todos os objetos descendentes
- GPO linkada a OU específica aplica-se apenas a objetos nessa OU e descendentes
- Ordem de aplicação: GPOs de OUs pais aplicam-se primeiro, seguidas por GPOs de OUs filhas
- Em caso de conflito, a GPO com ordem de link mais baixa prevalece (se não houver Block Inheritance)

## Regras de Design de OUs

### Separação por função

- OUs devem refletir a estrutura organizacional da empresa
- Departamentos devem ter OUs dedicadas para políticas específicas
- Funções distintas (administração, utilizadores, servidores) devem estar separadas

### Separação por tipo de objeto

- Utilizadores e computadores devem estar em OUs diferentes
- Contas de serviço devem ter OU dedicada para controlo de privilégios
- Servidores por função (DC, File, Print) devem estar em OUs separadas

### Escalabilidade

- Evitar profundidade excessiva (máximo recomendado: 4-5 níveis)
- Nomes numéricos (00_, 01_, 02_) facilitam ordenação lógica
- Estrutura deve suportar crescimento sem reorganização massiva

### Delegação de administração

- OUs permitem delegar permissões administrativas específicas
- Delegação deve seguir princípio do menor privilégio
- Evitar delegação excessiva em OUs de nível superior

## Limitações das OUs

As OUs não são utilizadas para:

- Permissões de acesso a recursos (ficheiros, impressoras, shares)
- Controlo de acesso direto (ACLs em NTFS, SMB)
- Agrupamento funcional para autorização

Permissões são implementadas através de grupos de segurança (GG e DL), não de OUs.

## Erros Comuns em Design de OUs

- Criar OUs para cada recurso ou aplicação individual
- Misturar utilizadores e computadores na mesma OU
- Utilizar OUs para controlo de acesso em vez de grupos
- Criar estrutura demasiado profunda dificultando gestão
- Ignorar impacto de herança de GPOs em design
- Delegar permissões administrativas sem necessidade
- Criar OUs duplicadas para mesma função
