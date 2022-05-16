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

**Generování certifikátu s vlastním podpisem pomocí OpenSSL**

1. Otevřeme si příkazovou řádku
1. Zadáme příkaz -  `openssl req -newkey rsa:2048 -nodes -  keyout [ zadáme své jméno ].key -x509 -days 365 -out [zadáme své jméno ].crt`      Příklad : **openssl req -newkey rsa:2048 -nodes -keyout Michal-Cernosek.key -x509 -days 365 -out Michal-Cernosek.crt**
1. Poté to na nás vyhodí zadání údajů, odklikáme entrem, vyplníme jen **Common Name** a **Email Address**
1. Nyní musíme spojit certifikáty, zadáme tento příkaz `openssl pkcs12 -inkey [zadáme své jméno].key -in [zadáme své jméno].crt -export -out [zadáme své jméno].p12`    Příklad : **openssl pkcs12 -inkey Michal-Cernosek.key -in Michal-Cernosek**.crt -export -out Michal-Cernosek.p12
1. Máme vytvořeno

**Nastavení služby Wireguard VPN**

__Na 172.16.0.1__

__vydajte tento příkaz__

wg genkey | tee yourname-privatekey | wg pubkey > /etc/wireguard/yourname-publickey

__zkopírujte si klíče, které budete potřebovat později__


cd /etc/wireguard

cat your.name-publickey

cat your.name-privatekey


/etc/init.d/network restart

## Pak se přihlaste do routeru 172.16.0.1 ##

Pak nastavtite v GUI na routeru 172.16.0.1 toto:

přejít na __network__, __interface__, __WG0__, and edit __Peers__.

Input new peer with: Description, Public Key, Allowed IP's



Check Route Allowed IPs

Endpoint Host can stay empty

Endpoint Port 51820

Persisten Keep Alive 25

__Klikněte na tlačítko uložit (save)__  


![[step1.png|step1.png]]




A budete přesunuti na hlavní obrazovku


__Ujistěte se, že jste změnu uložili v pravém horním rohu.__


![[step2.png|step2.png]]





## Vytvoření souboru conf ##



__Zadání vlastních dat ze Wireguard router__

zde zadáte své soukromé (__PrivateKey__) a veřejné (__PublicKey__) klíče

Under **Peer**
Publickey je ze serveru.

[Interface]

PrivateKey = XXXXXOwH/0BToe6/TdNBfWbCmLJSNXXXXXX=

Address = 10.0.0.X/24

ListenPort = 51820

DNS = 192.168.1.3

**[Peer]**

PublicKey = XXXXXXXXXXXX6dmdFp81xaXXXXXXXXXXXX1I=

AllowedIPs = 0.0.0.0/0

Endpoint = vpn.itli.ga:51820

PersistentKeepalive = 25

__Uložte jej a přesuňte do souboru /etc/wireguard__

mv /home/yourlocation/ to /etc/wireguard


__Takto vypadá "yourname.conf"__


![[step3.png|step3.png]]


## V počítači (klient nastavení) ##

__Nainstalujte Wiregurad a všechny níže uvedené příkazy.__

apt install wireguard

ip link add dev wg0 type wireguard

ip address add dev wg0 10.0.0.X/24

ip address show wg0

ip link set up dev wg0


wg-quick up /etc/wireguard/yourconfig.conf

nahraďte "yourconfig.conf" svým skutečným názvem konfigurace

__například milan.k.conf__

## A to je vše ##
