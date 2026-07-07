# DL Design - Domain Local Groups

## Definição de Domain Local Groups

Domain Local Groups são grupos de segurança com scope de domínio que podem conter utilizadores, grupos globais e grupos universais de qualquer domínio da floresta. São utilizados para atribuir permissões a recursos dentro do mesmo domínio.

No modelo AGDLP (Accounts → Global Groups → Domain Local Groups → Permissions), os grupos Domain Local representam a camada de autorização (permissions layer).

## Função de Autorização

Grupos Domain Local são utilizados exclusivamente para:

- Atribuição de permissões NTFS em file servers
- Atribuição de permissões de Share em SMB
- Atribuição de permissões de impressora
- Controlo de acesso a recursos específicos

DL groups definem o que pode ser acedido, não quem acede. A identidade do utilizador é gerida pelos grupos globais (GG).

## Estrutura de Grupos DL para File Server

### Permissões de Leitura e Escrita (RW)

```
DL-Files-Financeiro-RW
DL-Files-Comercial-RW
DL-Files-Direcao-RW
DL-Files-IT-Full
```

Grupos RW permitem:

- Read
- Write
- Modify
- Execute (se aplicável)

### Permissões de Apenas Leitura (RO)

```
DL-Files-Financeiro-RO
DL-Files-Comercial-RO
DL-Files-Direcao-RO
```

Grupos RO permitem:

- Read
- Execute (se aplicável)

### Permissões de Controlo Total (Full)

```
DL-Files-IT-Full
```

Grupo Full permite:

- Full Control
- Change Permissions
- Take Ownership

## Estrutura de Grupos DL para Print Server

```
DL-Print-Financeiro
DL-Print-Comercial
DL-Print-Direcao
DL-Print-IT
```

Grupos de impressora permitem:

- Print
- Manage Documents (se aplicável)
- Manage Printer (apenas para administradores)

## Relação com Shares SMB e NTFS Permissions

Permissões são aplicadas em duas camadas:

### Share Permissions (SMB)

- Aplicadas a nível de share
- Permissões típicas: Read, Change, Full Control
- Menos granulares que NTFS
- Cumulativas com NTFS (mais restritiva prevalece)

### NTFS Permissions

- Aplicadas a nível de ficheiro/pasta
- Permissões granulares: Read, Write, Modify, Full Control, etc.
- Herança de permissões de pastas parentes
- Controlo fino de acesso

DL groups são utilizados em ambas as camadas para consistência.

## Integração com GG no Modelo AGDLP

No modelo AGDLP, os grupos Domain Local (DL) são a camada de autorização:

```
A (Accounts) → G (Global Groups) → DL (Domain Local Groups) → P (Permissions)
```

### Fluxo de Associação

```
joao.silva@lan.homelab.ao (Account)
   ↓
GG-IT (Global Group)
   ↓
DL-Files-IT-Full (Domain Local Group)
   ↓
NTFS Permission: Full Control
```

### Mapeamento GG → DL

```
GG-Financeiro → DL-Files-Financeiro-RW
GG-Financeiro → DL-Files-Financeiro-RO
GG-Financeiro → DL-Print-Financeiro

GG-Comercial → DL-Files-Comercial-RW
GG-Comercial → DL-Files-Comercial-RO
GG-Comercial → DL-Print-Comercial

GG-Direcao → DL-Files-Direcao-RW
GG-Direcao → DL-Files-Direcao-RO
GG-Direcao → DL-Print-Direcao

GG-IT → DL-Files-IT-Full
GG-IT → DL-Print-IT
```

## Regras Críticas de Segurança

### Regra 1: DL Nunca Recebe Utilizadores Diretamente

```
User → DL (incorreto)
User → GG → DL (correto)
```

Utilizadores devem sempre ser adicionados a grupos globais, que por sua vez são membros de grupos Domain Local.

### Regra 2: DL Representa Recurso, Não Identidade

Nomes de DL devem refletir o recurso e tipo de permissão:

- Correto: DL-Files-Financeiro-RW
- Incorreto: DL-FinanceiroUsers

### Regra 3: Minimizar Número de DL

Cada recurso deve ter o número mínimo de DL necessário:

- Um DL por tipo de permissão (RW, RO, Full)
- Evitar DL redundantes
- Reutilizar DL quando possível

### Regra 4: Consistência entre Share e NTFS

Utilizar os mesmos DL groups em Share Permissions e NTFS Permissions para evitar conflitos e simplificar gestão.

## Boas Práticas de Escalabilidade

### Nomenclatura

- Prefixo DL- para identificar grupos Domain Local
- Nome do recurso (Files, Print)
- Departamento ou função
- Tipo de permissão (RW, RO, Full)
- Consistência em todo o domínio

### Gestão

- Documentar cada DL group e sua finalidade
- Revisar periodicamente utilização de DL groups
- Remover DL groups não utilizados
- Auditoria regular de membros de DL groups

### Performance

- Evitar DL groups com muitos membros aninhados
- Limitar profundidade de aninhamento de grupos
- Considerar impacto em logon time em grandes ambientes

## Fluxo Completo de Acesso

### Exemplo 1: Acesso a Ficheiros Financeiros

```
joao.silva@lan.homelab.ao (Account)
   ↓
Membro de GG-Financeiro (Global Group)
   ↓
GG-Financeiro é membro de DL-Files-Financeiro-RW (Domain Local Group)
   ↓
DL-Files-Financeiro-RW tem permissão RW em share Financeiro (SMB)
   ↓
DL-Files-Financeiro-RW tem permissão Modify em pasta Financeiro (NTFS)
   ↓
joao.silva@lan.homelab.ao acede ficheiros com permissão Modify
```

### Exemplo 2: Acesso a Impressora Comercial

```
pedro.ferreira@lan.homelab.ao (Account)
   ↓
Membro de GG-Comercial (Global Group)
   ↓
GG-Comercial é membro de DL-Print-Comercial (Domain Local Group)
   ↓
DL-Print-Comercial tem permissão Print em impressora Comercial
   ↓
pedro.ferreira@lan.homelab.ao imprime em impressora Comercial
```

## Considerações de Implementação

DL groups serão implementados na Fase 3 após integração do TrueNAS no Active Directory. A criação de DL groups deve coincidir com:

- Configuração de partilhas SMB departamentais
- Definição de estrutura de permissões NTFS
- Configuração de PaperCut NG para gestão de impressão
