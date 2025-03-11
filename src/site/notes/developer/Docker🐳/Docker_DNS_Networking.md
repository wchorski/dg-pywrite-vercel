---
{"dg-publish":true,"permalink":"/developer/Docker🐳/Docker_DNS_Networking/","dgPassFrontmatter":true}
---

#docker #docker-compose #DNS

[Docker Networks  DNS and How Containers Find Each Other](https://www.youtube.com/watch?v=2Fg2NfDZhmE)

- using docker-compose, a container automatically gets a **DNS name** matching it's **container name** 
- join containers together with a common network i.e.
`compose.yml`
```
version: "3.7"

services:

  obsidianDB_dnd:
    image: couchdb:3.1.2
    restart: always
    ports:
      - "5984:5984"
    networks:
      - nginx-prox-mgmt-3_default
    environment:
      - COUCHDB_USER=admin
      - COUCHDB_PASSWORD=pass
    volumes:
      - /mnt/uasis5/obsidian-livesync-data/dnd:/opt/couchdb/data
      - ./local.ini:/opt/couchdb/etc/local.ini

networks:
  nginx-prox-mgmt-3_default:
    external: true
```
