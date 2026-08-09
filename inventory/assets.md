# Inventário de Ativos (Assets Inventory)

Este documento lista e descreve os ativos de infraestrutura de TI da **HomeLab Consultoria & Contabilidade** (v0.2.0), incluindo hardware, máquinas virtuais, sistemas operativos e atribuições.

---

## 1. Ativos de Hardware Físico

| ID | Nome do Dispositivo | Fabricante / Modelo | Especificações | Localização | Função principal |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **HW-01** | `HL-LDA-P-SRV01` | Dell PowerEdge R650 | 1x Intel Xeon 4314, 128GB RAM, 4x 1.2TB SAS RAID 5 | Bastidor 1 (Sede Luanda) | Hypervisor de Produção (ESXi 8.0) |
| **HW-02** | `HL-LDA-P-FS01` | TrueNAS Mini X+ | Intel Atom 8-Core, 32GB ECC RAM, 4x 4TB SATA WD Red RAIDZ2 | Bastidor 1 (Sede Luanda) | Servidor de Ficheiros Central (ZFS / TrueNAS SCALE) |
| **HW-03** | `HL-LDA-P-SW01` | Ubiquiti UniFi Pro 24 PoE | 24 portas RJ45 1G, 2 portas SFP+ 10G, Gerido L3 | Bastidor 1 (Sede Luanda) | Switch Core & Distribuição (Trunking 802.1Q) |
| **HW-04** | `HL-LDA-P-AP01` | Ubiquiti UniFi U6 Pro | Wi-Fi 6, Dual-Band, Alimentação PoE | Teto (Sede Luanda) | Ponto de Acesso Wi-Fi (SSID Corporativo e SSID Guest) |

---

## 2. Máquinas Virtuais (VMs) no Hypervisor

As seguintes máquinas virtuais estão ativas no host `HL-LDA-P-VMW01` (ESXi / Proxmox):

| VM Name / Hostname | IP Address | Sistema Operativo | vCPU | RAM | Disco | Papel e Serviços |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`HL-LDA-P-FW01`** | `10.0.10.1` <br> `10.0.20.1` <br> `10.0.30.1` <br> `10.0.40.1` <br> `10.0.50.1` | pfSense 2.7.2-RELEASE | 2 | 4 GB | 20 GB | Firewall, Router virtualizado, Gateway IP e Servidor DHCP (VLAN 50) |
| **`HL-LDA-P-DC01`** | `10.0.20.10` | Windows Server 2025 Standard | 4 | 8 GB | 80 GB | Active Directory Domain Controller, Servidor DNS (corp.homelab.ao), DHCP Server (VLAN 20, 30) |
| **`HL-LDA-P-MON01`** | `10.0.20.20` | Ubuntu Server 24.04 LTS | 2 | 4 GB | 50 GB | Monitorização centralizada de ativos com Zabbix, Grafana e Prometheus |
| **`HL-LDA-P-BKP01`** | `10.0.20.30` | Windows Server 2025 Standard | 2 | 8 GB | 120 GB | Repositório e gestão de cópias de segurança (Veeam Backup & Replication) |

---

## 3. Estações de Trabalho (Clients)

Estações de trabalho Windows físicas ou virtuais distribuídas aos utilizadores:

| ID | Nome do Computador | Utilizador Atribuído | Departamento | Sistema Operativo | Tipo |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **CLI-01**| `HL-LDA-P-WKS001` | Ana Silva | Financeiro | Windows 10 Enterprise | Portátil L14 |
| **CLI-02**| `HL-LDA-P-WKS002` | Pedro Santos | Comercial | Windows 10 Enterprise | Desktop Micro |
| **CLI-03**| `HL-LDA-P-WKS003` | Carlos Lima (Diretor) | Direção | Windows 10 Enterprise | Portátil X1 Carbon |
| **CLI-04**| `HL-LDA-P-WKS004` | António Bunga | IT / Administração | Windows 11 Pro | Portátil L14 |
| **CLI-05**| `HL-LDA-P-WKS005` | *Disponível para testes* | Suporte / Helpdesk | Windows 10 Enterprise | Portátil L14 |

---

## 4. Software e Licenciamento

| Software | Tipo de Licença | Quantidade | Estado | Validade |
| :--- | :--- | :--- | :--- | :--- |
| **VMware vSphere Hypervisor (ESXi)** | Enterprise Plus (Estudo/Lab) | 1 | Ativo | Perpétuo |
| **Windows Server 2025 Standard** | Standard Core (16 Cores) | 2 | Ativo | Perpétuo |
| **Windows 10/11 Enterprise / Pro** | Retail / OEM | 5 | Ativo | Perpétuo |
| **TrueNAS SCALE** | Open Source | - | Ativo | N/A |
| **pfSense Community Edition** | Open Source | - | Ativo | N/A |
| **Veeam Backup & Replication** | NFR Community Edition (10 VMs) | 1 | Ativo | 1 Ano (Renovável) |
| **PaperCut NG** | Licença 5 Utilizadores (Avaliação) | 1 | Planeado | N/A |
