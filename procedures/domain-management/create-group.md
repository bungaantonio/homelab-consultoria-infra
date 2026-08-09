# Procedimento Operacional: Criação de Grupos de Segurança (v0.2.0)

Este documento descreve o procedimento para criar e associar Grupos Globais (GG) e Grupos Locais do Domínio (DL) no Active Directory (`corp.homelab.ao`), mantendo a integridade do modelo de acessos **AGDLP**.

---

## 1. Padrões de Alocação de Grupos

*   **Grupos Globais (GG):** Representam os departamentos ou funções e devem ser criados stritamente no caminho:
    `OU=Global,OU=Groups,OU=00_Admin,DC=corp,DC=homelab,DC=ao`
*   **Grupos Domain Local (DL):** Representam as permissões de recursos e devem ser criados no caminho:
    `OU=DomainLocal,OU=Groups,OU=00_Admin,DC=corp,DC=homelab,DC=ao`

---

## 2. Método 1: Criação via Consola Gráfica

1.  Abra a consola **Active Directory Users and Computers** (`dsa.msc`).
2.  Navegue até **`00_Admin`** -> **`Groups`**.
3.  Selecione a sub-OU correta para o tipo de grupo que pretende criar (**Global** ou **DomainLocal**).
4.  Clique com o botão direito e selecione **New** -> **Group**.
5.  Configure os atributos do grupo:
    *   **Group name:** Insira o nome de acordo com a convenção (ex: `GG-Financeiro` ou `DL-FS-Financeiro-RW`).
    *   **Group scope:**
        *   Se for um grupo funcional de utilizadores, selecione **Global**.
        *   Se for um grupo de atribuição de recursos, selecione **Domain Local**.
    *   **Group type:** Selecione **Security** (Segurança). *Não utilizar grupos de distribuição para controlo de acessos.*
6.  Clique em **OK**.

---

## 3. Método 2: Criação via PowerShell (Recomendado)

Abra a consola do PowerShell com privilégios de administrador de domínio e execute os comandos abaixo.

### 3.1 Criar Grupos Globais (Departamentais)

```powershell
# Variável com o nome do grupo departamental
$GroupName = "GG-Financeiro"
$PathGlobal = "OU=Global,OU=Groups,OU=00_Admin,DC=corp,DC=homelab,DC=ao"

# Criação do Grupo Global
New-ADGroup -Name $GroupName `
            -GroupScope Global `
            -GroupCategory Security `
            -Path $PathGlobal `
            -Description "Grupo Global para membros do departamento Financeiro" `
            -PassThru
```

### 3.2 Criar Grupos Domain Local (Recursos)

```powershell
# Variáveis para o grupo local do domínio
$DLGroupName = "DL-FS-Financeiro-RW"
$PathLocal = "OU=DomainLocal,OU=Groups,OU=00_Admin,DC=corp,DC=homelab,DC=ao"

# Criação do Grupo Domain Local
New-ADGroup -Name $DLGroupName `
            -GroupScope DomainLocal `
            -GroupCategory Security `
            -Path $PathLocal `
            -Description "Acesso de Leitura e Escrita na Partilha SMB Financeira do TrueNAS" `
            -PassThru
```

---

## 4. Associação de Grupos (Ligação do Modelo AGDLP)

De acordo com o modelo AGDLP, os utilizadores comuns pertencem aos Grupos Globais, e os Grupos Globais são adicionados como membros dos Grupos Domain Local.

### 4.1 Adicionar Utilizador a um Grupo Global
Para adicionar a utilizadora `ana.silva` ao grupo global `GG-Financeiro`:

```powershell
Add-ADGroupMember -Identity "GG-Financeiro" -Members "ana.silva"
```

### 4.2 Adicionar Grupo Global a um Grupo Domain Local (Aninhamento)
Para associar as permissões de leitura/escrita do Financeiro ao grupo funcional de utilizadores correspondente:

```powershell
Add-ADGroupMember -Identity "DL-FS-Financeiro-RW" -Members "GG-Financeiro"
```

### 4.3 Verificação das Relações de Grupo

Para validar os membros de um grupo:

```powershell
Get-ADGroupMember -Identity "DL-FS-Financeiro-RW" | Select-Object Name, SamAccountName, objectClass
```
