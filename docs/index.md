# Index

An _index_ is a smart _'look-up table'_ for an existing _field_ in an object store.

## Creating an Index

When the _onupgradeneeded_ callback is called, a transaction is created. This transaction can be used to manipulate the database _skema_.

A new index is created via a call to he _createIndex_ method.

```javascript
req.onupgradeneeded = function (e) {
  var db = e.target.result;
  var t = e.target.transaction;
  var v = e.oldVersion;
  var items;
  if (v < 2) {
    items = db.createObjectStore("items", { keyPath: "id" });
  }
  if (v < 3) {
    items = t.objectStore('items');
    items.createIndex("name", "name", { unique: false });
  }
}
```

## Using an Index

A index can be used to retrieve single documents via the index key.

```javascript
function getItemByName(name) {
  var t = db.transaction(["items"]);
  var items = t.objectStore('items');
  var index = items.index("name");
  var req = index.get(name);
  req.onerror = function (e) {
    console.log(e.target.error);
  };
  req.onsuccess = function (e) {
    console.log(e.target.result);
  };
}
```

