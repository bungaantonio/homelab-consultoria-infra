# Infraestrutura de Serviços Base

## 1. Objetivo

Este documento consolida a visão dos serviços fundamentais de rede e identidade que suportam a infraestrutura **HomeLab Consultoria & Contabilidade**.

## 2. Serviços base

- **DNS interno**: fornecido pelo Domain Controller para resolução de nomes do domínio principal.
- **DHCP**: serviço gerido pelo pfSense para atribuição dinâmica de endereços a clientes e dispositivos da rede.
- **NTP**: sincronização temporal essencial para autenticação Kerberos e integridade de logs.
- **PKI / certificação**: planeada para evolução futura, permitindo autenticação mais robusta e TLS/LDAPS.

## 3. Regras operacionais

- Todos os clientes devem usar o DNS interno do domínio.
- A configuração de horário deve ser consistente entre servidores e clientes.
- Os serviços base devem permanecer estáveis para garantir continuidade da autenticação e da gestão.
