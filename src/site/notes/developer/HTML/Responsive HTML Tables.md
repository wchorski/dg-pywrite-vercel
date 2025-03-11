---
{"dg-publish":true,"permalink":"/developer/html/responsive-html-tables/","dgPassFrontmatter":true}
---

Create a responsive table without libraries or frameworks.

## HTML
```html
<table>
	<caption> Table Caption </caption>

	<thead>
		<tr>
			<th>Name</th>
			<th>Type</th>
			<th>Height (m)</th>
			<th>Weight (kg)</th>
			<th>Base Experience</th>
		</tr>
	</thead>
	
	<tbody>
		<tr>
			<td data-cell="name">Pikachu</td>
			<td data-cell="type">Electric</td>
			<td data-cell="height">0.4</td>
			<td data-cell="weight">6.0</td>
			<td data-cell="base-exp">112</td>
		</tr>
		
		<tr>
			<td data-cell="name">Charmander</td>
			<td data-cell="type">Fire</td>
			<td data-cell="height">0.6</td>
			<td data-cell="weight">8.5</td>
			<td data-cell="base-exp">62</td>
		</tr>
		
		<tr>
			<td data-cell="name">Squirtle</td>
			<td data-cell="type">Water</td>
			<td data-cell="height">0.5</td>
			<td data-cell="weight">9.0</td>
			<td data-cell="base-exp">63</td>
		</tr>
		
		<tr>
			<td data-cell="name">Bulbasaur</td>
			<td data-cell="type">Grass/Poison</td>
			<td data-cell="height">0.7</td>
			<td data-cell="weight">6.9</td>
			<td data-cell="base-exp">64</td>
		</tr>
		
		<tr>
			<td data-cell="name">Jigglypuff</td>
			<td data-cell="type">Normal/Fairy</td>
			<td data-cell="height">0.5</td>
			<td data-cell="weight">5.5</td>
			<td data-cell="base-exp">95</td>
		</tr>
		
		<tr>
			<td data-cell="name">Snorlax</td>
			<td data-cell="type">Normal</td>
			<td data-cell="height">2.1</td>
			<td data-cell="weight">460.0</td>
			<td data-cell="base-exp">189</td>
		</tr>
	</tbody>
	

</table>
  

```

## SCSS
```scss


table{

  background-color: #909c9f;
  color: white;
  border-collapse: collapse;
  padding: 1rem;

  th, td, caption {
    padding: .1rem 1rem;
  }

  caption{
    background-color: #479282;
    font-size: 1.5rem;
    font-weight: 700;
    text-transform: uppercase;
  }

  th {
    text-align: left;
    background-color: hsl( 0 0% 0% / 0.5);
    border-right: solid 1px black;
  }
  tr:nth-of-type(2n) {
    background-color: hsl( 0 0% 0% / 0.1);
  }

  @media (max-width: 650px) {

    th{
      display: none;
    }

    td{
      display: grid;
      grid-template-columns: 15ch auto;
      gap: .5rem;
      padding: 0.5rem 1rem;
    }

    td:first-child{
      padding-top: 2rem;
    }
    td:last-child{
      padding-bottom: 2rem;
    }

    td::before {
      content: attr(data-cell) ': ';
      font-weight: 700;
      text-transform: capitalize;
    }
  }
}
```


## Accessablility 


---
## Credits 
- [A Responsive Accessible Table — Adrian Roselli](https://adrianroselli.com/2017/11/a-responsive-accessible-table.html)
- Kevin Powel - https://www.youtube.com/watch?v=czZ1PvNW5hk

## Backlinks
- [[developer/HTML/HTML, Back to Basics\|HTML, Back to Basics]]