# [Welcome to MongoDB Shell (mongosh)](https://www.mongodb.com/docs/mongodb-shell/)

## Connecting to Atlas

mongosh "MongoDb-Connection-String" --apiVersion 1 --username username

mongosh "mongodb+srv://YOUR_CLUSTER_NAME.YOUR_HASH.mongodb.net/" --apiVersion YOUR_API_VERSION --username YOUR_USERNAME

## Connecting to Local MongoDB 

mongosh
mongosh "mongodb://127.0.0.1:27017/"

## Configuration

### Syntax

Print the current mongosh configuration:

config

```
Map(16) {
  'displayBatchSize' => 20,
  'maxTimeMS' => null,
  'enableTelemetry' => true,
  'editor' => 'vi',
  'snippetIndexSourceURLs' => 'https://compass.mongodb.com/mongosh/snippets-index.bson.br',
  'snippetRegistryURL' => 'https://registry.npmjs.org',
  'snippetAutoload' => true,
  'inspectCompact' => 3,
  'inspectDepth' => 6,
  'historyLength' => 1000,
  'showStackTraces' => false,
  'redactHistory' => 'remove',
  'oidcRedirectURI' => undefined,
  'oidcTrustedEndpoints' => undefined,
  'browser' => undefined,
  'updateURL' => 'https://downloads.mongodb.com/compass/mongosh.json'

```

Return the current value for <property>:

config.get( "<property>" )

```
> config.get("editor");
vi
```