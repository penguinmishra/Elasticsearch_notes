### Adding mappings to existing index

This can be done the following way (new field):
```sh
PUT /products/_mappings
{
  "properties":{
    "discount":{
      "type":"double"
    }
  }
}
```
This gives:
```sh
{
  "acknowledged" : true
}
```
Now if we do
```sh
GET /products/_mappings
```
We get:
![add_mappings][addmap]

[addmap]: <https://github.com/penguinmishra/images_repo/blob/master/Elasticsearch/dynamic_mapping_ES.jpg>