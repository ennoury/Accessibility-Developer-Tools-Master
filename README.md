# Arvato Inventory Service

Browser and node module for making API requests against [Arvato Inventory Service](https://{baseUri}/tenants/{tenant}).

**Please note: This module uses [Popsicle](https://github.com/blakeembrey/popsicle) to make API requests. Promises must be supported or polyfilled on all target environments.**

## Installation

```
npm install arvato-inventory-service --save
bower install arvato-inventory-service --save
```

## Usage

### Node

```javascript
var ArvatoInventoryService = require('arvato-inventory-service');

var client = new ArvatoInventoryService();
```

### Browsers

```html
<script src="arvato-inventory-service/index.js">

<script>
  var client = new window.ArvatoInventoryService();
</script>
```

### Options

You can set options when you initialize a client or at any time with the `options` property. You may also override options for a single request by passing an object as the second argument of any request method. For example:

```javascript
var client = new ArvatoInventoryService({ ... });

client.options = { ... };

client.resource('/').get(null, {
  baseUri: 'http://example.com',
  headers: {
    'Content-Type': 'application/json'
  }
});
```

#### Base URI

You can override the base URI by setting the `baseUri` property, or initializing a client with a base URI. For example:

```javascript
new ArvatoInventoryService({
  baseUri: 'https://example.com'
});
```

#### Base URI Parameters

If the base URI has parameters inline, you can set them by updating the `baseUriParameters` property. For example:

```javascript
client.options.baseUriParameters.version = 'v3';
```

### Resources

All methods return a HTTP request instance of [Popsicle](https://github.com/blakeembrey/popsicle), which allows the use of promises (and streaming in node).

#### resources.sources

```js
var resource = client.resources.sources;
```

##### POST

Creates a new source. If you do not specify an ID, it will be auto-generated.

```js
resource.post().then(function (res) { ... });
```

##### Body

**application/json**

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "id": "https://api.yaas.io/arvato/inventory/v1/public/api/schema/Source.json",
  "title": "Source",
  "type": "object",
  "description": "Source",
  "properties": {
    "id": {
      "type": "string"
    },
    "name": {
      "type": "string"
    },
    "type": {
      "type": "string"
    },
    "latitude": {
      "type": "number"
    },
    "longitude": {
      "type": "number"
    },
    "country": {
      "type": "string"
    },
    "state": {
      "type": "string"
    },
    "thresholdLow": {
      "type": "integer"
    },
    "thresholdHigh": {
      "type": "integer"
    },
    "zones": {
      "type": "object",
      "properties": {
        "id": {
          "type": "string"
        },
        "name": {
          "type": "string"
        }
      },
      "required": [
        "id",
        "name"
      ]
    }
  },
  "required": [
    "name"
  ]
}
```

##### GET

Retrieves all sources. Supports pagination with 'limit' and 'start' query parameters. Next page link and previous page link are returned in the response headers. The results are returned in order of ASCII character code values of the source name. The sort order is ascending by default but can be reversed.

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **limit** _integer_

The maximum number of source items returned. Default value is 10. Max value is 50.

* **start** _string_

If present, the result set will start with the source specified.

* **prevStart** _string_

A list of previously visited pages. Required for browsing through pages.

#### resources.sources.sourceId(sourceId)

* **sourceId** _string_

Source ID

```js
var resource = client.resources.sources.sourceId(sourceId);
```

##### GET

Retrieves the specified source.

```js
resource.get().then(function (res) { ... });
```

##### PUT

Updates an existing source.

```js
resource.put().then(function (res) { ... });
```

##### Body

**application/json**

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "id": "https://api.yaas.io/arvato/inventory/v1/public/api/schema/Source.json",
  "title": "Source",
  "type": "object",
  "description": "Source",
  "properties": {
    "id": {
      "type": "string"
    },
    "name": {
      "type": "string"
    },
    "type": {
      "type": "string"
    },
    "latitude": {
      "type": "number"
    },
    "longitude": {
      "type": "number"
    },
    "country": {
      "type": "string"
    },
    "state": {
      "type": "string"
    },
    "thresholdLow": {
      "type": "integer"
    },
    "thresholdHigh": {
      "type": "integer"
    },
    "zones": {
      "type": "object",
      "properties": {
        "id": {
          "type": "string"
        },
        "name": {
          "type": "string"
        }
      },
      "required": [
        "id",
        "name"
      ]
    }
  },
  "required": [
    "name"
  ]
}
```

##### DELETE

Deletes the source. This operation will only succeed if the source is empty. You must delete all SKUs related to the source first.

```js
resource.delete().then(function (res) { ... });
```

#### resources.sources.sourceId(sourceId).batches.stocks

```js
var resource = client.resources.sources.sourceId(sourceId).batches.stocks;
```

##### POST

Creates a new batch for creating new or updating existing stock items and their histories.

```js
resource.post().then(function (res) { ... });
```

##### Body

**application/json**

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "title": "Stocks",
  "type": "array",
  "description": "An array of stocks",
  "items": {
    "type": "object",
    "properties": {
      "sourceId": {
        "type": "string"
      },
      "sku": {
        "type": "string"
      },
      "onHand": {
        "type": "integer"
      },
      "reserved": {
        "type": "integer"
      },
      "available": {
        "type": "integer"
      }
    }
  }
}
```

#### resources.sources.sourceId(sourceId).stocks

```js
var resource = client.resources.sources.sourceId(sourceId).stocks;
```

##### GET

Retrieves all stocks of a source. Supports pagination with 'limit' and 'start' query parameters. Next page link and previous page link are returned in the response headers. The results are returned in order of ASCII character code values of the SKU. The sort order is ascending.

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **limit** _integer_

The maximum number of stock items returned. Default value is 10. Max value is 50.

* **start** _string_

If present, the result set will start with the SKU specified.

* **prevStart** _string_

A list of previously visited pages. Required for browsing through pages.

##### POST

Creates a single new stock item and its history.

```js
resource.post().then(function (res) { ... });
```

##### Body

**application/json**

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "id": "https://api.yaas.io/hybris/schema/v1/inventorydemo/Stock.v1.json",
  "title": "Stock",
  "type": "object",
  "properties": {
    "sourceId": {
      "type": "string"
    },
    "sku": {
      "type": "string"
    },
    "onHand": {
      "type": "integer"
    },
    "reserved": {
      "type": "integer"
    },
    "available": {
      "type": "integer"
    }
  }
}
```

#### resources.sources.sourceId(sourceId).stocks.sku(sku)

* **sku** _string_

Product sku

```js
var resource = client.resources.sources.sourceId(sourceId).stocks.sku(sku);
```

##### GET

Retrieves the specified stock item for the product and source. You can optionally send the `requested` parameter. If the parameter is present, then the inventory service will perform a check against the amount available. If the amount available is less than the amount requested, a 409 error response is returned. A common use case is to perform this check when adding a product to the shopping cart and reject the action if there's not enough inventory. If you do not specify the requested parameter, then a snapshot of the the stock item is returned.

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **requested** _integer_

Optional quantity requested.

##### DELETE

Deletes the specific stock item and all releated history items. This operation is idempotent; running it multiple times on the same item does not result in an error response. This operation will only succeed if there are no EPC items related to this stock item.

```js
resource.delete().then(function (res) { ... });
```

#### resources.sources.sourceId(sourceId).stocks.sku(sku).transactions

```js
var resource = client.resources.sources.sourceId(sourceId).stocks.sku(sku).transactions;
```

##### GET

Retrieves the stock histories for the specified SKU and source.

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **limit** _integer_

The maximum number of stock transaction histories returned. Default value is 10. Max value is 50.

* **start** _string_

If present, the result set will start with the transaction as specified. The transaction is identified by time and id.

* **prevStart** _string_

A list of previously visited pages. Required for browsing through pages.

##### POST

Creates and executes a new stock transaction. Possible values for type are CREATE, UPDATE, RESERVE, RELEASE and COMMIT. A corresponding history record will also be created.

```js
resource.post().then(function (res) { ... });
```

##### Body

**application/json**

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "id": "https://api.yaas.io/hybris/schema/v1/inventorydemo/StockTransaction.v1.json",
  "type": "object",
  "description": "A stock transaction",
  "properties": {
    "type": {
      "type": "string"
    },
    "quantity": {
      "type": "integer"
    },
    "description": {
      "type": "string"
    },
    "time": {
      "type": "string",
      "format": "date-time"
    }
  },
  "required": [
    "type",
    "quantity"
  ]
}
```



### Custom Resources

You can make requests to a custom path in the API using the `#resource(path)` method.

```javascript
client.resource('/example/path').get();
```

## License

Apache 2.0
