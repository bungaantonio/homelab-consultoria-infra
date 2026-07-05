# 13 - Roadmap

## 1. Visão Geral

Este roadmap descreve a evolução planeada do ambiente **HomeLab Consultoria & Contabilidade**, uma infraestrutura de laboratório baseada em Active Directory e serviços de rede corporativos.

O objetivo é evoluir de uma implementação básica para um ambiente com práticas próximas de produção.

---

## 2. Fase 1 — Fundação (Concluída)

- [x] Configuração da rede base (`10.0.10.0/24`)
- [x] Implementação do pfSense como gateway
- [x] Criação do domínio `lan.homelab.ao`
- [x] Configuração do Domain Controller
- [x] Estrutura inicial de OUs
- [x] Integração de estação Windows ao domínio
- [x] DNS interno funcional
- [x] DHCP operacional via pfSense

---

## 3. Fase 2 — Gestão e Segurança

- [ ] Implementação de GPOs de hardening
- [ ] Bloqueio de dispositivos USB
- [ ] Políticas de segurança de endpoints
- [ ] Configuração de wallpaper corporativo
- [ ] Restrições de sistema (CMD, Painel de Controlo)
- [ ] Auditoria de eventos de segurança

---

## 4. Fase 3 — Serviços Corporativos

- [ ] Implementação do TrueNAS como servidor de ficheiros
- [ ] Integração com Active Directory
- [ ] Mapeamento de drives por GPO
- [ ] Implementação de PaperCut NG (print services)
- [ ] Partilhas departamentais estruturadas
- [ ] Permissões baseadas em grupos (AGDLP)

---

## 5. Fase 4 — Segurança Avançada

- [ ] Ativação do AD Recycle Bin
- [ ] Implementação de Windows LAPS
- [ ] Fine-Grained Password Policies (FGPP)
- [ ] Delegação de controlo (modelo helpdesk)
- [ ] Hardening avançado de servidores
- [ ] Restrição de privilégios administrativos

---

## 6. Fase 5 — Infraestrutura Enterprise

- [ ] Implementação de segundo Domain Controller (redundância)
- [ ] Segmentação de rede com VLANs
- [ ] IDS/IPS no perímetro (pfSense)
- [ ] Centralização de logs (SIEM)
- [ ] Monitorização de infraestrutura
- [ ] Políticas de compliance e auditoria

---

## 7. Fase 6 — Automação e Observabilidade

- [ ] Automatização de tarefas administrativas
- [ ] Scripts de provisionamento de utilizadores
- [ ] Monitorização em tempo real
- [ ] Alertas automáticos de falhas
- [ ] Documentação dinâmica de infraestrutura

---

## 8. Objetivo Final

O objetivo final é evoluir este laboratório para uma infraestrutura que simule práticas reais de administração de sistemas empresariais, com foco em:

- Segurança
- Escalabilidade
- Automação
- Governança de identidade
- Resiliência operacional