# Desenho de Roteamento e Regras de Firewall (v0.2.0)

```text
Estado: Implementado (pfSense, VLANs 10/20/30 e WAN); Planeado (VLANs 40/50 e VPN)
Versão: v0.2.0
```

Este documento descreve as definições de rede, interfaces lógicas e regras de filtragem implementadas no gateway de segurança **pfSense** (`HL-LDA-P-FW01`) virtualizado no VMware Workstation Pro.

---

## 1. Caracterização do Equipamento

*   **Hostname:** `HL-LDA-P-FW01`
*   **FQDN:** `HL-LDA-P-FW01.corp.homelab.ao`
*   **Sistema Operativo:** pfSense 2.8.1-RELEASE (baseado em FreeBSD)
*   **Ambiente:** VMware Workstation Virtual Machine

---

## 2. Configuração de Interfaces de Rede

O pfSense gere o roteamento inter-VLAN. A tabela abaixo especifica o endereçamento real de cada rede:

| Interface Lógica | ID VLAN | Estado | Tipo de Atribuição | IP e Máscara | Função / Descrição |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`WAN`** | - | **Implementado** | DHCP (VMware NAT) | `192.168.81.128/24` | Interface de saída para a Internet (NAT) |
| **`LAN_MGMT`** | `10` | **Implementado** | Estático | `10.0.10.1/24` | Rede de Gestão Administrativa de VMs |
| **`VLAN_SERVERS`** | `20` | **Implementado** | Estático | `10.0.20.1/24` | Sub-rede de Serviços Centrais (AD, DNS) |
| **`VLAN_CLIENTS`** | `30` | **Implementado** | Estático | `10.0.30.1/24` | Sub-rede para Estações de Trabalho |
| **`VLAN_STORAGE`** | `40` | **Planeado** | Estático | `10.0.40.1/24` | Rede isolada para tráfego TrueNAS |
| **`VLAN_GUEST`** | `50` | **Planeado** | Estático | `10.0.50.1/24` | Rede de visitantes com acesso exclusivo à WAN |

---

## 3. Matriz de Controlo de Tráfego Inter-VLAN

As regras do pfSense são configuradas para garantir o isolamento liso entre as redes locais.

### 3.1 Tráfego de Clientes para Servidores (VLAN 30 -> VLAN 20)
*   **Regra:** As estações de trabalho de utilizadores na VLAN 30 podem apenas aceder a serviços básicos do Domain Controller (`HL-LDA-P-DC01` em `10.0.20.10`):
    *   **DNS:** Porta `53` (TCP/UDP)
    *   **Kerberos:** Porta `88` (TCP/UDP) e `464` (Alteração de password)
    *   **LDAP/LDAPS:** Portas `389` e `636`
    *   **SMB/SYSVOL:** Porta `445` (Para aplicação de GPOs)
    *   **NTP:** Porta `123` (Sincronização de relógio)
*   *Qualquer outro tráfego para a VLAN 20 é bloqueado por defeito.*

### 3.2 Bloqueio de Acesso à Gestão (VLAN 30/50 -> VLAN 10)
*   **Regra:** É **proibida** qualquer ligação com origem em CLIENTS ou GUEST direcionada para a rede MGMT (`10.0.10.0/24`). A consola de gestão do VMware Workstation Host, WebGUI do pfSense (`10.0.10.1`) e IPMI são invisíveis para os utilizadores comuns.

### 3.3 Acesso de Dados ao Storage (VLAN 30 -> VLAN 40 - Planeado)
*   **Regra:** Clientes na VLAN 30 terão acesso exclusivo ao IP do TrueNAS (`10.0.40.10`) através do protocolo SMB (porta `445`). Acessos SSH (porta `22`) ou HTTP/HTTPS de administração são bloqueados.

### 3.4 Isolamento da Rede Guest (VLAN 50 -> Redes Internas - Planeado)
*   **Regra:** Qualquer tráfego com destino a `10.0.0.0/8` é bloqueado na interface GUEST. Apenas é permitido o acesso para a WAN.

---

## 4. Rede Privada Virtual (VPN) — *Planeado*

Para suporte e trabalho remoto, está planeada a criação de um túnel seguro no pfSense:
*   **Protocolo:** OpenVPN ou WireGuard.
*   **Autenticação:** Integração direta com o AD DS (NPS RADIUS no DC) ou verificação por chaves privadas.
*   **Acesso:** Mapeamento do utilizador na rede virtual isolada de VPN com ACLs restritivas de destino.
