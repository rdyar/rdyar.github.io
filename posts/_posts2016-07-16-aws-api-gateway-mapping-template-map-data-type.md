---
---
I am using API Gateway to provide a simple (or so I thought) API using AWS DynamoDB for a photo ordering web app. Some of the data is nested in List data type, and others in a Map data type.

After a lot of trial and error I came up with the following mapping template for the Inegration Response.

As part of our application we allow quantity discounts on a product (this is a photo ordering app so a product would be an 8x10 from an image uploaded). We express pricing on the back end as a string of numbers in the form of qty,price,qty,price. This is then transformed into an object when we put into Dynamo like this: "qty":price,"qty":price - this is then stored in Dynamo as a Map data type.

Getting both the quantity and the price was a little hard as I couldn't find any examples that were like this.

The trick seems to be the KeySet - without it I could only get the Number part of the Map.

Here is a screen shot of an item - this is basically one row of a table and represents shipping and option prices. The part the code below deals with is just getting the prices for the shipping - which like an 8x10 can be qty based - with the order total being the qty, price being the shipping price. This is not real data, I was just using this to work on to get the formatting.

![DynamoDB screenshot of an item with Map data type](/assets/images/dynamodb-api-mapping-template-map-type.png)
