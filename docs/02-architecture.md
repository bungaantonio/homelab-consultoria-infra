# 02 - Arquitetura Geral

A arquitetura atual do laboratório foi pensada para ser simples, legível e suficientemente realista para demonstrar o funcionamento de uma infraestrutura corporativa pequena.

## Componentes principais

- **pfSense** para gateway, firewall e roteamento básico;
- **Windows Server / AD DS** para identidade, DNS e autenticação;
- **TrueNAS SCALE** como proposta de servidor de ficheiros e armazenamento;
- **GPOs** para aplicar políticas de configuração e segurança.

## Estado atual

A versão atual do laboratório está organizada em torno de:

- rede segmentada por VLANs;
- domínio **`corp.homelab.ao`**;
- estrutura de OUs, grupos e regras de acesso;
- políticas de segurança baseadas em GPO.

## Referências detalhadas

- [docs/architecture/index.md](architecture/index.md)
- [docs/03-network.md](03-network.md)
- [docs/04-active-directory.md](04-active-directory.md)
- [docs/storage/storage-design.md](storage/storage-design.md)
