# Desenho de Armazenamento: TrueNAS SCALE (v0.2.0)

```text
Estado: Planeado (Desenho Arquitetural)
Versão: v0.2.0
```

Este documento especifica o planeamento arquitetural para a centralização de armazenamento de ficheiros da **HomeLab Consultoria & Contabilidade**, utilizando o sistema operativo **TrueNAS SCALE**. O objetivo é disponibilizar um repositório seguro para documentos de consultoria, relatórios contabilísticos, ficheiros de operação e dados que exigem controlo de acesso por departamento.

O desenho procura refletir a forma como uma empresa pequena organiza o acesso a informação sensível, mantendo partilhas separadas por área funcional e por nível de confiança.

---

## 1. Caracterização do Servidor Planeado

*   **Hostname:** `HL-LDA-P-FS01`
*   **IP de Dados (VLAN 40):** `10.0.40.10`
*   **IP de Gestão (VLAN 10):** `10.0.10.10`
*   **Plataforma:** Máquina Virtual TrueNAS SCALE a correr em VMware Workstation Pro.
*   **Sistema de Ficheiros:** ZFS (Zettabyte File System).

---

## 2. Topologia do ZFS Pool

O servidor utilizará um pool de armazenamento ZFS com redundância contra falha de unidades virtuais:

*   **Nome do Zpool:** `tank`
*   **Configuração de Discos:** RAIDZ2 (4 discos virtuais de 1 TB ou 2 TB mapeados a partir do host físico).
*   **Tolerância a Falhas:** Suporta a perda simultânea de até 2 discos rígidos virtuais sem quebra do pool de dados.

---

## 3. Datasets e Partilhas SMB Planeadas

O Zpool `tank` será organizado em sub-datasets dedicados por departamento para aplicar quotas de espaço e políticas de segurança:

```text
tank (Zpool)
├── Financeiro (Quota: 2TB) -> Partilha: \\HL-LDA-P-FS01\Financeiro
├── Comercial (Quota: 2TB)  -> Partilha: \\HL-LDA-P-FS01\Comercial
├── Direcao (Quota: 1TB)    -> Partilha: \\HL-LDA-P-FS01\Direcao
├── Publico (Quota: 1TB)    -> Partilha: \\HL-LDA-P-FS01\Publico
└── IT_Admin (Quota: 500GB) -> Partilha: \\HL-LDA-P-FS01\Administracao
```

---

## 4. Integração no AD e Mapeamento de Permissões

O TrueNAS SCALE será integrado ao domínio `corp.homelab.ao` utilizando a conta de serviço `s-truenas`. 

As permissões sobre as partilhas de rede serão geridas exclusivamente por **Grupos Domain Local (DL)** do Active Directory, aplicando o modelo AGDLP:

*   **Partilha Financeiro:**
    *   Leitura e Escrita: `DL-FS-FINANCEIRO-RW` (Contém o grupo global `GG-FINANCEIRO-USERS`).
    *   Apenas Leitura: `DL-FS-FINANCEIRO-RO` (Contém o grupo global `GG-DIRECAO-USERS`).
*   **Partilha Comercial:**
    *   Leitura e Escrita: `DL-FS-COMERCIAL-RW` (Contém o grupo global `GG-COMERCIAL-USERS`).
    *   Apenas Leitura: `DL-FS-COMERCIAL-RO` (Contém o grupo global `GG-FINANCEIRO-USERS` para auditoria).
*   **Partilha Direção:**
    *   Leitura e Escrita: `DL-FS-DIRECAO-RW` (Contém o grupo global `GG-DIRECAO-USERS`).
*   **Partilha Público:**
    *   Leitura e Escrita: `DL-FS-PUBLICO-RW` (Contém todos os utilizadores autenticados).
*   **Partilha Administração:**
    *   Leitura e Escrita: `DL-FS-ADMIN-RW` (Contém o grupo global `GG-IT-ADMINS`).

---

## 5. Mapeamento Automático de Unidades (GPO)

Quando ativado, os utilizadores receberão os seus mapeamentos através de GPO Preferences (`GPO_USR_DriveMapping`) filtradas por pertença a grupos `GG-`:
*   Unidade `F:` -> `\\HL-LDA-P-FS01\Financeiro`
*   Unidade `G:` -> `\\HL-LDA-P-FS01\Comercial`
*   Unidade `D:` -> `\\HL-LDA-P-FS01\Direcao`
*   Unidade `P:` -> `\\HL-LDA-P-FS01\Publico`
*   Unidade `M:` -> `\\HL-LDA-P-FS01\Administracao`
