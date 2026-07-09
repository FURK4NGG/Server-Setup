# 👀 Server-Setup Overview

This repository helps you build a complete self-hosted environment on your own VPS or home server. Host your own file server, Git repositories, video conferencing platform, online office suite, RSS reader, file sharing service, uptime monitoring, forms, synchronization tools, email server, vpn server and many other applications—all under your own control.  


![Server-Setup Demo Image]()



*Terminal icerisinde yukari asagi yapmak icin 'Shift+Page_Up' ve 'Shift+Page_Down' tuslarini kullanabilirsin*  
*'sudo dmesg -D' kodu ile terminale yazirilan auid yazilarini o oturum icin kisa sureli durdurur*  

BASE(Yunohost)  

Video-Voice Call(Element,Synapse)  
Rss(FreshRSS)  
Github Repo Clone(Gitea)  
Realtime Server Monitoring(Glances)  
Advanced Form Platform(Lime Survey)
Share files and notes with outserver safely(Lufi)  
Run your 'html,css,js' page(my_webapp)  
Store Files,Notes,Calendar,Forms,Photos and OnlyOffice(Nextcloud,OnlyOffice)  
Local file sharing(PairDrop)  
Realtime website analytics(Umami)  
Realtime apps'web urls' monitoring(Uptime Kuma)  
Webmail Client(Snappy Mail)  
File convertion utility(Vert)  

VPN Server(Wireguard VPN)*For long-term compatibility, it does not run inside YunoHost*  

Real VNC Viewer(For connecting to server easily)  
File Zilla(For upload your website documents to server)  


## 📦 Setup

ssh root@SUNUCU_IP  

apt update && apt full-upgrade -y  
reboot  

ssh root@SUNUCU_IP  

apt install -y curl wget sudo gnupg2 ca-certificates  


ya da 

terminal:  
vncviewer <vnc_ip>:<vnc_port>  
enter the vnc password(max 8 character)  


Enter your user:root and password:server password  
apt update  
apt full-upgrade -y  
apt autoremove -y  
reboot  


cat /etc/os-release  
hostnamectl  
hostname -I  
ip a  
timedatectl  

-test those-  

Debian 12  
IPv4  
IPv6 (inet6 2a01:c303:2456:2954::1/64 scope global, ipv6 is 2a01:c303:2456:2954::1)  
Saat doğru mu  
Network doğru mu  


# CLOUDFLARE DNS RECORDS  
Sign In into cloudflare and chose DNS Records>Connect a Domain  
Configure AI training & search policies:
Search:Allow  
Agent:Allow  
Training:Block  
Import DNS Records:Manual only  
*All Records should be DNS only*  
A domain IPV4  
AAAA domain IPV6  
CNAME www furk4ngg.me  
A * IPV4  
AAAA * IPV6  

A chat.domain IPV4  
A cloud.domain IPV4  
A convert.domain IPV4  
A docs.domain IPV4  
A forms.domain IPV4  
A git.domain IPV4  
A lufi.domain IPV4  
A mail.domain IPV4  
A pair.domain IPV4  
A rss.domain IPV4  
A syn.domain IPV4  
A uptime.domain IPV4  
A usage.domain IPV4  
A visitors.domain IPV4  

AAAA chat.domain IPV6  
AAAA cloud.domain IPV6  
AAAA convert.domain IPV6  
AAAA docs.domain IPV6  
AAAA forms.domain IPV6  
AAAA git.domain IPV6  
AAAA lufi.domain IPV6  
AAAA mail.domain IPV6  
AAAA pair.domain IPV6  
AAAA rss.domain IPV6  
AAAA syn.domain IPV6  
AAAA uptime.domain IPV6  
AAAA usage.domain IPV6  
AAAA visitors.domain IPV6  


In your domain provider,update your nameserver based on cloudflare nameserver such as 'sam.ns.cloudflare.com' and 'kack.ns.cloudflare.com'  

