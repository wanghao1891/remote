# 12、2015-05-19
* 索引学习整理

  The following diagram illustrates a query that selects and orders the matching documents using an index:

  ![](http://docs.mongodb.org/manual/_images/index-for-sort.png)

  ## 1) Single Field Index

  ### Example:

      { "_id" : ObjectId(...),
        "name" : "Alice",
        "age" : 27
      }

      db.friends.createIndex( { "name" : 1 } )

  ### Indexes on Embedded Fields

      {"_id": ObjectId(...),
       "name": "John Doe",
       "address": {
              "street": "Main",
              "zipcode": "53511",
              "state": "WI"
              }
      }

      db.people.createIndex( { "address.zipcode": 1 } )

  ### Indexes on Embedded Documents

      {
        _id: ObjectId(...),
        metro: {
                 city: "New York",
                 state: "NY"
               },
        name: "Giant Factory"
      }

      db.factories.createIndex( { metro: 1 } )

      db.factories.find( { metro: { city: "New York", state: "NY" } } )

      This query returns the above document. When performing equality matches on embedded documents, field order matters and the embedded documents must match exactly. For example, the following query does not match the above document:

      db.factories.find( { metro: { state: "NY", city: "New York" } } )

  ## 2) Compound Indexes

  ### EXAMPLE:
      {
       "_id": ObjectId(...),
       "item": "Banana",
       "category": ["food", "produce", "grocery"],
       "location": "4th Street Store",
       "stock": 4,
       "type": "cases",
       "arrival": Date(...)
      }

      db.products.createIndex( { "item": 1, "stock": 1 } )

  The order of the fields in a compound index is very important. In the previous example, the index will contain references to documents sorted first by the values of the item field and, within each value of the item field, sorted by values of the stock field.

  ### Sort Order

      db.events.find().sort( { username: 1, date: -1 } )
      db.events.find().sort( { username: -1, date: 1 } )
      db.events.createIndex( { "username" : 1, "date" : -1 } )

  However, the above index cannot support sorting by ascending username values and then by ascending date values, such as the following:

      db.events.find().sort( { username: 1, date: 1 } )

  ### Prefixes

  Index prefixes are the beginning subsets of indexed fields. For example, consider the following compound index:

      { "item": 1, "location": 1, "stock": 1 }

  The index has the following index prefixes:

      { item: 1 }
      { item: 1, location: 1 }

  For a compound index, MongoDB can use the index to support queries on the index prefixes. As such, MongoDB can use the index for queries on the following fields:

      the item field,
      the item field and the location field,
      the item field and the location field and the stock field.

  MongoDB can also use the index to support a query on item and stock fields since item field corresponds to a prefix. However, the index would not be as efficient in supporting the query as would be an index on only item and stock.

  However, MongoDB cannot use the index to support queries that include the following fields since without the item field, none of the listed fields correspond to a prefix index:

      the location field,
      the stock field, or
      the location and stock fields.

  If you have a collection that has both a compound index and an index on its prefix (e.g. { a: 1, b: 1 } and { a: 1 }), if neither index has a sparse or unique constraint, then you can remove the index on the prefix (e.g. { a: 1 }). MongoDB will use the compound index in all of the situations that it would have used the prefix index.

  ### Index Intersection
  New in version 2.6.

      { qty: 1 }
      { item: 1 }

  MongoDB can use the intersection of the two indexes to support the following query:

      db.orders.find( { item: "abc123", qty: { $gt: 15 } } )

  To determine if MongoDB used index intersection, run explain(); the results of explain() will include either an AND_SORTED stage or an AND_HASH stage.

  ### Index Prefix Intersection

  With index intersection, MongoDB can use an intersection of either the entire index or the index prefix. An index prefix is a subset of a compound index, consisting of one or more keys starting from the beginning of the index.

  Consider a collection orders with the following indexes:

      { qty: 1 }
      { status: 1, ord_date: -1 }

  To fulfill the following query which specifies a condition on both the qty field and the status field, MongoDB can use the intersection of the two indexes:

      db.orders.find( { qty: { $gt: 10 } , status: "A" } )

  ### Index Intersection and Compound Indexes

  For example, if a collection orders has the following compound index, with the status field listed before the ord_date field:
      { status: 1, ord_date: -1 }

  The compound index can support the following queries:
      db.orders.find( { status: { $in: ["A", "P" ] } } )
      db.orders.find(
         {
           ord_date: { $gt: new Date("2014-02-01") },
           status: {$in:[ "P", "A" ] }
         }
      )

  But not the following two queries:

      db.orders.find( { ord_date: { $gt: new Date("2014-02-01") } } )
      db.orders.find( { } ).sort( { ord_date: 1 } )

  However, if the collection has two separate indexes:

      { status: 1 }
      { ord_date: -1 }

  The two indexes can, either individually or through index intersection, support all four aforementioned queries.

  The choice between creating compound indexes that support your queries or relying on index intersection depends on the specifics of your system.

  ### Index Intersection and Sort

  Index intersection does not apply when the sort() operation requires an index completely separate from the query predicate.

  For example, the orders collection has the following indexes:

      { qty: 1 }
      { status: 1, ord_date: -1 }
      { status: 1 }
      { ord_date: -1 }

  MongoDB cannot use index intersection for the following query with sort:

      db.orders.find( { qty: { $gt: 10 } } ).sort( { status: 1 } )

  That is, MongoDB does not use the { qty: 1 } index for the query, and the separate { status: 1 } or the { status: 1, ord_date: -1 } index for the sort.

  However, MongoDB can use index intersection for the following query with sort since the index { status: 1, ord_date: -1 } can fulfill part of the query predicate.

      db.orders.find( { qty: { $gt: 10 } , status: "A" } ).sort( { ord_date: -1 } )

  ## 3) Create Indexes to Support Your Queries

  *** An index supports a query when the index contains all the fields scanned by the query. The query scans the index and not the collection. Creating indexes that support queries results in greatly increased query performance. ***

  This document describes strategies for creating indexes that support queries.

  ### Create a Single-Key Index if All Queries Use the Same, Single Key

  If you only ever query on a single key in a given collection, then you need to create just one single-key index for that collection. For example, you might create an index on category in the product collection:

      db.products.createIndex( { "category": 1 } )

  ### Create Compound Indexes to Support Several Different Queries

  If you sometimes query on only one key and at other times query on that key combined with a second key, then creating a compound index is more efficient than creating a single-key index. MongoDB will use the compound index for both queries. For example, you might create an index on both category and item.

      db.products.createIndex( { "category": 1, "item": 1 } )

  This allows you both options. You can query on just category, and you also can query on category combined with item. A single compound index on multiple fields can support all the queries that search a “prefix” subset of those fields.

  EXAMPLE

  The following index on a collection:

      { x: 1, y: 1, z: 1 }

  Can support queries that the following indexes support:

      { x: 1 }
      { x: 1, y: 1 }

  There are some situations where the prefix indexes may offer better query performance: for example if z is a large array.

  The { x: 1, y: 1, z: 1 } index can also support many of the same queries as the following index:

      { x: 1, z: 1 }

  Also, { x: 1, z: 1 } has an additional use. Given the following query:

      db.collection.find( { x: 5 } ).sort( { z: 1} )

  The { x: 1, z: 1 } index supports both the query and the sort operation, while the { x: 1, y: 1, z: 1 } index only supports the query. For more information on sorting, see Use Indexes to Sort Query Results.

  Starting in version 2.6, MongoDB can use index intersection to fulfill queries. The choice between creating compound indexes that support your queries or relying on index intersection depends on the specifics of your system. See Index Intersection and Compound Indexes for more details.

# 11、2015-05-18
* [Indexes > Indexing Reference > Text Search Languages](http://docs.mongodb.org/manual/reference/text-search-languages/#text-search-languages)

  The text index, the $text operator supports the following languages:

  Changed in version 2.6: MongoDB introduces version 2 of the text search feature. With version 2, text search feature supports using the two-letter language codes defined in ISO 639-1. Version 1 of text search only supported the long form of each language name.

      da or danish
      nl or dutch
      en or english
      fi or finnish
      fr or french
      de or german
      hu or hungarian
      it or italian
      nb or norwegian
      pt or portuguese
      ro or romanian
      ru or russian
      es or spanish
      sv or swedish
      tr or turkish

  NOTE  
  If you specify a language value of "none", then the text search uses simple tokenization with no list of stop words and no stemming.

  从文档上看，mongodb的text indexes目前还不支持中文。

# 10、2015-05-15
* [Indexes > Indexing Tutorials > Text Search Tutorials > Create a text Index](http://docs.mongodb.org/manual/tutorial/create-text-index-on-multiple-fields/)

  You can create a text index on the field or fields whose value is a string or an array of string elements. When creating a text index on multiple fields, you can specify the individual fields or you can use wildcard specifier ($**).

* [Indexes > Index Concepts > Index Types > Text Indexes](http://docs.mongodb.org/manual/core/index-text/)

  New in version 2.4.

  MongoDB provides text indexes to support text search of string content in documents of a collection.

  text indexes can include any field whose value is a string or an array of string elements. To perform queries that access the text index, use the $text query operator.

  Changed in version 2.6: MongoDB enables the text search feature by default. In MongoDB 2.4, you need to enable the text search feature manually to create text indexes and perform text search.

  A collection can have at most one text index.


* [Indexes > Index Concepts > Index Types > Geospatial Indexes and Queries](http://docs.mongodb.org/manual/applications/geospatial-indexes/)

  MongoDB offers a number of indexes and query mechanisms to handle geospatial information. This section introduces MongoDB’s geospatial features. For complete examples of geospatial queries in MongoDB, see Geospatial Index Tutorials.

* Multikey Indexes -> Limitations -> Shard Keys

  IMPORTANT  
  The index of a shard key cannot be a multikey index.

* Multikey Indexes -> Limitations -> Hashed Indexes

  Hashed indexes cannot be multikey.

* Multikey Indexes -> Limitations -> Compound Multikey Indexes

  But consider a collection that contains the following documents:

      { _id: 1, a: [1, 2], b: 1, category: "A array" }
      { _id: 2, a: 1, b: [1, 2], category: "B array" }

  A compound multikey index { a: 1, b: 1 } is permissible since for each document, only one field indexed by the compound multikey index is an array; i.e. no document contains array values for both a and b fields. After creating the compound multikey index, if you attempt to insert a document where both a and b fields are arrays, MongoDB will fail the insert.

# 9、2015-05-14
* Multikey Indexes -> Limitations -> Compound Multikey Indexes

  For a compound multikey index, each indexed document can have at most one indexed field whose value is an array. As such, you cannot create a compound multikey index if more than one to-be-indexed field of a document is an array. Or, if a compound multikey index already exists, you cannot insert a document that would violate this restriction.

  For example, consider a collection that contains the following document:

      { _id: 1, a: [ 1, 2 ], b: [ 1, 2 ], category: "AB - both arrays" }

  You cannot create a compound multikey index { a: 1, b: 1 } on the collection since both the a and b fields are arrays.

# 8、2015-05-13
* [Indexes > Index Concepts > Multikey Index Bounds](http://docs.mongodb.org/manual/core/multikey-index-bounds/)

  The bounds of an index scan define the portions of an index to search during a query. When multiple predicates over an index exist, MongoDB will attempt to combine the bounds for these predicates by either intersection or compounding in order to produce a scan with smaller bounds.

  > keywords: mongodb multikey index

* [Indexes > Index Concepts > Index Types > Multikey Indexes](http://docs.mongodb.org/manual/core/index-multikey/)

  To index a field that holds an array value, MongoDB creates an index key for each element in the array. These multikey indexes support efficient queries against array fields. Multikey indexes can be constructed over arrays that hold both scalar values (e.g. strings, numbers) and nested documents.
  ![](http://docs.mongodb.org/manual/_images/index-multikey.png)

  > keywords: mongodb multikey index

* [Reference > mongo Shell Methods > Collection Methods > db.collection.find()](http://docs.mongodb.org/manual/reference/method/db.collection.find)

  > mongodb find

* [MongoDB 索引的使用, 管理 和优化](http://blog.csdn.net/black_ox/article/details/22078501)

  > keywords: mongodb index

* [Indexes > Indexing Tutorials > Indexing Strategies > Create Indexes to Support Your Queries](http://docs.mongodb.org/manual/tutorial/create-indexes-to-support-queries/#compound-key-indexes)

  An index supports a query when the index contains all the fields scanned by the query. The query scans the index and not the collection. Creating indexes that support queries results in greatly increased query performance.

  > mongodb index

* [Indexes > Index Concepts > Index Intersection](http://docs.mongodb.org/manual/core/index-intersection/)

  New in version 2.6.

  MongoDB can use the intersection of multiple indexes to fulfill queries. [1] In general, each index intersection involves two indexes; however, MongoDB can employ multiple/nested index intersections to resolve a query.

  > keywords: mongodb index intersection

* [MongoDB Limits and Thresholds](http://docs.mongodb.org/manual/reference/limits/)

  This document provides a collection of hard and soft limitations of the MongoDB system.

  > keywords: mongodb limit threshold

* [Hashed Index](http://docs.mongodb.org/manual/core/index-hashed/)

  New in version 2.4.

  Hashed indexes maintain entries with hashes of the values of the indexed field. The hashing function collapses embedded documents and computes the hash for the entire value but does not support multi-key (i.e. arrays) indexes.

  > keywords: mongodb hash index

* [Number of Indexed Fields in a Compound Index](http://docs.mongodb.org/manual/reference/limits/#Number-of-Indexed-Fields-in-a-Compound-Index)

  There can be no more than 31 fields in a compound index.

  > keywords: mongodb index field limit

* [Indexes > Index Concepts > Index Types > Compound Indexes](http://docs.mongodb.org/manual/core/index-compound/)

  MongoDB supports compound indexes, where a single index structure holds references to multiple fields [1] within a collection’s documents. The following diagram illustrates an example of a compound index on two fields:
  ![](http://docs.mongodb.org/manual/_images/index-compound-key.png)

  If you have a collection that has both a compound index and an index on its prefix (e.g. { a: 1, b: 1 } and { a: 1 }), if neither index has a sparse or unique constraint, then you can remove the index on the prefix (e.g. { a: 1 }). MongoDB will use the compound index in all of the situations that it would have used the prefix index.

  > keywords: mongodb compund index

* [Use Indexes to Sort Query Results](http://docs.mongodb.org/manual/tutorial/sort-results-with-indexes/)

  In MongoDB, sort operations can obtain the sort order by retrieving documents based on the ordering in an index. If the query planner cannot obtain the sort order from an index, it will sort the results in memory. Sort operations that use an index often have better performance than those that do not use an index. In addition, sort operations that do not use an index will abort when they use 32 megabytes of memory.

  > keywords: mongodb index sort

* [Data Models > Data Model Reference > ObjectId](http://docs.mongodb.org/manual/reference/object-id/)

  ObjectId is a 12-byte BSON type, constructed using:

  a 4-byte value representing the seconds since the Unix epoch,
  a 3-byte machine identifier,
  a 2-byte process id, and
  a 3-byte counter, starting with a random value.

  > keywords: mongodb objectid

* [BSON Types](http://docs.mongodb.org/manual/reference/bson-types/)

  BSON is a binary serialization format used to store documents and make remote procedure calls in MongoDB. The BSON specification is located at bsonspec.org.

  > keywords: mongodb bson

* [BSON](http://docs.mongodb.org/manual/reference/glossary/#term-bson)

  A serialization format used to store documents and make remote procedure calls in MongoDB. “BSON” is a portmanteau of the words “binary” and “JSON”. Think of BSON as a binary representation of JSON (JavaScript Object Notation) documents. See BSON Types and MongoDB Extended JSON.

  > keywords: mongodb bson

* [ObjectId](http://docs.mongodb.org/manual/reference/glossary/#term-objectid)

  A special 12-byte BSON type that guarantees uniqueness within the collection. The ObjectId is generated based on timestamp, machine ID, process ID, and a process-local incremental counter. MongoDB uses ObjectId values as the default values for _id fields.

  > keywords: mongodb objectid

* [Single Field Indexes](http://docs.mongodb.org/manual/core/index-single/)

  MongoDB provides complete support for indexes on any field in a collection of documents. By default, all collections have an index on the _id field, and applications and users may add additional indexes to support important queries and operations.

  > keywords: mongodb index

* [Index Introduction](http://docs.mongodb.org/manual/core/indexes-introduction/)

  Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

  Indexes are special data structures [1] that store a small portion of the collection’s data set in an easy to traverse form. The index stores the value of a specific field or set of fields, ordered by the value of the field. The ordering of the index entries supports efficient equality matches and range-based query operations. In addition, MongoDB can return sorted results by using the ordering in the index.

  > keywords: mongodb index
