# GG Design - Global Groups

## Visão Geral

Global Groups são grupos de segurança com scope de domínio usados para representar identidade funcional dentro do domínio `lan.homelab.ao`.

No modelo AGDLP, os GG respondem à pergunta: quem é o utilizador?

---

## Objetivo

Os GG servem para:

- Agrupar utilizadores por função ou departamento
- Manter identidade separada de autorização
- Ser a base para associação com Domain Local Groups
- Permitir gestão simples e escalável de membros

---

## Arquitetura

Os grupos globais devem ficar organizados em `00_Admin > Groups > Global`.

```text
00_Admin
└── Groups
	└── Global
		├── GG-Comercial
		├── GG-Financeiro
		├── GG-Direcao
		├── GG-IT
		└── GG-Domain-Admins
```

### Regras de design

- GG representam pessoas, funções e departamentos
- GG não representam recursos
- GG não recebem permissões de share ou NTFS diretamente
- GG não devem misturar identidade com autorização

---

## Pré-requisitos

- Estrutura de OUs definida
- Convenção de nomes aprovada
- Separação entre grupos globais e Domain Local

---

## Implementação

### Exemplos de grupos

- `GG-IT`
- `GG-Financeiro`
- `GG-Comercial`
- `GG-Direcao`
- `GG-Domain-Admins`

### Mapeamento de utilizador para grupo

```text
joao.silva@lan.homelab.ao → GG-IT
maria.santos@lan.homelab.ao → GG-Financeiro
pedro.ferreira@lan.homelab.ao → GG-Comercial
ana.carvalho@lan.homelab.ao → GG-Direcao
```

### Casos especiais

Utilizadores com funções cruzadas podem pertencer a mais do que um GG, mas isso deve ser exceção.

---

## Validação / Testes

Validar:

- Membros corretos em cada GG
- Nome consistente com a função do grupo
- Ausência de grupos de recurso dentro da camada GG

---

## Boas Práticas

- Usar prefixo `GG-` para todos os grupos globais
- Manter nomes curtos e sem caracteres especiais
- Documentar a finalidade de cada grupo
- Evitar grupos excessivamente granulares

---

## Limitações Atuais

- Ainda existem documentos legados com nomes antigos de grupos
- A normalização completa depende da revisão dos procedimentos operacionais

---

## Evolução Futura

- Expandir o modelo para grupos de função mais granulares quando necessário
- Criar grupos administrativos separados para operações privilegiadas
- Rever periodicamente membros e permissões associadas
