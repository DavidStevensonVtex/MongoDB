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

The prompt will look like this:

```
1> show collections
2> use test
3>
```

### Display Database and Hostname

The current database name is part of the default mongosh prompt. To reformat the prompt to show the database and hostname, use a function like this one:

```
{
   const hostnameSymbol = Symbol('hostname');
   prompt = () => {
      if (!db[hostnameSymbol])
         db[hostnameSymbol] = db.serverStatus().host;
      return `${db.getName()}@${db[hostnameSymbol]}> `;
   };
}
```

### Display System Up Time and Document Count

To create a prompt that shows the system uptime and a count of documents across all collections in the current database, use a function like this one:

```
prompt = function() {
    return "Uptime:" + db.serverStatus().uptime +
            " Documents:" + db.stats().objects +
            " > ";
}
```

## Configure Telemetry Options

mongosh collects anonymous aggregated usage data to improve MongoDB products. mongosh collects this information by default, but you can disable this data collection at any time.

Data Tracked by mongosh
mongosh tracks the following data:

* The type of MongoDB mongosh is connected to. For example, Enterprise Edition, Community Edition, or Atlas Data Lake.

* The methods run in mongosh. mongosh only tracks the names of the methods, and does not track any arguments supplied to methods.

* Whether users run scripts with mongosh.

* Errors.

mongosh does not track:

* IP addresses, hostnames, usernames, or credentials.

* Queries run in mongosh.

* Any data stored in your MongoDB deployment.

* Personal identifiable information.

### Toggle Telemetry Collection
Use the following methods in mongosh to toggle telemetry data collection.

**disableTelemetry()**

Disable telemetry for mongosh.

`disableTelemetry()`

The command response confirms that telemetry is disabled:

`Telemetry is now disabled.`

Tip: You can also disable telemetry at startup by using the --eval startup option.

The following command starts mongosh with telemetry disabled:

`mongosh --nodb --eval "disableTelemetry()"`

**enableTelemetry()**

Enable telemetry for mongosh.

`enableTelemetry()`

The command response confirms that telemetry is enabled:

`Telemetry is now enabled.`

## Run Commands

To run commands in mongosh, you must first connect to a MongoDB deployment.

### Format Input and Output

