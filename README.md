# dynode [![Build Status](https://secure.travis-ci.org/Wantworthy/dynode.png)](http://travis-ci.org/Wantworthy/dynode)
node.js client for working with Amazon's [DynamoDB](http://docs.amazonwebservices.com/amazondynamodb/latest/developerguide/Introduction.html?r=5378) service.

## Installation

### Installing npm (node package manager)
``` bash
  $ curl http://npmjs.org/install.sh | sh
```

### Installing dynode
``` bash 
  $ [sudo] npm install dynode
```
## Motivation
Dynode is designed to be a simple and easy way to work with Amazon's DynamoDB service. Amazon's http api is complicated and non obvious how to interact with it. This driver aims to offer a simplified more obvious way of working with DynamoDB, but without getting in your way or limiting what you can do with DynamoDB.

## Usage
There are two different ways to use dynode: directly via the default dynamoDB client, or by instantiating your own client. The former is merely intended to be a convenient shared client to use throughout your application if you so choose.

### Using the Default DynamoDB Client
The default client is accessible through the dynode module directly. Any method that you could call on an instance of a client is available on the default client:

``` js
  var dynode = require('dynode');
  // When using the default client you must first give it auth credentials
  dynode.auth({accessKeyId: "AWSAccessKey", secretAccessKey: "SecretAccessKey"});

  dynode.createTable("NewTable", console.log);
  dynode.listTables(console.log);
```

### Instantiating your own DynamoDB Client
If you would prefer to manage your own client, potentially with different auth params:

``` js
  var client = new (dynode.Client)({
	accessKeyId: "AWSAccessKey", secretAccessKey: "SecretAccessKey"
  });
```

## API Documentation

* [Auth](#auth)
* [Create Table](#createTable)
* [List Tables](#listTables)
* [Describe Table](#describeTable)
* [updateTable](#updateTable)
* [Put Item](#putItem)
* [Update Item](#updateItem)
* [Get Item](#getItem)
* [Delete Item](#deleteItem)
* [Query](#query)
* [Scan](#scan)
* [Batch Get Item](#batchGetItem)

<a name="auth"></a>
## Auth

Before you can perform any operations on DynamoDB you need to provide your Amazon credentials.

``` js
  dynode.auth({accessKeyId: "AWSAccessKey", secretAccessKey: "SecretAccessKey"});
```

<a name="createTable"></a>
## Create Table
The CreateTable operation adds a new table to your account. For more info see [here][createTablesDocs]

By default `createTable` will create the given table with a primary key of id : String, a read capacity of 10 and write capacity of 5.

``` js
  dynode.createTable("ExampleTable", console.log);
```

`createTable` accepts an options hash to override any of the table creation defaults.

``` js
  var opts = {read: 20, write: 25, hash: {name: String}, range: {age: Number}};
  dynode.createTable("ExampleTable", opts, console.log);
```

<a name="listTables"></a>
## List Tables
Returns an array of all the tables associated with the current account. For more info see [here][listTablesDocs]

By default `listTables` will list all of your DynamoDB tables.

``` js
  dynode.listTables(console.log);
```

You can also pass in options to filter which tables to list. See [Amazon's docs][listTablesDocs] for more info 

``` js
  dynode.listTables({Limit: 3, ExclusiveStartTableName: "ExampleTable"}, console.log);
```

<a name="describeTable"></a>
## Describe Table

Returns information about the table, including the current status of the table, the primary key schema and when the table was created. 
For more info see [here][describeTableDocs]

``` js
  dynode.descibeTable("ExampleTable", console.log);
```

<a name="updateTable"></a>
## Update Table

Updates the provisioned throughput for the given table. For more info see [here][updateTableDocs]

``` js
  dynode.updateTable("ExampleTable", {read: 15, write: 10}, console.log);
```

<a name="putItem"></a>
## Put Item
Creates a new item, or replaces an old item with a new item (including all the attributes). For more info see [here][putItemDocs]

``` js
  dynode.putItem("ExampleTable", {name : "Foo", age: 80, baz : ["a", "b", "c"], nums: [1,2,3]}, console.log);
```

You can also pass in any option that Amazon accepts.

``` js
  var item = {name : "Bob"};
  var options = {ReturnValues:"ReturnValuesConstant", Expected :{"age":{"Value": {"N":"42"},{"Exists":Boolean}}}};

  dynode.putItem("ExampleTable", item, options, console.log);
```

## Tests
All tests are written with [mocha][0] and should be run with make:

``` bash
  $ make test
```

#### Author: [Ryan Fitzgerald](http://twitter.com/#!/TheRyanFitz)
#### License: [Apache 2.0][1]

[0]: http://visionmedia.github.com/mocha/
[1]: http://www.apache.org/licenses/LICENSE-2.0
[createTablesDocs]: http://docs.amazonwebservices.com/amazondynamodb/latest/developerguide/API_CreateTable.html
[listTablesDocs]: http://docs.amazonwebservices.com/amazondynamodb/latest/developerguide/API_ListTables.html
[describeTableDocs]: http://docs.amazonwebservices.com/amazondynamodb/latest/developerguide/API_DescribeTables.html
[updateTableDocs]: http://docs.amazonwebservices.com/amazondynamodb/latest/developerguide/API_UpdateTable.html
[putItemDocs]: http://docs.amazonwebservices.com/amazondynamodb/latest/developerguide/API_PutItem.html