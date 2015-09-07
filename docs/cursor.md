# Cursor

## Using a Cursor

```javascript
function getItems() {
  var t = db.transaction(["items"]);
  var items = t.objectStore('items');
  var req = items.openCursor();
  req.onerror = function (e) {
    console.log(e.target.error);
  };
  req.onsuccess = function (e) {
    var cursor = e.target.result;
    if (cursor) {
      console.log(cursor.key + ': ' + cursor.value.name);
      cursor.continue();
    }
  };
}
```

## Using a Cursor on an Index

```javascript
function getItems() {
  var t = db.transaction(["items"]);
  var items = t.objectStore('items');
  var index = items.index("name");
  var req = index.openCursor();
  req.onerror = function (e) {
    console.log(e.target.error);
  };
  req.onsuccess = function (e) {
    var cursor = e.target.result;
    if (cursor) {
      console.log(cursor.value);
      cursor.continue();
    }
  };
}
```

### Key Cursor

```javascript
function getItems() {
  var t = db.transaction(["items"]);
  var items = t.objectStore('items');
  var index = items.index("name");
  var req = index.openKeyCursor();
  req.onerror = function (e) {
    console.log(e.target.error);
  };
  req.onsuccess = function (e) {
    var cursor = e.target.result;
    if (cursor) {
      console.log(cursor.key + ' : ' + cursor.primaryKey);
      cursor.continue();
    }
  };
}
```

### Key Range

```javascript
function getItemsByName(name) {
  var t = db.transaction(["items"]);
  var items = t.objectStore('items');
  var index = items.index("name");
  var range = IDBKeyRange.only(name);
  var req = index.openCursor(range);
  req.onerror = function (e) {
    console.log(e.target.error);
  };
  req.onsuccess = function (e) {
    var cursor = e.target.result;
    if (cursor) {
      console.log(cursor.value);
      cursor.continue();
    }
  };
}
```

- `IDBKeyRange.lowerBound('Baz')` - ['Baz' .. ->
- `IDBKeyRange.lowerBound('Baz', true)` - ('Baz' .. ->
- `IDBKeyRange.upperBound('Foo', true)` - <- .. 'Foo')
- `IDBKeyRange.upperBound('Foo')` - <- .. 'Foo']
- `IDBKeyRange.bound('Baz', 'Foo', false, true);` - ['Baz' .. 'Foo')

### Direction

```javascript
var req = index.openCursor(null, 'prev');
```

- 'next'
- 'nextunique'
- 'prev'
- 'prevunique'

