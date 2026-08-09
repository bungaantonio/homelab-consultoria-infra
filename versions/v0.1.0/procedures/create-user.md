# create-user.md

## 1. Objetivo

Criar utilizadores no domínio `lan.homelab.ao` através do Active Directory Users and Computers, dentro do ambiente gerido por Active Directory.

---

## 2. Pré-requisitos

- Acesso a LAB-DC01
- Permissões de administrador ou delegação equivalente
- Estrutura de OUs definida

---

## 3. Processo

1. Abrir **Active Directory Users and Computers**
2. Navegar até:
   - `OU=01_Users`
3. Selecionar departamento correspondente:
   - Financeiro / Comercial / Direção
4. Clicar com botão direito → **New → User**

---

## 4. Configuração do Utilizador

- Nome completo
- Username (ex: f.silva)
- Password inicial
- Definir:
  - “User must change password at next logon”

---

## 5. Validação

- Confirmar criação no OU correto
- Testar login numa estação do domínio
- Verificar aplicação de GPOs

---

## 6. Problemas Comuns

- Utilizador criado na OU errada
- Password policy não cumprida
- Conta desativada por default policy

---

## 7. Resultado Esperado

Utilizador ativo, autenticável e sujeito a políticas de grupo.