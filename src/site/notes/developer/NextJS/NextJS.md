---
{"dg-publish":true,"tags":["javascript","typescript","reactjs"],"permalink":"/developer/NextJS/NextJS/","dgPassFrontmatter":true}
---

[Next.js by Vercel - The React Framework (nextjs.org)](https://nextjs.org/)
> The [[developer/Home Lab/ReactJS\|ReactJS]] Framework. hybrid static & server rendering, TypeScript support, smart bundling, route pre-fetching, and more. No config needed.
## connections
- [[developer/Home Lab/Nginx Proxy Manager\|Nginx Proxy Manager]] for home routing
- [[developer/Projects/heart-chart\|heart-chart]]
- [[developer/KYPN/KYPN Stack\|KYPN Stack]]
- [[developer/GameDev 🕹/Flame Warrior\|Flame Warrior]]

## installation
1. `./compose.yml` via [[developer/Home Lab/Docker\|Docker]]
	1. "app" - NextJS App
	2. "mongo" - MongoDB database
```yml
version: '3'

services:
  mongo:
    restart: unless-stopped
    image: mongo
    environment:
      - MONGO_INITDB_ROOT_USERNAME=<username>
      - MONGO_INITDB_ROOT_PASSWORD=<strongpassword>
      - MONGO_INITDB_DATABASE=auth
    volumes:
      - ./database/mongo-db:/data/db
    networks:
      - nexthearts-network

  # nginx built inside
  app:
    image: nextjs-<appname>
    restart: unless-stopped
    build:
      dockerfile: Dockerfile
    ports:
      - 4050:3000
    environment:
      - MONGO_URI=mongo
      - MONGO_PORT=27017
      - MONGO_COLLECTION=<appname>
      - MONGO_USER=<username>
      - MONGO_PASSWORD=<strongpassword>

      # PROVIDERS
      - GITHUB_ID=
      - GITHUB_SECRET=
      - FACEBOOK_ID=
      - FACEBOOK_SECRET=
      - NEXTAUTH_URL=https://<yourDomain>
    depends_on:
      - mongo
    networks:
      - nexthearts-network
      - nginx-prox-mgmt-3_default # add to ngnix-prox-mgmt network for Docker DNS

networks:
  nexthearts-network:
    driver: bridge
  nginx-prox-mgmt-3_default: # so we can call service by DNS
    driver: bridge

volumes:
  mongo-db:
```

### Server Side Rendered Dockerfile
2. `./Dockerfile`
```dockerfile
FROM node:16

RUN mkdir /app
WORKDIR /app
COPY package*.json ./

RUN npm install
COPY . ./
RUN npm run build

CMD ["npm", "start"]
```

### Static Site Dockerfile
```Dockerfile
FROM node:16-alpine AS deps

# Check https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine to understand why libc6-compat might be needed.
RUN apk add --no-cache libc6-compat

WORKDIR /app
COPY package.json package-lock.json* ./
RUN \

if [ -f yarn.lock ]; then yarn --frozen-lockfile; \
elif [ -f package-lock.json ]; then npm ci; \
elif [ -f pnpm-lock.yaml ]; then yarn global add pnpm && pnpm i --frozen-lockfile; \
else echo "Lockfile not found." && exit 1; \
fi

FROM node:16-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
# Next.js collects completely anonymous telemetry data about general usage.
# Learn more here: https://nextjs.org/telemetry
# Uncomment the following line in case you want to disable telemetry during the build.
ENV NEXT_TELEMETRY_DISABLED 1

RUN npm run build


FROM node:16-alpine AS runner

WORKDIR /app
ENV NODE_ENV production
# Uncomment the following line in case you want to disable telemetry during runtime.
ENV NEXT_TELEMETRY_DISABLED 1
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
# Automatically leverage output traces to reduce image size
# https://nextjs.org/docs/advanced-features/output-file-tracing
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs
EXPOSE 3000
ENV PORT 3000

CMD ["node", "server.js"]
```

3. `./.dockerignore`
```
Dockerfile
**/Dockerfile
Dockerfile.dev
.dockerignore
**/node_modules
node_modules
.git
.gitignore
npm-debug.log
.next
.env.dev
.env.prod
```

---
## Tips & Tricks
### multi nexted dynamic routes
- [Multi Nested Dynamic Routes in NextJs - DEV Community 👩‍💻👨‍💻](https://dev.to/willholmes/multi-nested-dynamic-routes-in-nextjs-30f7)

### absolute imports
- absolute paths instead of relative paths. Especially helpful when doing more automation / generation of links - [link](https://ahmadawais.com/next-js-absolute-imports-aliases/#:~:text=Next.js Absolute Imports %23 Basically you only need,via jsconfig.json (JS projects)%20or%20tsconfig.json%20(TS%20projects).)
```js
// Your jsconfig.json or tsconfig.json 
{ 
	"compilerOptions": { 
		"baseUrl": "." 
	} 
}
```

### multiple nested routes with getStaticPaths 
 [source](https://stackoverflow.com/questions/61732511/how-to-use-multiple-nested-dynamic-routes-with-getstaticpaths)
folder structure
```js
├── _app.jsx
├── _document.jsx
├── index.jsx
└── [type]
    └── [slug].jsx
```

`./pages/[type]/[slug].jsx`
```javascript
const Test = (props) => {
    return (
       <>
           {props.slug}
       </>
    );
};

export async function getStaticProps({params}) {
    return {
        props: params
    }
}

export async function getStaticPaths() {
    const posts = [
        {
            mainTag: 'programming',
            slug: 'hello-world'
        },
        {
            mainTag: 'programming',
            slug: 'nextjs-101'
        },
    ];

    return {
        paths: posts.map((posts) => {
            return {
                params: {
                    type: posts.mainTag,
                    slug: posts.slug,
                },
            };
        }),
        fallback: false,
    };
}

export default Test;
```

### markdown image with nextjs/image
[Use Next/Image with React Markdown – Amir Ardalan](https://amirardalan.com/blog/use-next-image-with-react-markdown) *tweaked code to fit with [[developer/Home Lab/Obsidian.md\|Obsidian.md]]*
https://forum.obsidian.md/t/how-can-i-change-the-size-of-images/2869/7?u=wchorski
```tsx
// Markdown.tsx 
import Image from 'next/image' 
import ReactMarkdown from 'react-markdown'

export const Markdown = () => {
	const MarkdownComponents: object = { 
		p: (paragraph: { children?: boolean; node?: any}) => {
		  const { node } = paragraph
		
		  if (node.children[0].tagName === "img") {
		    const image = node.children[0]
		    const metastring = image.properties.alt
		    const alt = metastring?.replace(/ *\{[^)]*\} */g, "")
		    const metaWidth = metastring.match(/{([^}]+)x/)
		    const metaHeight = metastring.match(/x([^}]+)}/)
		    const width = metaWidth ? metaWidth[1] : "768"
		    const height = metaHeight ? metaHeight[1] : "432"
		    const isPriority = metastring?.toLowerCase().match('{priority}')
		    const hasCaption = metastring?.toLowerCase().includes('{caption:')
		    const caption = metastring?.match(/{caption: (.*?)}/)?.pop()
		
		    return (
		      <div className="postImgWrapper">
		        <Image
		          src={image.properties.src}
			        width={width}
			        height={height}
			        className="postImg"
			        alt={alt}
			        priority={isPriority}
			      />
			        {hasCaption ? <div className="caption" aria-label={caption}>{caption}</div> : null}
		      </div>
		    )
		  }
		  return <p>{paragraph.children}</p>
		},
	}
	
	
	return ( 
		<ReactMarkdown children={your.content.here} components={MarkdownComponents} /> 
	)
}



```

### keep aspect ratio of sized nextjs/image 
I wanted to easily size images with one metric, the width, but keep the original aspect ratio intact.

[source](https://stackoverflow.com/a/71074979/15579591)
```jsx
const NaturalImage = (props: ImageProps) => {
  const [ratio, setRatio] = useState(16/9) // default to 16:9

  return (
    <NextImage
      {...props}
      // set the dimension (affected by layout)
      width={200}
      height={200 / ratio}
      layout="fixed" // you can use "responsive", "fill" or the default "intrinsic"
      onLoadingComplete={({ naturalWidth, naturalHeight }) => 
        setRatio(naturalWidth / naturalHeight)
      }
    />
  )
}
```

---
## Trials & Tribulations

### ESLint Error
> [!error] error - ESLint: Error while loading rule '@next/next/no-html-link-for-pages': Invalid regular expression: /^/testingchar/(/$/: Unterminated group Occurred while linting /vercel/path0/pages/_app.tsx

[source](https://stackoverflow.com/a/64664131/15579591)
add to `.eslint.json`. Assuming `tsconfig.json` is also in the root directory
```json
{
    "parserOptions": {
        "project": ["tsconfig.json"]
    },
}
```

### modual not found
> [!error] Module Not Found Can’t Resolve ‘fs’ in Next.js
> you'll find yourself commenting / uncommenting lines while troubleshooting. 
> 
> if you're loading a server side script from `getStaticPaths` or `getStaticProps` and decide to comment out the function, remember to comment out the **import** of said script at the top of the `./pages/file.jsx` 

`./pages/file.jsx`
```javascript
import { serverFunc } from "libs/serverScript"; 
// this will throw the *import* error unless you comment out above line too

export async function getStaticPaths() {

	// serverFunc() 

}
```

> [!warning] Catch alls can only be at end of structure
> `./pages/[...folder]/[...slug]`? - No sir

