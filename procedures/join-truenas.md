# join-truenas.md

## 1. Objetivo

Integrar o servidor TrueNAS ao domínio `lan.homelab.ao` para autenticação centralizada e gestão de permissões baseada em grupos do Active Directory.

---

## 2. Pré-requisitos

- TrueNAS operacional na rede `10.0.10.0/24`
- DNS apontado para `10.0.10.10`
- Conectividade com Domain Controller
- Credenciais de administrador do domínio

---

## 3. Processo de Integração

### 3.1 Configuração de DNS

- Definir DNS do TrueNAS para:
  - `10.0.10.10`

---

### 3.2 Junção ao domínio

1. Aceder à interface do TrueNAS
2. Ir a:
   - Directory Services
3. Selecionar **Active Directory**
4. Inserir:
   - Domain: `lan.homelab.ao`
   - Credentials de admin

---

## 4. Validação

- Estado: “Joined”
- Utilizadores do domínio visíveis
- Teste de autenticação SMB

---

## 5. Integração com permissões

- Mapear grupos do AD para datasets
- Aplicar ACLs baseadas em departamentos
- Separação por partilhas

---

## 6. Problemas Comuns

- DNS incorreto
- Hora desfasada (Kerberos falha)
- Credenciais inválidas
- Firewall bloqueando portas AD

---

## 7. Resultado Esperado

TrueNAS integrado ao domínio com autenticação centralizada e controlo de acesso baseado em grupos.