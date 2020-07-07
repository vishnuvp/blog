---
title: "Snowflake JSON Capabilities"
date: 2020-07-06T22:23:25+05:30
slug: ""
description: ""
keywords: [snowflake, JSON, ]
draft: false
tags: [snowflake, JSON, article]
math: false
toc: false
---
Snowflake is a relational DBMS. However, it supports querying and manipulating JSON data by providing a special data type called `Variant`. 
So, if we store the JSON documents in a single column table, Snowflake will effectively be querying JSON object.

For example, consider this table defenition:

```
CREATE TABLE JSON_DATA (DATA VARIANT);
```

If the JSON documents are in an S3 bucket, we can import the data using a copy into command. 

```
copy into JSON_DATA
  from external_s3_bucket
  file_format = (type = JSON_DATA);
```

The `external_s3_bucket` in this command is an external stage which we need to configure using the `CREATE EXTERNAL STAGE` command where we will specify the bucket access credentials.

This command will load the JSON documents into the JSON_DATA table in the variant column, DATA.

To better explain the data handling, consider this example JSON document which is imported into the aforesaid table.

```
{
  "version" : "1.5.1",
  "seed" : "2aed4300a33d6c86fd85f2c75fedfdba",
  "options" : {
    "http" : {
      "user_agent" : "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36",
      "request_timeout" : 10000,
      "request_redirect_limit" : 5,
      "request_concurrency" : 20,
      "request_queue_size" : 100,
      "request_headers" : {},
      "response_max_size" : 500000,
      "cookies" : {},
      "authentication_type" : "auto"
    }
   }
}
```

To query this JSON data, we can use the select query.
For example:
```
SELECT * FROM JSON_DATA;

RESULT
------
--------------------------------------------------------------------------------------------------------------------------------------------
|DATA 																																		|
--------------------------------------------------------------------------------------------------------------------------------------------
| {																																			|
|   "version" : "1.5.1",																													|
|   "seed" : "2aed4300a33d6c86fd85f2c75fedfdba",																							|
|   "options" : {																															|
|     "http" : {																															|
|       "user_agent" : "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36",	|
|       "request_timeout" : 10000,																											|
|       "request_redirect_limit" : 5,																										|
|       "request_concurrency" : 20,																											|
|       "request_queue_size" : 100,																											|
|       "request_headers" : {},																												|
|       "response_max_size" : 500000,																										|
|       "cookies" : {},																														|
|       "authentication_type" : "auto"																										|
|     }																																		|
|    }																																		|
|  }																																		|
--------------------------------------------------------------------------------------------------------------------------------------------
```

```
SELECT DATA:"version", DATA:"seed", DATA:"options":"http":"authentication_type" FROM JSON_DATA;

RESULT
------

----------------------------------------------------------------------------------------------------
|DATA:"version" | DATA:"seed"                         | DATA:"options":"http":"authentication_type" |
----------------------------------------------------------------------------------------------------
|  "1.5.1"		| "2aed4300a33d6c86fd85f2c75fedfdba"  | "auto"										|
----------------------------------------------------------------------------------------------------

```

----------------------------

There are a few useful built-in functions provided by Snowflake. A few of them are:
* flatten( input => json_tree ) - We can flatten a nested JSON sub tree, so that querying becomes easier
* PARSE_JSON - To convert a JSON string to JSON object
* GET_PATH - path to a particular key
* OBJECT_KEYS - extract keys from the document



RESOURCES
---------

An article in Snowflake documentation:
https://community.snowflake.com/s/article/json-data-parsing-in-snowflake

