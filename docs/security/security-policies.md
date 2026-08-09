# Diretivas de Segurança e Segmentação (v0.2.0)

```text
Estado: Implementado (Segmentação VLAN 10/20/30); Definido/Planeado (LAPS, BitLocker, GPOs de Restrição)
Versão: v0.2.0
```

Este documento descreve as políticas e controlos de segurança aplicados na infraestrutura corporativa **HomeLab Consultoria & Contabilidade** para proteger documentos financeiros, relatórios internos, dados de clientes e o funcionamento diário da organização fictícia. O foco é reduzir riscos sem tornar a operação demasiado complexa, mantendo um modelo de segurança orientado para o princípio do menor privilégio.

As regras de rede, identidade e endpoints foram pensadas para sustentar uma operação corporativa pequena, com separação entre utilizadores, administração e serviços críticos.

---

## 1. Segmentação Lógica de Rede (Isolamento)

A rede v0.2.0 utiliza segmentação rígida implementada através de regras de firewall no pfSense (`HL-LDA-P-FW01`).

### 1.1 Controlo de Clientes (VLAN 30 -> Outras Redes)
*   **Acesso à Internet:** Permitido. O pfSense efetua NAT do tráfego da VLAN 30 para a interface WAN (`192.168.81.128`).
*   **Acesso à Gestão (VLAN 10):** **Bloqueado.** Nenhum dispositivo de utilizador na VLAN 30 pode comunicar com a rede de gestão `10.0.10.0/24`. A consola web do pfSense (`10.0.10.1`) e hipervisores estão totalmente isolados.
*   **Acesso a Serviços (VLAN 20):** **Restrito.** As estações podem apenas comunicar com o Domain Controller (`10.0.20.10`) através das portas de protocolo necessárias (DNS, Kerberos, LDAP, SMB, NTP). Qualquer outro tráfego inter-VLAN é bloqueado por omissão.

### 1.2 Isolamento de Armazenamento (VLAN 40 — Planeado)
*   Quando implementada, a VLAN 40 (Storage TrueNAS) estará inacessível diretamente para administração a partir da rede de clientes. Clientes na VLAN 30 apenas poderão iniciar ligações SMB na porta `445` em direção a `10.0.40.10`.
*   A VLAN 40 **não terá** gateway de saída para a Internet (sub-rede estritamente local), eliminando o risco de exfiltração de dados por conexões reversas.

### 1.3 Isolamento de Visitantes (VLAN 50 — Planeado)
*   A VLAN 50 (Guest) estará isolada. Dispositivos nesta rede obterão IP via DHCP no pfSense e tráfego DNS público recursivo, sendo bloqueado qualquer acesso a endereços privados RFC 1918 (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`).

---

## 2. Hardening de Identidades no Active Directory

*   **Políticas de Password (GPO_CORP_DefaultDomainPasswordPolicy) [Implementado]:**
    *   Comprimento mínimo: 12 caracteres.
    *   Complexidade: Letras maiúsculas, minúsculas, números e caracteres especiais.
    *   Lockout: Bloqueio temporário da conta por 15 minutos após 5 tentativas de logon incorretas.
*   **Windows LAPS (Local Administrator Password Solution) [Planeado]:**
    *   Rotação automática de passwords de administradores locais dos clientes Windows, armazenadas cifradas na base de dados do AD.

---

## 3. Segurança Física e de Endpoints (Estações) — *Planeado*

*   **Encriptação de Discos (BitLocker):** Planeado para todas as estações de trabalho. As chaves de recuperação serão guardadas e centralizadas no Active Directory associadas a cada computador.
*   **Bloqueio de Unidades de Armazenamento USB (GPO_WKS_RestrictUSB):** Aplicação de diretiva restritiva para desativar a leitura e escrita em pens e discos amovíveis USB para utilizadores comuns, prevenindo fuga de ficheiros contabilísticos confidenciais de clientes.
