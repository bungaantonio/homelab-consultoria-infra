# 12 - Troubleshooting

## 1. Visão Geral

Este documento descreve abordagens de diagnóstico e resolução de problemas no ambiente **HomeLab Consultoria & Contabilidade**, baseado em infraestrutura centralizada em Active Directory e serviços de rede associados.

O objetivo é garantir resolução estruturada de falhas em autenticação, rede, DNS, DHCP e serviços corporativos.

---

## 2. Princípios de Diagnóstico

- Isolar o problema por camada (rede, identidade, aplicação)
- Validar dependências (DNS → AD → autenticação)
- Reproduzir o problema antes de alterar configurações
- Alterar uma variável de cada vez
- Documentar todas as alterações feitas

---

## 3. Áreas de Falha Mais Comuns

### 3.1 Autenticação no domínio

Sintomas:
- Login falha em estações
- Credenciais rejeitadas

Causas típicas:
- DNS incorreto
- Falha de comunicação com Domain Controller
- Conta bloqueada ou expirada

---

### 3.2 DNS

Sintomas:
- Sites internos não resolvem
- Falha em localizar domínio

Causas típicas:
- Cliente não usa DNS do DC
- Forwarders mal configurados
- Registos ausentes

---

### 3.3 DHCP

Sintomas:
- Sem IP atribuído
- IP incorreto ou fora da rede

Causas típicas:
- Serviço DHCP parado no pfSense
- Conflito de DHCP na rede
- Scope mal configurado

---

### 3.4 GPO

Sintomas:
- Políticas não aplicadas
- Configuração inconsistente entre utilizadores

Causas típicas:
- OU incorreta
- Filtragem por grupo
- Replicação não aplicada (em ambientes maiores)

---

### 3.5 Acesso a ficheiros

Sintomas:
- Acesso negado a partilhas
- Drives de rede não mapeadas

Causas típicas:
- Permissões NTFS incorretas
- Falta de associação a grupos
- Falha no servidor de ficheiros

---

## 4. Ferramentas de Diagnóstico

- `ping` (conectividade básica)
- `nslookup` (DNS)
- `ipconfig /all` (configuração de rede)
- `gpresult /r` (verificação de GPO)
- Event Viewer (logs do sistema)
- Active Directory Users and Computers (verificação de objetos)

---

## 5. Fluxo de Resolução (Metodologia)

### 1. Identificação
- Definir sintoma exato

### 2. Isolamento
- Rede / DNS / AD / aplicação

### 3. Validação
- Testar conectividade e resolução

### 4. Correção
- Ajuste mínimo e controlado

### 5. Verificação
- Confirmar resolução do problema

---

## 6. Problemas Críticos do Ambiente

- Dependência de um único Domain Controller
- DNS centralizado sem redundância
- DHCP único no pfSense
- Falta de monitorização centralizada

---

## 7. Boas Práticas

- Nunca alterar múltiplos serviços ao mesmo tempo
- Validar DNS antes de diagnosticar AD
- Priorizar logs antes de reiniciar serviços
- Documentar mudanças de configuração
- Evitar alterações diretas em produção sem teste

---

## 8. Evolução Prevista

- Centralização de logs (SIEM)
- Monitorização de infraestrutura em tempo real
- Alertas automáticos de falhas de serviço
- Runbooks automatizados para incidentes comuns
- Implementação de ferramentas de observabilidade