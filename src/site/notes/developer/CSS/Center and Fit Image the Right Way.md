---
{"dg-publish":true,"permalink":"/developer/css/center-and-fit-image-the-right-way/","dgPassFrontmatter":true}
---


I wanted a defacto way to center an image. Looks like I found a decent way

```css
picture {
  overflow: auto;
  display: grid;
  grid-template-rows: minmax(0,1fr) auto;
  resize: both;
  border: 2px solid red;
}

picture img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  object-position: 50% 0;
}
```

```xml
<picture style="width:100px;height:170px;">
  <img src="https://www.williamusic.com/wp-content/uploads/2021/07/WM_Logo4.png">
  
  <figcaption>myText caption</figcaption>
</picture>
```

---
## Credits
- [css - Align everything to the top when used object-fit: contain; - Stack Overflow](https://stackoverflow.com/questions/72429105/align-everything-to-the-top-when-used-object-fit-contain)