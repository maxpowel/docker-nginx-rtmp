# docker-nginx-rtmp

Nginx with rtmp and some custom modules. It is meant to be used in a multi stage build like this

```
FROM maxpowel/nginx-rtmp as nginx
FROM ubuntu
COPY --from=nginx /usr/bin/nginx /usr/bin/nginx
COPY my_nginx.conf /etc/nginx.conf
CMD ["/usr/bin/nginx", "-g", "daemon off;" "-c", "/etc/nginx.conf"]
```
You can use the example config file called `my_nginx.conf` for reference

# Build

Just run docker
```
docker buildx build -t gocoder --load . -f ./DockerfileAgent
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t maxpowel/nginx-rtmp:$VERSION --push .
```

Where $VERSION is the nginx version (or latest for the most recent one)