hostnamectl set-hostname domain  

apt update && apt full-upgrade -y  

curl https://install.yunohost.org | bash  

Localden yunohost islemlerini yaptiktan sonra actigimiz dns kayitlari icin su domaineri aciyoruz:  
Element -> chat.domain and Synapse -> syn.domain  
FressRSS -> rss.domain  
Gitea -> git.domain  
Glances -> usage.domain  
Lime Survey -> forms.domain  
Lufi -> lufi.domain  
my_webapp -> domain  
Nextcloud -> cloud.domain  
Only Office -> docs.domain  
Pair Drop -> pair.domain  
Umami -> visitors.domain  
Uptime Kuma -> uptime.domain  
Snappy -> mail.domain  
Vert -> convert.domain  




# Yunohost App Permissions  
*visitors(Ziyaretçiler)* *all_users(Tum yunohost kullanicilari)*

Element -> all_users  
Synapse -> all_users  
FreshRSS -> all_users  
Gitea -> visitors,all_users  
Glances -> all_users  
Lime Survey -> visitors,all_users  
Lufi -> all_users  
my_webapp -> visitors,all_users  
Nextcloud -> visitors,all_users,admins  
Only Office -> visitors,all_users  
Pair Drop -> all_users  
Umami(visitors.domain) -> all_users  
Uptime Kuma -> all_users  
Snappy -> all_users  
Vert -> all_users  


Display tile in portal -> (Yes)  




## URL CERTIFICATES

✅ DNS A kaydı doğru.  
✅ DNS AAAA kaydı doğru.  
✅ Nameserver'lar değiştirildi.  
✅ HTTP reachable from outside.  
✅ Nginx çalışıyor.  
✅ Let's Encrypt API'sine sunucu bağlanabiliyor.  
✅ Cloudflare proxy kapalı (DNS only).  
Nameserver değişikliğinin tamamen yayılmamış olmasının ardından,  

sudo yunohost domain cert install  




# -Web App Ayarlari (Make index work instead of index.html)-  
sudo nano /etc/nginx/conf.d/domain.d/my_webapp.conf  
index index.php index.html; --> index index.html index.htm;  

try_files $uri $uri/ /index.php?$args =404; --> try_files $uri $uri/ $uri.html =404;  

```
location = /favicon.ico {
    log_not_found off;
    access_log off;
}

location = /robots.txt {
    allow all;
    log_not_found off;
    access_log off;
}

location /maintenance/ {
    deny all;
}

location ~ ^/(.+/|)\.(?!well-known/) {
    deny all;
}
```

sudo nano /etc/nginx/conf.d/furk4ngg.me.d/my_webapp.d/custom_headers.conf  
```
add_header Referrer-Policy "no-referrer-when-downgrade" always;
add_header Cross-Origin-Opener-Policy "same-origin" always;
add_header Cross-Origin-Resource-Policy "same-origin" always;
```

Test  
sudo nginx -t  

if its ok  
sudo systemctl reload nginx  or  sudo systemctl restart nginx  



# -Lufi Settings-  
Install Lufi with LDAP configuration? -> (Yes)  

# -Uptime Kuma-  
Choose SQLite database  

# -Umami Settings-  
visitors.domain -> all_users  
visitors.domain/api -> visitors  
visitors.domain/recorder -> visitors  
visitors.domain/script -> visitors  


# -Element Settings-  
Enable fedration features by default -> (Yes)  
chat.domain -> all_users  
chat.domain/bundles -> visitors  

syn.domain -> all_users  
syn.domain/_synapse -> visitors  
syn.domain/livekit -> visitors  
syn.domain/_matrix -> visitors  
syn.domain/.well-known/matrix -> visitors  

# -Gitea Settings-  
Enable LFS support on this instance -> (Yes)  
Enable support hover SSH protocol -> (Yes)  
git.domain -> all_users,visitors  
admin -> admins  
git.domain.megit.furk4ngg.me/v2 -> visitors  


