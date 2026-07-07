# apply-gpo.md

## 1. Objetivo

Implementar Group Policy Objects (GPOs) no domínio `lan.homelab.ao` através do Group Policy Management Console, seguindo modelo faseado com validação por GPO.

---

## 2. Pré-requisitos

- Estrutura de OUs implementada e validada
- Utilizadores distribuídos pelas OUs corretas
- Computadores nas OUs corretas
- Pipeline AD/GPO funcional (testado com GPO de validação)
- Grupos globais (GG) criados e configurados
- Grupos Domain Local (DL) criados quando houver recursos associados
- File Server e partilhas validadas para GPOs de drive mapping

---

## 3. Ordem de Implementação

### FASE 1 - Base de Segurança (Obrigatória)

1. GPO_Desktop_Restrictions
2. GPO_Workstation_Baseline (parcial)

### FASE 2 - Segurança Avançada

3. GPO_Windows_Defender_Policy
4. GPO_Windows_Firewall_Policy

### FASE 3 - Operação

5. GPO_Drive_Mapping
6. GPO_Printer_Deployment

> Nota: estas GPOs automatizam a entrega de recursos. As permissões continuam a ser controladas por GG → DL → Permissions.

### FASE 4 - Hardening Avançado

7. GPO_BitLocker
8. GPO_LAPS_Policy
9. GPO_Remote_Desktop_Policy
10. GPO_Local_Administrators_Management

---

## 4. Processo

### 4.1 Criar GPO

#### Opção A: Via Interface Gráfica (GPMC)
1. Abrir **Group Policy Management Console (GPMC)**
2. Navegar para: Forest → Domains → lan.homelab.ao
3. Clique direito em "Group Policy Objects"
4. Selecionar "New"
5. Nomear a GPO (ex: `GPO_Desktop_Restrictions`)

#### Opção B: Via PowerShell (Administrador)
```powershell
New-GPO -Name "GPO_Desktop_Restrictions"
```

---

### 4.2 Linkar GPO

#### Opção A: Via Interface Gráfica (GPMC)
1. Navegar para OU alvo (ex: `01_Users`)
2. Clique direito na OU
3. Selecionar "Link an Existing GPO..."
4. Selecionar a GPO criada
5. Confirmar

#### Opção B: Via PowerShell (Administrador)
```powershell
New-GPLink -Name "GPO_Desktop_Restrictions" -Target "OU=01_Users,DC=lan,DC=homelab,DC=ao"
```

---

### 4.3 Configurar Security Filtering e Delegação (Ajuste MS16-072)

> **Nota Crítica:** Desde a atualização de segurança MS16-072, os computadores precisam de permissão de Leitura (*Read*) no GPO para que este seja aplicado aos utilizadores. Se remover apenas o grupo *Authenticated Users* do filtro de segurança, a aplicação do GPO falhará com erro de filtragem.

#### Opção A: Via Interface Gráfica (GPMC)
1. Selecionar a GPO criada no painel esquerdo.
2. Aceder ao separador **Scope (Escopo)**:
   - Na secção **Security Filtering**, selecionar **Authenticated Users** e clicar em **Remove**.
   - Clicar em **Add...** e adicionar o grupo global correspondente (ex: `GG-Comercial`).
3. Aceder ao separador **Delegation (Delegação)** (ao lado de *Scope*):
   - Clicar em **Add...** (no fundo da janela).
   - Digitar **Authenticated Users** (ou **Domain Computers**) e confirmar.
   - Definir a permissão estritamente como **Read (Leitura)** e clicar em OK.

#### Opção B: Via PowerShell (Administrador)
Execute a sequência abaixo no Controlador de Domínio:
```powershell
# 1. Remover Authenticated Users do filtro de aplicação direta
Set-GPPermission -Name "GPO_Desktop_Restrictions" -TargetName "Authenticated Users" -TargetType Group -PermissionLevel None -Replace -Confirm:$false

# 2. Adicionar o Grupo Global para aplicação do GPO
Set-GPPermission -Name "GPO_Desktop_Restrictions" -TargetName "GG-Comercial" -TargetType Group -PermissionLevel GpoApply

# 3. Adicionar permissão de Leitura à Delegação (Necessário para MS16-072)
Set-GPPermission -Name "GPO_Desktop_Restrictions" -TargetName "Authenticated Users" -TargetType Group -PermissionLevel GpoRead
```

