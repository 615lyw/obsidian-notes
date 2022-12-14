---
date created: 2022-12-07, 21:52:01
date modified: 2022-12-18, 10:32:53
---

# Meta

- alias: 
- parent :: 
- siblings :: 
- child :: 
- refs: 

---

**index**

An optimized collection of JSON documents. Each document is a collection of fields, the key-value pairs that contain your data.

**document**

A document is a JSON document which is stored in Elasticsearch. It is like a row in a table in a relational database. Each document is stored in an index and has a type and an id.

A document is a JSON object (also known in other languages as a hash / hashmap / associative array) which contains zero or more fields, or key-value pairs.

The original JSON document that is indexed will be stored in the _source field, which is returned by default when getting or searching for a document.

**field**

A document contains a list of fields, or key-value pairs. The value can be a simple (scalar) value (eg a string, integer, date), or a nested structure like an array or an object. A field is similar to a column in a table in a relational database.

The mapping for each field has a field type (not to be confused with document type) which indicates the type of data that can be stored in that field, eg integer, string, object. The mapping also allows you to define (amongst other things) how the value for a field should be analyzed.

Terms Aggregation

By default, the terms aggregation will return the buckets for the top ten terms ordered by the doc_count. One can change this default behaviour by setting the size parameter.

# Boolean Query

[ElasticSearch: demystifying the bool query | by Anam Hossain | CAMS Engineering](https://engineering.carsguide.com.au/elasticsearch-demystifying-the-bool-query-11da737a4efb)