mongosh uses the [Node.js BSON parser](https://www.npmjs.com/package/bson) parser to parse BSON data. You can use the parser's [EJSON](https://www.npmjs.com/package/bson#EJSON) interface to transform your data when you work with mongosh.

For examples that use EJSON, see: [EJSON](https://www.npmjs.com/package/bson#EJSON).

### Switch Databases
To display the database you are using, type db:

`db`

The operation should return test, which is the default database.

To switch databases, issue the use `<db>` helper, as in the following example:

`use <database>`

To access a different database from the current database without switching your current database context, see the [db.getSiblingDB()](https://www.mongodb.com/docs/manual/reference/method/db.getSiblingDB/#mongodb-method-db.getSiblingDB) method.

To list the databases available to the user, use the helper `show dbs`.

### Create a New Database and Collection

To create a new database, issue the use `<db>` command with the database that you would like to create. For example, the following commands create both the database myNewDatabase and the collection myCollection using the `insertOne()` operation:

```
use myNewDatabase
db.myCollection.insertOne( { x: 1 } );
```

If a collection does not exist, MongoDB creates the collection when you first store data for that collection.

### Terminate a Running Command

To terminate a running command or query in mongosh, press `Ctrl + C`.

When you enter `Ctrl + C`, mongosh:

* interrupts the active command,
* tries to terminate the ongoing, server-side operation, and
* returns a command prompt.

If mongosh cannot cleanly terminate the running process, it issues a warning.

Note: Pressing `Ctrl + C` in mongosh does not terminate asynchronous code. Asynchronous operations such as setTimeout or operations like fs.readFile continue to run.

There is no way to terminate asynchronous code in mongosh. This is the same behavior as in the Node.js REPL.

Pressing `Ctrl + C` once will not exit mongosh, press `Ctrl + C` twice to exit mongosh.

You can also terminate a script from within the script code by calling the exit(`<code>`) command. For more information, refer to [Terminate a Script on Error](https://www.mongodb.com/docs/mongodb-shell/write-scripts/#std-label-mongosh-terminate-script).

### Command Exceptions

For commands whose output includes `{ ok: 0 }`, mongosh returns a consistent exception and omits the server raw output. The legacy mongo shell returns output that varies for each command.

### Clear the mongosh Console

The `cls` command clears the console. You can also clear the console with `Ctrl + L` and `console.clear()`.

## Perform CRUD Operations

CRUD operations create, read, update, and delete documents.

### Create Operations

Create or insert operations add new documents to a collection. If the collection does not exist, create operations also create the collection.

You can insert a single document or multiple documents in a single operation.

For examples, see [Insert Documents](https://www.mongodb.com/docs/mongodb-shell/crud/insert/#std-label-mongosh-insert).

### Read Operations

Read operations retrieve documents from a collection; i.e. query a collection for documents.

You can specify criteria, or filters, that identify the documents to return.

For examples, see [Query Documents](https://www.mongodb.com/docs/mongodb-shell/crud/read/#std-label-mongosh-read).

### Update Operations

Update operations modify existing documents in a collection. You can update a single document or multiple documents in a single operation.

You can specify criteria, or filters, that identify the documents to update. These filters use the same syntax as read operations.

For examples, see [Update Documents](https://www.mongodb.com/docs/mongodb-shell/crud/update/#std-label-mongosh-update).

### Delete Operations

Delete operations remove existing documents from a collection. You can remove a single document or multiple documents in a single operation.

You can specify criteria, or filters, that identify the documents to remove. These filters use the same syntax as read operations.

For examples, see [Delete Documents](https://www.mongodb.com/docs/mongodb-shell/crud/delete/#std-label-mongosh-delete).

### Insert Documents

The MongoDB shell provides the following methods to insert documents into a collection:

* To insert a single document, use db.collection.insertOne().
* To insert multiple documents, use db.collection.insertMany().

The examples on this page reference the [Atlas sample dataset](https://www.mongodb.com/docs/atlas/sample-data/). You can create a free Atlas cluster and populate that cluster with sample data to follow along with these examples. To learn more, see [Get Started with Atlas](https://www.mongodb.com/docs/atlas/).

#### Insert a Single Document

[db.collection.insertOne()](https://www.mongodb.com/docs/manual/reference/method/db.collection.insertOne/#mongodb-method-db.collection.insertOne) inserts a single document into a collection. If the document does not specify an _id field, MongoDB adds the _id field with an ObjectId value to the new document. See [Insert Behavior](https://www.mongodb.com/docs/manual/tutorial/insert-documents/#std-label-write-op-insert-behavior).

```
use sample_mflix

db.movies.insertOne(
  {
    title: "The Favourite",
    genres: [ "Drama", "History" ],
    runtime: 121,
    rated: "R",
    year: 2018,
    directors: [ "Yorgos Lanthimos" ],
    cast: [ "Olivia Colman", "Emma Stone", "Rachel Weisz" ],
    type: "movie"
  }
)

{
  acknowledged: true,
  insertedId: ObjectId('676e150f9a13ef38a5567a2c')
}
```

```
db.movies.find({ title: "The Favourite" })
[
  {
    _id: ObjectId('676e150f9a13ef38a5567a2c'),
    title: 'The Favourite',
    genres: [ 'Drama', 'History' ],
    runtime: 121,
    rated: 'R',
    year: 2018,
    directors: [ 'Yorgos Lanthimos' ],
    cast: [ 'Olivia Colman', 'Emma Stone', 'Rachel Weisz' ],
    type: 'movie'
  }
]
```

#### Insert Multiple Documents

[db.collection.insertMany()](https://www.mongodb.com/docs/manual/reference/method/db.collection.insertMany/#mongodb-method-db.collection.insertMany) can insert multiple documents into a collection. Pass an array of documents to the method. If the documents do not specify an _id field, MongoDB adds the _id field with an ObjectId value to each document. See Insert Behavior.

Example

To insert two new documents into the sample_mflix.movies collection:

```
use sample_mflix

db.movies.insertMany([
   {
      title: "Jurassic World: Fallen Kingdom",
      genres: [ "Action", "Sci-Fi" ],
      runtime: 130,
      rated: "PG-13",
      year: 2018,
      directors: [ "J. A. Bayona" ],
      cast: [ "Chris Pratt", "Bryce Dallas Howard", "Rafe Spall" ],
      type: "movie"
    },
    {
      title: "Tag",
      genres: [ "Comedy", "Action" ],
      runtime: 105,
      rated: "R",
      year: 2018,
      directors: [ "Jeff Tomsic" ],
      cast: [ "Annabelle Wallis", "Jeremy Renner", "Jon Hamm" ],
      type: "movie"
    }
])

{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('676e17469a13ef38a5567a2d'),
    '1': ObjectId('676e17469a13ef38a5567a2e')
  }
}
```

[insertMany()](https://www.mongodb.com/docs/manual/reference/method/db.collection.insertMany/#mongodb-method-db.collection.insertMany) returns a document that includes the newly inserted documents' _id field values.

To read documents in the collection:

`db.movies.find( {} )`

#### Insert Behavior

To learn more about the specific behavior of inserting documents, see [Insert Behavior](https://www.mongodb.com/docs/manual/tutorial/insert-documents/#std-label-write-op-insert-behavior).

Learn More

To see more examples of inserting documents into a collection, see the [insertOne()](https://www.mongodb.com/docs/manual/reference/method/db.collection.insertOne/#mongodb-method-db.collection.insertOne) and [db.collection.insertMany()](https://www.mongodb.com/docs/manual/reference/method/db.collection.insertMany/#mongodb-method-db.collection.insertMany) method pages.

To see all available methods to insert documents into a collection, see [Additional Methods for Inserts](https://www.mongodb.com/docs/manual/reference/insert-methods/#additional-inserts)

### [Query Documents](https://www.mongodb.com/docs/mongodb-shell/crud/read/)