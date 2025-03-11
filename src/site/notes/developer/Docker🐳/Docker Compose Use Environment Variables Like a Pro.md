---
{"tags":["nodejs","yaml","docker","docker-compose"],"dg-publish":true,"permalink":"/developer/Docker🐳/Docker Compose Use Environment Variables Like a Pro/","dgPassFrontmatter":true}
---

Finding yourself building, rebuilding, and deploying [[developer/Home Lab/Docker\|Docker]] containers? My stack of [[developer/NodeJS/NodeJS\|NodeJS]] projects have started to unruly, and I need a better way to deploy my apps with little fuss. 

## Env Vars

Keep your environment variables in one place. You'll start pulling your hair out if they're split between `.env`, `compose.yml`, `envs.ts`, etc.  I recommend keeping them all in `.env`

`.env`
```yml
NODE_ENV="production"

IMAGE_FRONTEND_NAME="cutefruit-kypn-frontend"
IMAGE_BACKEND_NAME="cutefruit-kypn-backend"

SITE_TITLE="MY COOL SITE"
SITE_DESC="Learn how to use env like a pro"
ADMIN_EMAIL_ADDRESS="Enviroment@Variables.lan"
```
### import in compose.yml
```yml
version: '3'

services:

  backend:
    container_name: ${IMAGE_BACKEND_NAME}
    image: ${IMAGE_BACKEND_NAME}:latest 
    restart: unless-stopped
    build: 
      context: ./
      dockerfile: ./Dockerfile.backend
    # ports: # expose this if you're not using a proxy
    #   - 3001:3001
    networks:
      - this

  frontend:
    container_name: ${IMAGE_FRONTEND_NAME}
    image: ${IMAGE_FRONTEND_NAME}:latest # rename ALL 'cutefruit-kypn' to a unique name 
    restart: unless-stopped
    build: 
      context: ./
      dockerfile: ./Dockerfile.frontend
    # ports: # expose this if you're not using a proxy
    #   - 3000:3000
    networks:
      - this

networks:    
  this:
    name: cutefruit-kypn-network
    driver: bridge  
    external: true 

```

If all you tweaked was the `.env` file, then just hit the container with the `docker compose up -d` and it should replace the new values

### Import in Bash Scripts
```shell
#!/bin/bash

source .env

# if you're having stale configuration/environment issues, 
# add the --no-cache argument
# docker compose build --no-cache
docker compose build 

docker save -o "./images/$IMAGE_FRONTEND_NAME.tar" $IMAGE_FRONTEND_NAME
docker save -o "./images/$IMAGE_BACKEND_NAME.tar" $IMAGE_BACKEND_NAME
```

Excellent for bash scripting. In the above example I'm setting the file name to match the container and image name `${IMAGE_FRONTEND_NAME}`

## Import in Javascript, Typescript

Wrap your env variables in a `envs.ts` has benefits
- autocomplete intellesense
- import into other `.js` `.ts` files with ease
- set default values 
- check if value is non-null `!`. Like a *required* attribute

```ts
// FRONTEND      
const SITE_TITLE          = process.env.NEXT_PUBLIC_SITE_TITLE! 
const SITE_DESC          = process.env.NEXT_PUBLIC_SITE_DESC! 
const ADMIN_EMAIL_ADDRESS = process.env.NEXT_PUBLIC_ADMIN_EMAIL_ADDRESS!

export const envs = {
  SITE_TITLE,
  SITE_DESC,
  ADMIN_EMAIL_ADDRESS,
} 
```

## Import
## Credits
- [How to reload environment variables in docker-compose container with minimum downtime? - Stack Overflow](https://stackoverflow.com/questions/42149529/how-to-reload-environment-variables-in-docker-compose-container-with-minimum-dow)

- [[developer/Home Lab/Docker\|Docker]]
- [[developer/developer_box📦\|developer_box📦]]
- [[developer/NodeJS/NodeJS\|NodeJS]]