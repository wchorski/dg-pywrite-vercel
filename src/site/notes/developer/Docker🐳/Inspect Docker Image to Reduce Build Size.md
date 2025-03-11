---
{"dg-publish":true,"tags":["docker","docker-compose","software","build","optimization"],"permalink":"/developer/Docker🐳/Inspect Docker Image to Reduce Build Size/","dgPassFrontmatter":true}
---

## Frontend build
My Dockerfile for the frontend [[developer/NextJS/NextJS\|NextJS]] is very optimized for small build size outputting to the **standalone** feature
### Frontend Dockerfile
```shell
ARG PRODUCTION_PLATFORM
FROM --platform=$PRODUCTION_PLATFORM node:20-alpine AS base
RUN echo '#### LETS GET STARTED HEYYY ####'

FROM base AS builder
# # Check https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine to understand why libc6-compat might be needed.
RUN apk add --no-cache libc6-compat
WORKDIR /app

# Install dependencies based on the preferred package manager
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* ./
# COPY keystone.ts schema.* session.ts envs.ts ./
# COPY src/keystone/schema.ts ./src/keystone/schema.ts
COPY . ./

RUN \
  if [ -f yarn.lock ]; then yarn install --frozen-lockfile; \
  elif [ -f package-lock.json ]; then npm ci; \
  elif [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm i --frozen-lockfile; \
  else echo "Lockfile not found." && exit 1; \
  fi
RUN yarn add sharp
RUN yarn next telemetry disable

# Learn more here: https://nextjs.org/telemetry
ENV NEXT_TELEMETRY_DISABLED 1

RUN \
  if [ -f yarn.lock ]; then yarn run build; \
  elif [ -f package-lock.json ]; then npm run build; \
  elif [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm run build; \
  else echo "Lockfile not found." && exit 1; \
  fi
RUN yarn next telemetry disable

# Production image, copy all the files and run next
FROM base AS runner
WORKDIR /app
ENV NEXT_TELEMETRY_DISABLED 1

ENV NODE_ENV production
# Uncomment the following line in case you want to disable telemetry during runtime.

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
# COPY --from=builder /app/.keystone ./.keystone
# COPY --from=builder /app/schema.prisma ./schema.prisma

# Set the correct permission for prerender cache
RUN mkdir .next
RUN chown nextjs:nodejs .next

# Automatically leverage output traces to reduce image size
# https://nextjs.org/docs/advanced-features/output-file-tracing
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000

ENV PORT 3000

# server.js is created by next build from the standalone output
# https://nextjs.org/docs/pages/api-reference/next-config-js/output
CMD HOSTNAME="0.0.0.0" node server.js
```
### Frontend end Docker Image inspected
```shell
## run this command to get below output
docker history myapp-frontend

IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
5c6aa2e91e8b   16 minutes ago   CMD ["/bin/sh" "-c" "HOSTNAME=\"0.0.0.0\" no…   0B        buildkit.dockerfile.v0
<missing>      16 minutes ago   ENV PORT=3000                                   0B        buildkit.dockerfile.v0
<missing>      16 minutes ago   EXPOSE map[3000/tcp:{}]                         0B        buildkit.dockerfile.v0
<missing>      16 minutes ago   USER nextjs                                     0B        buildkit.dockerfile.v0
<missing>      16 minutes ago   COPY --chown=nextjs:nodejs /app/.next/static…   2.57MB    buildkit.dockerfile.v0
<missing>      16 minutes ago   COPY --chown=nextjs:nodejs /app/.next/standa…   154MB     buildkit.dockerfile.v0
<missing>      16 minutes ago   RUN /bin/sh -c chown nextjs:nodejs .next # b…   0B        buildkit.dockerfile.v0
<missing>      16 minutes ago   RUN /bin/sh -c mkdir .next # buildkit           0B        buildkit.dockerfile.v0
<missing>      16 minutes ago   COPY /app/public ./public # buildkit            163kB     buildkit.dockerfile.v0
<missing>      18 minutes ago   RUN /bin/sh -c adduser --system --uid 1001 n…   3.25kB    buildkit.dockerfile.v0
<missing>      18 minutes ago   RUN /bin/sh -c addgroup --system --gid 1001 …   1.07kB    buildkit.dockerfile.v0
<missing>      18 minutes ago   ENV NODE_ENV=production                         0B        buildkit.dockerfile.v0
<missing>      18 minutes ago   ENV NEXT_TELEMETRY_DISABLED=1                   0B        buildkit.dockerfile.v0
<missing>      18 minutes ago   WORKDIR /app                                    0B        buildkit.dockerfile.v0
<missing>      18 minutes ago   RUN /bin/sh -c echo '#### LETS GET STARTED H…   0B        buildkit.dockerfile.v0
<missing>      8 days ago       CMD ["node"]                                    0B        buildkit.dockerfile.v0
<missing>      8 days ago       ENTRYPOINT ["docker-entrypoint.sh"]             0B        buildkit.dockerfile.v0
<missing>      8 days ago       COPY docker-entrypoint.sh /usr/local/bin/ # …   388B      buildkit.dockerfile.v0
<missing>      8 days ago       RUN /bin/sh -c apk add --no-cache --virtual …   5.59MB    buildkit.dockerfile.v0
<missing>      8 days ago       ENV YARN_VERSION=1.22.22                        0B        buildkit.dockerfile.v0
<missing>      8 days ago       RUN /bin/sh -c addgroup -g 1000 node     && …   118MB     buildkit.dockerfile.v0
<missing>      8 days ago       ENV NODE_VERSION=20.16.0                        0B        buildkit.dockerfile.v0
<missing>      10 days ago      /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B        
<missing>      10 days ago      /bin/sh -c #(nop) ADD file:a71f7e9bc66668361…   8.83MB  
```
## Backend build
My backend build of a [[developer/KeystoneJS/KeystoneJS\|KeystoneJS]] app outputs to an eye watering `5gb` image. 

