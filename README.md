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
  
##### Create docker network
```
sudo docker network create ghost
```
### Clone this git repository:
```
echo -n "Enter directory name: "; read NAME; mkdir -p "$NAME"; cd "$NAME" \
&& git clone https://github.com/vdarkobar/Ghost-blog.git .
```
##### Change domain name
```
sudo nano docker-compose.yml
sudo nano ghost/config.production.json
```
  
##### Dynamic config
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

##### Start
```
sudo docker-compose up -d
```
##### Log
```
sudo docker logs -tf --tail="50" ghost
```
##### Config your own email
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
