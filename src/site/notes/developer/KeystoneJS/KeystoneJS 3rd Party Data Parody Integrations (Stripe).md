---
{"dg-publish":true,"permalink":"/developer/KeystoneJS/KeystoneJS 3rd Party Data Parody Integrations (Stripe)/","dgPassFrontmatter":true}
---

> [!note] Payment Processing Modularity
> The big goal here (and what is causing this birds nest of an issue) is to keep my payment processor as a *plugin style* architecture. Having Stripe as an optional add-on to this template is non-negotiable. The app should be able to work with our without Stripe.

## Problem 1. Passing Product Id on Creation
> [!info] Example
In this example I'll be referring to creating and updating a Stripe Product (in parody with the app's db), but this also extends to creating **Customers**, **Coupons**, **Subscriptions**, etc.
### Using afterOperation hook
My initial thought was to create the **Stripe Product** so I can retrieve the `id` of the newly created object through the `item` property

the `stripeProductCreate()` is a wrapper function for Node Stripe API. Only thing to note is `item.id` is set to the `metadata` 

```json
{
	metadata: {
		productId: item.id
	}
}
```

```ts
afterOperation: {
	create: async ({ item, resolvedData, context }) => {
		const {
			id,
			name,
			excerpt,
			status,
			author,
			price,
			stripeProductId,
			stripePriceId,
			image,
		} = item

		try {
			const createdProduct = await stripeProductCreate({
				id,
				name: String(name),
				price: Number(price),
				excerpt,
				category: "product",
				status,
				authorId: author?.connect?.id || "no_author_id",
				type: "product",
				image,
				url: envs.FRONTEND_URL + `/products/${id}`,
				stripeProductId,
				stripePriceId,
				billing_interval: undefined,
			})

			if (createdProduct) {
				resolvedData.stripeProductId = createdProduct.id
				resolvedData.stripePriceId = String(createdProduct.default_price)
			}
		} catch (error: any) {
			console.log("!!! 💳 STRIPE:: ", {
				code: error.code,
				message: error.raw.message,
			})
		}
	},
	update: async ({ resolvedData, context, item }) => {
		await stripeProductUpdate({
			stripeProductId: item.stripeProductId,
			stripePriceId: item.stripePriceId,
			image: resolvedData.image as string | undefined,
			price: resolvedData.price as number | undefined,
			name: resolvedData.name as string | undefined,
			status: resolvedData.status as string | undefined,
			// category: resolvedData.category,
			category: "product",
			excerpt: resolvedData.excerpt as string | undefined,
			authorId: resolvedData.author?.connect?.id,
			billing_interval: undefined,
		}).then(async (res) => {
			if (!res) return

			if (res.default_price) {
				resolvedData.stripePriceId = String(res.default_price)
			}
		})

		resolvedData.dateModified = new Date().toISOString()
	},
},
```

> [!error] resolvedData does not set new data
> data set with `resolvedData` in the `afterOperation` does NOT save to the database.
### Update Via Context
Ok... so instead I'll use a `context.db.Product.updateOne` right? Except that will create an infinite loop of the app 
```
updating db -> hook triggers stripe api -> `.updateOne` re-triggers Product hook -> repeat
```

I could just run a check and see if all data is the same? like price, name, status, url. But that seems very cumbersome.

I could use a combination of beforeOperation and afterOperation, creating the product and immediately updating only the metadata. But I don't like this idea of multiple round trips to solve a seemingly trivial problem.
### Using beforeOperation hook
After running into the `afterOperation` brick wall, It seemed more logical to create a Stripe Product in the `beforeOperation`

The code is very similar with one key exception
```ts
beforeOperation: {
	create: async ({ item, resolvedData, context }) => {
		const {
			id,
			name,
			excerpt,
			status,
			author,
			price,
			stripeProductId,
			stripePriceId,
			image,
		} = resolvedData

		try {
			const createdProduct = await stripeProductCreate({
				name: String(name),
				price: Number(price),
				excerpt,
				category: "product",
				status,
				authorId: author?.connect?.id || "no_author_id",
				type: "product",
				image,
				url: envs.FRONTEND_URL + `/products/${id}`,
				stripeProductId,
				stripePriceId,
				billing_interval: undefined,
			})

			if (createdProduct) {
				resolvedData.stripeProductId = createdProduct.id
				resolvedData.stripePriceId = String(createdProduct.default_price)
			}
		} catch (error: any) {
			console.log("!!! 💳 STRIPE:: ", {
				code: error.code,
				message: error.raw.message,
			})
		}
	},
	update: async ({ resolvedData, context, item }) => {
		await stripeProductUpdate({
			stripeProductId: item.stripeProductId,
			stripePriceId: item.stripePriceId,
			image: resolvedData.image as string | undefined,
			price: resolvedData.price as number | undefined,
			name: resolvedData.name as string | undefined,
			status: resolvedData.status as string | undefined,
			// category: resolvedData.category,
			category: "product",
			excerpt: resolvedData.excerpt as string | undefined,
			authorId: resolvedData.author?.connect?.id,
			billing_interval: undefined,
		}).then(async (res) => {
			if (!res) return

			if (res.default_price) {
				resolvedData.stripePriceId = String(res.default_price)
			}
		})

		resolvedData.dateModified = new Date().toISOString()
	},
},
```

