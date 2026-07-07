# 09 - File Services Implementation

## 1. Papel do Documento

Este ficheiro serve como nota complementar ao runbook principal em [14 - TrueNAS SCALE 25.10.4 - File Server Integration with Active Directory](docs/14-truenas-scale-preparacao-integracao-ad.md).

O capítulo 14 contém o procedimento fechado e validado. Este documento existe apenas para consolidar decisões de implementação sem repetir instruções operacionais.

---

## 2. Princípios Aplicados

- O serviço de ficheiros é fornecido pelo TrueNAS SCALE
- A autenticação e a resolução de identidades vêm do Active Directory
- A autorização segue o modelo AGDLP
- A aplicação de permissões ocorre em ACLs NFSv4 no dataset
- As partilhas SMB apenas expõem o recurso já autorizado

---

## 3. Estrutura Funcional

Implementação lógica:

- `GG-*` para identidade de utilizador
- `DL-FS-*` para autorização de recursos
- `tank/Dados/*` para datasets por departamento
- SMB Shares publicadas a partir dos datasets finais

---

## 4. Regras de Manutenção

- Não duplicar aqui os passos de join, ACL ou criação de share
- Usar o capítulo 14 como fonte de verdade operacional
- Manter este ficheiro apenas como referência de arquitetura e decisões

---

## 5. Estado

Os File Services deste repositório devem ser entendidos como concluídos no estado descrito no runbook principal.
