# 01 - Project Overview

## 1. Contexto do Projeto

Este projeto representa a conceção e implementação de uma infraestrutura de TI corporativa simulada para a organização fictícia **HomeLab Consultoria & Contabilidade**, orientada a serviços fiscais e auditoria financeira para PME.

O ambiente foi desenhado para refletir práticas reais de administração de sistemas em pequenas organizações, com foco em centralização de identidade, segurança, controlo de acesso e gestão de infraestrutura.

---

## 2. Objetivo

O objetivo principal deste laboratório é consolidar competências práticas em:

- Administração de sistemas Windows Server
- Implementação de domínio em Windows Server
- Gestão de rede e segmentação lógica
- Aplicação de políticas de grupo (GPO)
- Modelação de estrutura organizacional (OUs e grupos)
- Hardening e boas práticas de segurança

---

## 3. Escopo do Ambiente

O ambiente simula uma infraestrutura corporativa com:

- 1 domínio central (`lan.homelab.ao`)
- 1 controlador de domínio (DC)
- Estações de trabalho Windows
- Serviços de rede internos
- Serviços de ficheiros operacionais e impressão em fase seguinte

O sistema opera numa rede isolada `10.0.10.0/24`.

---

## 4. Serviços Principais

- Firewall e gateway: pfSense
- Serviço de diretório: Active Directory Domain Services
- DNS interno integrado ao domínio
- DHCP externo via pfSense
- Estações Windows integradas ao domínio

---

## 5. Modelo de Design

O projeto segue um modelo de administração baseado em:

- Separação de identidade (Users, Groups, Service Accounts)
- Separação de máquinas (Computers, Servers)
- Aplicação de políticas via OUs
- Controlo de acesso via grupos de segurança (AGDLP concept)
- Gestão centralizada de recursos

---

## 6. Limitações do Ambiente

Este é um ambiente de laboratório com:

- Número reduzido de utilizadores (≈5)
- Infraestrutura física simulada ou virtualizada
- Ausência de SLA e produção real
- Escopo único (single-tenant)

---

## 7. Evolução do Projeto

O ambiente evolui em fases:

- Fundação da infraestrutura (rede + domínio)
- Implementação de GPOs e segurança
- Integração de serviços corporativos (file concluído, print em fase seguinte)
- Hardening e features avançadas (AD Recycle Bin, LAPS, FGPP, AD CS)

---

## 8. Nota Final

Este projeto não representa uma infraestrutura produtiva real, mas sim um modelo de estudo orientado a práticas de administração de sistemas em ambiente corporativo controlado.