# Config for automated nginx proxy

repo for this config - https://github.com/nginx-proxy/nginx-proxy

---
Default file structure.

```
proxy/
├─ acme/
├─ certs/
├─ conf/
├─ html/
├─ vhost/
├─ nginx.tmpl
├─ docker-compose.yml
```

---

Run this comand to create the docker network

```shell
sudo docker network create nginx-proxy
```

---

Sample docker-compose file
```yml
version: '3'

services:
  cointainer:
    image: my_proxied_container
    expose:
      - "80"
    environment:
      VIRTUAL_HOST: example.mysite.tld #under which domain should this container be reachable.
      VIRTUAL_PORT: 80
      LETSENCRYPT_HOST: example.mysite.tld #which domain request LE cert for
    restart: unless-stopped
    networks:
      - proxy

networks:
  proxy:
    external:
      name: nginx-proxy
```