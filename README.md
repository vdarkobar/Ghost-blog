# Ghost-blog
## deploy Ghost blog 
### 

##### Create docker network
```
sudo docker network create ghost
```
### Clone this git repository:
```
echo -n "Enter directory name: "; read NAME; mkdir -p "$NAME"; cd "$NAME" \
&& git clone https://vdarkobar:2211620c9da5dab0c7bb77e9aeb02087d293b293@github.com/vdarkobar/Ghost-blog.git .
```
##### Change domain name
```
sudo nano docker-compose.yml
sudo nano ghost/config.production.json
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
