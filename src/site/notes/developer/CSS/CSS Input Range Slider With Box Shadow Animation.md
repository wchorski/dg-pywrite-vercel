---
{"dg-publish":true,"permalink":"/developer/css/css-input-range-slider-with-box-shadow-animation/","dgPassFrontmatter":true}
---


```html
<label htmlFor="slider" className="slider-cont">

		<span> Range Input </span>
		
		<input
			type="range"
			name="slider"
			list="slider-marks"
			min="0"
			max="100"
			step="10"
			onChange={(e) => setSliderVal(e.target.value)}
		/>
	
			<output id="value" className="tooltip">
				50% 
			</output>
	 
	</label>
```

for the `<output>` tag I use [[developer/ReactJS/ReactJS\|ReactJS]] to handle state, so you're on your own if you want to use vanilla #javascript 

```scss
label.slider-cont{
  max-width: 30em;
  margin: 1em auto;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1em;

  &:hover input[type='range']::-webkit-slider-thumb{
    box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.774), inset 1px 1px 20px #25ecd1;
  }

  input[type='range']{
    -webkit-appearance: none;
    width: 100%;
    height: 0.7em;
    outline: none;
    border-radius: 99px;
    background: rgb(99, 99, 99);
    box-shadow: inset 3px 3px 2px rgb(87, 87, 87);
    cursor: pointer;
    transition: all ease-in-out .3s;

    &::-webkit-slider-thumb{
      -webkit-appearance: none;
      position: relative;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background: rgb(66,66,66);
      border: 1px solid #664AFF;
      box-shadow: 3px 3px 8px rgba(0, 0, 0, 0.774), inset 1px 1px 1px #25ecd1;
      transition: all ease-in-out .5s;
      cursor:ew-resize;

      &::before{
        position: absolute;
        content: 'xxxxxxx';
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
        
      }
    }
  }
}
```