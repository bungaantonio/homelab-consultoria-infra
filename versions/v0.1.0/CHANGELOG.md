```markdown
# Changelog

Todas as alterações notáveis a este laboratório serão documentadas neste ficheiro.

## [v0.1.0] - 2026-07-05
### Adicionado
- **Documentação:** Criação da estrutura base do repositório (`README.md`, `CHANGELOG.md`, diretórios `docs/`, `procedures/`, `scripts/`).
- **Rede:** Configuração da sub-rede `10.0.10.0/24` no pfSense.
- **Rede:** Configuração do servidor DHCP no pfSense com entrega de DNS apontando para o DC local (`10.0.10.10`).
- **Servidor:** Provisionamento da VM Windows Server 2025 (`LAB-DC01`).
- **Active Directory:** Promoção do Windows Server a Domain Controller (Domínio: `lan.homelab.ao`).
- **Active Directory:** Criação da estrutura de base (OUs: Empresa_Utilizadores, Empresa_Computadores, Empresa_Grupos, etc.).
- **Active Directory:** Provisionamento de 5 contas de utilizador de acordo com o caso de negócio da "HomeLab Consultoria".
- **DNS:** Configuração do DNS interno no DC com Forwarders ativos para a internet.
- **Endpoint:** Provisionamento e associação da VM Windows 10 (`LAB-WS-001`) ao domínio. Autenticação de domínio validada com sucesso.
```
