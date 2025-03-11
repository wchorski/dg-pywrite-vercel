---
{"dg-publish":true,"tags":["typescript","text_editor","vscode","code"],"permalink":"/developer/Typescript/VS Code Import Typescript Error/","dgPassFrontmatter":true}
---

If you've noticed a strange error saying your import is wrong. Then you delete and retype the *EXACT* same thing and... wow the error is gone? wtf.

> Cannot find module '@/envs' or its corresponding type declarations.

![attachments/Pasted image 20240724132838.png](/img/user/attachments/Pasted%20image%2020240724132838.png)

This is because the typescript built into your code editor needs a kick in the pants. 
## tsconfig.json
I've set up custom paths in my `tsconfig.json` files to easily import scripts into each other. 
```json
...
"baseUrl": ".",
"paths": {
	"@styles/*": [
		"src/styles/*"
	],
	"@components/*": [
		"src/components/*"
	],
	"@/*": [
		"./*"
	]
}
...
```
## Pallet command
In your code editor command pallet this is how you refresh typescript and make all good with the world 

`> TypeScript: Restart TS server`

---
## Credit
- [Typescript - Cannot find module ... or its corresponding type declarations - Stack Overflow](https://stackoverflow.com/questions/64732623/typescript-cannot-find-module-or-its-corresponding-type-declarations)