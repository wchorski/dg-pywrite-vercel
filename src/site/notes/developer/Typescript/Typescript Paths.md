---
{"dg-publish":true,"permalink":"/developer/Typescript/Typescript Paths/","dgPassFrontmatter":true}
---

with [[developer/Typescript/Typescript\|Typescript]] paths, you're able to set custom paths to make imports much easier to remember and type
## tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES6",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "paths": {
      "@/components/*": ["./components/*"],
      "@/styles/*": ["./styles/*"],
      "@/fonts": ["./styles/fonts"],
    }
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}

```

## Importing in a JS file

Then in your `file.tsx` you can import that file with sweet sweet autocomplete

```tsx
import styles from '@/styles/ecommerce/Product.module.scss'
```