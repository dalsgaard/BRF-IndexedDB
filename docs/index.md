# Index

## Creating an Index

```javascript
req.onupgradeneeded = function (e) {
  var db = e.target.result;
  var t = e.target.transaction;
  var v = e.oldVersion;
  var items;
  if (v < 2) {
    items = db.createObjectStore("items", { keyPath: "id" });
  }
  if (v < 5) {
    items = t.objectStore('items');
    items.createIndex("name", "name", { unique: false });
  }
}
```

## Using an Index

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

