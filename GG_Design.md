# GG Design - Global Groups

## Definição de Global Groups

Global Groups são grupos de segurança com scope de domínio que podem conter utilizadores e outros grupos globais do mesmo domínio. São utilizados para organizar utilizadores por função ou departamento dentro de um único domínio.

No modelo AGDLP (Accounts → Global Groups → Domain Local Groups → Permissions), os grupos globais representam a camada de identidade funcional.

## Estrutura dos Grupos por Departamento

```
GG-IT
GG-Financeiro
GG-Comercial
GG-Direcao
```

### GG-IT

Contém utilizadores do departamento de TI. Responsáveis pela administração de sistemas, infraestrutura e suporte técnico.

### GG-Financeiro

Contém utilizadores do departamento financeiro. Responsáveis pela gestão contabil, fiscal e financeira da empresa.

### GG-Comercial

Contém utilizadores do departamento comercial. Responsáveis pela relação com clientes, vendas e negociação.

### GG-Direcao

Contém utilizadores da direção. Responsáveis pela gestão estratégica, decisão e supervisão da empresa.

## Mapeamento Utilizador → Grupo

Cada utilizador deve ser membro do grupo global correspondente ao seu departamento:

```
Utilizador TI → GG-IT
Utilizador Financeiro → GG-Financeiro
Utilizador Comercial → GG-Comercial
Utilizador Direcao → GG-Direcao
```

Um utilizador pode pertencer a múltiplos grupos globais se tiver funções cruzadas, mas a prática recomendada é manter associação única para simplificar gestão.

## Regras de Design de GG

### Nomenclatura

- Prefixo GG- para identificar grupos globais
- Nome do departamento ou função após prefixo
- Consistência em todo o domínio
- Evitar caracteres especiais ou espaços

### Granularidade

- Grupos devem representar funções de negócio, não recursos
- Evitar grupos por aplicação ou sistema específico
- Manter número de grupos gerenciável
- Agrupar por departamento ou função lógica

### Função

- GG representam quem é o utilizador na organização
- GG são base para grupos de permissão (DL)
- GG não devem ter permissões diretas em recursos
- GG são utilizados apenas para agrupamento funcional

## Papel dos GG no Modelo AGDLP

No modelo AGDLP, os grupos globais (G) são a camada intermédia entre contas de utilizador (A) e grupos de permissão (DL):

```
A (Accounts) → G (Global Groups) → DL (Domain Local Groups) → P (Permissions)
```

Os GG permitem:

- Desacoplamento entre identidade e recurso
- Facilidade em alterar permissões sem modificar grupos de utilizadores
- Gestão centralizada de identidade funcional
- Escalabilidade em ambientes multi-domínio

## Limitações dos GG

Grupos globais não devem ser utilizados para:

- Permissões diretas em recursos (ficheiros, impressoras, shares)
- Controlo de acesso a recursos específicos
- Autorização em recursos de outros domínios (em florestas multi-domínio)

Permissões devem ser aplicadas através de grupos Domain Local (DL), que recebem os grupos globais como membros.

## Exemplos de Associação de Utilizadores a GG

### Associação Básica

```
joao.silva@lan.homelab.ao → GG-IT
maria.santos@lan.homelab.ao → GG-Financeiro
pedro.ferreira@lan.homelab.ao → GG-Comercial
ana.carvalho@lan.homelab.ao → GG-Direcao
```

### Associação Múltipla (caso especial)

```
joao.silva@lan.homelab.ao → GG-IT
joao.silva@lan.homelab.ao → GG-Financeiro (se tiver função cruzada)
```

Nota: Associação múltipla deve ser exceção, não regra, para manter simplicidade.
