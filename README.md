# Server-Setup

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
IPv6  
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


In your domain provider,update your nameserver based on cloudflare nameserver such as 'kim.ns.cloudflare.com' and 'mack.ns.cloudflare.com'  

hostnamectl set-hostname domain  

apt update && apt full-upgrade -y  

curl https://install.yunohost.org | bash  
