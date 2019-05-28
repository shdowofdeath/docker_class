## Tips and tricks 

building service api in 1 minute :) 

```

echo '{"devops": [{"name": "lola"}, {"name": "pola"}, {"name": "gola"}, {"name": "jenkins"}]}' > /tmp/devops.json
docker run -d -p 7443:80 -v /tmp/devops.json:/data/db.json elhay/api-server

# Listing my favourite cities
curl http://localhost:7443/devops [
  {"name": "lola"},
  {"name": "pola"},
  {"name": "gola"},
  {"name": "jenkins"}
]

# Listing my favourite cities containing *lo*
curl http://localhost:7443/devops?name_like=lo
[
  {"name": "lola"}
]
```
