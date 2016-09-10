---
---
I am using API Gateway to provide a simple (or so I thought) API using AWS DynamoDB for a photo ordering web app. Some of the data is nested in List data type, and others in a Map data type.

After a lot of trial and error I came up with the following mapping template for the Integration Response.

As part of our application we allow quantity discounts on a product (this is a photo ordering app so a product would be an 8x10 from an image uploaded). We express pricing on the back end as a string of numbers in the form of qty,price,qty,price. This is then transformed into an object when we put into Dynamo like this: "qty":price,"qty":price - this is then stored in Dynamo as a Map data type.

Getting both the quantity and the price was a little hard as I couldn't find any examples that were like this.

The trick seems to be the keySet() - without it I could only get the Number part of the Map.

Here is a screen shot of an item - this is basically one row of a table and represents shipping and option prices. The part the code below deals with is just getting the prices for the shipping - which like an 8x10 can be qty based - with the order total being the qty, price being the shipping price. This is not real data, I was just using this to work on to get the formatting.

![DynamoDB screenshot of an item with Map data type](/assets/images/dynamodb-api-mapping-template-map-type.png)

{% highlight javascript linenos %}
#set($inputRoot = $input.path('$'))
#if($inputRoot.Count == '0' ) {"error":"no records"} #{else} 
#foreach($elem in $inputRoot.Items) 
{
   "lab_id":"$elem.lab_id.S",
   "require_userid": $elem.require_userid.BOOL,
   "sales_tax": $elem.sales_tax.N,
   "allowed_file_types": "$elem.allowed_file_types.S",
    "global_checkout_options": $elem.global_checkout_options.S,
    "shipping": [#foreach($shipitem in $elem.shipping.L)
    {"shippingmethod": "$shipitem.M.shippingmethod.S",
    "id": "$shipitem.M.id.S",
    "description": "$shipitem.M.description.S",
    "shiptype": "$shipitem.M.shiptype.S",
    "pos_code": "$shipitem.M.pos_code.S",
    "tieruses": "$shipitem.M.tieruses.S",
    "price":{#foreach($sprice in $shipitem.M.price.M.keySet())"$sprice":$shipitem.M.price.M.get($sprice).N#if($foreach.hasNext),#end #end},
    "taxable": $shipitem.M.taxable.BOOL}#if($foreach.hasNext),#end #end]   
} #end #end
{% endhighlight %}

And this is what it returns from the gateway (yeah, I know, the global_checkout_options is not valid json as it should be null or something other than blank):

{% highlight javascript linenos %}
{
   "lab_id":"rontest",
   "require_userid": true,
   "sales_tax": 0.08,
   "allowed_file_types": "jpg,jpeg,tif,psd",
    "global_checkout_options": ,
    "shipping": [    {"shippingmethod": "Lab Pickup",
    "id": "SJSdI0wD",
    "description": "Pickup at our lab in San Diego",
    "shiptype": "pickup",
    "pos_code": "labpickup",
    "tieruses": "ordertotal",
    "price":{"300":0.22, "10":0.4, "1":0.65, "100":0.27 },
    "taxable": false}, {"shippingmethod": "Lab Pickup2",
    "id": "SJzxSdI0wD",
    "description": "test",
    "shiptype": "pickup",
    "pos_code": "labpickupt",
    "tieruses": "ordertotal",
    "price":{"0":1, "7":0 },
    "taxable": false} ]   
}
{% endhighlight %}
