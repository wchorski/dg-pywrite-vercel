---
{"dg-publish":true,"permalink":"/developer/css/css-color-mixing/","dgPassFrontmatter":true}
---

mixing colors in [[developer/CSS/CSS\|CSS]] is now a breeze. Create quick color pallet themes that can easily be tweaked.

```html
<div class="wrapper">
	<div class="box">1</div>
	<div class="box">2</div>
	<div class="box">3</div>
	<div class="box">4</div>
	<div class="box">5</div>
	<div class="box">6</div>
	<div class="box">7</div>
	<div class="box">8 color 3 light</div>
	<div class="box">9 color 3 dark </div>
</div>
```

```scss
	--color-1: red;
  --color-2: blue;
  --color-3: green;
  --color-3-light: color-mix(
      in srgb, white 70%, var(--color-3)
    );
  --color-3-dark: color-mix(
      in srgb, black 50%, var(--color-3)
    );
 
  display: flex;
  width: 100%;

  .box{
    /* width: 30px; */
    flex: 1;
    height: 100px;
  }

  .box:nth-child(1){
    background: var(--color-1)
  }
  .box:nth-child(2){
    background: color-mix(
      in srgb, var(--color-1) 80%, var(--color-2)
    );
  }
  .box:nth-child(3){
    background: color-mix(
      in srgb, var(--color-1) 40%, var(--color-2)
    );
  }

  .box:nth-child(4){
    background: var(--color-2)
  }
  .box:nth-child(5){
    background: color-mix(
      in srgb, var(--color-2) 80%, var(--color-3)
    );
  }
  .box:nth-child(6){
    background: color-mix(
      in srgb, var(--color-2) 40%, var(--color-3)
    );
  }

  .box:nth-child(7){
    background: var(--color-3)
  }
  .box:nth-child(8){
    background: var(--color-3-light)
  }
  .box:nth-child(9){
    background: var(--color-3-dark)
  }
```

---
## Credits
- [This new CSS function makes color schemes a breeze - YouTube](https://www.youtube.com/shorts/CZ_LaL5DZk0)

## Backlinks
- [[developer/CSS/CSS\|CSS]]