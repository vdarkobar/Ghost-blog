# Ghost-blog
  
<p align="left">
  <a href="https://github.com/vdarkobar/Home_Cloud#small-home-cloud">Home</a> |
  <a href="https://github.com/vdarkobar/NextCloud#nextcloud">NextCloud</a> |
  <a href="https://github.com/vdarkobar/Bitwarden#bitwarden">Bitwarden</a> |
  <a href="https://github.com/vdarkobar/WordPress#wordpress">WordPress</a> |
  <a href="https://github.com/vdarkobar/Portainer">Joomla</a>  
  <br><br>
</p>   
  
Login to <a href="https://dash.cloudflare.com/">CloudFlare</a> and set Domain name, or Domain name and Subdomain for your Ghost-blog.
```
    A | example.com | YOUR WAN IP
```
or:
```
    A | example.com | YOUR WAN IP
```
```
    CNAME | subdomain | @ (or example.com)
```

---
  
### Create docker network:
```
sudo docker network create ghost
```
### Clone WordPress Git Repository:
```
RED='\033[0;31m'; echo -n "Enter directory name: "; read NAME; mkdir -p "$NAME"; cd "$NAME" \
&& git clone https://github.com/vdarkobar/Ghost-blog.git .
```
  
#### *Decide what you will use for*:
```
Subdomain,
Domain name.
```
  
### Select and run all at once. Enter required data:
*Only works once, on wrong data input delete folder and clone again*.
```
clear
RED='\033[0;31m'
echo -ne "${RED}Enter Time Zone: "; read TZONE; \
echo -ne "${RED}Enter Domain name: "; read DNAME; \
sed -i "s|01|${TZONE}|" .env && \
sed -i "s|02|${DNAME}|" .env && \
echo | openssl rand -base64 48 > secrets/mysql_root_password.secret && \
echo | openssl rand -base64 20 > secrets/wp_mysql_password.secret && \
sudo chown -R root:root secrets/ && \
sudo chmod -R 600 secrets/
```
  
### Start:  
*Change Container names (docker-compose.yml), if multiple instances are planed.*
```
sudo docker-compose up -d
```
### Log:
```
sudo docker logs -tf --tail="50" ghost
```

### Dynamic config (Traefik VM):
Create file: *service_name.yml* in Traefik: */data/configurations/* folder for routing and to get a free SSL certificate.
```
  # All Routers
  routers:

    # Ghost router
    Ghost:
      service: ghost-service
      middlewares:
        - www-redirect@file
      entryPoints:
        - "websecure"
      rule: "Host(`domain`) || Host(`www.domain`)" # adjust domain and www.domain


  # All Services
  services:

    # Ghost service
    ghost-service:
      loadBalancer:
        servers:
          - url: "http://local-ip:2368" # adjust ip and port nummber (fixed port?!)

```

### Config your own email:
```
# 
"mail": {
    "from": "'Ghost Support' <YOUR_EMAIL>",
    "transport": "SMTP",
    "options": {
        "host": "smtp.exmail.qq.com",
        "port": 465,
        "secureConnection": true,
        "auth": {
          "user": "YOUR_EMAIL",
          "pass": "YOUR_EMAIL_PASSWORD"
        }
    }
}
```
