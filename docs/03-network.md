# 03 - Rede e Topologia

A rede do laboratório foi desenhada para demonstrar uma topologia corporativa simples, com segmentação de tráfego e controlo de acesso.

## Segmentação atual

- **VLAN 10 / MGMT**: gestão da infraestrutura.
- **VLAN 20 / SERVERS**: serviços de domínio e infraestruturas comuns.
- **VLAN 30 / CLIENTS**: estações de trabalho de utilizadores.
- **VLAN 40 / STORAGE**: planeada para armazenamento dedicado.
- **VLAN 50 / GUEST**: planeada para acesso isolado e testes.

## Componentes principais

- **pfSense** como gateway e firewall.
- **Domain Controller** em `10.0.20.10`.
- **DHCP** para clientes da VLAN 30.

## Referências técnicas

- [docs/03-network/ipam.md](03-network/ipam.md)
- [docs/network/routing-firewall.md](network/routing-firewall.md)
- [docs/02-architecture.md](02-architecture.md)
