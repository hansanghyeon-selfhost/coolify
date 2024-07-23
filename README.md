# coolify
An open-source &amp; self-hostable Heroku / Netlify / Vercel alternative.

## 아키텍쳐

![image](https://github.com/user-attachments/assets/7561aa43-2601-40ae-a1fb-702b131b74cb)


## install

https://coolify.io/docs/installation#installation

설치를 메뉴얼 설치로진행한다. coolify 대시보드 배포를 traefik으로 하기위해서.

여기에 `source/docker-compose.override.yml` 파일을 추가한다 treafik 설정

```yml
services:
  coolify:
    restart: always
    networks:
      - traefik_proxy
      - coolify
    labels:
      - traefik.enable=true
      - traefik.http.routers.coolify.entrypoints=https
      - traefik.http.routers.coolify.rule=Host(`coolify.hyeon.pro`)
      - traefik.http.routers.coolify.tls.certresolver=leresolver
      - traefik.http.services.coolify.loadbalancer.server.port=80
  soketi:
    networks:
      - traefik_proxy
      - coolify
    labels:
      - traefik.enable=true
      - traefik.http.routers.coolify-wss.entrypoints=https
      - traefik.http.routers.coolify-wss.rule=Host(`coolify.hyeon.pro`) && Headers(`Upgrade`, `websocket`)
      - traefik.http.routers.coolify-wss.tls.certresolver=leresolver
      - traefik.http.services.coolify-wss.loadbalancer.server.port=6001

########################### NETWORKS
networks:
  traefik_proxy:
    external: true
  coolify:
        name: coolify
        driver: bridge
        external: true
```
