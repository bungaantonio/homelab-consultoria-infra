# DL Design - Domain Local Groups

## Visão Geral

Domain Local Groups são grupos de segurança usados para representar autorização sobre recursos específicos.

No modelo AGDLP, os DL respondem à pergunta: que recurso ou permissão está a ser entregue?

---

## Objetivo

Os DL servem para:

- Atribuir permissões NTFS em file servers
- Atribuir permissões SMB/Share
- Controlar acesso a impressoras ou outros recursos
- Manter a camada de autorização separada da identidade

---

## Arquitetura

Os grupos Domain Local devem ficar organizados em `00_Admin > Groups > DomainLocal`.

### Estrutura recomendada para File Services

```text
00_Admin
└── Groups
    └── DomainLocal
        ├── DL-FS-Comercial-RW
        ├── DL-FS-Comercial-RO
        ├── DL-FS-Financeiro-RW
        ├── DL-FS-Financeiro-RO
        ├── DL-FS-Direcao-RW
        └── DL-FS-Direcao-RO
```

### Regras de design

- DL representam recursos e permissões
- DL não representam pessoas
- DL não devem receber utilizadores diretamente
- DL podem receber GG como membros

---

## Pré-requisitos

- GG já criados
- Estrutura de partilhas definida
- Nomeação dos recursos aprovada
- Servidor de ficheiros preparado

---

## Implementação

### 4.1 Permissões de leitura e escrita

- `DL-FS-Comercial-RW`
- `DL-FS-Financeiro-RW`
- `DL-FS-Direcao-RW`

### 4.2 Permissões de apenas leitura

- `DL-FS-Comercial-RO`
- `DL-FS-Financeiro-RO`
- `DL-FS-Direcao-RO`

### 4.3 Relação com GG

```text
GG-Comercial → DL-FS-Comercial-RW
GG-Comercial → DL-FS-Comercial-RO
GG-Financeiro → DL-FS-Financeiro-RW
GG-Financeiro → DL-FS-Financeiro-RO
GG-Direcao → DL-FS-Direcao-RW
GG-Direcao → DL-FS-Direcao-RO
```

### 4.4 Exemplo de fluxo

```text
Ana Silva
  ↓
GG-Comercial
  ↓
DL-FS-Comercial-RW
  ↓
Permissão NTFS e SMB
  ↓
\\truenas\Comercial
```

---

## Validação / Testes

Validar:

- O DL contém GG, não utilizadores diretos
- O nome do DL reflete recurso e permissão
- A permissão aplicada no recurso corresponde ao tipo `RW` ou `RO`

---

## Boas Práticas

- Usar prefixo `DL-FS-` para file services
- Separar leitura e escrita quando isso simplificar governação
- Reutilizar DL apenas quando o recurso for realmente o mesmo
- Documentar o propósito de cada DL

---

## Limitações Atuais

- Os DL ainda estão em fase de normalização documental
- Alguns procedimentos antigos ainda referem nomes de grupos legados

---

## Evolução Futura

- Expandir o modelo para outras classes de recursos, como impressão
- Consolidar a nomenclatura em todos os documentos e procedimentos
- Rever periodicamente membros e permissões associadas
