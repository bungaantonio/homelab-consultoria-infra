# 10 - GPO_Drive_Mapping

## 1. Visão Geral

Esta GPO automatiza o mapeamento de unidades de rede para os utilizadores do domínio `lan.homelab.ao`, com base em grupos globais e nos recursos disponibilizados pelo File Server.

---

## 2. Objetivo

Distribuir drives de forma consistente por departamento, sem recorrer a permissões diretas em utilizadores e sem usar GPOs como substituto de ACLs de recursos.

---

## 3. Arquitetura

### 3.1 Princípio de funcionamento

- O File Server fornece a partilha
- O AD identifica o utilizador
- O GG define a função do utilizador
- O DL define a autorização no recurso
- A GPO automatiza a entrega do drive

### 3.2 Fluxo de entrega

```text
Account
↓
GG
↓
DL
↓
Permissions no File Server
↓
Drive Map via GPO
```

---

## 4. Pré-requisitos

- File Server funcional
- Partilhas criadas
- Permissões NTFS e SMB configuradas
- Grupos `GG-*` e `DL-*` existentes
- Testes SMB concluídos
- Utilizadores já distribuídos pelos grupos corretos

---

## 5. Implementação

### 5.1 Localização da configuração

Usar:

`User Configuration → Preferences → Windows Settings → Drive Maps`

### 5.2 Configuração base

- Security Filtering: `Authenticated Users`
- Item-Level Targeting: `GG-*`
- Atribuição por departamento
- Criar mapeamento por letra de unidade

### 5.3 Exemplo de mapeamentos

#### Drive F:

- Path: `\\truenas\Financeiro`
- Target: `GG-Financeiro`

#### Drive S:

- Path: `\\truenas\Comercial`
- Target: `GG-Comercial`

#### Drive X:

- Path: `\\truenas\Direcao`
- Target: `GG-Direcao`

### 5.4 Regras de desenho

- Usar letras consistentes entre ambientes
- Não mapear drives sem recurso real atrás
- Não usar a GPO para compensar permissões erradas no File Server
- Garantir que cada mapeamento corresponde a um DL válido

---

## 6. Validação / Testes

Testar com um utilizador por departamento:

- Confirmar que o drive aparece no logon
- Confirmar que o caminho abre corretamente
- Confirmar que o acesso é negado quando o utilizador não pertence ao grupo alvo
- Confirmar que o drive desaparece quando o grupo é removido

---

## 7. Boas Práticas

- Manter a GPO apenas para automação de acesso
- Evitar lógica de autorização dentro da GPO
- Preferir Item-Level Targeting para granularidade
- Testar primeiro na OU `Test`

---

## 8. Limitações Atuais

- Dependência total de File Services operacionais
- Dependência de grupos e permissões corretamente configurados
- Não substitui o modelo AGDLP

---

## 9. Evolução Futura

- Expandir para Home Drives, se aplicável
- Adicionar redirecionamento de pastas, se necessário
- Integrar remediação automática de drives inconsistentes