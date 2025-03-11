---
{"dg-publish":true,"permalink":"/developer/Docker🐳/Build small docker image sizes/","dgPassFrontmatter":true}
---

I was building a [[developer/NextJS/NextJS\|NextJS]] image with [[developer/Home Lab/Docker\|Docker]] and ended up with over **6gb** file sizes. 

> Yikes

## Bad

my initial,  bloated `Dockerfile` looked like this

```Dockerfile
FROM node:18
WORKDIR /app

COPY . .

RUN yarn install

ENV NODE_ENV production
ENV PORT 3000

CMD ["yarn", "start"]
EXPOSE 3000
```

### Better

use `alpine` versions
```Dockerfile
FROM node:18-alpine
WORKDIR /app

COPY . .

RUN yarn install

ENV NODE_ENV production
ENV PORT 3000

EXPOSE 3000

CMD ["yarn", "start"]

```

### Multi Stage Build

multi stage builds can share files and reduce duplication during build process
```Dockerfile
FROM node:18-alpine as builder
WORKDIR /app

COPY yarn.lock .
COPY package.json .
RUN yarn
COPY . .
RUN yarn build

FROM node:18-alpine as runner
WORKDIR /app

COPY --from=builder /app/package.json .
COPY --from=builder /app/yarn.lock .

RUN yarn --production
COPY --from=builder /app/next.config.js .
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next ./.next

EXPOSE 3000

CMD ["yarn", "start"]

```

### BEST
Use a variable in your `.env` file 
```env
## Linux 64 bit
PRODUCTION_PLATFORM='linux/amd64'

## MacOS M1 Silicon
PRODUCTION_PLATFORM='linux/arm64/v8'
```

```Dockerfile
ARG PRODUCTION_PLATFORM
FROM --platform=$PRODUCTION_PLATFORM node:20-alpine AS base


FROM base as builder
WORKDIR /app

COPY yarn.lock .
COPY package.json .
RUN yarn install --frozen-lockfile
COPY . .
RUN yarn build


FROM base as runner
WORKDIR /app

COPY --from=builder /app/package.json .
COPY --from=builder /app/yarn.lock .

COPY --from=builder /app/next.config.js .
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next

EXPOSE 3000

CMD ["yarn", "start"]

```


### Standalone
In your `next.config.mjs` file, enable the standalone output for **Next 14**
```mjs
export default {
  // todo look into build optimization with `standalone` mode
  output: "standalone",
  ...
```

```
Use a variable in your `.env` file 
```env
## Linux 64 bit
PRODUCTION_PLATFORM='linux/amd64'

## MacOS M1 Silicon
PRODUCTION_PLATFORM='linux/arm64/v8'
```

```Dockerfile
ARG PRODUCTION_PLATFORM
FROM --platform=$PRODUCTION_PLATFORM node:20-alpine AS base

FROM base as builder
WORKDIR /app

COPY yarn.lock .
COPY package.json .
RUN yarn install --frozen-lockfile
COPY . .
RUN yarn build

FROM base as runner
WORKDIR /app

COPY --from=builder /app/package.json .
COPY --from=builder /app/yarn.lock .

COPY --from=builder /app/next.config.js .
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

EXPOSE 3000

CMD ["node", "server.js"]

```
---
## Credits
- [Reduce Docker Image size for your Next.js App | by Kyle Le | JavaScript in Plain English](https://javascript.plainenglish.io/reduce-docker-image-size-for-your-next-js-app-bcb65d322222)
## Backlinks
- [[developer/developer_box📦\|developer_box📦]]