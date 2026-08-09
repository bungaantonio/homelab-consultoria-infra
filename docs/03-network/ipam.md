# Plano IPAM

## 1. Objetivo

Este documento define a estratégia de endereçamento para o laboratório **HomeLab Consultoria & Contabilidade**. O objetivo é manter a rede previsível, documentada e simples de administrar, sem perder a capacidade de crescer com novas funções e serviços.

## 2. Esquema de rede atual

- Domínio principal: `corp.homelab.ao`
- Rede base da infraestrutura atual: `10.0.10.0/24` para a rede de gestão inicial, com evolução para segmentação por VLANs
- Gateway principal: `10.0.10.1`
- DNS interno: `10.0.20.10` (Domain Controller)

## 3. Segmentação por VLANs

A infraestrutura atual segue a seguinte lógica:

| VLAN | Nome | Rede | Estado | Função |
| --- | --- | --- | --- | --- |
| `10` | `MGMT` | `10.0.10.0/24` | Implementado | Gestão de infraestruturas e dispositivos de administração |
| `20` | `SERVERS` | `10.0.20.0/24` | Implementado | Serviços de domínio, DNS, DHCP e infraestrutura |
| `30` | `CLIENTS` | `10.0.30.0/24` | Implementado | Estações de trabalho e utilizadores |
| `40` | `STORAGE` | `10.0.40.0/24` | Planeado | TrueNAS SCALE e tráfego de ficheiros |
| `50` | `GUEST` | `10.0.50.0/24` | Planeado | Acesso isolado para convidados e testes |

## 4. Atribuições principais

| IP | Tipo | Função |
| --- | --- | --- |
| `10.0.10.1` | Estático | pfSense, gateway e firewall |
| `10.0.20.10` | Estático | Domain Controller, DNS e AD |
| `10.0.20.20` | Estático | Servidor de monitorização (futuro) |
| `10.0.20.30` | Estático | Servidor de backup (futuro) |
| `10.0.30.100` a `10.0.30.200` | DHCP | Estações de trabalho e clientes |
| `10.0.40.10` | Estático | TrueNAS SCALE (planeado) |

## 5. Regras de utilização

- Os serviços críticos devem manter endereços fixos e estáveis.
- As estações de utilizador devem usar DHCP para simplificar a gestão diária.
- Qualquer novo dispositivo com papel operacional deve receber um endereço fixo e ser registado aqui.
- O espaço reservado deve ser usado apenas com aprovação e documentação.

## 6. Considerações de operação

Este plano suporta o modelo do laboratório em três áreas principais:

- Identidade e autenticação: o Domain Controller fica em `10.0.20.10`.
- Armazenamento e partilhas: o TrueNAS fica em `10.0.40.10` quando integrado.
- Utilizadores finais: recebem endereços por DHCP para manter a experiência mais realista.

## 7. Evolução futura

Quando o laboratório evoluir para uma estrutura mais próxima de uma rede corporativa, este plano pode ser expandido para incluir:

- regras de filtragem inter-VLAN mais rígidas;
- atribuição estática mais detalhada para impressoras e servidores;
- documentação de reservas por departamento ou função.
