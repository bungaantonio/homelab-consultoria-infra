# 06 - DHCP

## 1. Visão Geral

O serviço de DHCP no ambiente **HomeLab Consultoria & Contabilidade** é responsável pela atribuição automática de endereços IP aos dispositivos da rede.

É gerido diretamente pelo firewall pfSense e opera em conjunto com o serviço de DNS integrado ao domínio Active Directory.

---

## 2. Arquitetura do Serviço

- Servidor DHCP: pfSense
- Rede: `10.0.10.0/24`
- Escopo: dispositivos clientes (workstations e equipamentos não estáticos)

---

## 3. Parâmetros de Configuração

Os clientes recebem automaticamente:

- Endereço IP: dinâmico dentro do range `10.0.10.100 - 10.0.10.200`
- Máscara de rede: `255.255.255.0`
- Gateway padrão: `10.0.10.1` (pfSense)
- DNS primário: `10.0.10.10` (Domain Controller)
- DNS secundário: não configurado

---

## 4. Funcionamento do DHCP

### Fluxo básico:

1. Cliente entra na rede
2. Envia DHCP Discover
3. pfSense responde com oferta de IP
4. Cliente aceita configuração
5. Lease é atribuído

---

## 5. Integração com Active Directory

O DHCP suporta o funcionamento do domínio através de:

- Distribuição do DNS correto (DC)
- Garantia de resolução de nomes interna
- Suporte à autenticação no domínio
- Evita configuração manual de rede nos endpoints

---

## 6. Reservas de IP

Alguns dispositivos possuem IP fixo ou reservado:

- LAB-DC01 → `10.0.10.10`
- TrueNAS → `10.0.10.20` (reservado)
- pfSense → `10.0.10.1`

---

## 7. Escopo DHCP

O escopo atual cobre:

- Estações de trabalho
- Equipamentos de teste
- Dispositivos não críticos

---

## 8. Segurança DHCP

Medidas implementadas:

- DHCP centralizado no firewall
- Sem servidores DHCP adicionais na rede
- Controle de gateway obrigatório
- DNS forçado para servidor interno

---

## 9. Limitações Atuais

- Sem DHCP failover
- Sem segmentação por VLAN
- Sem políticas DHCP avançadas (ex: opções por grupo)
- Dependência única do pfSense

---

## 10. Evolução Prevista

- Implementação de DHCP failover
- Segmentação por VLAN (scopes separados)
- Integração com políticas de rede mais avançadas
- Monitorização de leases e deteção de dispositivos desconhecidos