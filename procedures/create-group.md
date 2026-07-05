# create-group.md

## 1. Objetivo

Criar grupos de segurança para controlo de acesso baseado em modelo AGDLP no domínio `lan.homelab.ao`, gerido por Active Directory.

---

## 2. Pré-requisitos

- Acesso administrativo ao domínio
- Estrutura de OUs definida
- Definição clara do departamento

---

## 3. Processo

1. Abrir **Active Directory Users and Computers**
2. Navegar para:
   - `OU=00_Admin > Groups`
3. Criar novo grupo:
   - **New → Group**

---

## 4. Configuração

- Group Scope: **Global**
- Group Type: **Security**
- Nome padrão:
  - `GG-Departamento` (ex: GG-Financeiro)

---

## 5. Utilização

- Adicionar utilizadores ao grupo
- Associar grupo a permissões de recursos
- Usar para GPO filtering quando necessário

---

## 6. Validação

- Confirmar membros do grupo
- Testar acesso a recursos associados

---

## 7. Problemas Comuns

- Uso de Distribution Group em vez de Security Group
- Nome inconsistente
- Grupo criado fora da OU correta

---

## 8. Resultado Esperado

Grupo funcional integrado no modelo AGDLP.