# -Lime Survey-  
forms.domain/admin -> admin page  
https://forms.domain/index.php/dashboard/view

# -NextCloud Settings-  
Add the users' home directory in Nextcloud? -> (No)  

# -OnlyOffice Setup-  
Verify that the server settings were successfully updated  
Check healthcheck and api.js  
```
sudo yunohost service status | grep -i onlyoffice
curl https://docs.domain/healthcheck
curl -I https://docs.domain/web-apps/apps/api/documents/api.js
```
Verify that the secret keys match  
```
sudo grep -n -A5 -B5 "secret" /var/www/onlyoffice/config/local.json
```
Verify the SSL certificate  
Run the curl tests  
```
sudo -i

php /var/www/nextcloud/occ app:disable richdocuments
php /var/www/nextcloud/occ app:disable richdocumentscode
php /var/www/nextcloud/occ app:disable office
php /var/www/nextcloud/occ app:list | grep -i "onlyoffice\|richdocuments\|richdocumentscode\|office"
```
```
sudo apt update
sudo apt install php8.2-xml php8.2-mbstring php8.2-zip php8.2-gd php8.2-curl
sudo systemctl restart php8.2-fpm
php -m | grep -E "SimpleXML|mbstring|zip|gd|curl"
```
Verify occ  
```
sudo -i
php /var/www/nextcloud/occ status
```

Final diagnosis
```
sudo tail -100 /var/log/onlyoffice/docservice.log
```



Create a new document  
If the page loads forever, first open F12 → Console  
If analytics.js fails to load:  
    Disable Firefox Enhanced Tracking Protection or turn off uBlock, AdGuard, Brave Shields, or any similar blocker for this site  
CTRL + F5  

# -Mail Settings-  
(Cloudflare)  
*All Records should be DNS only*  
MX domain    mail.domain (Priority 10)  
TXT domain    "v=spf1 a mx -all"  
TXT _dmarc    "v=DMARC1; p=none"  
TXT mail._domainkey    "v=DKIM1; h=sha256; k=rsa; p='long value that you can see in diagnosis screen'"  
CAA domain    issue "letsencrypt.org"  
PTR domain    mail.domain  


sudo yunohost diagnosis run  

sudo yunohost service status | grep -E "postfix|dovecot|rspamd|opendkim"  




sudo postconf myhostname  
sudo grep -R "mail.domain" /etc/opendkim /etc/postfix /etc/dovecot 2>/dev/null  

Which ports are open? ->  
IMAP  
sudo ss -tln | grep -E ":143|:993"  
>143 → IMAP + STARTTLS  
>993 → IMAPS (SSL/TLS)  

SMTP  
sudo ss -tln | grep -E ":25|:465|:587"  
>✅ 25  
>❌ 465  
>✅ 587  


Is STARTTLS actually working? ->  
SMTP  
openssl s_client -starttls smtp -connect domain:587  
>Verify return code: 0 (ok)

IMAP  
openssl s_client -connect domain:993  
openssl s_client -starttls imap -connect domain:143  


Is SMTP Authentication working ->  
doveadm auth test furk4ngg@domain  
>auth succeeded


LDAP mail adresini görüyor mu? ->  
postmap -q "furk4ngg@domain" ldap:/etc/postfix/ldap-accounts.cf  


What is the mail server hostname? ->  
sudo postconf myhostname  
>myhostname = domain


Verify the mail domains ->  
sudo yunohost user info furk4ngg  
>mail:furk4ngg@domain


Which domains accept mail? ->  
sudo cat /etc/postfix/virtual-mailbox-domains  

Show the dedicated sender addresses used by applications (Nextcloud, Synapse, etc.) ->  
sudo postmap -s /etc/postfix/app_senders_login_maps  

