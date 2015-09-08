# Key Path and Auto Increment

Key Path and Auto Increment is to options that can be applied when a _object store_ is created.

## No Path and No Auto

This is the default behaviour. A key has to be provided when data is added.

```javascript
if (v < 4) {
  db.createObjectStore("noPathNoAuto");
}
```

```javascript
function addNoPathNoAuto(id, name) {
  var t = db.transaction(["noPathNoAuto"], "readwrite");
  t.oncomplete = function (e) {
    console.log('Done!');
  };
  t.onerror = function (e) {
    console.log(e.target.error);
  };
  var os = t.objectStore('noPathNoAuto');
  var req = os.add(name, id);
  req.onsuccess = function (e) {
    console.log(e.target.result);
  };
}
```

## Path and No Auto

Like in the previous examples. The key is read from the _path_.

## No Path and Auto

The key is auto-generated (incremented).

```javascript
if (v < 5) {
  db.createObjectStore("noPathAuto", { autoIncrement: true });
}
```

```javascript
function addNoPathAuto(name) {
  var t = db.transaction(["noPathAuto"], "readwrite");
  t.oncomplete = function (e) {
    console.log('Done!');
  };
  t.onerror = function (e) {
    console.log(e.target.error);
  };
  var os = t.objectStore('noPathAuto');
  var req = os.add(name);
  req.onsuccess = function (e) {
    console.log(e.target.result);
  };
}
```

## Path and Auto

The key is auto-generated (incremented) and placed in _path_.

```javascript
if (v < 6) {
  db.createObjectStore("pathAuto", { keyPath: 'id', autoIncrement: true });
}
```

```javascript
function addPathAuto(name) {
  var t = db.transaction(["pathAuto"], "readwrite");
  t.oncomplete = function (e) {
    console.log('Done!');
  };
  t.onerror = function (e) {
    console.log(e.target.error);
  };
  var os = t.objectStore('pathAuto');
  var req = os.add({
    name: name
  });
  req.onsuccess = function (e) {
    console.log(e.target.result);
  };
}
```