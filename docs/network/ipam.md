# Plano de Endereçamento IP (IPAM)

```text
Estado: Implementado (VLANs 10, 20, 30); Planeado (VLANs 40, 50)
Versão: v0.2.0
```

Este documento descreve o plano de endereçamento IP e a segmentação de sub-redes corporativas adotada para a infraestrutura da **HomeLab Consultoria & Contabilidade** (v0.2.0) virtualizada no VMware Workstation Pro.

---

## 1. Tabela Geral de VLANs e Redes

A infraestrutura foi segmentada logicamente em VLANs de acordo com a tabela abaixo:

| VLAN | Nome da Sub-rede | Gama de Rede (CIDR) | Gateway de Rede | Estado Real | Finalidade |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`10`** | **MGMT** | `10.0.10.0/24` | `10.0.10.1` | **Implementado** | Gestão de computadores virtuais e hipervisores |
| **`20`** | **SERVERS** | `10.0.20.0/24` | `10.0.20.1` | **Implementado** | Serviços comuns de domínio (DC, DNS, DHCP) |
| **`30`** | **CLIENTS** | `10.0.30.0/24` | `10.0.30.1` | **Implementado** | Estações de trabalho Windows de utilizadores |
| **`40`** | **STORAGE** | `10.0.40.0/24` | `10.0.40.1` | **Planeado** | Tráfego dedicado de armazenamento do TrueNAS |
| **`50`** | **GUEST** | `10.0.50.0/24` | `10.0.50.1` | **Planeado** | Acesso isolado de visitantes à Internet |

---

## 2. Alocação de IPs Fixos e Reservas

A atribuição de endereços de rede para os hosts está desenhada de acordo com as seguintes categorias:

### 2.1 Rede WAN (VMware Workstation Bridge/NAT)
*   **IP Firewall (`HL-LDA-P-FW01`):** `192.168.81.128` (Atribuído por DHCP no switch de saída do VMware Workstation). Estado: **Implementado**.

### 2.2 VLAN 10 — MGMT (Gestão)
*   **Gateway pfSense (`HL-LDA-P-FW01`):** `10.0.10.1` (Interface LAN virtual no VMware). Estado: **Implementado**.
*   *Nota:* DHCP desativado nesta rede por razões de segurança. Todos os IPs de gestão futuros serão estáticos.

### 2.3 VLAN 20 — SERVERS (Serviços)
*   **Gateway pfSense (`HL-LDA-P-FW01`):** `10.0.20.1`. Estado: **Implementado**.
*   **Domain Controller (`HL-LDA-P-DC01`):** `10.0.20.10`. Estado: **Implementado**.
*   **Servidor de Monitorização (`HL-LDA-P-MON01`):** `10.0.20.20`. Estado: **Planeado**.
*   **Servidor de Backup (`HL-LDA-P-BKP01`):** `10.0.20.30`. Estado: **Planeado**.

### 2.4 VLAN 30 — CLIENTS (Clientes)
*   **Gateway pfSense (`HL-LDA-P-FW01`):** `10.0.30.1`. Estado: **Implementado**.
*   **Gama DHCP (Gerida pelo DC):** `10.0.30.100` a `10.0.30.200`. Estado: **Implementado**.
*   **Estação de Testes / Utilizador (`HL-LDA-P-WKS001`):** IP dinâmico atribuído no âmbito DHCP (ex: `10.0.30.101`). Estado: **Implementado**.

### 2.5 VLAN 40 — STORAGE (Armazenamento - Planeado)
*   **Gateway pfSense (`HL-LDA-P-FW01`):** `10.0.40.1`. Estado: **Planeado**.
*   **TrueNAS SCALE (`HL-LDA-P-FS01`):** `10.0.40.10`. Estado: **Planeado**.

### 2.6 VLAN 50 — GUEST (Visitantes - Planeado)
*   **Gateway pfSense (`HL-LDA-P-FW01`):** `10.0.50.1`. Estado: **Planeado**.
*   **Gama DHCP (Gerida pelo pfSense):** `10.0.50.100` a `10.0.50.254`. Estado: **Planeado**.
