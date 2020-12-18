# Ghost-blog
## deploy Ghost blog 
### (router/service/port enabled using Labels in docker-compose.yml)


##### Create docker network
```
sudo docker network create proxy
```

### Clone this git repository:
```
git clone https://vdarkobar:2211620c9da5dab0c7bb77e9aeb02087d293b293@github.com/vdarkobar/Ghost-blog.git
```

##### Change domain name
```
sudo nano Ghost-blog/docker-compose.yml
sudo nano Ghost-blog/ghost/config.production.json
```
##### Start
```
sudo docker-compose -f Ghost-blog/docker-compose.yml up -d
```
##### Log
```
sudo docker logs -tf --tail="50" Ghost-blog
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
