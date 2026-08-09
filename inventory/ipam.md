# Inventário IPAM (Atribuição de Endereços IP Activos)

Este ficheiro serve como o registo operacional e inventário ativo de endereços IP atribuídos na infraestrutura corporativa **HomeLab Consultoria & Contabilidade** (v0.2.0).

Para a especificação técnica e lógica das VLANs, consulte a [Documentação de Rede (IPAM)](../docs/network/ipam.md).

---

## Registo Activo de IP (IP Matrix)

| Endereço IP | Hostname / Nome do Ativo | VLAN | Tipo | Estado | Função / Descrição |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`10.0.10.1`** | `HL-LDA-P-FW01` | VLAN 10 (MGMT) | Estático | Ativo | Gateway / Interface WebGUI de Administração pfSense |
| **`10.0.10.2`** | `HL-LDA-P-SW01` | VLAN 10 (MGMT) | Estático | Ativo | Switch Switch Gerido Principal |
| **`10.0.10.5`** | `HL-LDA-P-VMW01` | VLAN 10 (MGMT) | Estático | Ativo | Gestão do Hypervisor ESXi / Proxmox |
| **`10.0.10.10`**| `HL-LDA-P-FS01-IPMI`| VLAN 10 (MGMT) | Estático | Ativo | Interface de Gestão Física IPMI - TrueNAS |
| **`10.0.10.11`**| `HL-LDA-P-VMW01-IPMI`| VLAN 10 (MGMT) | Estático | Ativo | Interface de Gestão Física IPMI - Hypervisor |
| | | | | | |
| **`10.0.20.1`** | `HL-LDA-P-FW01` | VLAN 20 (SRV) | Estático | Ativo | Gateway de Roteamento para a VLAN de Servidores |
| **`10.0.20.10`**| `HL-LDA-P-DC01` | VLAN 20 (SRV) | Estático | Ativo | Domain Controller Principal (AD DS, DNS, DHCP Server) |
| **`10.0.20.20`**| `HL-LDA-P-MON01` | VLAN 20 (SRV) | Estático | Ativo | Servidor de Monitorização (Zabbix/Prometheus) |
| **`10.0.20.30`**| `HL-LDA-P-BKP01` | VLAN 20 (SRV) | Estático | Ativo | Servidor de Backup (Veeam Backup & Replication) |
| | | | | | |
| **`10.0.30.1`** | `HL-LDA-P-FW01` | VLAN 30 (CLI) | Estático | Ativo | Gateway de Roteamento para a VLAN de Clientes |
| **`10.0.30.101`**| `HL-LDA-P-WKS001` | VLAN 30 (CLI) | DHCP Res | Ativo | Windows Client Workstation (Financeiro) |
| **`10.0.30.102`**| `HL-LDA-P-WKS002` | VLAN 30 (CLI) | DHCP Res | Ativo | Windows Client Workstation (Comercial) |
| **`10.0.30.103`**| `HL-LDA-P-WKS003` | VLAN 30 (CLI) | DHCP Res | Ativo | Windows Client Workstation (Direção) |
| **`10.0.30.104`**| `HL-LDA-P-WKS004` | VLAN 30 (CLI) | DHCP Res | Ativo | Windows Client Workstation (IT/Administração) |
| **`10.0.30.105`**| `HL-LDA-P-WKS005` | VLAN 30 (CLI) | DHCP Res | Reserva | Windows Client Workstation (Estação Suplente) |
| | | | | | |
| **`10.0.40.1`** | `HL-LDA-P-FW01` | VLAN 40 (STO) | Estático | Ativo | Gateway de Roteamento para a VLAN de Armazenamento |
| **`10.0.40.10`**| `HL-LDA-P-FS01` | VLAN 40 (STO) | Estático | Ativo | Interface de Rede de Dados TrueNAS (SMB Shares) |
| | | | | | |
| **`10.0.50.1`** | `HL-LDA-P-FW01` | VLAN 50 (GST) | Estático | Ativo | Gateway da VLAN Guest (DHCP / DNS Externo) |
| **`10.0.50.100`** a **`10.0.50.254`** | Gama DHCP | VLAN 50 (GST) | Dinâmico | Ativo | Dispositivos móveis e computadores de visitantes |
