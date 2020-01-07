# Specifying the Result Size

- default result size is 10
- control how many hits are returned.
- can be done by specifying `size` in request URI or in the query.

Specifying in the request URI:
```
GET /recipe/_search?size=5
{
  "_source": false, 
  "query": {
    "match": {
      "title": "pasta"
    }
  }
}
```
Specifying in the query:
```
GET /recipe/_search
{
  "_source": false, 
  "size": 5, 
  "query": {
    "match": {
      "title": "pasta"
    }
  }
}
```


# Specifying an offset

- to retrieve next n number of results.
- we can use offset indicating the number of matches to skip before returning the matches.
- `size` and offset enable pagination which is kind of a sliding window.
- This is done by specifying a `from` parameter which defaults to zero.
- can be specified in either query or request parameter.
```
GET /recipe/_search
{
  "_source": false, 
  "size": 5, 
  "from": 0, 
  "query": {
    "match": {
      "title": "pasta"
    }
  }
}
```
This query is as good as not specifying any `size` parameter. Results:
```
{
    "hits" : [
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "8",
        "_score" : 1.0835491
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "18",
        "_score" : 0.8755694
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "13",
        "_score" : 0.8229182
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "19",
        "_score" : 0.8229182
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.77624005
      }
    ]
  }

```
If we specify a value to `from`, then we get a sliding window for the next set of results:
```
GET /recipe/_search
{
  "_source": false, 
  "size": 5, 
  "from": 2, 
  "query": {
    "match": {
      "title": "pasta"
    }
  }
}
```
Which then gives:
```
{
    "hits" : [
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "13",
        "_score" : 0.8229182
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "19",
        "_score" : 0.8229182
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.77624005
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "17",
        "_score" : 0.77624005
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.7345731
      }
    ]
  }
```

# Pagination
- can do with `size` and `from`.
- calculating total number of pages to be rendered:
```
total pages = ceil(total_hits/page_size)
```
- to go from one page to another:
```
from = (page_size * (page_number - 1))
```
- So, if we have page size of 10 and we want to go to page number 6, then 10 \*(6-1) = 50 would be the value of `from`.
- This approach is limited to 10,000  results. Reason is when performing deep pagination, the requests take up more and more heap memory and request takes longer.
- If you need to do a deep pagination, you need to use `search_after` parameter which is a bit more complicated.
- an important thing to note is a case in ES. If a user sees first page and while he navigates to the second page, a new document was added which matches well with his search results and places it on first page. Then in that case, the user will see the same first page which now got pushed to the second page.
- results are always based on the latest data because the queries are stateless. The results are not based on the data that was available when running the first query.

# Sorting Results

- default sorting order is ascending.
- use `sort` key.
```
GET /recipe/_search
{
  "_source": false, 
  "query": {
    "match_all": {}
  },
  "sort": [
    "preparation_time_minutes"
  ]
}
```
This gives:
```
[TRUNCATED]
"hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "5",
        "_score" : null,
        "sort" : [
          8
        ]
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "6",
        "_score" : null,
        "sort" : [
          10
        ]
      },
[TRUNCATED]
```
- notice that we have a new key in result namely `sort`. This contains the value of the field which was used for sorting. Even when we specified `_source` as `false`, we got the value in `sort`.
- for descending order:
```
GET /recipe/_search
{
  "_source": "created", 
  "query": {
    "match_all": {}
  },
  "sort": [
    {"created": "desc"}
  ]
}
```
which gives:
```
[TRUNCATED]
"hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "12",
        "_score" : null,
        "_source" : {
          "created" : "2017/05/20"
        },
        "sort" : [
          1495238400000
        ]
      },
      {
        "_index" : "recipe",
        "_type" : "_doc",
        "_id" : "10",
        "_score" : null,
        "_source" : {
          "created" : "2017/04/27"
        },
        "sort" : [
          1493251200000
        ]
      },
[TRUNCATED]
```
- reason to include `created` in `_source` field is that in case of a date field, the `sort` would contain the miliseconds since epoch and we wanted the actual value. Which we got by including it in `_source` field.
- Sorting by multiple fields:
```
GET /recipe/_search
{
  "_source": ["created", "preparation_time_minutes"], 
  "query": {
    "match_all": {}
  },
  "sort": [
    {"preparation_time_minutes": "asc"},
    {"created": "desc"}
  ]
}
```
- sorting is applied in order they are defined in the query. So, results are sorted based on `preparation_time_minutes` and in case of same value for this field, the results are sorted by `created`.

# Sorting by Multi-value fields

- You can specify a mode for sorting when using a field which has multiple values.
- For example, we have a ratings field with multiple values and we can sort that using `mode`:
```
GET /recipe/_search
{
  "_source": "ratings", 
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "ratings": {
        "order": "desc",
        "mode": "avg"
      }
    }
  ]
}
```
- What ES did was it sorted the results based on the average value of the field that was specified in `sort` object.
- we have `sum`, `median` (limited to numeric values) and `min`, `max` for sorting.