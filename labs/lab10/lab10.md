# Lab 10

### Checkpoint 0

https://github.com/lhain08/FitbitDashboard/wiki/Project-Blog

### Checkpoint 1

<img src="https://i.imgur.com/aKGRCxc.png" width=700>

### Checkpoint 2
<img src="https://i.imgur.com/XEBAnDW.png" width=700>
<img src="https://i.imgur.com/LoA8liS.png" width=700>
<img src="https://i.imgur.com/eRj4BwP.png" width=700>
<img src="https://i.imgur.com/eeIGYFx.png" width=700>

### Checkpoint 3
<img src="https://i.imgur.com/4rAKflr.png" width=700>
<img src="https://i.imgur.com/GNVf6sX.png" width=700>
<img src="https://i.imgur.com/O5OEgo9.png" width=700>
<img src="https://i.imgur.com/sYQG5KO.png" width=700>

### Checkpoint 4

#### 1.
```
curl -X POST localhost:5984/hello-world/_find -d '{
   "selector": {
      "year": {
         "$gt": 1987
    }
  }
}' -H 'Content-Type: application/json'
```

Response:

```
{
"docs":[
    {
        "_id":"00a271787f89c0ef2e10e88a0c0001f4",
        "_rev":"1-c7f3ec5e61c7a461d83da2d42f64a9e4",
        "type":"movie",
        "title":"My Neighbour Totoro",
        "year":1988,
        "director":"miyazaki",
        "rating":8.2
    },
    {
        "_id":"00a271787f89c0ef2e10e88a0c0003f0",
        "_rev":"1-5facb9c84b721a5fe8d667c1140a54d3",
        "type":"movie",
        "title":"Kikis Delivery Service",
        "year":1989,
        "director":"miyazaki",
        "rating":7.8
    },
    {
        "_id":"00a271787f89c0ef2e10e88a0c00048b",
        "_rev":"1-90be3f3651b72d82da71a875e63de9c0",
        "type":"movie",
        "title":"Princess Mononoke",
        "year":1997,
        "director":"miyazaki",
        "rating":8.4
        }
],
"bookmark": "g2wAAAACaAJkAA5zdGFydGtleV9kb2NpZG0AAAAgMDBhMjcxNzg3Zjg5YzBlZjJlMTBlODhhMGMwMDA0OGJoAmQACHN0YXJ0a2V5bAAAAAFiAAAHzWpq"
}
```

#### 2.
```
curl -X POST http://admin:password@127.0.0.1:5984/hello-world/_find -d '{
    "selector": {
        "title": {
            "$gt": "L"
        }
    }
}' -H 'Content-Type: application/json'
```

Response:
```
{
"docs":[
    {
        "_id":"00a271787f89c0ef2e10e88a0c0001f4",
        "_rev":"1-c7f3ec5e61c7a461d83da2d42f64a9e4",
        "type":"movie",
        "title":"My Neighbour Totoro",
        "year":1988,
        "director":"miyazaki",
        "rating":8.2
    },
    {
        "_id":"00a271787f89c0ef2e10e88a0c00048b",
        "_rev":"1-90be3f3651b72d82da71a875e63de9c0",
        "type":"movie",
        "title":"Princess Mononoke",
        "year":1997,
        "director":"miyazaki",
        "rating":8.4
    }
],
"bookmark": "g1AAAABweJzLYWBgYMpgSmHgKy5JLCrJTq2MT8lPzkzJBYorGBgkGpkbmluYp1lYJhukphmlGhqkWlgkGiQbGBiYWCSB9HHA9BGlIwsAfPcdnw",
"warning": "No matching index found, create an index to optimize query time."
}
```

#### 3.

```
curl -X POST http://admin:password@localhost:5984/hello-world/_index -d '{
    "index": {
        "fields": [
            "title"
        ]
    },
"name": "title-json-index",
"type": "json"
}' -H 'Content-Type: application/json'
```

Response:

```
{
    "result":"created",
    "id":"_design/5e0c1e3422759aefaa2aa14ed5a8323ba33f06e3",
    "name":"title-json-index"
}
```

#### 4.

```
{
"docs":[
    {
        "_id":"00a271787f89c0ef2e10e88a0c0001f4",
        "_rev":"1-c7f3ec5e61c7a461d83da2d42f64a9e4",
        "type":"movie",
        "title":"My Neighbour Totoro",
        "year":1988,
        "director":"miyazaki",
        "rating":8.2
    },
    {
        "_id":"00a271787f89c0ef2e10e88a0c00048b",
        "_rev":"1-90be3f3651b72d82da71a875e63de9c0",
        "type":"movie",
        "title":"Princess Mononoke",
        "year":1997,
        "director":"miyazaki",
        "rating":8.4
    }
],
"bookmark": "g1AAAABneJzLYWBgYMpgSmHgKy5JLCrJTq2MT8lPzkzJBYorGBgkGpkbmluYp1lYJhukphmlGhqkWlgkGiQbGBiYWCSB9HHA9OUAdTCCtAkGFGXmJacWFyv45ucBYXZqVhYAgz0cvw"
}
```