For linking the `stripeProductId` via `resolvedData` this works great... except now I can't pass the Product's Id to `metadata` because the object *hasn't been created yet*. I know it doesn't make sense, but if there was a way to "pass Id to 3rd party integration in beforeOperation hook" that would theoretically solve all of this.
### Using Stripe Ids to search
No matter, I'll use Stripe's provided Id's to *identify* my database items. This leads right into my second problem "Finding Product during Webhook"

## Custom Unique or Blank Validation
Brief pitstop here. I'll need custom validation to allow these **Stripe Id** fields to be unique OR be blank (`null`) to prevent any overlap between data. 

### isIndexed 'Unique'
How about I set `stripeProductId` field to `isIndexed: 'Unique'`? That way I can `where: ProductWhereUniqueInput!` and use `{ connect: stripeProductId }` or `.findOne()` easily during search or creation of data.
 
**Except** the `stripeProductId` is possibly a blank field (if the dev decides to forgo using Stripe). I realize I can make this app depend on Stripe and always set the `stripeProductId`, but I will re-emphasize the introductory note above

> [!note] Payment Processing Modularity
> The big goal here (and what is causing this birds nest of an issue) is to keep my payment processor as a *plugin style* architecture. Having Stripe as an optional add-on to this template is non-negotiable. The app should be able to work with our without Stripe.

Instead of the built in `isIndexed: 'unique'` I'll create a custom validate hook inside the field

`Product.ts` schema fields
```ts
...
stripeProductId: text({
	isIndexed: true,
	hooks: {
		validate: {
			create: async ({ resolvedData, context, addValidationError }) => {
				//? custom `unique` validation that also allows the field to be empty is not using stripe at all
				if (!resolvedData.stripeProductId) return
				const subPlans = await context.sudo().db.Product.findMany({
					where: {
						stripeProductId: {
							equals: resolvedData.stripeProductId,
						},
					},
				})
				if (subPlans.length > 0)
					addValidationError(
						"!!! Product can not share same stripeProductId with others"
					)
			},
			update: async ({ resolvedData, context, addValidationError }) => {
				if (!resolvedData.stripeProductId) return
				const subPlans = await context.sudo().db.Product.findMany({
					where: {
						stripeProductId: {
							equals: resolvedData.stripeProductId,
						},
					},
				})
				if (subPlans.length > 1)
					addValidationError(
						"!!! Product can not share same stripeProductId with others"
					)
			},
		},
	},
}),
stripePriceId: text({
	isIndexed: true,
	hooks: {
		validate: {
			create: async ({ resolvedData, context, addValidationError }) => {
				//? custom `unique` validation that also allows the field to be empty is not using stripe at all
				if (!resolvedData.stripePriceId) return
				const subPlans = await context.sudo().db.Product.findMany({
					where: {
						stripePriceId: {
							equals: resolvedData.stripePriceId,
						},
					},
				})
				if (subPlans.length > 0)
					addValidationError(
						"!!! Product can not share same stripePriceId with others"
					)
			},
			update: async ({ resolvedData, context, addValidationError }) => {
				if (!resolvedData.stripePriceId) return
				const subPlans = await context.sudo().db.Product.findMany({
					where: {
						stripePriceId: {
							equals: resolvedData.stripePriceId,
						},
					},
				})
				if (subPlans.length > 1)
					addValidationError(
						"!!! Product can not share same stripePriceId with others"
					)
			},
		},
	},
}),
...
```

I'm calling this a "custom unique validation" that also allows the field to be empty if dev is omitting the use of Stripe. This is a necessary step to prevent end users from the foot gun of linking the 2 or more products to the same Stripe Product
## Problem 2. Finding Product during Webhook
With correct validation set, only using stripe's Ids as keys to search the app's database still comes with a few undesired side effects.
### Data Search Workaround
Ok so I can't use `where: ProductWhereUniqueInput!`, but I can still use `db.Product.findMany()` to

```ts
async function findProductId(stripePriceId:string){
  const products = await keystoneContext.sudo().db.Product.findMany({
    where: {
      stripePriceId: {
        equals: stripePriceId,
      },
    },
  })  
  if(!products[0].id) throw new Error(`!!! products not found with stripePriceId:: ${stripePriceId}`, )
  //? schema field validation should not allow multiple items in this array
  return products[0].id
}
```

while the code works, the integrity still feels fragile. 

### Stripe Webhook
In this scenario, I've successfully completed a Stripe checkout session and Stripe returns back to my `webhook` with the response. 

I'm able to use this helper function to find and connect a product to this theoretical *Order Item* data object for my app's database.  Using `await Promise.all()`
```json
{
	orderItem: {
		product: {
			connect: { id: await findProductId(stripe.product.id)  },
		}
	}
}

```
## Conclusion
As to refrain from rambling on, I'm left with a deep "There must be a better way" feeling to all of this. 

I apologies If there is a simple solution I'm not seeing. Perhaps I've fiddled for too long in this code base and am reaching out to fresh eyes and experienced devs who have a better idea of how to handles this 1 to 1 setup between stand alone NextJS app and Stripe API integration. 