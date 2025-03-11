---
{"dg-publish":true,"tags":["js","reactjs","nodejs","javascript","API"],"permalink":"/developer/next-js/next-js-middleware-dynamic-redirect/","dgPassFrontmatter":true}
---

Through my [[developer/KYPN/KYPN Stack\|KYPN Stack]] I've set up dynamic routes that fetches a posts in the db through the blog route by searching it's `slug`

But I'd also like to support linking to to a post via it's `id` as well. *BUT* using a redirect instead so I only have to code one page.

> [!error] You may not set 2 dynamic routes next to each other
> ```txt
> ❌ 
> /blog/[slug]
> /blog/[id]
> 
> ✅
> /blog/[slug]
> /blog/id/[id]
> ```

## Middleware
With the magic of [[developer/NextJS/NextJS\|NextJS]]. slap in a `/src/middleware.ts` and check out the code

Note that I'm using a *graphql* api but this could easily be stripped down to the basic *REST* api. The idea being, create a simple database query with **NextJs's** built in api router and fetch a Post's `slug` via it's `id`

```ts
import { Post } from '@ks/types';
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'


export async function middleware(request: NextRequest) {

  //todo set user chosen theme here? from cookies

  if (request.nextUrl.pathname.startsWith('/blog/id')) {

    try {
      const url = new URL(request.url)
      const postId = url.pathname.split('/blog/id/')[1]

      const res = await fetch( url.origin + `/api/gql/noauth`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          query: `
            query Post($where: PostWhereUniqueInput!) {
              post(where: $where) {
                slug
              }
            }
          `,
          variables: {
            where: {
              id: postId
            }
          }
        }),
      })
      const {post} = await res.json() as {post: Post}
      if(!post.slug) return console.log('no post:slug found');
      
      return NextResponse.redirect(new URL(`/blog/${post?.slug}`, request.url))
      
    } catch (error) {
      return new Response('Error processing request', { status: 500 });
    }
  }
}
 
// See "Matching Paths" https://nextjs.org/docs/app/building-your-application/routing/middleware#matching-paths
export const config = {
  matcher:[ '/blog/id/:id*'],
}
```

## Tips
### Matcher
The `matcher` narrows the scope of the middleware so it doesn't run on every route in your app. Just routes that are `/blog/id/:id*`. 
### Url Origin
because We can't use server side **Node** code. We must rely on what the client already has. That is why we use `url.orgin` to get the current site's address, then append the api endpoint.
### Default BlogId Page
Just for safety I'd still create a simple `/blog/id/[id]/page.tsx` page incase the middleware breaks. Use the same `fetch()` logic to do the same `id` to `slug` juggle

---
## Credit
- https://www.youtube.com/watch?v=xrvul-JrKFI
- https://borstch.com/blog/development/middleware-usage-in-nextjs-14