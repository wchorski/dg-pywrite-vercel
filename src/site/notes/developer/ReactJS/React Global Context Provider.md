---
{"dg-publish":true,"permalink":"/developer/react-js/react-global-context-provider/","dgPassFrontmatter":true}
---


A snippet for using a virtual authorization context. Using it as a way to check for login isn't the most practical use case (when you'll probably use tokens, cookies, etc.) but hopefully this example still gets the point across of how to use **React's** `createContext` 

> [!warning] This does not persist when browser is refreshed

`useGlobalContext.tsx`
```tsx

import { createContext, useContext, useState } from "react";

interface DataType {
  id: string | null,
  email: string | null,
  name: string | null,
  isAdmin: boolean | null,
}

const defaultCtx= {
  session:{
    id: '',
    email: '',
    name: '',
    isAdmin: false,
  },
  setSession: (session:DataType | null) => {}
}

const GlobalContext = createContext(defaultCtx)

export const GlobalContextProvider = ({ children }: {children: React.ReactNode}) => {

  const [session, setSession] = useState(null);
  
  return (
    <GlobalContext.Provider value={{session, setSession}}>
      {children}
    </GlobalContext.Provider>
  )
};

export const useGlobalContext = () => useContext(GlobalContext);
```

don't forget to wrap your app

`_app.tsx`
```tsx
// import '../styles/globals.css'
import type { AppProps } from 'next/app'
import Page from "../components/Page";
import dynamic from 'next/dynamic';
import { GlobalStyles } from "../styles/GlobalThemeStyle.styled.js";
import { ApolloProvider } from "@apollo/client";

import '../styles/nprogress.css'
import { GlobalContextProvider } from '../lib/useGlobalContext';



function App({ Component, pageProps }: any) {

  return (<>

    <GlobalContextProvider>

		<Component {...pageProps} />

    </GlobalContextProvider>
  </>)
}

export default (App)
```

---
## Credits
- https://github.com/pagecow/nextjs-13-context-api/blob/main/app/Context/store.tsx
- https://felixgerschau.com/react-typescript-context/
- https://stackoverflow.com/questions/58193424/passing-state-with-usecontext-in-typescript

## Backlinks
- [[developer/ReactJS/ReactJS\|ReactJS]]