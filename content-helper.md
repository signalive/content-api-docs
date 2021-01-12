# Omma Content Helper

All the content helper stuff lives under `omma` namespace which can be accessed as `window.omma`.

> ## DEPRECATED: `signalive` content helper
>
> If you are looking for old `window.signalive` helper, it is now deprecated. With the release of omma content helper v2, `signalive` namespace will be removed.
>
> [Click here to see API reference of legacy `signalive` content helper.](content-helper.legacy.md)

## Versioning

Omma content helper uses versioning for future API-breaking changes. By default, the only version `v1` is being used. But you should always explicitly call `omma.setVersion()` method before using omma helper.

```js
omma.setVersion('v1');
// now you can use omma helper version v1
```

### `omma.setVersion(version)`

- version: `string`

Changes omma helpers version. If specified version is not found, throws an error.

### `omma.version`

Read-only string indicating omma helper's current version.


# `v1` API Reference

## Initialization

### `omma.ready([handler])` -> `null|Promise<null>`

- handler: `Function` Optional handler function.

If handler function is provided, executes it when content helper is fully loaded and usable. If not, returns a promise which resolves when it's ready.

```js
omma.ready(function() {
    // Do stuff with omma helper, like:
    // omma.device.uuid
});
```

```js
omma
    .ready()
    .then(function() {
        // Do stuff with omma helper, like:
        // omma.device.uuid
    });
```

If target client supports new ES6 features:

```js
await omma.ready();
// Do stuff with omma helper, like:
// omma.device.uuid
```

### `omma.isReady`

A boolean property indicates whether content helper is ready to be used.

### `omma.reload()` -> `Promise<null>`

Reloads the current content.

## Device API (`omma.device`)

Property|Type|Description
----|----|-----------
`omma.device.id`|number|Device ID
`omma.device.uuid`|string|Device UUID
`omma.device.name`|number|Device Name
`omma.device.platform`|string|Device platform: `android`, `electron`, `ios`, `weboss`, `webostv`
`omma.device.orientation`|string|Device orientation: `landscape`, `portrait`, `portrait-ccw`
`omma.device.labels`|string[]|Device label list
`omma.device.appVersion`|string|Current version of omma app running on the device
`omma.device.baseAppVersion`|string|Current base version of omma app running on the device
`omma.device.owner.id`|string|Device owner account id
`omma.device.owner.name`|number|Device owner account name
`omma.device.attribute`|object|Device attribute key-value pairs

### `omma.device.on('update', handler)`

- handler: `Function` A function executed after device information is updated.

```js
omma.device.on('update', function(device) {
    // omma.device properties is now updated
});
```

## Shop API (`omma.shop`)

Property|Type|Description
----|----|-----------
`omma.shop.id`|number|Shop ID
`omma.shop.name`|string|Shop Name
`omma.shop.location.latitude`|number|Shop location latitude
`omma.shop.location.longitude`|number|Shop location longitude
`omma.shop.labels`|string[]|Shop labels
`omma.shop.orchestrator.id`|number|Shop orchestrator device id
`omma.shop.orchestrator.name`|string|Shop orchestrator device name
`omma.shop.orchestrator.ip`|string|Shop orchestrator ip address
`omma.shop.orchestrator.internalIp`|string|Shop orchestrator internal (local) ip address
`omma.shop.orchestrator.platform`|string|Shop orchestrator device platform
`omma.shop.orchestrator.appVersion`|string|Shop orchestrator device app version
`omma.shop.attribute`|object|Shop attribute key-value pairs

### `omma.shop.on('update', handler)`

- handler: `Function` A function executed after shop information is updated.

```js
omma.shop.on('update', function(shop) {
    // omma.shop properties is now updated
});
```

## Channel API (`omma.channel`)

Property|Type|Description
----|----|-----------
`omma.channel.id`|number|Channel ID
`omma.channel.name`|string|Channel Name
`omma.channel.contentVersionId`|number|Content version ID

### `omma.channel.on('update', handler)`

- handler: `Function` A function executed after channel information is updated.

```js
omma.channel.on('update', function(channel) {
    // omma.channel properties is now updated
});
```

## Datasource API (`omma.datasource`)

