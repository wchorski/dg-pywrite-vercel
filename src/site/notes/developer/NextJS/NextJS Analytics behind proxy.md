---
{"dg-publish":true,"permalink":"/developer/next-js/next-js-analytics-behind-proxy/","dgPassFrontmatter":true}
---

Add blockers track for common external endpoints that track data. We can get around this by setting up a proxy in [[developer/NextJS/NextJS\|NextJS]] to funel the external script to an enpoint that is from our domain

For a live example of this, check out [[developer/Projects/Make-a-Gram\|Make-a-Gram]]

## .env
```env
NEXT_PUBLIC_UMAMI_ID="asdlk-8398-23ksdfas-sa343"
NEXT_PUBLIC_UMAMI_URL="https://DOMAIN_WHERE_UMAMI_IS_HOSTED.site"
NEXT_PUBLIC_UMAMI_SCRIPT="ramen"
```

You've probably noticed the `ramen` as the **Umami_Script**. This is a custom endpoint set up via the [Environment variables | umami](https://umami.is/docs/environment-variables) "TRACKER_SCRIPT_NAME"

Checkout the [[developer/Home Lab/umami\|umami]] to see how I set it up in the `docker-compose.yml`

## next.config.js
```js
/** @type {import('next').NextConfig} */

const ANALYTICS_URL = process.env.NEXT_PUBLIC_UMAMI_URL + '/:match*'
console.log('ANALYTICS_URL', ANALYTICS_URL);

const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
  async rewrites() {
    return [
      {
        source: '/stts/:match*',
        destination: ANALYTICS_URL,
      },
    ]
  },
}

module.exports = nextConfig
```

## __app.js
```jsx
// /pages/__app.js

import '../styles/globals.css'
import  Script  from "next/script";

const UMAMI_SCRIPT = process.env.NEXT_PUBLIC_UMAMI_SCRIPT
const UMAMI_ID = process.env.NEXT_PUBLIC_UMAMI_ID

function MyApp({ Component, pageProps }) {


  return (<>

    <Script
      id="umami-next"
      strategy="afterInteractive"
      async
      data-website-id={UMAMI_ID}
      src={`/stts/${UMAMI_SCRIPT}`}
    />
 
    
    <Component {...pageProps} />
</>)}

export default MyApp
```


---
## Credits
- [Running on Vercel | umami](https://umami.is/docs/running-on-vercel)