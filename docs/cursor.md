# Cursor

A _curser_ can be used to traverse a collection of _elements_.

## Using a Cursor

A cursor can be created to traverse a _object store_. This is done via a call to the _openCursor_ method.

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

A cursor can also be created for a _index_.

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

A _Key Curser_ is a curser that iterates over the key and not the hole document.

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

A curser can be restricted by a _Key Range_.

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

Many different types of key ranges can be constructed.

- `IDBKeyRange.lowerBound('Baz')` - ['Baz' .. ->
- `IDBKeyRange.lowerBound('Baz', true)` - ('Baz' .. ->
- `IDBKeyRange.upperBound('Foo', true)` - <- .. 'Foo')
- `IDBKeyRange.upperBound('Foo')` - <- .. 'Foo']
- `IDBKeyRange.bound('Baz', 'Foo', false, true);` - ['Baz' .. 'Foo')


### Direction

The cursor can iterate in different ways - _'next'_ is default.

```javascript
var req = index.openCursor(null, 'prev');
```

- _'next'_
- _'nextunique'_
- _'prev'_
- _'prevunique'_

