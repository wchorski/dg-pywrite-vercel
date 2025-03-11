---
{"dg-publish":true,"permalink":"/developer/KeystoneJS/KeystoneJS Apollo Filter Search/","dgPassFrontmatter":true}
---

This is more Apollo related, but [[developer/KeystoneJS/KeystoneJS\|KeystoneJS]] is built ontop so still is relevant 

creating a search filter that is not case sensitive was a nightmare to solve. Here I'll layout the Apollo Sandbox queries that I contstructed as an example

## Operation
```gql
query Products($where: ProductWhereInput!) {
  products(where: $where) {
    name
    description
  }

}
```

## Variables
```json
{
  "where": {
    "OR": [
      {
        "name": {
          "contains": "pear",
          "mode": "insensitive"
        }
      },
      {
        "description": {
          "mode": "insensitive",
          "contains": "pear"
        }
      }
    ]
  },

}
```

Here I'm searching all of the **Products** in my database. I'm looking for any *Product Name* or *Product Description* that has the searchterm "pear". 


Here is a more complex example that also evaluates tags and category names

## Operation v2
```gql
query Products($whereProduct: ProductWhereInput!) {
  products(where: $whereProduct) {
    name
    description
    id
    price
    photo {
      altText
      image {
        url
        width
        height
      }
    }
  }
}
```

## Variables v2
```json
{
	whereProduct: {
		OR: [
			{
				name: {
					contains: inputValue,
					mode: 'insensitive'
				}
			},
			{
				description: {
					contains: inputValue,
					mode: 'insensitive',
				}
			},
			{
				tags: {
					every: {
						name: {
							contains: inputValue,
							mode: 'insensitive'
						}
					}
				}
			},
			{
				categories: {
					every: {
						name: {
							contains: inputValue,
							mode: 'insensitive'
						}
					}
				}
			}
		]
	}
}
```