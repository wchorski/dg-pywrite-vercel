---
{"dg-publish":true,"tags":["html","SSG","obsidian","markdown","#selfhosted"],"permalink":"/developer/Digital Gardening/Obisidan Digital Garden/","dgPassFrontmatter":true}
---

[Digital Garden - Publish Obsidian Notes For Free](https://dg-docs.ole.dev/) is an app that builds a static site with the help of your Obsidian markdown files, and styles. This tool has great support for popular plugins like 
- mermaidjs
- excalidraw
- dataview
- graphview

So far this tool has everything I need in a #SSG builder. 

## How I Host

Usually I always opt for a #selfhosted solution, and there is a good [link](https://github.com/oleeskild/obsidian-digital-garden/discussions/160#discussioncomment-7153146)on house. to do it. Maybe I will try out building the site with [[developer/Home Lab/Docker\|Docker]] and connecting it with my [[developer/Home Lab/Nginx Proxy Manager\|Nginx Proxy Manager]]. 

## Self Hosted

Putting the self hosted solution here just in case I want to do it later. This is assuming you have a [[developer/Home Lab/Nginx Proxy Manager\|Nginx Proxy Manager]] set up to point to the #SSG files.

```dockerfile
FROM node:18 as base
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .

FROM base as builder
WORKDIR /usr/src/app
RUN npm run build

FROM scratch AS export
COPY --from=builder /usr/src/app/dist /
```

```shell
docker build -f Dockerfile.export --output dist .
```

> [!error] ERROR: "get-theme" exited with 1.
> `AxiosError: unable to get local issuer certificate`
> 
> I was getting this error when trying to build the site to the `dist` folder. Looks like it was a network issue. Prodded around with my wifi and turned off my VPN and things started to work.

For now I'm using **GitHub Pages**, as it's very similar to a self hosted build, while taking the burden off of my [[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]]

### GitHub Pages

For anyone who initially built this project with **Vercel** like me, I added this to `vercel.json` to stop it from building to their platform

```json
{
  "outputDirectory": "dist",
  "installCommand": "npm install",
  "buildCommand": "npm run build",
  "devCommand": "npm run start",
  "routes": [
      { "handle": "filesystem" },
      { "src": "/(.*)", "status": 404, "dest": "/404" }
    ],
    "git": {
      "deploymentEnabled": {
        "main": false,
        "gh-pages": false
      }
    }
}
```

If you'd like a more indepth tutorial, check out [How to deploy your Eleventy website to GitHub Pages with GitHub Actions - Maarten Tibau](https://maarten.be/blog/20220730/how-to-deploy-your-eleventy-website-to-github-pages-with-github-actions/). This is more the short version.

.Clone your digital garden repo and open up your fav text editor and make a few files in the root of the repo

```bash
.github
├── dependabot.yml
└── workflows
    ├── build-and-deploy.yml
    └── build.yml
```

`build.yml`
```yml
name: Build on PR

on:
  pull_request:
    branches: ['main']

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: ['18']

    steps:
      - uses: actions/checkout@v3

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install packages
        run: npm ci

      - name: Run npm build
        run: npm run build
```

`build-and-deploy.yml`
```yml
name: Build & Deploy Digital Garden

on:
  push:
    branches: ['main']

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: ['18']

    steps:
      - uses: actions/checkout@v3

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install packages
        run: npm ci

      - name: Run npm build
        run: npm run build

      - name: Deploy to gh-pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          publish_dir: ./dist
          # omit if not using a custom domain
          cname: pywrite-garden.tawtaw.site
```

#### GitHub Keys

You'll need to create some keys to authenticate your local machine with the GitHub repo. If you like pictures, check out this [link](https://github.com/peaceiris/actions-gh-pages#️-create-ssh-deploy-key). But I'll copy the good bits here

Assuming your still in the digital garden repo on your local machine run this

```bash
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
```

You will get 2 files:

- `gh-pages.pub` is a public key
- `gh-pages` is a private key

copy `gh-pages.pub` to `https://github.com/YOURUSERNAME/GARDENREPO/settings/keys`

copy `gh-pages` to `https://github.com/YOURUSERNAME/GARDENREPO/settings/secrets/actions#repository-secrets` with the Title = `ACTIONS_DEPLOY_KEY`

```bash
git add .
git commit -m 'digital garden initial build'
git push
```

#### Custom Domain

Go to your domain host's DNS edit page. 

create a CNAME record that points to `YOURUSERNAME.github.io`

Go to `https://github.com/YOURUSERNAME/GARDENREPO/settings/pages` and add a custom domain that you created
## Analytics

I'll be using [[developer/Home Lab/umami\|umami]] as my #selfhosted choice

create an `analytics.njk` file 

```bash
_includes
├── components
│   ├── calloutScript.njk
│   ├── filetree.njk
...
│   ├── timestamps.njk
│   └── user
│       └── common
│           └── head
│               └── analytics.njk
```

`analytics.njk`
```html
<script async src="https://animals.YOURDOMAIN.site/dogs" data-website-id="fa4f8c5b-***"></script>
```

Essentially, your injecting a new `<script>` into the head of every page in your published garden. You'll also notice the strange choice of domain name, this explained in my setup of [[developer/Home Lab/umami\|umami]]

## Custom Styles

edit `src/site/styles/custom-style.scss` for any custom style needs

---
## Credits
- [Local deployment · oleeskild/obsidian-digital-garden · Discussion #160 (github.com)](https://github.com/oleeskild/obsidian-digital-garden/discussions/160)
- [Google Analytics · oleeskild/obsidian-digital-garden · Discussion #195 (github.com)](https://github.com/oleeskild/obsidian-digital-garden/discussions/195)