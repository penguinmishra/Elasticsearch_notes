# Exercises
1. Please write a query matching products that didn’t sell very well, being products where the “sold” field has a value of less than 10 (sold < 10).
```
GET /product/_doc/_search
{
  "query": {
    "range": {
      "sold": {
        "lt": 10
      }
    }
  }
}
```
2. Please write a query that matches products that sold okay, meaning less than 30 and greater than or equal to 10 (sold < 30 && sold >= 10).
```
GET /product/_doc/_search
{
  "query": {
    "range": {
      "sold": {
        "lt": 30,
        "gte": 10
      }
    }
  }
}
```
3. Please write a query that matches documents containing the term “Meat” within the “tags” field.
```
GET /product/_doc/_search
{
  "query": {
    "term": {
      "tags.keyword": "Meat"
    }
  }
}
```
4. Please write a query matching documents containing one of the terms "Tomato" and "Paste" within the "name" field.
```
GET /product/_doc/_search
{
  "query": {
    "terms": {
      "name": [ "Tomato", "Paste" ]
    }
  }
}
```
5. Please write a query that matches products with a "name" field including “pasta”, “paste”, or similar. The query should be dynamic and not use the "terms" query clause.
```
GET /product/_doc/_search
{
  "query": {
    "wildcard": {
      "name": "past?"
    }
  }
}
```
6. Please write a query that matches products that contain a number within their "name" field.
```
GET /product/_doc/_search
{
  "query": {
    "regexp": {
      "name": "[0-9]+"
    }
  }
}
```