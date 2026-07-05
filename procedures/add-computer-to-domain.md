# add-computer-to-domain.md

## 1. Objetivo

Este procedimento descreve como adicionar uma estação de trabalho ao domínio `lan.homelab.ao`, gerido por Active Directory.

---

## 2. Pré-requisitos

- A máquina deve estar na rede `10.0.10.0/24`
- DNS configurado para `10.0.10.10`
- Credenciais de administrador de domínio
- Conectividade com o Domain Controller (LAB-DC01)

---

## 3. Configuração Inicial

1. Abrir **System Properties**
2. Aceder a **Computer Name**
3. Clicar em **Change**
4. Selecionar opção **Domain**

---

## 4. Processo de Junção ao Domínio

1. Inserir o domínio:
   - `lan.homelab.ao`
2. Autenticar com credenciais administrativas
3. Confirmar associação
4. Reiniciar o sistema

---

## 5. Validação

Após reinício:

- Verificar login com utilizador do domínio
- Confirmar resolução de DNS interno
- Executar:
  - `whoami`
  - `gpresult /r`

---

## 6. Problemas Comuns

- DNS incorreto impede descoberta do domínio
- Firewall local bloqueando tráfego
- Credenciais inválidas ou sem permissões

---

## 7. Resultado Esperado

A máquina deve aparecer em:

- `OU=02_Computers > Workstations`

no Active Directory.