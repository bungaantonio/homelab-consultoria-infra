# 14 - TrueNAS SCALE 25.10.4 - File Server Integration with Active Directory

## 1. Objetivo

Este documento descreve a implementação completa do TrueNAS SCALE como servidor corporativo de ficheiros integrado com Active Directory.

O resultado esperado é:

- Utilizadores autenticam através do domínio `lan.homelab.ao`;
- O TrueNAS resolve utilizadores e grupos do AD;
- Partilhas SMB utilizam autenticação AD;
- Permissões são controladas através de grupos de segurança;
- Cada departamento possui isolamento de dados;
- A autorização segue o modelo AGDLP.

Arquitetura final:

```text
Utilizador AD
    |
    v
Global Group
(GG-Comercial)
    |
    v
Domain Local Group
(DL-FS-Comercial-RW)
    |
    v
ACL NFSv4
(TrueNAS Dataset)
    |
    v
SMB Share
    |
    v
Windows Client
```

---

## 2. Ambiente

### 2.1 Active Directory

Controlador de domínio:

```text
Hostname:
LAB-DC01

FQDN:
LAB-DC01.lan.homelab.ao

IP:
10.0.10.10

Domínio:
lan.homelab.ao
```

Funções:

- Active Directory Domain Services
- DNS
- Kerberos
- LDAP

### 2.2 TrueNAS

```text
Sistema:
TrueNAS SCALE 25.10.4

Hostname:
truenas

FQDN:
truenas.lan.homelab.ao

IP:
10.0.10.20
```

Função:

- Storage ZFS
- SMB File Server
- Integração AD

---

## 3. Pré-requisitos

### Active Directory

Confirmar:

```text
Get-ADDomain
```

Confirmar domínio:

```text
lan.homelab.ao
```

### DNS

O TrueNAS deve utilizar apenas o DNS interno.

Configuração:

```text
DNS:
10.0.10.10
```

Não configurar:

```text
8.8.8.8
1.1.1.1
```

Motivo:

O AD depende dos registos:

```text
_ldap._tcp
_kerberos._tcp
```

### Hora

Validar no TrueNAS:

```text
date
```

A diferença temporal entre TrueNAS e DC deve ser mínima.

Kerberos depende da sincronização.

---

## 4. Configuração inicial do TrueNAS

### 4.1 Rede

Configurar:

```text
IP:
10.0.10.20

Gateway:
10.0.10.1

DNS:
10.0.10.10
```

### 4.2 Validar conectividade

No TrueNAS:

```text
ping 10.0.10.10
```

Resultado esperado:

```text
64 bytes from 10.0.10.10
```

### 4.3 Validar DNS

Executar:

```text
nslookup lan.homelab.ao
```

Esperado:

```text
Name:
lan.homelab.ao

Address:
10.0.10.10
```

Validar DC:

```text
nslookup LAB-DC01.lan.homelab.ao
```

Esperado:

```text
10.0.10.10
```

Validar LDAP:

```text
nslookup -type=SRV _ldap._tcp.lan.homelab.ao
```

Esperado:

```text
LAB-DC01.lan.homelab.ao
```

Validar Kerberos:

```text
nslookup -type=SRV _kerberos._tcp.lan.homelab.ao
```

Esperado:

```text
LAB-DC01.lan.homelab.ao
```

---

## 5. Criar armazenamento ZFS

### 5.1 Criar Pool

Interface:

```text
Storage
 |
Pools
 |
Create Pool
```

Nome:

```text
tank
```

Discos:

```text
sdb
sdc
```

Layout:

```text
Mirror
```

Resultado:

```text
tank
 |
 +-- Mirror
    |
    +-- sdb
    +-- sdc
```

### 5.2 Criar Dataset principal

Criar:

```text
tank/Dados
```

Configuração:

```text
Compression:
LZ4

Atime:
OFF

Deduplication:
OFF

ACL Type:
NFSv4
```

---

## 6. Preparação dos grupos Active Directory

A autorização utiliza AGDLP.

### 6.1 Criar Global Groups

Local:

```text
OU=Groups
 |
 OU=Global
```

Criar:

```text
GG-Comercial
GG-Financeiro
GG-Direcao
GG-IT
```

### 6.2 Criar Domain Local Groups

Local:

```text
OU=Groups
 |
 OU=DomainLocal
```

Criar:

