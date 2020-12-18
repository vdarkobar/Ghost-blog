# Ghost-blog
## deploy Ghost blog

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
```
##### Start
```
sudo docker-compose -f Ghost-blog/docker-compose.yml up -d
```
##### Log
```
sudo docker logs -tf --tail="50" Ghost-blog
```
```
# 


```
