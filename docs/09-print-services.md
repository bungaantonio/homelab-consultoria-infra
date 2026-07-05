# 09 - Print Services

## 1. Visão Geral

Os serviços de impressão no ambiente **HomeLab Consultoria & Contabilidade** têm como objetivo centralizar, controlar e auditar o uso de impressoras na rede corporativa.

A gestão será baseada em servidor dedicado (PaperCut NG, em roadmap), integrado com o domínio gerido por Active Directory.

---

## 2. Arquitetura de Impressão

### Componentes previstos:

- Servidor de impressão: Windows Server (role Print Server)
- Sistema de gestão: PaperCut NG (planeado)
- Autenticação: Active Directory
- Distribuição: via GPO

---

## 3. Modelo de Funcionamento

### Fluxo básico:

1. Utilizador autentica no domínio
2. GPO aplica impressoras disponíveis
3. Job de impressão é enviado ao servidor
4. Servidor processa e encaminha para impressora física
5. (Opcional) registo e auditoria do trabalho

---

## 4. Estrutura de Impressoras

As impressoras serão organizadas por contexto organizacional:

- Impressora Financeiro
- Impressora Comercial
- Impressora Direção
- Impressora Geral (shared)

---

## 5. Mapeamento via GPO

As impressoras serão distribuídas automaticamente com base em:

- OU do utilizador (`01_Users`)
- Grupo de segurança (modelo AGDLP)
- Políticas de departamento

---

## 6. Controlo e Auditoria

O sistema permitirá:

- Monitorização de volume de impressão
- Identificação de utilizador por job
- Controlo de custos (simulado)
- Relatórios por departamento

---

## 7. Segurança

Medidas previstas:

- Autenticação obrigatória via domínio
- Restrição de impressoras por grupo
- Bloqueio de impressão não autorizada (políticas futuras)
- Logging de eventos de impressão

---

## 8. Limitações Atuais

- Sem servidor de impressão implementado
- Sem PaperCut NG ativo
- Sem impressoras físicas configuradas
- Sem políticas de quota de impressão

---

## 9. Evolução Prevista

- Implementação do Print Server em Windows Server
- Integração com PaperCut NG
- Deploy automático via GPO
- Relatórios de utilização por utilizador e departamento
- Implementação de políticas de quotas e restrições