```text
DL-FS-Comercial-RW
DL-FS-Comercial-RO

DL-FS-Financeiro-RW
DL-FS-Financeiro-RO

DL-FS-Direcao-RW
DL-FS-Direcao-RO

DL-FS-Admins
```

---

## 7. Associar permissões AD

Exemplo Comercial:

Adicionar:

```text
GG-Comercial
```

dentro de:

```text
DL-FS-Comercial-RW
```

Comando:

```text
Add-ADGroupMember `
-Identity "DL-FS-Comercial-RW" `
-Members "GG-Comercial"
```

Validar:

```text
Get-ADGroupMember "DL-FS-Comercial-RW"
```

Resultado:

```text
GG-Comercial
```

---

## 8. Integrar TrueNAS no domínio

Interface:

```text
Credentials
 |
Directory Services
 |
Active Directory
```

Configurar:

Domínio:

```text
lan.homelab.ao
```

Credencial:

Administrador de domínio.

Executar:

```text
Join Domain
```

Validar no DC:

```text
Get-ADComputer TRUENAS
```

Esperado:

```text
Name:
TRUENAS
```

---

## 9. Validar Winbind

### Testar confiança

```text
wbinfo -t
```

Esperado:

```text
checking the trust secret succeeded
```

### Testar DC

```text
wbinfo --ping-dc
```

Esperado:

```text
connection succeeded
```

### Testar grupos

```text
getent group "LAN\\gg-comercial"
```

Esperado:

```text
LAN\\gg-comercial
```

---

## 10. Criar Dataset Comercial

Criar:

```text
tank/Dados/Comercial
```

Não criar permissões POSIX.

Usar:

```text
ACL NFSv4
```

---

## 11. Configurar ACL Comercial

Abrir:

```text
Datasets
 |
tank/Dados/Comercial
 |
Edit ACL
```

Configurar:

### Owner

```text
User:
LAN\\admin.it
```

### Group

```text
LAN\\dl-fs-admins
```

ACL:

Adicionar:

```text
LAN\\admin.it
```

Permissão:

```text
Full Control
```

Adicionar:

```text
LAN\\dl-fs-admins
```

Permissão:

```text
Full Control
```

Adicionar:

```text
LAN\\dl-fs-comercial-rw
```

Permissão:

```text
Modify
```

Adicionar:

```text
LAN\\dl-fs-comercial-ro
```

Permissão:

```text
Read
```

Aplicar.

---

## 12. Configurar Dataset Pai

No:

```text
tank/Dados
```

Garantir:

```text
DL-FS-Comercial-RW
Execute
```

Motivo:

O utilizador precisa atravessar o dataset pai para chegar ao filho.

---

## 13. Criar SMB Share

Menu:

```text
Shares
 |
Windows SMB Shares
 |
Add
```

Configuração:

Path:

```text
/mnt/tank/Dados/Comercial
```

Name:

```text
Comercial
```

Resultado:

```text
\\truenas\Comercial
```

Opções:

Ativar:

```text
Access Based Share Enumeration
```

Desativar:

```text
Guest Access
```

Guardar.

---

## 14. Teste funcional

### Administrador

Utilizador:

```text
LAN\\admin.it
```

Teste:

```text
\\truenas\Comercial
```

Validar:

- Criar ficheiro
- Editar
- Apagar

### Utilizador Comercial

Adicionar:

```text
ana.silva
```

ao:

```text
GG-Comercial
```

Terminar sessão Windows.

Entrar novamente.

Abrir:

```text
\\truenas\Comercial
```

Resultado esperado:

```text
Acesso permitido
```

Validar:

- Criar ficheiro
- Editar
- Apagar

---

## 15. Diagnóstico de problemas

### Erro:

```text
Access Denied
```

Verificar:

```text
Get-ADGroupMember DL-FS-Comercial-RW
```

O grupo deve conter:

```text
GG-Comercial
```

### Grupo não aparece no TrueNAS

Executar:

```text
wbinfo -g
```

ou:

```text
getent group "LAN\\grupo"
```

### ACL bloqueada

Validar dataset pai:

```text
tank/Dados
```

Deve permitir:

```text
Execute
```

nos grupos departamentais.

---

## 16. Estado Final

Implementação concluída:

```text
[OK] Rede
[OK] DNS
[OK] ZFS
[OK] AD Join
[OK] Kerberos
[OK] Winbind
[OK] AGDLP
[OK] ACL NFSv4
[OK] SMB
[OK] Teste utilizador
```
