---
title: "Docker Compose 部署 plumelog 日志"
date: 2022-05-10
tags: ["Docker Compose", "Docker"]
draft: false
---

## DockerCompose.yml
```yaml
services:
  redis:
    image: redis
    ports:
      - "6379:6379"
    command: redis-server --requirepass YW54aW5AcmVkaXM=
    networks:
      - saas-manage-net
  elasticsearch:
    image: elasticsearch:7.17.0
    ports:
      - "9200:9200"
      - "9300:9300"
    environment:
      discovery.type: single-node
    networks:
      - saas-manage-net
  plumelog-v3.5:
    image: xiaobai021sdo/plumelog:3.5
    ports:
      - "8891:8891"
    volumes:
      - ./plumelog-server/config/application.properties:/plumelog-server/config/application.properties:ro
      - ./logs:/plumelog-server-3.5/logs
    networks:
      - saas-manage-net
  nginx:
    image: nginx:1.21.6
    ports:
      - "80:80"
    volumes:
      - ./nginx-config:/etc/nginx/conf.d
    networks:
      - saas-manage-net
networks:
  saas-manage-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 174.17.0.0/16
          gateway: 174.17.0.1

```