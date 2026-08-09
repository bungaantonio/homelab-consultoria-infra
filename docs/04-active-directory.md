# 04 - Active Directory e Autorização

O domínio atual do laboratório é **`corp.homelab.ao`**. A sua estrutura lógica foi pensada para refletir uma organização pequena com separação entre administração, utilizadores comuns, servidores e contas de serviço.

## Estrutura lógica

A organização do domínio é baseada em OUs para separar:

- administração;
- utilizadores por departamento;
- computadores e estações;
- servidores;
- contas de serviço.

## Modelo de autorização

A lógica de acesso segue o modelo **AGDLP**:

- **GG** para grupos globais e funções de negócio;
- **DL** para permissões sobre recursos;
- **ACLs e permissões** sobre partilhas e pastas.

## Referências técnicas

- [docs/identity/active-directory.md](identity/active-directory.md)
- [procedures/domain-management/create-user.md](../procedures/domain-management/create-user.md)
- [procedures/domain-management/create-group.md](../procedures/domain-management/create-group.md)
