**Přidání balíčků co chceme na každém kontejneru / virtuálu:**

Standardní programy co chceme používat všude:

```bash
apt install -y sudo apt-utils apt-transport-https software-properties-common htop net-tools wget curl gnupg zip unzip lsb-release apt-utils ca-certificates debian-keyring
```

**Přidání uživatele, klíče a ssh přihlášení:**

```bash
adduser user
gpasswd -a user sudo
```

**Nastavení ssh pro přihlášení bez hesla**

Vytvořte uživatele, který se může přihlásit pomocí ssh na jiný server  
Poté na serveru vytvořte klíč:

```bash
ssh-keygen -t rsa
ssh someuser@192.168.122.12 mkdir -p .ssh
```

Zkopírování  klíče:

```bash
cat .ssh/id_rsa.pub | ssh someuser@192.168.122.12 'cat >> .ssh/authorized_keys'
ssh someuser@192.168.122.12 
chmod 700 .ssh
chmod 640 .ssh/authorized_keys"
cd /home
chown -R user:user user
```

**Root and password login:**

Root a passwd login jsou zakázány.  
sshd_config je upraven:  
PermitRootLogin no  
PasswordAuthentication no  
ChallengeResponseAuthentication no