---

## 5. Configuração: GPO_Desktop_Restrictions

### 5.1 Bloqueio de CMD

User Configuration → Policies → Administrative Templates → System

- Localizar: "Prevent access to the command prompt"
- Definir como: "Enabled"
- Opção: "Disable command prompt script processing also"

### 5.2 Bloqueio de PowerShell

User Configuration → Policies → Administrative Templates → System → PowerShell

- Localizar: "Turn off PowerShell"
- Definir como: "Enabled"

### 5.3 Bloqueio de Painel de Controlo

User Configuration → Policies → Administrative Templates → Control Panel

- Localizar: "Prohibit access to Control Panel and PC Settings"
- Definir como: "Enabled"

---

## 6. Validação

No cliente Windows:

```cmd
gpupdate /force
gpresult /r
```

Confirmar:
- GPO aparece em "Applied Group Policy Objects" (Objetos de Política de Grupo Aplicados)
- Definições aparecem em "User Configuration" (Configuração do Utilizador)

Testes funcionais:
- Tentar abrir CMD (deve ser bloqueado)
- Tentar abrir PowerShell (deve ser bloqueado)
- Tentar abrir Painel de Controlo (deve ser bloqueado)

---

## 7. Problemas Comuns

- **GPO não aplicada na OU correta:** Validar se a OU do utilizador está sob o caminho correto onde o link do GPO foi criado.
- **Security filtering incorreto:** O grupo de utilizadores está no filtro de segurança, mas falta atribuir a permissão de leitura (*Read*) para *Authenticated Users* ou *Domain Computers* no separador *Delegation* (gera o erro `Filtering: Not Applied (Unknown Reason)`).
- **Utilizador não pertence ao GG correto:** Validar a pertença do utilizador ao grupo global (pode exigir nova sessão no cliente para atualizar o token de segurança).
- **Replicação do AD incompleta:** O GPO pode ter sido criado num Controlador de Domínio diferente do que está a autenticar o cliente no momento.
- **PowerShell diz que GPO não existe:** Verificar se há espaços invisíveis no nome da política ou gralhas. Executar `Get-GPO -All | Select-Object DisplayName` para listar os nomes exatos.

---

## 8. Regras de Execução

- Implementar em ondas controladas
- Validar cada GPO antes de avançar para a próxima
- Não criar múltiplas GPOs antes de validar a anterior
- Evitar usar domínio inteiro como scope inicial
- Utilizar OU Test para validação quando aplicável
- Usar grupos de segurança (GG) para filtering
- Para Drive Mapping, validar primeiro as permissões do File Server e só depois aplicar a GPO
- Não usar GPO para compensar ausência de DL ou ACL corretas

---

## 9. GPO_Drive_Mapping

### Pré-condições específicas

- File Server funcional
- Partilhas criadas
- Permissões configuradas
- GG e DL existentes
- Testes SMB concluídos

### Implementação

1. Criar a GPO `GPO_Drive_Mapping`
2. Linkar a GPO à OU de utilizadores apropriada
3. Usar `User Configuration → Preferences → Windows Settings → Drive Maps`
4. Aplicar `Security Filtering: Authenticated Users`
5. Definir `Item-Level Targeting` por `GG-*`

### Exemplo

- `F:` → `\\truenas\Financeiro` → `GG-Financeiro`
- `S:` → `\\truenas\Comercial` → `GG-Comercial`
- `X:` → `\\truenas\Direcao` → `GG-Direcao`

---

## 10. Resultado Esperado

GPO aplicada corretamente, visível em gpresult (sob a secção de políticas aplicadas) e funcionalmente validada no cliente com o bloqueio de acessos ativo.