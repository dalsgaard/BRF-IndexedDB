# Intro

## Opening a Database

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

### _onsuccess_



### _onerror_



### _onupgradeneeded_


## Upgrading the Database Schema

```javascript
var req = window.indexedDB.open('Foo', 2);
```

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

