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

1. Otevřeme si příkazovou řádku
1. Zadáme příkaz -  `openssl req -newkey rsa:2048 -nodes -  keyout [ zadáme své jméno ].key -x509 -days 365 -out [zadáme své jméno ].crt`      Příklad : **openssl req -newkey rsa:2048 -nodes -keyout Michal-Cernosek.key -x509 -days 365 -out Michal-Cernosek.crt**
1. Poté to na nás vyhodí zadání údajů, odklikáme entrem, vyplníme jen **Common Name** a **Email Address**
1. Nyní musíme spojit certifikáty, zadáme tento příkaz `openssl pkcs12 -inkey [zadáme své jméno].key -in [zadáme své jméno].crt -export -out [zadáme své jméno].p12`    Příklad : **openssl pkcs12 -inkey Michal-Cernosek.key -in Michal-Cernosek**.crt -export -out Michal-Cernosek.p12
1. Máme vytvořeno
