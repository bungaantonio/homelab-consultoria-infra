# 05 - DNS

## 1. Visão Geral

O serviço de DNS no ambiente **HomeLab Consultoria & Contabilidade** é um componente crítico da infraestrutura de rede e identidade.

Está integrado diretamente com o serviço de domínio Active Directory, garantindo resolução de nomes interna e localização de serviços do domínio.

---

## 2. Arquitetura de DNS

A resolução de nomes segue um modelo centralizado:

- Servidor DNS principal: LAB-DC01
- Zona principal: `lan.homelab.ao`
- Forwarders: configurados no DNS Manager do Windows Server (resolução externa)

---

## 3. Funções do DNS

O DNS no ambiente suporta:

- Resolução de nomes internos do domínio
- Localização de serviços do Active Directory
- Resolução de nomes externos via forwarders
- Suporte a autenticação de clientes no domínio

---

## 4. Integração com Active Directory

O DNS está totalmente integrado ao Active Directory:

- Criação automática de registos SRV
- Localização de Domain Controllers
- Descoberta de serviços do domínio
- Atualizações dinâmicas de registos de host

---

## 5. Fluxo de Resolução de Nomes

### 5.1 Nomes internos

1. Cliente consulta DNS
2. Pedido chega ao LAB-DC01
3. Resolução na zona `lan.homelab.ao`
4. Resposta devolvida ao cliente

---

### 5.2 Nomes externos

1. Cliente consulta domínio externo
2. DNS interno não resolve
3. Pedido é encaminhado para os forwarders configurados no DNS Manager
4. Forwarders resolvem via internet

---

## 6. Configuração de Clientes

Todos os clientes do domínio utilizam:

- DNS primário: 10.0.10.10 (Domain Controller)
- DNS secundário: não configurado (evita bypass do domínio)

---

## 7. Registos DNS

O ambiente utiliza os seguintes tipos de registos:

- A records (hosts internos)
- SRV records (serviços do domínio)
- CNAME (aliases internos quando aplicável)
- PTR (reverse lookup dentro da sub-rede)

---

## 8. Segurança DNS

Medidas implementadas:

- DNS interno obrigatório para clientes do domínio
- Bloqueio de resolução externa direta nos endpoints
- Controlo centralizado via Domain Controller
- Encaminhamento externo via forwarders configurados no DNS Manager

---

## 9. Limitações Atuais

- Sem DNS redundancy (apenas um servidor)
- Sem DNSSEC implementado
- Sem split-brain DNS (não necessário neste estágio)
- Dependência direta do Domain Controller

---

## 10. Evolução Prevista

- Implementação de segundo DNS (redundância)
- Hardening de DNS (DNS policies)
- Logging avançado de queries
- Integração com monitorização centralizada