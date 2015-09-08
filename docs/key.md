# Key Path and Auto Increment

## No Path and No Auto

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


## No Path and Auto

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