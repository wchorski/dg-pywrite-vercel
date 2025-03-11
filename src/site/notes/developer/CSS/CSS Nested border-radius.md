---
{"dg-publish":true,"permalink":"/developer/CSS/CSS Nested border-radius/","dgPassFrontmatter":true}
---

You've probably set the parent and child element `border-radius` to the same value expecting them to line up, but it's the padding that throws the angle of the curve off

The inner radius needs to equal the outer radius - padding

```scss
.parent{
	padding: 1.25rem;
	border-radius: 3rem;
}

.child{
	border-radius: 1.75rem;
}
```

---
## Credits
- [Do you make this mistake with border-radius? #css - YouTube](https://www.youtube.com/shorts/D0lIR1qVJOk)

## Backlinks
- [[developer/CSS/CSS\|CSS]]