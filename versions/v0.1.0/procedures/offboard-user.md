# offboard-user.md

## 1. Objetivo

Remover ou desativar utilizadores no domínio `lan.homelab.ao`, garantindo controlo de acesso e segurança no ambiente gerido por Active Directory.

---

## 2. Tipos de Offboarding

- Desativação temporária
- Remoção definitiva
- Arquivamento de conta

---

## 3. Processo

1. Abrir Active Directory Users and Computers
2. Localizar utilizador
3. Escolher ação:
   - Disable Account
   - Move para OU de desativados (se existir)
   - Remover grupos associados

---

## 4. Limpeza de acessos

- Remover acessos a partilhas
- Revogar permissões em grupos
- Invalidar sessões ativas

---

## 5. Validação

- Conta não consegue autenticar
- Remoção de acesso a recursos
- Auditoria de logs (opcional)

---

## 6. Problemas Comuns

- Conta apenas desativada mas ainda com permissões herdadas
- Falha na remoção de grupos
- Acesso persistente via cache

---

## 7. Resultado Esperado

Utilizador sem qualquer acesso ativo ao ambiente.