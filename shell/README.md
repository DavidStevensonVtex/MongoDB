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

Change the current setting of <property> to <value>:

config.set( "<property>", <value> )

```
> config.set( "editor", "nano" );
Setting "editor" has been changed
> config.get("editor");
nano
```

Reset a <property> to the default value:

config.reset( "<property>" )

```
> config.reset( "editor" )
Setting "editor" has been reset to its default value
> config.get("editor")
null
```

[Supported property parameters](https://www.mongodb.com/docs/mongodb-shell/reference/configure-shell-settings-api/)


### Behavior with Configuration File

Settings specified with the config API:

* Override settings specified in the [configuration file](https://www.mongodb.com/docs/mongodb-shell/reference/configure-shell-settings-global/#std-label-configure-settings-global).

* Persist across restarts.

Example Configuration File

```
mongosh:
  displayBatchSize: 50
  inspectDepth: 20
  redactHistory: "remove-redact"
```

Configuration File Location

* Windows - mongosh.cfg, in the same directory as the mongosh.exe binary.
* Linux - /etc/mongosh.conf

### Customize the mongosh Prompt

By default the mongosh prompt includes the current database name. You can modify the prompt variable to display custom strings or to return dynamic information about your mongosh session.

Custom prompts are not stored when you exit mongosh. To have a custom prompt persist through restarts, add the code for your custom prompt to [.mongoshrc.js](https://www.mongodb.com/docs/mongodb-shell/mongoshrc/#std-label-mongoshrc-js).

### Display Line Numbers

To display line numbers in the mongosh prompt, run the following code inside mongosh:

```
let cmdCount = 1;
prompt = function() {
    return (cmdCount++) + "> ";
}
```
