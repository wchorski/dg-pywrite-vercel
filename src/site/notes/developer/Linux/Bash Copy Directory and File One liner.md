---
{"dg-publish":true,"permalink":"/developer/Linux/Bash Copy Directory and File One liner/","dgPassFrontmatter":true}
---

I needed a #bash shell script that automatically copied over private files such as `.env` and `home.tsx` within my web development stack

Here is the stack overflow post by [Mark Amery](https://stackoverflow.com/a/32596855) that helped


```shell
mkdir -p /foo/bar && cp myfile.txt $_
```

> `$_` - expands to the last argument to the previous command

if the path as spaces, the strings need to be wrapped in quotes
```shell
mkdir -p "/my directory/name with/spaces" && cp "my filename with spaces.txt" "$_"
```

if you want to name the file with a new name
```shell
mkdir -p /foo/bar && cp ../from/another/dir/myfile.txt $_/my-file-with-new-name
```