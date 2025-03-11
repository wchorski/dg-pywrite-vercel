---
{"dg-publish":true,"permalink":"/developer/HTML/Native HTML form validation/","dgPassFrontmatter":true}
---

another helpful video form Kevin Powell [Improve your form validation hints without JS! - YouTube](https://www.youtube.com/watch?v=s2ThIxm7FyA) helps me understand how to get quick and easy validation without needing another JS library.

## HTML (JSX)
```jsx
<label className="formInput" htmlFor={inputProps.name}>

		<span className="label"> {label} </span>
		
		<input
			name='title',
      type='text',
      placeholder='my note title...', 
      label='title',
      errorMessage='title error',
      required=true,
      initial='',
      pattern='[a-zA-Z0-9 ]+',
			onFocus={() =>
				inputProps.name === "confirmPassword" && setFocused(true)
			}
			focused={focused.toString()}
		/>
		
		<span className="tooltip error">{errorMessage}</span>
		<span className="tooltip hint">{hint}</span>
</label>
```

showing the error was a bit tricky. The error was showing when the input was empty, that's where `:not(:placeholder-shown)` came in to help

## SCSS
```scss
	span.tooltip {
    font-size: 12px;
    /* padding: 3px; */
    display: block;
    transition: all .3s;

    max-height: 0;
    overflow: hidden;
    
    &.error{
      color: red;
      /* display: none; */
    }

    &.hint{
      color: #858585;
    }

    &:empty{
      padding: 0;
    }
  }

  input:invalid:not(:placeholder-shown):focus{
    border: 1px solid red;
  }

  input:invalid:not(:placeholder-shown):focus ~ span{
    max-height: 30px;
  }
```