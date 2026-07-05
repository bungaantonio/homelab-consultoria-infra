# 08 - File Services

## 1. Visão Geral

Os serviços de ficheiros no ambiente **HomeLab Consultoria & Contabilidade** têm como objetivo fornecer armazenamento centralizado, seguro e estruturado para utilizadores e departamentos.

A implementação está planeada com base num servidor dedicado TrueNAS (em roadmap), integrado com permissões baseadas em identidade do domínio gerido por Active Directory.

---

## 2. Arquitetura de File Services

### 2.1 Componente principal

- Servidor de ficheiros: TrueNAS (planeado)
- Protocolo principal: SMB/CIFS
- Autenticação: Active Directory
- Rede: `10.0.10.0/24`

---

## 3. Modelo de Partilhas

As partilhas são organizadas por função organizacional:

### Estrutura prevista:

- Financeiro
- Comercial
- Direção
- IT
- Recursos Gerais

---

## 4. Modelo de Permissões

O controlo de acesso segue o princípio de menor privilégio:

- Acesso baseado em grupos de segurança
- Integração com modelo AGDLP
- Permissões aplicadas ao nível de share e NTFS/ACL

---

## 5. Integração com Active Directory

O servidor de ficheiros será integrado ao domínio para:

- Autenticação de utilizadores
- Mapeamento automático de drives
- Aplicação de permissões por grupo
- Auditoria de acessos

---

## 6. Mapeamento de Drives (Planeado)

As unidades serão mapeadas via GPO:

- `H:` → Home drive do utilizador
- `F:` → Financeiro
- `C:` → Comercial
- `D:` → Direção
- `T:` → IT / técnicos

---

## 7. Segurança

Medidas previstas:

- Autenticação obrigatória via domínio
- Desativação de acesso anónimo SMB
- Controlo de permissões por grupo
- Auditoria de acessos a ficheiros sensíveis
- Criptografia em trânsito (SMB signing quando aplicável)

---

## 8. Backup e Retenção

Modelo previsto:

- Snapshots automáticos no TrueNAS
- Política de retenção por períodos definidos
- Possível integração com backup externo (fase futura)

---

## 9. Limitações Atuais

- Servidor de ficheiros ainda não implementado
- Sem integração real com AD neste momento
- Sem políticas de backup ativas
- Sem quotas de armazenamento configuradas

---

## 10. Evolução Prevista

- Implementação do TrueNAS em produção
- Integração total com Active Directory
- Configuração de quotas por utilizador/departamento
- Implementação de snapshots automáticos
- Auditoria avançada de acessos a ficheiros