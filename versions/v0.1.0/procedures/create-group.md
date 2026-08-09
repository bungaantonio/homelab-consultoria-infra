# create-group.md

## 1. Objetivo

Criar grupos de segurança para controlo de acesso baseado em modelo AGDLP no domínio `lan.homelab.ao`, gerido por Active Directory.

Este procedimento cobre apenas grupos globais de identidade. Os grupos Domain Local são criados separadamente para permissões de recursos.

---

## 2. Pré-requisitos

- Acesso administrativo ao domínio
- Estrutura de OUs definida
- Definição clara do departamento
- Convenção de nomes aprovada para GG e DL

---

## 3. Processo

1. Abrir **Active Directory Users and Computers**
2. Navegar para:
   - `OU=00_Admin > Groups > Global`
3. Criar novo grupo:
   - **New → Group**

---

## 4. Configuração

- Group Scope: **Global**
- Group Type: **Security**
- Nome padrão:
  - `GG-Departamento` (ex: GG-Financeiro)

Exemplos válidos:

- `GG-Comercial`
- `GG-Financeiro`
- `GG-Direcao`
- `GG-IT`
- `GG-Domain-Admins`

---

## 5. Utilização

- Adicionar utilizadores ao grupo
- Associar grupo a Domain Local Groups quando necessário
- Usar para GPO filtering quando necessário
- Não atribuir permissões NTFS ou Share diretamente ao GG

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