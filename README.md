# Server-Setup

*Terminal icerisinde yukari asagi yapmak icin 'Shift+Page_Up' ve 'Shift+Page_Down' tuslarini kullanabilirsin*  
*'sudo dmesg -D' kodu ile terminale yazirilan auid yazilarini o oturum icin kisa sureli durdurur*  

BASE(Yunohost)  

Video-Voice Call(Element,Synapse)  
Rss(FreshRSS)  
Github Repo Clone(Gitea)  
Realtime Server Monitoring(Glances)  
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


-SETUP-  

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


-CLOUDFLARE-  
Sign In into cloudflare and chose DNS Records>Connect a Domain  
Configure AI training & search policies:
Search:Allow  
Agent:Allow  
Training:Block  
Import DNS Records:Manual only  
A domain IPV4  
AAAA domain IPV6  
CNAME www furk4ngg.me  
MX domain domain (Priority 10)  
TXT domain "v=spf1 a mx -all"  
TXT _dmarc "v=DMARC1; p=none"  
TXT mail._domainkey "v=DKIM1; h=sha256; k=rsa; p='long value that you can see in diagnosis screen'"  

A rss IPV4  
AAAA rss IPV6  


In your domain provider,update your nameserver based on cloudflare nameserver such as 'sam.ns.cloudflare.com' and 'kack.ns.cloudflare.com'  

hostnamectl set-hostname domain  

apt update && apt full-upgrade -y  

curl https://install.yunohost.org | bash  

Localden yunohost islemlerini yaptiktan sonra actigimiz dns kayitlari icin su domaineri aciyoruz:  
my_webapp -> domain  
Nextcloud -> cloud.domain  
Vert -> convert.domain  
Gitea -> git.domain  
Uptime Kuma -> uptime.domain  
Glances -> usage.domain  
Element -> chat.domain and Synapse -> syn.domain  
Lufi -> lufi.domain  
Pair Drop -> pair.domain  
FressRSS -> rss.domain  
Only Office -> docs.domain  
Umami -> visitors.domain  


-Permissions-  

Element -> all_users(Tum yunohost kullanicilari)  
Synapse -> all_users(Tum yunohost kullanicilari)  
Glances -> all_users(Tum yunohost kullanicilari)  
Gitea -> visitors,all_users  
Lufi -> all_users(Tum yunohost kullanicilari)  
my_webapp -> visitors,all_users  
Pair Drop -> all_users(Tum yunohost kullanicilari)  
Umami(visitors.domain) -> all_users(Tum yunohost kullanicilari)  
Uptime Kuma -> all_users(Tum yunohost kullanicilari)  
Vert -> all_users(Tum yunohost kullanicilari)  


Display tile in portal -> (Yes)  




-URL CERTIFICATES-

✅ DNS A kaydı doğru.  
✅ DNS AAAA kaydı doğru.  
✅ Nameserver'lar değiştirildi.  
✅ HTTP reachable from outside.  
✅ Nginx çalışıyor.  
✅ Let's Encrypt API'sine sunucu bağlanabiliyor.  
✅ Cloudflare proxy kapalı (DNS only).  
Nameserver değişikliğinin tamamen yayılmamış olmasının ardından,  

sudo yunohost domain cert install  




-Web App Ayarlari   (index.html yerine index yazinca calismasi icin)-  
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



-Lufi Ayarlari-  
Install Lufi with LDAP configuration? -> (Yes)  

-Uptime Kuma-  
Choose SQLite database  

-Umami Ayarlari-  
visitors.domain -> all_users  
visitors.domain/api -> visitors  
visitors.domain/recorder -> visitors  
visitors.domain/script -> visitors  


-Element Ayarlari-  
Enable fedration features by default -> (Yes)  
chat.domain -> all_users  
chat.domain/bundles -> visitors  

syn.domain -> all_users  
syn.domain/_synapse -> visitors  
syn.domain/livekit -> visitors  
syn.domain/_matrix -> visitors  
syn.domain/.well-known/matrix -> visitors  

-Gitea Ayarlari-  
Enable LFS support on this instance -> (Yes)  
Enable support hover SSH protocol -> (Yes)  
git.domain -> all_users,visitors  
admin -> admins  
git.domain.megit.furk4ngg.me/v2 -> visitors  


-Lime Survey-  
forms.domain/admin -> admin page  

-LimeSurvey Ayarlari-  
https://forms.domain/index.php/dashboard/view

-NextCloud Ayarlari-  
Add the users' home directory in Nextcloud? -> (No)  

-OnlyOffice Setup-  
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

-Mail Ayarlari-  
sudo yunohost service status | grep -E "postfix|dovecot|rspamd|opendkim"  
In your Server Hosting change (Reverse DNS Management>PTR Records)  


CAA issue "letsencrypt.org"

https://dmarcdkim.com/tools/check-dkim-record?domain=furk4ngg.me
DKIM 1024 bit uyarısı
