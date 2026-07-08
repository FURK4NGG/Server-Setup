# 👀 Server-Setup Overview

This repository helps you build a complete self-hosted environment on your own VPS or home server. Host your own file server, Git repositories, video conferencing platform, online office suite, RSS reader, file sharing service, uptime monitoring, forms, synchronization tools, email server, and many other applications—all under your own control.  

*Terminal icerisinde yukari asagi yapmak icin 'Shift+Page_Up' ve 'Shift+Page_Down' tuslarini kullanabilirsin*  
*'sudo dmesg -D' kodu ile terminale yazirilan auid yazilarini o oturum icin kisa sureli durdurur*  

BASE(Yunohost)  

Video-Voice Call(Element,Synapse)  
Rss(FreshRSS)  
Github Repo Clone(Gitea)  
Realtime Server Monitoring(Glances)  
(Lime Survey)
Share files and notes with outserver safely(Lufi)  
Run your 'html,css,js' page(my_webapp)  
Store Files,Notes,Calendar,Forms,Photos and OnlyOffice(Nextcloud,OnlyOffice)  
Local file sharing(PairDrop)  
Realtime website analytics(Umami)  
Realtime apps'web urls' monitoring(Uptime Kuma)  
File convertion utility(Vert)  

vpn  
mailserver  

Real VNC Viewer(For connecting to server easily)  
File Zilla(For upload your website documents to server)  

DNS LOOKUP and google PageSpeed Insights  


# -SETUP-  

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


# -CLOUDFLARE-  
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

A rss IPV4  
AAAA rss IPV6  


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
Vert -> convert.domain  




# -App Permissions-  
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
Vert -> all_users  


Display tile in portal -> (Yes)  




# -URL CERTIFICATES-

✅ DNS A kaydı doğru.  
✅ DNS AAAA kaydı doğru.  
✅ Nameserver'lar değiştirildi.  
✅ HTTP reachable from outside.  
✅ Nginx çalışıyor.  
✅ Let's Encrypt API'sine sunucu bağlanabiliyor.  
✅ Cloudflare proxy kapalı (DNS only).  
Nameserver değişikliğinin tamamen yayılmamış olmasının ardından,  

sudo yunohost domain cert install  




# -Web App Ayarlari   (index.html yerine index yazinca calismasi icin)-  
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



# -Lufi Ayarlari-  
Install Lufi with LDAP configuration? -> (Yes)  

# -Uptime Kuma-  
Choose SQLite database  

# -Umami Ayarlari-  
visitors.domain -> all_users  
visitors.domain/api -> visitors  
visitors.domain/recorder -> visitors  
visitors.domain/script -> visitors  


# -Element Ayarlari-  
Enable fedration features by default -> (Yes)  
chat.domain -> all_users  
chat.domain/bundles -> visitors  

syn.domain -> all_users  
syn.domain/_synapse -> visitors  
syn.domain/livekit -> visitors  
syn.domain/_matrix -> visitors  
syn.domain/.well-known/matrix -> visitors  

# -Gitea Ayarlari-  
Enable LFS support on this instance -> (Yes)  
Enable support hover SSH protocol -> (Yes)  
git.domain -> all_users,visitors  
admin -> admins  
git.domain.megit.furk4ngg.me/v2 -> visitors  


# -Lime Survey-  
forms.domain/admin -> admin page  
https://forms.domain/index.php/dashboard/view

# -NextCloud Ayarlari-  
Add the users' home directory in Nextcloud? -> (No)  

# -OnlyOffice Setup-  
Server settings successfully updated doğrula.  
healthcheck ve api.js kontrol et.  
```
sudo yunohost service status | grep -i onlyoffice
curl https://docs.domain/healthcheck
curl -I https://docs.domain/web-apps/apps/api/documents/api.js
```
Secret key eşitliğini doğrula.  
```
sudo grep -n -A5 -B5 "secret" /var/www/onlyoffice/config/local.json
```
SSL sertifikası  
curl testleri  
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
occ dogrula  
```
sudo -i
php /var/www/nextcloud/occ status
```

son teşhis
```
sudo tail -100 /var/log/onlyoffice/docservice.log
```



Yeni belge aç.  
Sonsuz yüklenme varsa ilk iş F12 → Console aç.  
Analytics.js yüklenemiyor görülürse:  
    Firefox Enhanced Tracking Protection'ı kapat veya uBlock/AdGuard/Brave Shields gibi engelleyicileri bu site için devre dışı bırak.  
CTRL + F5  

# -Mail Ayarlari-  
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
In your Server Hosting change (Reverse DNS Management>PTR Records(ip_adress to domain))  


ptr test  
dig -x IPV4 +short  



sudo postconf myhostname  
sudo grep -R "mail.domain" /etc/opendkim /etc/postfix /etc/dovecot 2>/dev/null  

Hangi portların açık olduğu ->  
IMAP  
sudo ss -tln | grep -E ":143|:993"  
>143 → IMAP + STARTTLS  
>993 → IMAPS (SSL/TLS)  

SMTP  
sudo ss -tln | grep -E ":25|:465|:587"  
>✅ 25  
>❌ 465  
>✅ 587  


STARTTLS gerçekten çalışıyor mu ->  
SMTP  
openssl s_client -starttls smtp -connect domain:587  
>Verify return code: 0 (ok)

IMAP  
openssl s_client -connect domain:993  
openssl s_client -starttls imap -connect domain:143  


SMTP Authentication çalışıyor mu? ->  
doveadm auth test furk4ngg@domain  
>auth succeeded


LDAP mail adresini görüyor mu? ->  
postmap -q "furk4ngg@domain" ldap:/etc/postfix/ldap-accounts.cf  


Mail sunucusunun hostname'i ->  
sudo postconf myhostname  
>myhostname = domain


Mail domainlerini kontrol ->  
sudo yunohost user info furk4ngg  
>mail:furk4ngg@domain


Hangi domainler mail kabul eder ->  
sudo cat /etc/postfix/virtual-mailbox-domains  

Bu uygulamaların (Nextcloud, Synapse vb.) kullandığı özel gönderen adreslerini gösterir. ->  
sudo postmap -s /etc/postfix/app_senders_login_maps  


# IMAP SETTINGS
Server: domain  

Port: 993  

Security: SSL/TLS  

Use short login: OFF  

Lowercase login: ON  

Require verification: ON  

Allow self signed: OFF  


# SMTP SETTINGS
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
https://dmarcdkim.com/tools/check-dkim-record?domain=domain  
https://www.mail-tester.com/  
and Blacklist check -> https://mxtoolbox.com/SuperTool.aspx  
SPF Test -> https://mxtoolbox.com/spf.aspx  
Domain Health Report -> https://mxtoolbox.com/emailhealth/  
SEO TEST -> https://www.seobility.net/en/seocheck/  


If its all good  
dmarc@domain diye yeni hesap acmali yunohostta  
TXT _dmarc "v=DMARC1; p=reject; rua=mailto:dmarc@domain; adkim=s; aspf=s; pct=100"  
Test it -> https://easydmarc.com/tools/dmarc-lookup  


Maillerin Yunohost>Users icinde gozukur  
You can connect and use your mail with these mail providers:Thunderbird,Gmail,Outlook,Proton Mail
