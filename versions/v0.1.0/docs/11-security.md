# 11 - Security

## 1. Visão Geral

A segurança no ambiente **HomeLab Consultoria & Contabilidade** é baseada em controlo centralizado de identidade, segmentação lógica de permissões e aplicação de políticas via domínio gerido por Active Directory.

O objetivo é simular um ambiente corporativo com dados sensíveis e exigência de controlo rigoroso de acesso.

---

## 2. Princípios de Segurança

O modelo de segurança segue os seguintes princípios:

- Princípio do menor privilégio
- Separação de contas administrativas e normais
- Controlo centralizado de identidade
- Auditoria de ações críticas
- Redução de superfície de ataque

---

## 3. Controlo de Identidade

### 3.1 Utilizadores e grupos

- Utilizadores organizados por departamento
- Acesso concedido via grupos de segurança
- Modelo baseado em AGDLP

### 3.2 Contas privilegiadas

- Separação em OU própria (`00_Admin`)
- Restrições de login em estações de trabalho
- Uso exclusivo para administração do domínio

---

## 4. Políticas de Segurança (GPO)

As GPOs são o principal mecanismo de enforcement de segurança:

- Hardening de estações de trabalho
- Bloqueio de dispositivos externos (planeado)
- Configuração de firewall local
- Políticas de password e bloqueio de conta
- Auditoria de eventos de login

---

## 5. Segurança de Rede

- Segmentação lógica em subnet única (`10.0.10.0/24`)
- Firewall centralizado no pfSense
- DNS interno obrigatório via Domain Controller
- Bloqueio de resolução externa direta nos clientes

---

## 6. Segurança de Servidores

- Hardening de sistemas Windows Server
- Acesso remoto restrito (RDP controlado)
- Auditoria avançada de eventos
- Separação de funções por tipo de servidor

---

## 7. Auditoria e Logging

- Registo de logins e falhas de autenticação
- Monitorização de alterações no Active Directory
- Auditoria de acesso a ficheiros (planeado)
- Logs de rede centralizados (futuro)

---

## 8. Controlo de Acesso a Recursos

- Permissões baseadas em grupos
- Evitar permissões diretas a utilizadores
- Uso de modelo AGDLP para consistência
- Separação entre acesso e identidade

---

## 9. Limitações Atuais

- Sem SIEM implementado
- Sem IDS/IPS ativo
- Sem MFA integrado
- Sem monitorização centralizada de logs
- Sem políticas avançadas de compliance

---

## 10. Evolução Prevista

- Implementação de Windows LAPS
- Ativação de AD Recycle Bin
- Integração com SIEM (ex: Wazuh ou similar)
- Implementação de AD CS (PKI)
- Hardening avançado de endpoints (AppLocker / WDAC)
- Monitorização centralizada de eventos de segurança