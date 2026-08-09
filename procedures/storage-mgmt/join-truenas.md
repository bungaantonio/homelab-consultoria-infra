# Procedimento Operacional: Integração do TrueNAS SCALE no AD (v0.2.0)

Este documento descreve o procedimento operacional padrão para associar (Join) o servidor de ficheiros **TrueNAS SCALE** (`HL-LDA-P-FS01`) ao domínio Active Directory `corp.homelab.ao`.

---

## 1. Pré-requisitos e Prevenção de Erros

Antes de tentar associar o TrueNAS ao domínio, valide os seguintes pontos de configuração. A maior parte dos erros de join decorre de falhas de DNS ou discrepâncias de horário.

### 1.1 Configuração de DNS (Crítico)
*   Aceda a **Network** -> **Global Configuration** no TrueNAS.
*   O **Nameserver 1** deve apontar estritamente para o IP do Domain Controller: `10.0.20.10`.
*   *Nota:* O TrueNAS na VLAN 40 (`10.0.40.10`) consegue comunicar com a VLAN 20 (`10.0.20.10`) através das regras de roteamento inter-VLAN autorizadas no pfSense.

### 1.2 Configuração de NTP (Sincronização de Relógio)
*   Aceda a **System** -> **General** -> **NTP Servers**.
*   Remova servidores públicos genéricos e adicione apenas o Domain Controller `10.0.20.10` (ou o FQDN `corp.homelab.ao`).
*   Verifique na consola do TrueNAS se a hora local tem uma diferença inferior a **5 minutos** em relação ao Domain Controller.

### 1.3 Conta de Serviço de Join no AD
*   Assegure-se de que a conta de serviço `s-truenas` existe no Active Directory e que lhe foi delegada a permissão de associar computadores ao domínio (Join Computers to Domain) na OU **`03_Servers/File Servers`**.

---

## 2. Passo a Passo de Integração (Web GUI do TrueNAS)

1.  Aceda à consola de gestão do TrueNAS SCALE via navegador de internet a partir da rede de gestão (MGMT): `https://10.0.10.10` (IPMI ou IP local da interface de gestão).
2.  No painel esquerdo, navegue para **Directory Services** -> **Active Directory**.
3.  Preencha as configurações do Active Directory:
    *   **Domain Name:** `corp.homelab.ao`
    *   **Domain Account Username:** `s-truenas` (ou uma conta com privilégios de Domain Admin se não utilizar conta de serviço delegada).
    *   **Domain Account Password:** Introduza a password correspondente.
    *   **Enable Active Directory:** Marque esta caixa para ativar o serviço.
4.  Clique em **Advanced Options** e configure o seguinte:
    *   **Idmap Backend:** `RID` (Garante mapeamentos estáveis e consistentes para as ACLs SMB do Windows).
    *   **LDAP SASL Wrapping:** `sign` ou `seal` (Garante encriptação e assinatura de segurança nas comunicações LDAP).
5.  Clique em **Save**.
6.  O TrueNAS iniciará a comunicação com o Domain Controller, registará o objeto de computador `HL-LDA-P-FS01` na base de dados do AD e sincronizará as identidades. O estado deverá mudar para **"STATUS: FAULT"** para **"STATUS: HEALTHY"** (ou similar indicador verde).

---

## 3. Comandos de Validação e Diagnóstico

Abra a consola Shell integrada no TrueNAS SCALE ou aceda via SSH a partir da rede de gestão (MGMT) para validar a integração:

### 3.1 Testar Comunicação com o Active Directory

```bash
# 1. Verificar se o TrueNAS consegue listar utilizadores do domínio
wbinfo -u

# 2. Verificar se o TrueNAS consegue listar grupos do domínio
wbinfo -g

# 3. Testar a resolução de identidades pelo sistema operativo Linux (deve retornar UID/GID numéricos)
getent passwd ana.silva
```

### 3.2 Verificar Registo no Domain Controller
Na consola do Active Directory do DC, execute o comando PowerShell para verificar o registo do servidor de ficheiros na OU correta:

```powershell
Get-ADComputer -Identity "HL-LDA-P-FS01" -Properties OperatingSystem, IPv4Address, DistinguishedName
```
A propriedade `DistinguishedName` deve mostrar o computador localizado dentro de `OU=File Servers,OU=03_Servers,DC=corp,DC=homelab,DC=ao`.
