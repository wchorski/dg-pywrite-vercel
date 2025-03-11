---
{"dg-publish":true,"permalink":"/developer/node-js/tiptap/","dgPassFrontmatter":true}
---

> [!info] [Headless WYSIWYG Text Editor – Tiptap Editor](https://tiptap.dev/) 
> Tiptap gives you full control about every single aspect of your text editor experience. It’s customizable, comes with a ton of extensions, is open source and has an extensive documentation. Join our welcoming community and start building cool things!

## examples
- [editor-sandbox - CodeSandbox](https://codesandbox.io/s/fok5cu?file=/src/components/Tiptap.vue)
## Troubleshooting

### hot reloading and initial content
I wanted a clever way to load in a default **Tiptap** template if no data was found. Tiptap was only taking the inital state of the react `useState` until I dropped this piece in
```jsx
  useEffect(() => {
    if (editor && !editor.isDestroyed) {
      editor.chain().focus().setContent(defTemplate).run();
    }
  }, [defTemplate, editor]);
```

Here is the whole script for context
```jsx
import React, {useState, useEffect} from 'react'

import Document from '@tiptap/extension-document'
import Gapcursor from '@tiptap/extension-gapcursor'
import Paragraph from '@tiptap/extension-paragraph'
import Table from '@tiptap/extension-table'
import TableCell from '@tiptap/extension-table-cell'
import TableHeader from '@tiptap/extension-table-header'
import TableRow from '@tiptap/extension-table-row'
import Text from '@tiptap/extension-text'
import History from '@tiptap/extension-history'
import { EditorContent, useEditor } from '@tiptap/react'
import { StyledTableEditor } from '../styles/TableEditor.styled';


export const TiptapTable = ({id, section, handleSave}) => {

  const [currTimeStart, setTimeStart] = useState()
  const [defTemplate, setdefTemplate] = useState('<p>loading<p>')
  const [isLoaded, setIsLoaded] = useState(false)


  const editor = useEditor({
    extensions: [
      History,
      Document,
      Paragraph,
      Text,
      Gapcursor,
      Table.configure({
        resizable: true,
      }),
      TableRow.extend({
        content: '(tableCell | tableHeader)*',
      }),
      TableHeader,
      TableCell,
    ],
    // I wish it was this easy
    content: (section.data) ? section.data : defTemplate,

  }, [section])

  const  pickTemplate = async (name) => {
    try{
	// grab from 'public' folder
      const res = await fetch(`/templates/${name}.json`,{
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
        },
      });
			const data = await res.json()
      
      console.log('data, ', data);
      setIsLoaded(true)
      setdefTemplate(data)

    } catch (err){
      console.warn('template error: ', err);
    }
  }


  useEffect(() => {
    setTimeStart(section.timeStart)

    if(!section.data) pickTemplate(section.header).catch(console.warn)
 
  // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [id, section, isLoaded])

  useEffect(() => {
    if (editor && !editor.isDestroyed) {

      if(section.data) editor.chain().focus().setContent(section.data).run()

      if(!section.data) editor.chain().focus().setContent(defTemplate).run()

      setIsLoaded(true)
      
    }
  }, [section, defTemplate, editor]);


  
  if (!editor) {
    return null
  }

  return isLoaded ? (<>

    <StyledTableEditor>
        ...
    </StyledTableEditor>
  </>) 
  : null
}

```

## reveal editor on focus
Needed a slick way to show and hide the editor once the user interacts with a specific section. It can all be done in CSS
```scss
.parent-wrapper{
	.toolbar{
	    opacity: 0;
	    z-index: -5;
	    transition: .2s;
	}
	
	&:focus-within{
	    .toolbar{
	      background-color: #00000082;
	      opacity: 1;
	      z-index: 5;
	    }
	}
}
```

---
## Credits 
- [link](https://github.com/ueberdosis/tiptap/issues/1451#issuecomment-941988769)
- [[developer/NodeJS/NodeJS\|NodeJS]]