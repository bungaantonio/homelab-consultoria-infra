# onboard-user.md

## 1. Objetivo

Padronizar o processo de entrada de novos utilizadores no ambiente `lan.homelab.ao`, integrado com Active Directory.

---

## 2. Pré-requisitos

- Utilizador criado no AD
- Grupo departamental definido
- Estação de trabalho disponível

---

## 3. Processo

### 3.1 Criação de identidade

- Criar utilizador no AD
- Atribuir grupo departamental

### 3.2 Acesso à estação

- Associar estação ao domínio
- Login inicial com credenciais do utilizador
- Aplicar GPO de drive mapping e confirmar acesso aos recursos departamentais

---

## 4. Configurações aplicadas

- GPO de utilizador
- Mapeamento de drives via GPO
- Configuração de desktop padrão

---

## 5. Validação

- Login bem-sucedido
- Aplicação de políticas de grupo
- Acesso a recursos departamentais

---

## 6. Problemas Comuns

- Utilizador não pertence a grupo correto
- Máquina fora de OU correta
- DNS incorreto

---

## 7. Resultado Esperado

Utilizador totalmente operacional no ambiente corporativo.