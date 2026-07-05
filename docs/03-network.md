# 03 - Network

## 1. Visão Geral

A rede do ambiente **HomeLab Consultoria & Contabilidade** foi desenhada como uma infraestrutura simples e controlada, simulando uma pequena rede corporativa com serviços centralizados de autenticação e gestão.

A rede opera num único segmento:

- `10.0.10.0/24`

---

## 2. Topologia de Rede

### 2.1 Elementos principais

| Dispositivo | Endereço IP | Função |
|-------------|------------|--------|
| pfSense | 10.0.10.1 | Gateway, firewall e DHCP |
| LAB-DC01 | 10.0.10.10 | DNS, Active Directory Domain Controller |
| LAB-WS-001 | DHCP | Estação de trabalho |
| TrueNAS | 10.0.10.20 | Storage (planeado) |

---

## 3. Endereçamento IP

### 3.1 Esquema

A rede utiliza um esquema simples de endereçamento estático e dinâmico:

- `10.0.10.1` → Gateway (pfSense)
- `10.0.10.10` → Domain Controller (fixo)
- `10.0.10.20` → Storage (reservado)
- Clientes → DHCP dinâmico

---

## 4. DHCP (pfSense)

O serviço DHCP é gerido pelo pfSense.

### Funções:
- Atribuição automática de IPs a clientes
- Distribuição de DNS interno (DC)
- Garantia de gateway padrão

### Parâmetros principais:
- Range DHCP: dinâmico (clientes)
- DNS fornecido: 10.0.10.10
- Gateway: 10.0.10.1

---

## 5. DNS e Resolução de Nomes

A resolução de nomes segue modelo centralizado:

### Fluxo:
1. Cliente consulta DNS
2. Pedido chega ao Domain Controller
3. Resolução interna via zona `lan.homelab.ao`
4. Queries externas são encaminhadas para pfSense

---

## 6. Segmentação de Rede

Atualmente o ambiente utiliza:

- Uma única subnet (`10.0.10.0/24`)
- Sem VLANs implementadas

### Justificação:
- Simplicidade operacional
- Ambiente de laboratório
- Foco em serviços de identidade e GPO

---

## 7. Segurança de Rede

Medidas implementadas:

- Firewall ativo no pfSense
- Acesso externo bloqueado por defeito
- DNS interno obrigatório para clientes de domínio
- Controlo de tráfego centralizado no gateway

---

## 8. Comunicação Interna

### Fluxo típico:

- Cliente → DC (autenticação)
- Cliente → DC (DNS interno)
- Cliente → pfSense (internet)
- DC → pfSense (forward de DNS externo)

---

## 9. Limitações Atuais

- Sem VLAN segmentation
- Sem IDS/IPS ativo
- Sem redundância de gateway
- Apenas um ponto de falha (pfSense + DC)

---

## 10. Evolução Prevista

- Implementação de VLANs (Users / Servers / Management)
- Introdução de IDS/IPS no firewall
- Redundância de DNS/Domain Controller
- Segmentação de tráfego por função