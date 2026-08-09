# Serviços Corporativos de Operação

## 1. Objetivo

Este documento descreve os serviços auxiliares que suportam a operação contínua do laboratório, incluindo backup, monitorização e logging.

## 2. Serviços previstos

- **Backup**: Veeam Backup para recuperação de dados e proteção de partilhas sensíveis.
- **Monitorização**: Zabbix ou Prometheus para observabilidade de recursos e saúde de serviços.
- **Syslog / centralização de logs**: suporte a auditoria e deteção de anomalias.
- **Gestão de mudanças**: documentação e validação das alterações críticas na infraestrutura.

## 3. Regras de operação

- Todos os serviços auxiliares devem ser documentados antes de serem implementados em produção.
- Alterações críticas devem ser testadas e registadas.
- A auditoria e a retenção de logs devem ser tratadas como parte essencial da segurança do laboratório.