## IMAP SETTINGS
>Admin page -> https://mail.domain//app/?admin  
>username:admin  
>password:/var/www/snappymail/app/data/_data_/_default_/admin_password.txt


Server: domain  

Port: 993  

Security: SSL/TLS  

Use short login: OFF  

Lowercase login: ON  

Require verification: ON  

Allow self signed: OFF  


## SMTP SETTINGS
Server: domain  

Port: 587  

Security: STARTTLS  

Use short login: OFF  

Lowercase login: ON  

Use authentication: ON  

Use login as sender: OFF  

Force AUTH PLAIN: OFF  

Use php mail(): OFF  

Require verification: ON  

Allow self signed: OFF  


DKIM 1024 bit uyarısı  
sudo opendkim-testkey -d domain -s mail -vvv  
1048 bit starts with MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQ...  
2048 bit starts with MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A...  

/baska maillerden yonlendirme hesabi/


# TEST ADRESSES
DNS Checker -> https://mxtoolbox.com/DNSLookup.aspx  
Validate your DKIM and SPF DNS records. -> https://dmarcdkim.com/tools/check-dkim-record?domain=domain  
Mail Tester -> https://www.mail-tester.com/  
MX Records and Blacklist check -> https://mxtoolbox.com/SuperTool.aspx  
SPF Test -> https://mxtoolbox.com/spf.aspx  
Domain Health Report -> https://mxtoolbox.com/emailhealth/  
SEO TEST -> https://www.seobility.net/en/seocheck/  
Web Page Quality -> https://pagespeed.web.dev/  


## If its all good  
dmarc@domain diye yeni hesap acmali yunohostta  
TXT _dmarc "v=DMARC1; p=reject; rua=mailto:dmarc@domain; adkim=s; aspf=s; pct=100"  
Test it -> https://easydmarc.com/tools/dmarc-lookup  


In your Server Hosting change (Reverse DNS Management>PTR Records(IPV4 to mail.domain / IPV6 to mail.domain))  

sudo cp /etc/postfix/main.cf /etc/postfix/main.cf.bak  
export TERM=xterm-256color  
sudo vim /etc/postfix/main.cf > myhostname = mail.domain  


sudo postconf myhostname  
>myhostname = mail.domain  

openssl s_client -starttls smtp -connect mail.domain:587  
Type>EHLO test 
>250-mail.domain  

PTR Test
dig -x <IPv4> +short  
>mail.domain  

If all three point to mail.domain, the mail configuration is fully consistent.



Emails appear under YunoHost → Users  
You can connect and use your mail with these mail providers:Thunderbird,Gmail,Outlook,Proton Mail or with your Webmail Client(Snappy Mail)  



# 📦 Wireguard VPN Setup
❌ In my opinion, this is not anonymity, since all tunnel traffic exits through a single VPS IP address  

✅ On public Wi-Fi, all traffic is encrypted until it reaches your VPS  

✅ You can securely access services such as Nextcloud, Gitea, and SSH  

✅ You can restrict SSH and management panels so they are only accessible through the VPN  

✅ You can also route DNS queries through your own server, preventing the local network from seeing them  


sudo apt update  
sudo apt install wireguard qrencode  
Test Et>wg --version  
>wireguard-tools v1.0.20210914 - https://git.zx2c4.com/wireguard-tools/  


export TERM=xterm-256color  
sudo nano /etc/sysctl.conf  
Make sure the following are present:  
>net.ipv4.ip_forward=1  
>net.ipv6.conf.all.forwarding=1  

sudo sysctl -p  


sudo -i  
sudo mkdir -p /etc/wireguard  
cd /etc/wireguard  


umask 077  
wg genkey > server_private.key  
wg pubkey < server_private.key > server_public.key  

chmod 600 server_private.key  
chmod 644 server_public.key  
Server Public Key -> sudo cat /etc/wireguard/server_public.key  
>We will use this key for future clients  


Server Private Key -> sudo cat /etc/wireguard/server_private.key  

