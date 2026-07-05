# 10 - Backup

## 1. Visão Geral

A estratégia de backup no ambiente **HomeLab Consultoria & Contabilidade** visa garantir a proteção de dados críticos, consistência do sistema e recuperação em caso de falhas operacionais.

A infraestrutura atual é baseada em serviços centralizados de armazenamento e identidade através de Active Directory, com plano de integração futura com storage dedicado (TrueNAS).

---

## 2. Objetivos do Backup

- Garantir recuperação de dados em caso de falha
- Proteger dados de utilizadores e departamentos
- Reduzir impacto de erro humano
- Suportar continuidade operacional do ambiente
- Permitir rollback de configurações críticas

---

## 3. Escopo de Backup

### 3.1 Dados incluídos

- Perfis de utilizador (dados em file server)
- Partilhas departamentais
- Configurações de servidor
- Dados de aplicações internas

---

### 3.2 Dados excluídos (temporariamente)

- Estações de trabalho locais (não persistentes)
- Ambientes de teste
- Logs não críticos

---

## 4. Estratégia de Backup

### 4.1 Modelo atual (conceitual)

- Sem backup automatizado implementado
- Preparação para integração com TrueNAS snapshots
- Estrutura preparada para backup centralizado

---

### 4.2 Modelo futuro (planeado)

- Snapshots automáticos no storage
- Backup incremental diário
- Backup completo semanal
- Retenção por política (curto/médio prazo)

---

## 5. Tipos de Backup

- Full backup (completo)
- Incremental backup
- Snapshot-based backup (planeado)
- Configuration backup (servidores)

---

## 6. Recuperação (Restore)

### Cenários previstos:

- Recuperação de ficheiros individuais
- Restauro de partilhas completas
- Recuperação de configuração de servidor
- Rollback de alterações críticas no sistema

---

## 7. Segurança do Backup

- Acesso restrito por grupo administrativo
- Isolamento do storage de backup
- Proteção contra eliminação acidental
- Controlo de permissões baseado em domínio

---

## 8. Limitações Atuais

- Sem sistema de backup ativo
- Sem snapshots configurados
- Sem storage dedicado implementado
- Sem automação de retenção

---

## 9. Evolução Prevista

- Implementação de TrueNAS como backend de backup
- Snapshots automáticos por dataset
- Replicação para storage secundário (futuro)
- Integração com políticas de retenção corporativa
- Testes periódicos de recuperação (DR drills)