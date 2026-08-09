# Changelog

Todas as alterações estruturais e de planeamento da infraestrutura corporativa **HomeLab Consultoria & Contabilidade** serão documentadas neste ficheiro a partir da versão v0.2.0.

*Nota: O registo de alterações da versão descontinuada v0.1.0 encontra-se arquivado em [Changelog v0.1.0](versions/v0.1.0/CHANGELOG.md).*

---

## [v0.2.0] - 2026-07-19

### Adicionado
- **Reorganização do Repositório:**
  - Criação do diretório de arquivo `versions/v0.1.0/` e movimentação de todos os ficheiros antigos da v0.1.0 para lá.
  - Adicionado aviso de desativação de ambiente em `versions/v0.1.0/README.md`.
- **Desenho de Rede & IPAM:**
  - Redesenho da topologia de rede com segmentação baseada em 5 VLANs (10 - MGMT, 20 - SERVERS, 30 - CLIENTS, 40 - STORAGE, 50 - GUEST).
  - Criação do plano IPAM detalhado em `docs/03-network/ipam.md` e `inventory/ipam.md`.
  - Documentação das interfaces e regras do pfSense em `docs/03-network/routing-firewall.md`.
- **Padrões de TI:**
  - Definição do padrão de nomenclatura de hosts (`[ORG]-[LOCAL]-[ENV]-[ROLE][INDEX]`) e de objetos do Active Directory (AGDLP) em `docs/standards/naming-convention.md`.
  - Criação do inventário ativo de ativos físicos e lógicos em `inventory/assets.md`.
- **Identidade e Segurança:**
  - Desenho lógico do Active Directory (`corp.homelab.ao`) em `docs/02-identity/active-directory.md` com a árvore de OUs atualizada.
  - Definição das políticas de segurança, isolamento de rede, bloqueio de portas USB via GPO, encriptação BitLocker e LAPS em `docs/05-security/security-policies.md`.
- **Armazenamento e Serviços:**
  - Desenho do armazenamento baseado em TrueNAS SCALE com ZFS RAIDZ2 e matriz de permissões associadas a grupos Domain Local em `docs/04-storage/storage-design.md`.
  - Planeamento de serviços base (DNS, DHCP, NTP e PKI/LDAPS) em `docs/06-services/infrastructure.md`.
  - Desenho do servidor de backup Veeam fora do domínio, monitorização via Zabbix/Prometheus e coletor de logs Syslog em `docs/06-services/corporate.md`.
- **Procedimentos Operacionais Padrão (SOPs):**
  - Criação dos procedimentos de criação de utilizadores e grupos no Active Directory.
  - Criação de procedimentos detalhados de Onboarding e Offboarding de colaboradores.
  - Criação do procedimento de backup do pfSense XML.
  - Criação do procedimento de integração (Join) do TrueNAS SCALE ao domínio.
