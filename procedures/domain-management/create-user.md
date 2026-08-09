# Procedimento Operacional: Criação de Utilizador no Active Directory (v0.2.0)

Este documento descreve o procedimento padrão para criação de contas de utilizadores comuns no domínio corporativo `corp.homelab.ao` da **HomeLab Consultoria & Contabilidade**.

---

## 1. Pré-requisitos
*   Credenciais administrativas com permissões de escrita na OU correspondente (ex: membros do grupo `GG-IT-Admins` delegados, ou `Domain Admins`).
*   Nome completo do colaborador, departamento de alocação e cargo.

---

## 2. Método 1: Interface Gráfica (AD Administrative Center / ADUC)

1.  No Domain Controller ou numa estação de trabalho de IT com as RSAT instaladas, abra o **Active Directory Administrative Center** (`dsac.exe`) ou **Active Directory Users and Computers** (`dsa.msc`).
2.  Navegue até à árvore do domínio: `corp.homelab.ao` -> **`01_Users`**.
3.  Selecione a sub-OU correspondente ao departamento do utilizador:
    *   `Financeiro`
    *   `Comercial`
    *   `Direcao`
    *   `IT_Admin`
4.  Clique com o botão direito na sub-OU correspondente e selecione **New** -> **User**.
5.  Preencha os campos de identificação seguindo a convenção de nomenclatura padrão:
    *   **First Name:** *Primeiro Nome do Utilizador* (ex: `Ana`)
    *   **Last Name:** *Último Nome do Utilizador* (ex: `Silva`)
    *   **User Logon Name (UPN):** `primeiro.ultimo` (ex: `ana.silva`) correspondente ao sufixo `@corp.homelab.ao`.
    *   **User Logon Name (Pre-Windows 2000):** `CORP\primeiro.ultimo`
6.  Defina uma palavra-passe inicial temporária e complexa (mínimo de 12 caracteres).
7.  Ative a opção **"User must change password at next logon"** (O utilizador deve alterar a password no próximo logon).
8.  Verifique se a opção **"Account is enabled"** está ativa.
9.  Clique em **OK** para criar a conta.

---

## 3. Método 2: PowerShell (Recomendado para Rapidez e Automação)

A criação via PowerShell garante consistência e reduz erros manuais. Execute a consola do PowerShell como Administrador.

### 3.1 Script de Criação Base

Substitua as variáveis conforme necessário antes de executar:

```powershell
# 1. Definir variáveis do utilizador
$FirstName = "Ana"
$LastName = "Silva"
$SamAccountName = "ana.silva"  # padrao: nome.apelido
$Department = "Financeiro"     # Opcoes: Financeiro, Comercial, Direcao, IT_Admin
$Description = "Contabilista Sénior"
$UserPrincipalName = "$SamAccountName@corp.homelab.ao"
$TargetOU = "OU=$Department,OU=01_Users,DC=corp,DC=homelab,DC=ao"

# 2. Gerar password temporária segura
$PasswordRaw = "TempPass2026!" # Deve ser alterada no primeiro logon
$SecurePassword = ConvertTo-SecureString $PasswordRaw -AsPlainText -Force

# 3. Executar a criação da conta
New-ADUser -Name "$FirstName $LastName" `
           -GivenName $FirstName `
           -Surname $LastName `
           -SamAccountName $SamAccountName `
           -UserPrincipalName $UserPrincipalName `
           -Path $TargetOU `
           -AccountPassword $SecurePassword `
           -ChangePasswordAtLogon $true `
           -Enabled $true `
           -Department $Department `
           -Description $Description `
           -PassThru
```

### 3.2 Validação da Criação

Para confirmar se a conta foi provisionada no local correto com os atributos necessários:

```powershell
Get-ADUser -Identity $SamAccountName -Properties Path, Department, Description
```
