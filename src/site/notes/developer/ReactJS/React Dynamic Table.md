---
{"dg-publish":true,"permalink":"/developer/ReactJS/React Dynamic Table/","dgPassFrontmatter":true}
---

This is a [[developer/ReactJS/ReactJS\|ReactJS]] expansion to my [[developer/HTML/Responsive HTML Tables\|Responsive HTML Tables]]. 

## index.js
```tsx
import React from 'react';
import { Table } from '../components/elements/Table';

export default function Home() {

  return (<>
    <h1>Home</h1>

    <Table
      caption='Pokemon'
      route='/pokemon'
      headers={[
        'name',
        'type',
        'height',
        'weight',
        'baseExp',
        'link'
      ]}
      cells={[
        {
          name: 'pikachu',
          type: 'electric',
          height: '0.4',
          weight: '6.0',
          baseExp: '112',
        },
        {
          name: 'Charmander',
          type: 'Fire',
          height: '0.6',
          weight: '8.5',
          baseExp: '62',
        },
      ]}
    />

  </>)
}
```

If the table links to other data, then It dynamically adds a link in one of the chosen cells named `link` `url` `uri` `a` `anchor` `account`  ect. 

## Table.js
```tsx
import Link from 'next/link';
import React, { useState } from 'react';
import styled from 'styled-components';

interface TableProps {
  caption: string;
  cells: object[];
  headers?: string[];
  route?: string;
}

export function Table({ caption, cells, headers, route }: TableProps) {

  const [keys, setKeys] = useState<any>(headers)
  const linkterms = ['link', 'url', 'uri', 'a', 'anchor', 'account',]

  // ? if u want to automatically grab keys from object
  
  // const [keys, setKeys] = useState<any>(Object.keys(cells[0]))
  
  // // Extract the keys from the first item in the data array
  // useState(() => {
  //   if (cells.length > 0) {
  //     setKeys(Object.keys(cells[0]))
  //   }
  // }, [cells]);

  return (
    <div role="region" aria-labelledby="Cap" tabIndex={0}>
      <StyledTable role="table">
        <caption role="caption"> {caption} </caption>

        <thead role="rowgroup">
          <tr role="row">
            {keys.map((key: string) => (
              <th key={key} role="columnheader">{key}</th>
            ))}
          </tr>
        </thead>

        <tbody role="rowgroup">
          {cells.map((item: any, index: number) => (
            <tr key={index} role="row">
              {keys.map((key: string) => (
                <td key={key} data-cell={key} role="cell">
                  {route && linkterms.includes(key) ? (
                    <Link href={`${route}/${item['id']}`}> {key} </Link>
                  ) : (
                    item[key]
                  )}
                </td>
              ))}
            </tr>
          ))}
        </tbody>
      </StyledTable>
    </div>
  );
}

const StyledTable = styled.table`
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
`

```

## Credit
- ChatGPT   🤖 
- [[developer/HTML/Responsive HTML Tables\|Responsive HTML Tables]]
- [Functions to Add ARIA to Tables and Lists — Adrian Roselli](https://adrianroselli.com/2018/05/functions-to-add-aria-to-tables-and-lists.html)