---
{"dg-publish":true,"permalink":"/developer/NextJS/next-auth/","dgPassFrontmatter":true}
---

#NextJS 

## custom signin signout pages

`pages/api/auth/[...nextauth].js`
```
...  pages: {    signIn: '/auth/signin',    signOut: '/auth/signout',    error: '/auth/error', // Error code passed in query string as ?error=    verifyRequest: '/auth/verify-request', // (used for check email message)    newUser: '/auth/new-user' // New users will be directed here on first sign in (leave the property out if not of interest)  }...
```

## display signin errors
```jsx
// pages/api/[...nextauth].js
export default NextAuth({
  ...
  pages: {
    ...
    error: '/signin', // Error code passed in query string as ?error=
  }
});
```

```jsx
// pages/signin.jsx
import { getProviders } from "next-auth/react"
import { useRouter } from "next/router";
const SignIn = ({ providers }) => {
  const { error } = useRouter().query;
  return (
    <>
      <h1>Login</h1>
      {/* Error message */}
      {error && <SignInError error={error} />}
      {/* Login options */}
      {Object.values(providers).map((provider) => (
        <div key={provider.name}>...</div>
      ))}
    </>
  );
};
export async function getServerSideProps(context) {
  return { props: { providers: await getProviders() } };
}
```

```jsx
// pages/signin.jsx
const errors = {
  Signin: "Try signing with a different account.",
  OAuthSignin: "Try signing with a different account.",
  OAuthCallback: "Try signing with a different account.",
  OAuthCreateAccount: "Try signing with a different account.",
  EmailCreateAccount: "Try signing with a different account.",
  Callback: "Try signing with a different account.",
  OAuthAccountNotLinked:
    "To confirm your identity, sign in with the same account you used originally.",
  EmailSignin: "Check your email address.",
  CredentialsSignin:
    "Sign in failed. Check the details you provided are correct.",
  default: "Unable to sign in.",
};
const SignInError = ({ error }) => {
  const errorMessage = error && (errors[error] ?? errors.default);
  return <div>{errorMessage}</div>;
};
```
## Authorization

### Facebook App Auth
1. [link](https://next-auth.js.org/providers/facebook#configuration)
3. [https://developers.facebook.com/apps](https://developers.facebook.com/apps)
4. create a new app
5. Left menu `Facebook Login` > `Settings`
6. `Valid OAuth Redirect URIs`
	1. paste in `https://MYAPP.MYDOMAIN.com/api/auth/callback/facebook`
7. profit?

### Google Authorization
1. [link](https://next-auth.js.org/providers/google#configuration)
2. [https://console.developers.google.com/apis/credentials](https://console.developers.google.com/apis/credentials)
3. select appropriate app
4. `Authorized redirect URIs`
	1. add `https://MYAPP.MYDOMAIN.com/api/auth/callback/google`


## Error Message Handling 
- [link](https://logfetch.com/next-auth-custom-error-page/#:~:text=Once the signin process is set up with,at %2Fpages%2Fsignin.jsx and show the error message there.)


--- 
## Credits
- [Pages | NextAuth.js (next-auth.js.org)](https://next-auth.js.org/configuration/pages)
- [link](https://logfetch.com/next-auth-custom-error-page/#:~:text=Once the signin process is set up with,at %2Fpages%2Fsignin.jsx and show the error message there.)
- [Nextauth Complete Tutorial #12 Add Facebook provider - YouTube](https://www.youtube.com/watch?v=eTpkgNBmrX8&t=198s)
- [NextAuth.js (next-auth.js.org)](https://next-auth.js.org/)


## Backlinks
[[developer/NextJS/NextJS\|NextJS]]