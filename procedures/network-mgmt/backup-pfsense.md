# Procedimento Operacional: Cópia de Segurança do pfSense (v0.2.0)

Este documento especifica o procedimento operacional padrão para realizar e restaurar cópias de segurança (backups) das configurações do firewall e router virtualizado **pfSense** (`HL-LDA-P-FW01`).

---

## 1. Importância do Backup de Configuração

O pfSense armazena todas as suas definições (regras de firewall, mapeamento de VLANs, definições de DHCP, certificados VPN, utilizadores locais e rotas) num único ficheiro formatado em **XML**. Manter cópias atualizadas deste ficheiro permite recuperar a rede em minutos em caso de corrupção ou falha do Hypervisor.

---

## 2. Procedimento de Backup Manual (Interface Web GUI)

Este procedimento deve ser executado antes de qualquer alteração estrutural de rede (ex: criação de novas regras de firewall ou VLANs) ou de atualizações do pfSense.

### Passo 1: Acesso ao pfSense
1.  A partir de uma estação de trabalho autorizada na VLAN 10 (MGMT), abra o navegador de internet.
2.  Aceda à consola de gestão: `https://10.0.10.1`.
3.  Inicie sessão com credenciais de administrador de IT.

### Passo 2: Exportação das Configurações
1.  No menu superior, navegue até **Diagnostics** -> **Backup & Relation** (ou **Backup & Restore**).
2.  Na secção **Backup Configuration**, configure as seguintes opções:
    *   **Backup area:** `All` (Copia toda a configuração global do sistema).
    *   **Skip packages:** *Deixe desmarcado* (para incluir as configurações dos pacotes instalados como ntopng, pfBlockerNG, etc.).
    *   **Encrypt this configuration file:**
        > [!IMPORTANT]
        > **Obrigatório.** Ative esta caixa de verificação para encriptar o ficheiro XML. Isto impede que palavras-passe, chaves privadas de certificados e chaves pré-partilhadas de VPN sejam lidas em texto limpo caso o ficheiro seja extraviado.
    *   **Encryption Password:** Defina uma palavra-passe forte para proteção criptográfica e registe-a no gestor de credenciais corporativo seguro.
3.  Clique no botão **Download configuration as XML**.
4.  O download do ficheiro começará automaticamente com o nome no formato: `config-HL-LDA-P-FW01-YYYYMMDDHHMMSS.xml`.

### Passo 3: Armazenamento Seguro
1.  Mova o ficheiro descarregado para a partilha de rede segura de IT:
    `\\HL-LDA-P-FS01\Administracao\Backups\Network_Configs\`
2.  A partilha de IT é incluída no ciclo diário de backup incremental gerido pelo `HL-LDA-P-BKP01`, garantindo conformidade com a regra de cópias redundantes.

---

## 3. Procedimento de Restauro (Disaster Recovery)

Em caso de falha de hardware e necessidade de substituir o pfSense por uma nova VM limpa:

1.  Instale o pfSense de raiz no Hypervisor atribuindo as interfaces de rede físicas correspondentes (Trunk).
2.  Aceda à interface web temporária criada pelo pfSense (geralmente `https://192.168.1.1` ou o IP DHCP inicial).
3.  Navegue até **Diagnostics** -> **Backup & Restore**.
4.  Na secção **Restore Configuration**, configure as seguintes opções:
    *   **Restore area:** `All`.
    *   **Configuration file:** Clique em *Browse* e selecione o ficheiro XML de backup pretendido.
    *   **Decrypt configuration file:** Ative esta opção.
    *   **Decryption Password:** Introduza a palavra-passe utilizada para encriptar o ficheiro durante o backup.
5.  Clique no botão **Restore Configuration**.
6.  O pfSense descodificará as definições, aplicará a configuração interna e iniciará um reinício (reboot) automático.
7.  Após o reinício, valide se todas as interfaces VLAN (10, 20, 30, 40, 50) e regras associadas estão ativas e a responder a Pings.
