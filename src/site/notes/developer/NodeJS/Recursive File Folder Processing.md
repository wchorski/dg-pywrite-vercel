---
{"dg-publish":true,"tags":["nodejs","javascript","typescript"],"permalink":"/developer/node-js/recursive-file-folder-processing/","dgPassFrontmatter":true}
---

building a  [[developer/NodeJS/NodeJS\|NodeJS]] tool to recursively drill through a folder file structure to return all folders and files for processing
## Node 20.x way

Here is a snippet to get a general feel. This code is not complete, so check the [this repo](https://github.com/wchorski/wikilink_markdownlink_conversion/blob/main/app.ts) to see it in action.

Functions
- `processFile()`
- `findAllFilePaths()`
- `app()`

```ts
async function processFile(filePath:string, exportType:ExportType) {

  try {
    let contents = await readFile(filePath, 'utf8')
    let contentsModified = contents
    const foundLinks = findTypeSelect(contents, exportType)

    if (foundLinks && foundLinks.length > 0) {
      console.log('---')
      console.log('## FILE: ', filePath)      

      for(const link of foundLinks) {
        const convertedLink = linkTypeSelectConverter(link, exportType)
        console.log('- foundLink: ', link.input)
        console.log('- converted: ', convertedLink)
        contentsModified = contentsModified.replace(link.input, convertedLink)
      }

      await writeFile(filePath, contentsModified, 'utf8')
      console.log('### File Modified: ' + filePath)
    }
    
  } catch (err) {
    console.log(err);
    
  }
}

async function findAllFilePaths(folderPath:string, fileList:string[] = [],  exportType:ExportType){

  try {
    const items = await readdir(folderPath)    
    
    for(const itemName of items) {
      
      const itemPath = `${folderPath}/${itemName}`
      const stats = await stat(itemPath)
      
      if(stats.isFile()) 
        fileList.push(itemPath)
      else 
        if(stats.isDirectory()) 
          await findAllFilePaths(itemPath, fileList, exportType);
    }

  } catch (err) { console.error(err) }

  
  return fileList
}

async function app(){

  if(process.env.DIRECTORY){
    const files = await findAllFilePaths(process.env.DIRECTORY, [], process.env.EXPORTTYPE as ExportType)
    files.map(file => processFile(file, process.env.EXPORTTYPE as ExportType))
  }
  
  if(process.env.FILE)
    processFile(process.env.FILE, process.env.EXPORTTYPE as ExportType)
  
  if(!process.env.DIRECTORY && !process.env.FILE) console.log('Edit `.env` file to include a FILE or DIRECTORY & EXPORTTYPE');
}

app()
```
## Old Way

I was going through an old [video](https://www.youtube.com/watch?v=GMf30xyRv9M) and got as far as returning all folders/subfolder paths

```ts
async function readDirectories(folderPaths:string[], exportType:ExportType){
  
  folderPaths.forEach(folderPath => {

    const items = fs.readdirSync(path.resolve(folderPath))
    const folderNames = items.filter(item => 
      fs.lstatSync(path.resolve(folderPath, item)).isDirectory()
    )
    const subFolderPaths = folderNames.map(folderName => path.resolve(folderPath, folderName))
    if(subFolderPaths.length <= 0) return

    subFolderPaths.forEach(subFolder => foldersList.push(subFolder))

    readDirectories(subFolderPaths, exportType)
  })
  
}
```

---
## Credits
- [Loop Through Folders & Subfolders - Node JS & Apps Script, JavaScript, Google Drive, Learn Recursion (youtube.com)](https://www.youtube.com/watch?v=GMf30xyRv9M)
- [Node.js fs.readdir recursive directory search - Stack Overflow](https://stackoverflow.com/questions/5827612/node-js-fs-readdir-recursive-directory-search)