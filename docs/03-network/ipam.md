# Plano IPAM

## 1. Objetivo

Este documento define a estratégia de endereçamento para o laboratório **HomeLab Consultoria & Contabilidade**. O objetivo é manter a rede previsível, documentada e simples de administrar, sem perder a capacidade de crescer com novas funções e serviços.

## 2. Esquema de rede

- Rede base: `10.0.10.0/24`
- Gateway principal: `10.0.10.1`
- DNS interno: `10.0.10.10`
- Domínio principal: `lan.homelab.ao`

## 3. Política de atribuição

A política de endereçamento segue três princípios:

- Endereços fixos para serviços críticos e infraestrutura.
- Endereços dinâmicos para estações de utilizador e dispositivos temporários.
- Espaço reservado para expansão futura sem necessidade de reconfigurar a estrutura base.

## 4. Atribuições principais

| IP | Tipo | Função |
| --- | --- | --- |
| `10.0.10.1` | Estático | pfSense, gateway, firewall e DHCP |
| `10.0.10.10` | Estático | Domain Controller, DNS e AD |
| `10.0.10.20` | Estático | TrueNAS, servidor de ficheiros |
| `10.0.10.21` | Estático | Servidor de impressão (futuro) |
| `10.0.10.22` | Estático | Servidor de backup (futuro) |
| `10.0.10.30` a `10.0.10.99` | Estático reservado | Infraestrutura adicional, serviços especializados e dispositivos de gestão |
| `10.0.10.100` a `10.0.10.199` | DHCP | Estações de trabalho, laptops e dispositivos clientes |
| `10.0.10.200` a `10.0.10.254` | Reservado | Crescimento futuro, testes e novas funções |

## 5. Regras de utilização

- Os serviços críticos devem manter endereços fixos e estáveis.
- As estações de utilizador devem usar DHCP para simplificar a gestão diária.
- Qualquer novo dispositivo que precise de um papel operacional deve receber um endereço fixo e ser registado aqui.
- O espaço reservado deve ser usado apenas com aprovação e documentação.

## 6. Considerações de operação

Este plano suporta o modelo do laboratório em três áreas principais:

- Identidade e autenticação: o Domain Controller fica em `10.0.10.10`.
- Armazenamento e partilhas: o TrueNAS fica em `10.0.10.20`.
- Utilizadores finais: recebem endereços por DHCP para manter a experiência mais realista.

## 7. Evolução futura

Quando o laboratório evoluir para uma estrutura mais próxima de uma rede corporativa, este plano pode ser expandido para incluir:

- VLANs para utilizadores, servidores e gestão;
- atribuição estática mais detalhada para impressoras e servidores;
- documentação de reservas por departamento ou função.
