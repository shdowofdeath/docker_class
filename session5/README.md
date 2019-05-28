## Tips and tricks 

building service api in 1 minute :) 

```

echo '{"cities": [{"name": "Barcelona"}, {"name": "Copenhagen"}, {"name": "Edinburgh"}, {"name": "Hanoi"}]}' > /tmp/cities.json
docker run -d -p 80:80 -v /tmp/cities.json:/data/db.json clue/json-server

# Listing my favourite cities
curl http://localhost/cities
[
  {"name": "Barcelona"},
  {"name": "Copenhagen"},
  {"name": "Edinburgh"},
  {"name": "Hanoi"}
]

# Listing my favourite cities containing *bar*
curl 'http://localhost/cities?name_like=bar'
[
  {"name": "Barcelona"}
]
```
