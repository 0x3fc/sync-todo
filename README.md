# Sync Todo

## Development Set Up

### Database

```bash
docker run --name couchdb -d \
    -p 5984:5984 \
    -v $(pwd)/data:/opt/couchdb/data \
    -v $(pwd)/config/couchdb.ini:/usr/local/etc/couchdb/local.ini \
    couchdb:1.7
```

### Web

```bash
yarn
yarn dev
```