### Backend Dockerfile
```shell
ARG PRODUCTION_PLATFORM
FROM --platform=$PRODUCTION_PLATFORM node:20-alpine as builder
WORKDIR /app

COPY package.json yarn.lock ./
COPY . .

RUN yarn install --production --frozen-lockfile
RUN yarn generate
RUN yarn ks:build
RUN yarn next telemetry disable
# Learn more here: https://nextjs.org/telemetry
ENV NEXT_TELEMETRY_DISABLED 1

FROM builder as runner
WORKDIR /app

COPY --from=builder /app/package.json .
# COPY --from=builder /app/yarn.lock .
# COPY --from=builder /app/.env .

COPY --from=builder /app/.keystone ./.keystone
# COPY --from=builder /app/schema.prisma ./schema.prisma
# COPY --from=builder /app/next.config.mjs ./
## todo try getting rid of `public` folder?
# COPY --from=builder /app/public ./public
## todo why is this image build to +5gb size?
# COPY --from=builder /app/node_modules ./node_modules

EXPOSE 3001
ENV NODE_ENV production
ENV PORT 3001

CMD ["yarn", "ks:start"]
```
### Backend Docker Image inspected
```shell
docker history ksnext14-kypn-backend 
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
17c95e78a409   6 minutes ago    CMD ["yarn" "ks:start"]                         0B        buildkit.dockerfile.v0
<missing>      6 minutes ago    ENV PORT=3001                                   0B        buildkit.dockerfile.v0
<missing>      6 minutes ago    ENV NODE_ENV=production                         0B        buildkit.dockerfile.v0
<missing>      6 minutes ago    EXPOSE map[3001/tcp:{}]                         0B        buildkit.dockerfile.v0
<missing>      6 minutes ago    COPY /app/.keystone ./.keystone # buildkit      125MB     buildkit.dockerfile.v0
<missing>      6 minutes ago    COPY /app/package.json . # buildkit             1.4kB     buildkit.dockerfile.v0
<missing>      6 minutes ago    WORKDIR /app                                    0B        buildkit.dockerfile.v0
<missing>      6 minutes ago    ENV NEXT_TELEMETRY_DISABLED=1                   0B        buildkit.dockerfile.v0
<missing>      6 minutes ago    RUN /bin/sh -c yarn next telemetry disable #…   2.2MB     buildkit.dockerfile.v0
<missing>      6 minutes ago    RUN /bin/sh -c yarn ks:build # buildkit         223MB     buildkit.dockerfile.v0
<missing>      7 minutes ago    RUN /bin/sh -c yarn generate # buildkit         17MB      buildkit.dockerfile.v0
<missing>      7 minutes ago    RUN /bin/sh -c yarn install --production --f…   4.18GB    buildkit.dockerfile.v0
<missing>      24 minutes ago   COPY . . # buildkit                             1.15MB    buildkit.dockerfile.v0
<missing>      24 minutes ago   COPY package.json yarn.lock ./ # buildkit       414kB     buildkit.dockerfile.v0
<missing>      38 minutes ago   WORKDIR /app                                    0B        buildkit.dockerfile.v0
<missing>      8 days ago       CMD ["node"]                                    0B        buildkit.dockerfile.v0
<missing>      8 days ago       ENTRYPOINT ["docker-entrypoint.sh"]             0B        buildkit.dockerfile.v0
<missing>      8 days ago       COPY docker-entrypoint.sh /usr/local/bin/ # …   388B      buildkit.dockerfile.v0
<missing>      8 days ago       RUN /bin/sh -c apk add --no-cache --virtual …   5.59MB    buildkit.dockerfile.v0
<missing>      8 days ago       ENV YARN_VERSION=1.22.22                        0B        buildkit.dockerfile.v0
<missing>      8 days ago       RUN /bin/sh -c addgroup -g 1000 node     && …   118MB     buildkit.dockerfile.v0
<missing>      8 days ago       ENV NODE_VERSION=20.16.0                        0B        buildkit.dockerfile.v0
<missing>      10 days ago      /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B        
<missing>      10 days ago      /bin/sh -c #(nop) ADD file:a71f7e9bc66668361…   8.83MB    
```
### The Problem
From the `RUN /bin/sh -c yarn install --production --f… 4.18GB buildkit.dockerfile.v0` line I can see that most of that bloat comes from the `yarn install` command

But why would the frontend at a tiny size?

### inspecting a running backend container
With a bit of prodding, I find that the yarn cache is gigantic
```shell
cd /app 
du -sh /usr/local/share/.cache/yarn/v6/

3.2G
```

WOH, even with my `node_modules` installed, the local dev directory does not even reach that size.
## I'm dumb
I overlooked the line `FROM builder AS runner`. It should have been `FROM base AS runner` as to not copy over any files from the build step