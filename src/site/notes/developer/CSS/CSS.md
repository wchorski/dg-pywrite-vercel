---
{"dg-publish":true,"permalink":"/developer/CSS/CSS/","dgPassFrontmatter":true}
---


### if parent has input that is checked
in a todo list, I wanted to cross out or strikethrough all children if the input was checked
```html
<li class="task-list-item">
	<input type="checkbox" disabled checked>
	front page with growing sprout garden. flowers are clickable graph view?
</li>
```

```css
li.task-list-item:has(input:checked){

  opacity: .6;
  text-decoration:line-through;
}
```

### Centered header label with horizontal line on sides
[link](https://iqcode.com/code/css/css-technique-for-a-horizontal-line-with-icons-in-the-middle#:~:text=CSS technique for a horizontal line with icons,line-through%3B } View another examples Add Own solution)
check out the `graph ` and  `Table of Contents` labels in this screenshot 

![attachments/Pasted image 20221109140342 1.png|sections with headers   |     500](/img/user/attachments/Pasted%20image%2020221109140342%201.png)
with the power of `:before` & `:after` you too can make this stylish "strike through" effect

```scss
p.icon-label{
    color: #5e5e5e;
    font-size: 1rem;
    overflow: hidden;
    text-align: center;

    &:before,
    &:after {
      background-color: #5e5e5e;
      content: "-";
      display: inline-block;
      height: 1px;
      position: relative;
      vertical-align: middle;
      width: 50%;
    }  

    &:before {
      right: 0.5em;
      margin-left: -50%;
    }

    &:after {
      left: 0.5em;
      margin-right: -50%;
    }
  }
```

### disable interaction and or click through element
very useful when hiding a top layer UI element with opacity = 0 

```css
pointer-events: none;
```

### hide scrollbar on hover
[link](https://iqcode.com/code/css/hover-show-scrollbar-css#:~:text=hover show scrollbar css CPP div { height%3A,overflow%3A hidden%3B } div%3Ahover { overflow-y%3A scroll%3B })
```css
div {
  height: 100px;
  width: 50%;
  margin: 0 auto;
  overflow: hidden;
}

div:hover {
  overflow-y: scroll;
}
```

### Fix word wrap indentation in list elements
[CSS - Indenting the second line of LI (List Items) - Silva Web Designs](https://silvawebdesigns.com/css-indenting-second-line-li-list-items/)
```css
ul { 
	list-style: none; 
	width: 200px; 
	text-indent: -20px; /* MATCH key property */ 
	margin-left: 20px; /* MATCH key property */ 
} 

li { margin-bottom: 10px; }
```

### Custom Scrollbar 
[How To Create a Custom Scrollbar (w3schools.com)](https://www.w3schools.com/howto/howto_css_custom_scrollbar.asp)
```css
/* width */  
::-webkit-scrollbar {  width: 20px; }  
  
/* Track */  
::-webkit-scrollbar-track {  
	box-shadow: inset 0 0 5px grey;  
	border-radius: 10px;
}  
  
/* Handle */  
::-webkit-scrollbar-thumb {  
	background: red;  
	border-radius: 10px;
}
```

## Code Pen Inspiration
- [link](https://codepen.io/b1tn3r/embed/YjOzRv?height=300&default-tab=css%2Cresult&slug-hash=YjOzRv&editable=true&user=b1tn3r&name=cp_embed_40#result-box)
- [link](https://codepen.io/avstorm/embed/rNBZby?default-tab=css%2Cresult&editable=true&height=300&name=cp_embed_26&slug-hash=peCbd&user=avstorm#result-box)
- [link](https://codepen.io/lbebber/embed/LELBEo?height=300&default-tab=css%2Cresult&slug-hash=LELBEo&editable=true&user=lbebber&name=cp_embed_6#result-box)
- [link](https://codepen.io/Grsmto/embed/RPQPPB?height=300&default-tab=css%2Cresult&slug-hash=RPQPPB&editable=true&user=Grsmto&name=cp_embed_5#result-box)
- [link](https://codepen.io/andrewmillen/embed/MoKLob?height=300&default-tab=css%2Cresult&slug-hash=MoKLob&editable=true&user=andrewmillen&name=cp_embed_1#result-box)
- [[mousedown for rotation) (codepen.io)](mousedown for rotation\|Pure CSS 3D Synthesizer (mousedown for rotation) (codepen.io)]]%20(codepen.io))

### Web Tools
- [Gradient Generator - CSS gradients made easy - ColorGradient](https://colorgradient.dev/gradient-generator)

### how to really mask
[Using CSS Masks to Create Jagged Edges | CSS-Tricks - CSS-Tricks](https://css-tricks.com/using-css-masks-to-create-jagged-edges/)
```html
<div class="container">
	<img src="...">
</div>
```

```css
.container{
	background-color: blue;
}

img {
    mask-image: linear-gradient(
      to bottom right,
      white,
      white 50%,
      transparent 50%,
      transparent),
    linear-gradient(
      to top,
      transparent 30px,
      black 30px,
      white);
      
    mask-size: 30px 30px, 100% 100%;
    mask-repeat: repeat-x;
    mask-position: left bottom;
    display: block;
    margin-bottom: 0.5em;
}
```

[[developer/CSS/Cascading Style Sheet Tips & Tricks\|Cascading Style Sheet Tips & Tricks]]