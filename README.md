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
  
### Create Docker network:
```
sudo docker network create ghost
```
### Clone Ghost-blog Git Repository:
```
RED='\033[0;31m'; echo -ne "${RED}Enter directory name: "; read NAME; mkdir -p "$NAME"; cd "$NAME" \
&& git clone https://github.com/vdarkobar/Ghost-blog.git .
```
  
#### *Decide what you will use for*:
```
Domain name,
Subdomain (if planned),
Ghost-blog Port Number.
```
  
### Select and run all at once. Enter required data:
*Only works once, on wrong data input delete folder and clone again*.
```
clear
RED='\033[0;31m'
echo -ne "${RED}Enter Domain name: "; read DNAME; \
echo -ne "${RED}Enter Subdomain with . (dot) at the end, or just press Enter to default to Domain name: "; read SDNAME; \
echo -ne "${RED}Enter Ghost-blog Port Number (GPORTN:2368): "; read GPORTN; \
sed -i "s|01|${DNAME}|" ghost/config.production.json && \
sed -i "s|02|${SDNAME}|" ghost/config.production.json && \
sed -i "s|01|${DNAME}|" .env && \
sed -i "s|02|${SDNAME}|" .env && \
sed -i "s|03|${GPORTN}|" .env && \
rm README.md
```
  
### Start:  
*Change Container names/Port numbers, if multiple instances are planed.*
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
http:

  # All Routers
  routers:

    # Ghost-blog router
    ghost-blog:
      service: ghost-blog-service
      middlewares:
#        - www-redirect@file # Uncomment, give it a unique name. Set the same name in WWW-Redirect (middlewares.yml) if using domain name only.
      entryPoints:
        - "websecure"
      rule: "Host(`subdomain.example.com`)" # comment out if using domain name
#      rule: "Host(`example.com`) || Host(`www.example.com`)" # comment out if using subdomain, used for non-www to www redirect


  # All Services
  services:

    # Ghost service
    ghost-blog-service:
      loadBalancer:
        servers:
          - url: "http://local-ip:GPORTN" # adjust ip and port number

```
  
### Middlewares *(Traefik VM)*:
Add to: *middlewares.yml* in Traefik: */data/configurations/* for non-www to www redirect  
  
* *If using Domain name for Ghost-blog*
```
http:

  # All middlewares
  middlewares:
  
    # WWW-Redirect
    www-redirect: # match the name from Ghost-blog router in service_name.yml
      redirectRegex:
        regex: "^https://example.com/(.*)"
        replacement: "https://www.example.com/${1}"
```  
  
### Option to config your own email:
Add to *ghost/config.production.json* file.
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
  
<p align="center">
<a href="https://github.com/vdarkobar/Ghost-blog#ghost-blog">top of the page</a>
</p>
