# Config for automated nginx proxy

repo for this config - https://github.com/nginx-proxy/nginx-proxy

---
Default file structure.

```bash
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
      VIRTUAL_HOST: example.mysite.tld  #under which domain should this container be reachable
      VIRTUAL_PORT: 80  #tells which port to forward traffic to
      LETSENCRYPT_HOST: example.mysite.tld  #which domain request LE cert for
    restart: unless-stopped
    networks:
      - proxy

networks:
  proxy:
    external:
      name: nginx-proxy
```

---

Multicontainer application

```yml
version: '3'

services:

  frontend:
    image: frontend_image
    expose:
      - "8888"
    environment:
      - VIRTUAL_HOST=example.mysite.tld
      - VIRTUAL_PORT=8888
      - LETSENCRYPT_HOST=example.mysite.tld
    networks:
      - proxy
      - app

  backend:
    image: backend_image
    networks:
      - app

networks:
  proxy:
    external:
      name: nginx-proxy
  app:

```
