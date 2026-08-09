# Estrutura do Active Directory Domain Services (v0.2.0)

```text
Estado: Implementado (Serviços Base e OUs); Definido arquiteturalmente (Contas de Serviço e Grupos)
Versão: v0.2.0
```

Este documento detalha o desenho lógico e a implementação do serviço de identidades **Active Directory Domain Services (AD DS)** no domínio `corp.homelab.ao`, com foco em apoiar a operação corporativa da **HomeLab Consultoria & Contabilidade**. A estrutura foi pensada para separar claramente administração, utilizadores comuns, departamentos, servidores e contas de serviço, refletindo a forma como uma organização de consultoria e contabilidade organiza o acesso à informação.

A intenção é garantir que os utilizadores tenham acesso apenas ao que necessitam para trabalhar, preservando a confidencialidade de documentos fiscais, relatórios internos e dados de clientes.

---

## 1. Informações Básicas do Domínio

*   **Nome de Domínio (FQDN):** `corp.homelab.ao`
*   **Nome NetBIOS:** `CORP`
*   **Domain Controller Principal:** `HL-LDA-P-DC01` (IP: `10.0.20.10`)
*   **Serviços Ativos:** AD DS, DNS Integrado, Kerberos, LDAP (Segurança de Canal Ativa).

---

## 2. Desenho de Unidades Organizacionais (OUs)

A árvore de OUs foi criada na raiz do domínio `corp.homelab.ao` para organizar logicamente utilizadores, computadores e servidores:

```text
corp.homelab.ao
│
├── 00_Admin
│   ├── Users (Administradores nominativos da rede)
│   └── Groups
│       ├── Global (Grupos Globais de utilizadores)
│       └── DomainLocal (Grupos Locais de recursos)
│
├── 01_Users
│   ├── Financeiro (Utilizadores do depto. financeiro)
│   ├── Comercial (Utilizadores do depto. comercial)
│   ├── Direcao (Sócios e gestores séniores)
│   ├── IT_Admin (Administração técnica)
│   └── Disabled (Contas inativas/ex-colaboradores)
│
├── 02_Computers
│   ├── Workstations (Desktops corporativos)
│   └── Laptops (Equipamentos portáteis)
│
├── 03_Servers
│   ├── Domain Controllers (Garantido por defeito)
│   ├── Infrastructure (Backup, monitorização)
│   └── File Servers (Servidores de ficheiros como o TrueNAS)
│
└── 04_Service_Accounts (Contas de serviço para automatizações)
```

---

## 3. Padrão de Grupos de Segurança (Modelo AGDLP)

O controlo de acessos segue o padrão AGDLP em letras maiúsculas:

### 3.1 Grupos Globais (GG)
Representam as funções de negócio e agrupam utilizadores pertencentes ao mesmo departamento.

*   **Padrão:** `GG-[DEPARTAMENTO]-[FUNÇÃO]`
*   **Grupos Definidos:**
    *   **`GG-FINANCEIRO-USERS`**: Agrupa os contabilistas e técnicos do financeiro (Membro: `ana.silva`).
    *   **`GG-COMERCIAL-USERS`**: Agrupa a equipa de vendas e comercial (Membro: `pedro.santos`).
    *   **`GG-DIRECAO-USERS`**: Administradores executivos da empresa (Membro: `carlos.lima`).
    *   **`GG-IT-ADMINS`**: Técnicos e administradores de infraestrutura (Membro: `antonio.bunga`).

### 3.2 Grupos Domain Local (DL)
Associam as permissões de acesso lógicas diretamente sobre as partilhas SMB do Storage.

*   **Padrão:** `DL-[RECURSO]-[PERMISSÃO]`
*   **Grupos Definidos:**
    *   **`DL-FS-FINANCEIRO-RW`**: Leitura e Escrita (Read/Write) na pasta `Financeiro`.
    *   **`DL-FS-FINANCEIRO-RO`**: Apenas Leitura (Read-Only) na pasta `Financeiro`.
    *   **`DL-FS-COMERCIAL-RW`**: Leitura e Escrita na pasta `Comercial`.
    *   **`DL-FS-COMERCIAL-RO`**: Apenas Leitura na pasta `Comercial`.
    *   **`DL-FS-PUBLICO-RW`**: Acesso completo de leitura/escrita na pasta de partilha comum.
    *   **`DL-FS-ADMIN-RW`**: Acesso exclusivo de leitura/escrita à pasta de IT.

---

## 4. Permissões de Pastas (Matriz ACL)

Quando o TrueNAS SCALE (`HL-LDA-P-FS01` - **Planeado**) for integrado, as ACLs de partilha e NTFS serão associadas da seguinte forma aos Grupos Domain Local:

| Partilha SMB | Grupo AD Autoritativo | Nível de Acesso NTFS | Membros (GG do AD) |
| :--- | :--- | :--- | :--- |
| **Financeiro** | `DL-FS-FINANCEIRO-RW` <br> `DL-FS-FINANCEIRO-RO` | Modificar / Escrever <br> Leitura / Execução | `GG-FINANCEIRO-USERS` <br> `GG-DIRECAO-USERS` |
| **Comercial** | `DL-FS-COMERCIAL-RW` <br> `DL-FS-COMERCIAL-RO` | Modificar / Escrever <br> Leitura / Execução | `GG-COMERCIAL-USERS` <br> `GG-FINANCEIRO-USERS` |
| **Direção** | `DL-FS-DIRECAO-RW` | Modificar / Escrever | `GG-DIRECAO-USERS` |
| **Público** | `DL-FS-PUBLICO-RW` | Modificar / Escrever | `GG-FINANCEIRO-USERS`, `GG-COMERCIAL-USERS`, `GG-DIRECAO-USERS` |
| **Administração**| `DL-FS-ADMIN-RW` | Modificar / Escrever | `GG-IT-ADMINS` |

---

## 5. Group Policy Objects (GPOs)

A aplicação de políticas de grupo controla as definições de segurança:

*   **`GPO_CORP_DefaultDomainPasswordPolicy`** (**Implementado**): Aplicado na raiz do domínio. Exige 12+ caracteres, histórico de 24 chaves, e bloqueio de conta temporário após 5 falhas consecutivas.
*   **`GPO_WKS_SecurityBaseline`** (**Implementado**): Aplicado na OU `02_Computers`. Ativa Firewall do Windows em todas as redes e atualizações automáticas.
*   **`GPO_WKS_RestrictUSB`** (**Planeado**): Restringe o acesso de escrita e leitura de discos removíveis nas estações de trabalho de utilizadores comuns.
*   **`GPO_USR_DriveMapping`** (**Planeado**): Mapeamento automático de unidades no logon (`F:`, `G:`, `P:`, `M:`) usando Group Policy Preferences baseadas na filiação do utilizador aos grupos `GG-`.

---

## 6. Contas de Serviço (Service Accounts)

Criadas na OU `04_Service_Accounts` com privilégios reduzidos:

*   **`s-truenas`** (**Planeado**): Conta utilizada para autenticação segura e leitura de utilizadores do AD pelo TrueNAS SCALE.
*   **`s-backup`** (**Planeado**): Conta com permissões de leitura exclusivas em partilhas para cópias de segurança locais.
*   **`s-monitoring`** (**Planeado**): Utilizada por agentes de monitorização local dos servidores.
