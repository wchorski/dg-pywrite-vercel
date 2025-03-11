---
{"dg-publish":true,"tags":["bash","linux","automation","batch"],"permalink":"/developer/Linux/Bash Sed Modify all files in directory/","dgPassFrontmatter":true}
---

Needed to batch edit the #frontmatter of my markdown files for my [[developer/Home Lab/Obsidian.md\|Obsidian.md]] vault. 

This script checks if the first line is `---` (i.e. the start of frontmatter) and decides if we just add the `dg-publish: true` or do we need a whole frontmatter block

```yml
---
dg-publish: true
---
```



```bash
#!/bin/bash

cd /home/icicle/scripts/test-md

for file in ./*.md; do

  FIRSTLINE=$(head -n 1  "$file")
  SECONDLINE=$(sed -n '2p' "$file")

  ## check if frontmatter already exists
  if [ "$FIRSTLINE" = "---" ]; then

    ## see if "dg-publish: true is already 2nd line"
    if [ "$SECONDLINE" = 'dg-publish: true' ]; then
      echo "Already PUBLISHED: $file"
    else
      sed -i '2i\dg-publish: true' "$file"
    fi
  else
    # add a frontmatter block
    sed -i '1s/^/---\ndg-publish: true\n---\n/' "$file"
  fi

done
```
## MacOS
For #MacOs users  use `gsed` instead of `sed` 

```shell
brew install gnu-sed
```

---
## Credits
- [bash - using sed to modify all files in a directory and name the outputs accordingly - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/270085/using-sed-to-modify-all-files-in-a-directory-and-name-the-outputs-accordingly)
- [How to get the first line of a file in a bash script? - Stack Overflow](https://stackoverflow.com/questions/2439579/how-to-get-the-first-line-of-a-file-in-a-bash-script)
- [bash - Execute command on all files in a directory - Stack Overflow](https://stackoverflow.com/questions/10523415/execute-command-on-all-files-in-a-directory)
- [linux - Syntax error near unexpected token 'then' - Stack Overflow](https://stackoverflow.com/questions/20235217/syntax-error-near-unexpected-token-then)
- [bash - How to insert a string into second to last line of a file - Stack Overflow](https://stackoverflow.com/questions/45483844/how-to-insert-a-string-into-second-to-last-line-of-a-file)
- https://stackoverflow.com/a/60562182/15579591