### `omma.datasource.get(name)` -> `null|omma.Datasource`

- name: `string` Datasource name

Returns the datasource instance (`omma.Datasource`) if provided datasource found otherwise it returns `null`.

### `omma.datasource.getAll()` -> `omma.Datasource[]`

Returns all the datasources of current content.

### `omma.Datasource`

Private class for datasource instances.

Property|Type|Description
----|----|-----------
`id`|number|Datasource ID
`name`|string|Datasource Name
`data`|JSON|Data
`updatedAt`|string|Last update date string

### `omma.Datasource.prototype.on('update', handler)`

- handler: `Function` A function executed after datasource is updated.

```js
var ds = omma.datasource.get('datasource1');

if (ds) {
    // Do something with `ds.data`
    console.log(ds.data);

    // Listen for future updates
    ds.on('update', function(datasource) {
        // `ds` properties is updated,
        // or you can use `datasource` parameter
    });
} else {
    console.log('Datasource not found');
}
```

## Media API (`omma.media`)

### `omma.media.get(idOrName)` -> `Object?`

- idOrName: `number|string` Media id or file name

Returns the media if provided media found in the current content otherwise it returns `null`. If `idOrName` parameter is number, media is searched via id. If it is string, the search is conducted via media file name. Please note that media file name is not unique, so there can be multiple medias with the same file name. In that case, the first occurence will be returned.

Media object properties:

Property|Type|Description
----|----|-----------
`id`|number|Media ID
`name`|string|Media file name
`size`|number|File size in bytes
`src`|string|Downloaded file URL, ready to be used in content
`type`|string|File type: `image`, `video` or `font`

```js
var media = omma.media.get(1234);
// or, via media file name
// var media = omma.media.get('logo.jpg');

if (media) {
    var image = new Image();
    image.src = media.src;
    document.body.appendChild(image);
} else {
    console.log('Media not found');
}
```

### `omma.media.getAll()` -> `Object[]`

Returns all the medias of current content.

## Logging API (`omma.logger`)

```js
omma.logger.silly(message [, payload])
omma.logger.debug(message [, payload])
omma.logger.verbose(message [, payload])
omma.logger.info(message [, payload])
omma.logger.warn(message [, payload])
omma.logger.error(message [, payload])
```

- message: `string` Log message
- payload: `any` Optional payload

## Hardware API (`omma.hardware`)

All the hardware API is async, so all the methods below return Promise.

### `omma.hardware.info()` -> `Promise<Object>`

Gets general hardware info. Returns a promise which resolves to an object with these properties:

Property|Type|Description
----|----|-----------
`manufacturer`|string|Hardware manufacturer
`model`|string|Hardware model
`os`|string|Hardware operating system
`more`|Object?|An object with extra information

### `omma.hardware.location()` -> `Promise<Object>`

Gets device's current location if related APIs are avaliable. Returns a promise which resolves to an object with these properties:

Property|Type|Description
----|----|-----------
`latitude`|number?|Latitude
`longitude`|number?|Longitude

### `omma.hardware.screen()` -> `Promise<Object>`

Gets screen dimensions. Returns a promise which resolves to an object with these properties:

Property|Type|Description
----|----|-----------
`width`|number|Width
`height`|number|Height

### `omma.hardware.devicePixelRatio()` -> `Promise<number>`

Gets the ratio of the resolution in physical pixels to the resolution in CSS pixels for the current display device. Returns a promise which resolves to a number.

### `omma.hardware.network()` -> `Promise<Object>`

Gets network information. Returns a promise which resolves to an object with these properties:

Property|Type|Description
----|----|-----------
`connection`|string|Connection type: `ethernet`, `wifi`, `cellular`, `unknown`
`ip`|string|Ip address in local network

## FileSystem API (`omma.fs`)

Omma content helper provides a sandboxed file system for each content. So, all the versions of a content can access the same file system, while other contents cannot.

> Please note that omma app can delete these files if more free space is required. So, we do not recommend to store important information in file system.

### `omma.fs.list(folderPath)` -> `Promise<Object[]>`

- folderPath: `string` Folder path

Reads the contents of a directory. Returns a promise that resolves array of file/folder entries.