ls -lah /etc/wireguard  
It should look similar to this:  
>server_private.key  
>server_public.key  
>wg0.conf  


ip route | grep default
If resoult has eth:

elif(ens18):



## Create wg0.conf 
export TERM=xterm-256color  
nano /etc/wireguard/wg0.conf  
```
[Interface]
Address = 10.8.0.1/24
ListenPort = 51820
PrivateKey = SERVER_PRIVATE_KEY

PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -A FORWARD -o wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -D FORWARD -o wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```
chmod 600 /etc/wireguard/wg0.conf  

Test -> ls -l /etc/wireguard  
For wg0.conf and server_private.key, you should see the following:  
>-rw-------  


IPV6 eklenecek  


## Servisi Etkinlestir
systemctl enable wg-quick@wg0  
systemctl start wg-quick@wg0  

Test>  
systemctl status wg-quick@wg0 --no-pager  
wg  
ip addr show wg0  


sudo yunohost firewall list  
sudo yunohost firewall allow UDP 51820  
sudo yunohost firewall reload  

sudo ss -lun | grep 51820  
>*:51820  




<details>
<summary>Create Client Key</summary>

   <details>
   <summary>For Mobile Clients</summary>
       
   ## Create Mobile Client Key  
       
   cd /etc/wireguard  
   umask 077  
   wg genkey | tee phone_private.key > /dev/null  
   wg pubkey < phone_private.key > phone_public.key  
   chmod 600 phone_private.key  
   chmod 644 phone_public.key  
        
   Test -> ls -l phone_*  
   >-rw------- phone_private.key  
   >-rw-r--r-- phone_public.key  
        
        
   Phone Public Key -> sudo cat phone_public.key  
        
   export TERM=xterm-256color  
   nano /etc/wireguard/wg0.conf  
   Add this into end of the page  
```
   [Peer]
   # Phone
   PublicKey = PHONE_PUBLIC_KEY
   AllowedIPs = 10.8.0.2/32
```
   systemctl restart wg-quick@wg0  
   wg  
   >peer: XXXXXXXXXXXXXXXXXXXXXXXXXXXXX  
   >allowed ips: 10.8.0.2/32  
        
   ## Phone.conf
   Phone Private Key -> sudo cat phone_private.key  
   Server Public Key -> sudo cat /etc/wireguard/server_public.key  
   export TERM=xterm-256color  
   nano /etc/wireguard/phone.conf  
```
   [Interface]
   PrivateKey = PHONE_PRIVATE_KEY
   Address = 10.8.0.2/24
   DNS = 1.1.1.1
    
   [Peer]
   PublicKey = SERVER_PUBLIC_KEY
   Endpoint = VPS_IP:51820
   AllowedIPs = 0.0.0.0/0, ::/0
   PersistentKeepalive = 25
```
   QR Code -> qrencode -t ansiutf8 < /etc/wireguard/phone.conf  

   
     
   Resmi WireGuard uygulamasını yükle  
   Add Tunel(+)  
   Scan the QR code  
   🎉 You are ready to use yor VPN  

   ## Delete Mobile Client Key
```
   rm -f \
   /etc/wireguard/phone_private.key \
   /etc/wireguard/phone_public.key \
   /etc/wireguard/phone.conf
```
   and delete this block in wg0.conf  
