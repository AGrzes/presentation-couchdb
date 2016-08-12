# Couchdb
CouchDB is NoSQL, open-source document database available under Apache 2.0 license. Implemented in Erlang, first released in 2005, in 2008 became an Apache project. Available on Windows, Linux and Mac.
## Data Organization
CouchDB **Server** contains multiple **Databases** each in turn containing multiple **Documents**.
**Document** have an \_id that is unique in the **database** scope and \_rev that is used for optimistic concurrency control. **Document** can contain any data in JSON format and optionally additional binary attachments.
## CRUD
CouchDB server provides REST interface for all operations and web administration application.
### Create
To create document we simply  **POST** content to the ${DatabaseURI} or - if we want to define our own \_id **PUT** it to ${DatabaseURI}/${\_id}
### Read
To read the document we simply **GET** ${DatabaseURI}/${\_id}.
To retrieve all documents we get ${DatabaseURI}/\_all\_docs view. If we want retrieve document contents we also have to specify *include_docs* option.
### Update
To update document we use **PUT** to the ${DatabaseURI}/${\_id}. We have to include \_rev and if it is not latest \_rev then server will reject our change providing optimistic locking.
### Delete
To delete document we use **DELETE** HTTP method for ${DatabaseURI}/${\_id} and again we have to provide latest \_rev to proceed.
## Views
Views are the way to index, filter and aggregate documents. Views are defined in special **design documents** with \_id in form "\_design/${design name}". The views are defined using JavaScript - but you may connect other language executor to CouchDB server.
### Map
Map function of view accept every created or modified document and can emit zero or more "index" entries in form of key value pairs - where both key and value may be any JSON.
```js
function(doc) {
    if(doc.date && doc.title) {
        emit(doc.date, doc.title);
    }
}
```
### Reduce
Reduce function aggregates results of map function to generate summary results.
```js
function(keys, values, rereduce) {
    return sum(values);
}
```
### View Collation
The keys in view are sorted and you can query view by specifying key range - enabling sophisticated retrievals.
### View joins
If you emit value with \_id field defined - then view will retrieve indicated document - when asked. So you can retrieve related documents based on base document index values.
## Advanced Concepts
### Synchronization
You can enable database to database replication across the servers easily in CouchDB and the server will take care of it.
#### Conflict resolution
If conflicting changes were made in parallel on different replicas both document version will be available for applications to resolve conflict.
### Show and List functions
The **Show** and **List** functions allow you to define server side script that will transform single document or list of documents when requested. You can implement html rendering with them for example.
### Validate functions
Update function allows you to write server side script that will run every time document is created or updated and may reject change according to custom logic.
### Change feeds
The CouchDB server provides live change feed for every change in database. Client application may listen on this change feed and react accordingly.
### Update functions
You may define server side update function that will create or update document in the database when triggered.
## Advantages
* Simple REST API - accessible from browser
* Schema-less
* Powerful - distributed indexes
* OTB Synchronization

# PouchDB
PouchDB is an open-source JavaScript database inspired by Apache CouchDB. It is avalieble under Apache 2.0 license. It may work as JavaScript client for CouchDB server or manage it own database instances. Its API is promise based and vary simple in use.
# Example

```js
var db = new PouchDB('dbname');

db.put({
  _id: 'dave@gmail.com',
  name: 'David',
  age: 69
});

db.changes().on('change', function() {
  console.log('Ch-Ch-Changes');
});

db.replicate.to('http://example.com/mydb');
```
## Stores
For browser
* IndexedDB
* WebSQL
* In Memory
* Local Storage
For Node.js
* LevelDB
* In Memory
For Cordova
* SQLite

## Advantages
* Pure JavaScript
* Promise based API
* Works consistently on desktop in browser and on hybrid mobile application
* May work as thin client or fully replicated database for CouchDB
* May act as CouchDB compatible server