Entry object properties:

Property|Type|Description
----|----|-----------
`file`|boolean|`true` if the entry is a file, otherwise `false`
`folder`|boolean|`true` if the entry is a folder, otherwise `false`
`name`|string|File or folder name
`path`|string|Full path on the file system
`size`|number|File size in bytes

Sample code for listing the contents of (content's) root folder:
```js
omma.fs
    .list('/')
    .then(function(entries) {
        // Do some thing with `entries`
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

Sample entries output:
```json
[
    {
        "file": false,
        "folder": true,
        "name": "some-folder",
        "path": "file:///Users/ommasign/content-1234/some-folder",
        "size": 128
    },
    {
        "file": true,
        "folder": false,
        "name": "lena.jpg",
        "path": "file:///Users/ommasign/content-1234/lena.jpg",
        "size": 29993
    }
]
```

### `omma.fs.mkdirp(path)` -> `Promise<null>`

- path: `string` Folder path

Creates a new directory and any necessary subdirectories for given path.

```js
omma.fs
    .mkdirp('/some-folder/another-folder')
    .then(function() {
        // Folder is successfully created
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

### `omma.fs.move(oldPath, newPath)` -> `Promise<null>`

- oldPath: `string` Source file or folder path
- newPath: `string` Destination file or folder path

Moves or renames a file or a directory.

```js
omma.fs
    .move('/lena.jpg', '/some-folder/lena-renamed.jpg')
    .then(function() {
        // File is successfully moved
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

### `omma.fs.copy(oldPath, newPath)` -> `Promise<null>`

- oldPath: `string` Source file or folder path
- newPath: `string` Destination file or folder path

Copies a file or a directory.

```js
omma.fs
    .copy('/lena.jpg', '/some-folder/lena-backup.jpg')
    .then(function() {
        // File is successfully copied
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

### `omma.fs.download(sourceURL, toPath)` -> `Promise<Object>`

- sourceURL: `string` Source file URL to be downloaded
- toPath: `string` Download path in file system

Downloads a file.

```js
omma.fs
    .download('https://bellard.org/bpg/lena30.jpg', '/lena.jpg')
    .then(function(entry) {
        // File is successfully downloaded
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

### `omma.fs.remove(path)` -> `Promise<null>`

- path: `string` File or folder path to be deleted

Removes a file or a folder.

```js
omma.fs
    .remove('/lena.jpg')
    .then(function() {
        // File is successfully deleted
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

### `omma.fs.write(path, data)` -> `Promise<null>`

- path: `string` File path to be written
- data: `string` Text content

Writes a given text content (`data`) to given `path` with using `utf8` encoding. A sample that writes a json file:

```js
var data = { some: 'content' };

omma.fs
    .write('/data.json', JSON.stringify(data))
    .then(function() {
        // File is successfully written
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

### `omma.fs.read(path)` -> `Promise<string>`

- path: `string` File path to be read

Reads the content of a given file as text file (with `utf8` encoding). A sample that reads a json file:

```js
omma.fs
    .read('/data.json')
    .then(function(dataString) {
        var data = JSON.parse(dataString);
        console.log(data); // => { some: 'content' }
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

### `omma.fs.cache(sourceURL)` -> `Promise<string>`

- sourceURL: `string` Source file URL to be cached

A helper method to cache a remote file. It returns a promise that resolves cached file path which can be used directly in content. If source url is already cached, it will immediately resolve.

```js
omma.fs
    .cache('https://bellard.org/bpg/lena30.jpg')
    .then(function(filePath) {
        // Use the `filePath`, for example:
        var image = new Image();
        image.src = filePath;
        document.body.appendChild(image);
    })
    .catch(function(err) {
        console.log('An error occured', err);
    });
```

## Extras
### `omma.locals` -> `object`

An object which values can be set & get freely. Values of the locals object can be used inside of scene editor's `text elements` by setting value of the element to `{{key}}` where `key` resides in `locals`.

```js
omma.locals.burgerPrice = 15.34569

/*
*  Add a scene item and insert a text element with value of:
*  {{ burgerPrice.toFixed(2) + "$"}}
*/
```