```
   [Peer]
   # Phone
   PublicKey = PHONE_PUBLIC_KEY
   AllowedIPs = 10.8.0.2/32
```
   sudo systemctl restart wg-quick@wg0  
   </details>
   

   <details>
   <summary>For Desktop Clients</summary>
   
   <details>
   <summary>Arch</summary>
       sudo pacman -S wireguard-tools
   </details>
       
   <details>
   <summary>Debian/Ubuntu/Raspberry Pi OS</summary>
        sudo apt install wireguard
   </details>
    
   <details>
   <summary>Fedora</summary>
        sudo dnf install wireguard-tools
   </details>

   ## Create Desktop Client Key

   cd /etc/wireguard  
   umask 077  
   wg genkey | tee desktop_private.key > /dev/null  
   wg pubkey < desktop_private.key > desktop_public.key  
   chmod 600 desktop_private.key  
   chmod 644 desktop_public.key  

   Test -> ls -l phone_*  
   >-rw------- desktop_private.key  
   >-rw-r--r-- desktop_public.key  


   Desktop Public Key -> sudo cat desktop_public.key  

   export TERM=xterm-256color  
   nano /etc/wireguard/wg0.conf  
   Add this into end of the page  
```
   [Peer]
   # Desktop
   PublicKey = DESKTOP_PUBLIC_KEY
   AllowedIPs = 10.8.0.3/32
```

   systemctl restart wg-quick@wg0  
   wg  
   >peer: XXXXXXXXXXXXXXXXXXXXXXXXXXXXX  
   >allowed ips: 10.8.0.3/32  


   ## desktop.conf
   Desktop Private Key -> sudo cat desktop_private.key  
   Server Public Key -> sudo cat /etc/wireguard/server_public.key  

   In your PC  
   sudo nano /etc/wireguard/wg0.conf  

```
   [Interface]
   PrivateKey = DESKTOP_PRIVATE_KEY
   Address = 10.8.0.3/24
   PreUp = ip route add VPS_IP/32 via 192.168.1.1 dev enp11s0
   PostDown = ip route del VPS_IP/32 via 192.168.1.1 dev enp11s0
        
   [Peer]
   PublicKey = SERVER_PUBLIC_KEY
   Endpoint = VPS_IP:51820
   AllowedIPs = 0.0.0.0/0
   PersistentKeepalive = 25
```


   sudo chmod 600 /etc/wireguard/wg0.conf
    
   Open VPN -> sudo wg-quick up wg0
   Close VPN -> sudo wg-quick down wg0
   Status -> sudo wg
    
   Otomatic Connect when Pc is opened -> sudo systemctl enable wg-quick@wg0
   Close that option -> sudo systemctl disable wg-quick@wg0
    
   If you get connection error delete 'DNS = 1.1.1.1' from your desktop.conf and try again
   sudo wg-quick up wg0 

   🎉 You are ready to use yor VPN  

   ## Delete Mobile Client Key
```
   rm -f \
   /etc/wireguard/desktop_private.key \
   /etc/wireguard/desktop_public.key \
   /etc/wireguard/desktop.conf
```
   and delete this block in wg0.conf  
```
   [Peer]
   # Desktop
   PublicKey = DESKTOP_PUBLIC_KEY
   AllowedIPs = 10.8.0.3/32
```
   sudo systemctl restart wg-quick@wg0  
   </details>
</details>




wg  
>peer: XXXXXXXXXXXXX  
>latest handshake: 5 seconds ago  
>transfer: 120 KiB received, 90 KiB sent  


Test -> https://ifconfig.me  
>You should see the VPS_IP

DNS Leak Test -> https://browserleaks.com/dns  

⚠️ Eger yeni cihaz clienti eklemek istiyorsan yeni key olusturup VPN Agini(10.8.0.2, 10.8.0.3, 10.8.0.4...) degistirip yukaridaki adimlari phone/desktop yerine baska isimlendirmeler yaparak izleyebilirsin.  


phone.conf/desktop.conf  
Full Tunnel Mode -> AllowedIPs = 0.0.0.0/0 Bütün internet trafiğin VPN'den geçer.  

Split Tunnel Mode -> AllowedIPs = 10.8.0.0/24 Sadece sunucuna ait trafik VPN'den geçer.  


## Delete Server Key
```
   rm -f \
   /etc/wireguard/server_private.key \
   /etc/wireguard/server_public.key \
   /etc/wireguard/wg0.conf
```
   sudo systemctl disable --now wg-quick@wg0  
