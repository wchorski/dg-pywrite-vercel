---
{"dg-publish":true,"permalink":"/developer/CSS/CSS Gradient Background Image Transition/","dgPassFrontmatter":true}
---

Transiitioning a background image isn't a built in feature, but is a trick using pseudo elements

```html
<div className="grad-trans"> gradient </div>
```

```scss
.grad-trans{
  position: relative;
  background: rgb(66,66,66);
  height: 10em;
  background-image: linear-gradient(
    to right,
    hsl(211, 100%, 50%),
    hsl(179, 100%, 30%)
  );
  
  z-index: 1;
  transition: all 1s;

  &::before {
    position: absolute;
    content: '';
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-image: linear-gradient(
      to bottom,
      hsl(344, 100%, 50%),
      hsl(31, 100%, 40%)
    );
    z-index: -1;
    transition: opacity 0.5s linear;
    opacity: 0;
  }
  
  &:hover::before {
    opacity: 1;
  }
}
```


try using other CSS like `background-position: right` to also animate a single gradient

---
## Credit
- [Transitioning Gradients (keithjgrant.com)](https://keithjgrant.com/posts/2017/07/transitioning-gradients/)
- [Fun ways to animate CSS gradients - YouTube](https://www.youtube.com/watch?v=f3mwKLXpOLk&t=619s)