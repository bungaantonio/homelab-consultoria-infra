# 13 - Roadmap

## 1. Visão Geral

Este roadmap descreve a evolução do ambiente **HomeLab Consultoria & Contabilidade** com base no estado atual já documentado e no que ainda falta implementar.

O objetivo é manter a documentação coerente com a arquitetura alvo: Active Directory para identidades, OUs para organização lógica, GG para funções, DL para autorização, File Services para recursos e GPOs para automação.

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

### Concluído

- [x] `GPO_Desktop_Restrictions`
- [x] `GPO_Workstation_Baseline`
- [x] `GPO_USB_Storage_Restriction`
- [x] `GPO_Corporate_Wallpaper`
- [x] `GPO_Windows_Defender_Policy`
- [x] `GPO_Windows_Firewall_Policy`
- [x] `GPO_Audit_Policy`

### Previsto

- [ ] `GPO_LAPS_Policy`
- [ ] `GPO_BitLocker`
- [ ] `GPO_Local_Administrators_Management`
- [ ] `GPO_Remote_Desktop_Policy`

---

## 4. Fase 3 — Serviços Corporativos

### Concluído

- [x] `08 - File Services Design`
- [x] `09 - File Services Implementation`
- [x] `14 - TrueNAS SCALE Preparation and AD Integration`
- [x] `10 - GPO_Drive_Mapping`
- [x] Join do TrueNAS ao domínio `lan.homelab.ao`
- [x] Criação das partilhas departamentais
- [x] Criação dos DL para autorização de recursos
- [x] Configuração final de SMB e ACLs no TrueNAS

### Previsto

- [ ] Implementação do PaperCut NG (Print Services)
- [ ] Distribuição automática de impressoras via GPO

---

## 5. Fase 4 — Segurança Avançada

- [ ] Ativação do AD Recycle Bin
- [ ] Implementação de Windows LAPS
- [ ] Fine-Grained Password Policies (FGPP)
- [ ] Delegação de controlo (modelo helpdesk)
- [ ] Hardening avançado de servidores
- [ ] Restrição de privilégios administrativos
- [ ] AD CS e LDAPS
- [ ] Autenticação do pfSense no AD

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