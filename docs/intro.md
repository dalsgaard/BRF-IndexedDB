# Intro

The IndexedDB API is places as the property _indexedDB_ on the _window_ (root) object.

Most of the interaction is asynchronous and take the form.

```javascript
var req = someObject.someMethod(someArgs);
req.onerror = function (e) {
  // Error Occurred!
}
req.onsuccess = function (e) {
  	
}
```

## Opening a Database

A database is opened via a call to _open_. The first argument is the name of the database, and the second is the version number.

```javascript
(function () {
  var db;
  var req = window.indexedDB.open('Foo', 1);
  req.onsuccess = function (e) {
    db = e.target.result;
  };
  req.onerror = function (e) {
    console.log(e);
  };
  req.onupgradeneeded = function (e) {
    console.log(e);
  }
})();
```

## Upgrading the Database Schema

If the version number of the existing database is lower than the requested version, a callback assigned to the _onupgradeneeded_ property is called.

```javascript
var req = window.indexedDB.open('Foo', 2);
```
The event given to the callback contains (among others) a reference to the database and the version number of the existing database.

An _Object Store_ can be created via a call to _createObjectStore_ method, where the first argument is the name, and the second is an object with options.

```javascript
req.onupgradeneeded = function (e) {
  var db = e.target.result;
  var v = e.oldVersion;
  if (v < 2) {
    var items = db.createObjectStore("items", { keyPath: "id" });
  }
}
```

## Adding Data to the Database

All reading and writing from and to the database is done in a transaction. A transaction is created via a call to the _transaction_ method, where the first argument is an array of the _object stores_ participating in the transaction, and the optional second argument is the _mode_ (default is 'readonly').

Data is added to an object store via a call to the _add_ method. 

```javascript
function addItem(id, name) {
  var t = db.transaction(["items"], "readwrite");
  t.oncomplete = function (e) {
    console.log('Done!');
  };
  t.onerror = function (e) {
    console.log(e.target.error);
  };
  var items = t.objectStore('items');
  var req = items.add({
    id: id,
    name: name
  });
  req.onsuccess = function (e) {
    console.log(e.target.result);
  }
}
```

## Getting Data from the Database

Data is retrieved from an object store via a call to the _get_ method.

```javascript
function getItem(id) {
  var t = db.transaction(["items"]);
  var items = t.objectStore('items');
  var req = items.get(id);
  req.onerror = function (e) {
    console.log(e.target.error);
  };
  req.onsuccess = function (e) {
    console.log(e.target.result);
  };
}
```

## Updating Data in the Database

Data in the object store is updated via a call to the _update_ method.


```javascript
function updateItem(item) {
  var t = db.transaction(["items"], "readwrite");
  var items = t.objectStore('items');
  var req = items.put(item);
  req.onerror = function (e) {
    console.log(e.target.error);
  };
  req.onsuccess = function (e) {
    console.log(e.target.result);
  };
}
```

## Deleting Data from the Database

Data in the object store is deleted via a call to the _delete_ method.

```javascript
function deleteItem(id) {
  var t = db.transaction(["items"], "readwrite");
  var items = t.objectStore('items');
  var req = items.delete(id);
  req.onerror = function (e) {
    console.log(e.target.error);
  };
  req.onsuccess = function () {
    console.log('Gone!');
  };
}
```

