# 04 - Active Directory

## 1. Visão Geral

O serviço de diretório do ambiente **HomeLab Consultoria & Contabilidade** é baseado em Active Directory, implementado através de um único domínio corporativo.

O domínio central é:

- `lan.homelab.ao`

Este domínio suporta autenticação centralizada, gestão de identidades, aplicação de políticas e controlo de acesso a recursos internos.

---

## 2. Função do Active Directory

O Active Directory no ambiente desempenha as seguintes funções:

- Autenticação central de utilizadores e computadores
- Autorização de acesso a recursos internos
- Aplicação de Group Policy Objects (GPOs)
- Organização lógica de objetos do domínio
- Suporte a delegação administrativa

---

## 3. Estrutura do Domínio

O domínio está organizado de forma funcional para suportar gestão operacional e aplicação de políticas.

```text
lan.homelab.ao
├── 00_Admin
├── 01_Users
├── 02_Computers
├── 03_Servers
├── 04_Service_Accounts
````

---

## 4. Organização por OUs

### 4.1 00_Admin

Contém contas privilegiadas e grupos administrativos.

Objetivo:

* separação de identidades com privilégios elevados
* aplicação de políticas de segurança reforçadas

---

### 4.2 01_Users

Contém utilizadores finais organizados por departamento:

* Financeiro
* Comercial
* Direção

Objetivo:

* aplicação de GPOs por departamento
* gestão de acesso a recursos internos

---

### 4.3 02_Computers

Contém estações de trabalho e equipamentos cliente.

Subdivisões:

* Workstations
* Test

Objetivo:

* controlo de segurança e hardening de endpoints
* gestão de software e políticas de sistema

---

### 4.4 03_Servers

Contém servidores da infraestrutura.

Subdivisões:

* Domain Controllers
* Infrastructure
* Applications
* File Servers
* Print Servers

Objetivo:

* aplicação de políticas de segurança específicas para servidores
* hardening de sistemas críticos

---

### 4.5 04_Service_Accounts

Contém contas de serviço utilizadas por aplicações e processos automáticos.

Subdivisões:

* Applications
* Infrastructure
* Automation

Objetivo:

* isolamento de contas não interativas
* controlo de permissões e auditoria

---

## 5. Modelo de Administração

A organização do Active Directory segue três princípios:

* Separação de identidade (Users, Service Accounts)
* Separação de infraestrutura (Computers, Servers)
* Controlo centralizado por políticas (GPO)

---

## 6. Integração com DNS

O Active Directory integra-se com o serviço DNS interno:

* Zona principal: `lan.homelab.ao`
* Resolução interna de nomes
* Suporte a localização de serviços do domínio

---

## 7. Group Policy (Visão Geral)

As GPOs são aplicadas com base em:

* OUs de utilizadores (políticas por departamento)
* OUs de computadores (hardening de endpoints)
* OUs de servidores (segurança reforçada)

---

## 8. Modelo de Segurança

Medidas estruturais:

* separação de contas administrativas
* isolamento de service accounts
* segmentação de políticas por OU
* controlo de acesso baseado em grupos de segurança

---

## 9. Limitações Atuais

* Single Domain Controller (sem redundância)
* Sem implementação de AD Sites & Services
* Sem Fine-Grained Password Policies (ainda não aplicadas)
* Sem LAPS ainda implementado

---

## 10. Evolução Prevista

* Ativação do AD Recycle Bin
* Implementação de LAPS
* Delegação granular de administração (Helpdesk model)
* Fine-Grained Password Policies (FGPP)
* Integração com serviços de certificados (AD CS)
