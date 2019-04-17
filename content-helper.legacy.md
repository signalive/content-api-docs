# Legacy Signalive Content Helper

> ### DEPRECATED
>
> Old `window.signalive` helper is now deprecated and legacy. With the release of omma content helper v2, `signalive` namespace will be removed.

### `signalive.once('initialized', handler)`

- handler: `Function` Handler function.

Executes handler function when content helper is fully loaded and usable.

```js
signalive.once('initialized', function() {
    // Do stuff with signalive helper
});
```

> After `initialized` event is fired, some data is initially fetched and ready to be used, such as:
>
> - **`signalive.device`** (see: `signalive.getDevice()`)
> - **`signalive.channel`** (see: `signalive.getChannel()`)
> - **`signalive.shop`** (see: `signalive.getShop()`)
> - **`signalive.owner`** (see: `signalive.getOwner()`)
> - **`signalive.datasources`** (see: `signalive.getAllDatasources()`)
>
> **!!! CAUTION !!!**
>
> These data is fetched initially, therefore all the shortcut objects mentioned above may not represent the final (most recent) version of related data.
>
> So, there is nothing wrong to use these objects on initialization. But for later usages, we recommend to use related getter method by fetching it again.

### `signalive.getDevice()` -> `Promise<Object>`

Gets device information. Device object properties:

Property|Type|Description
----|----|-----------
`uuid`|string|Device UUID
`platform`|string|Device platform: `android`, `electron`, `ios`, `weboss`, `webostv`
`screen.width`|number|Screen width
`screen.height`|number|Screen height
`orientation`|string|Device orientation: `landscape`, `portrait`, `portrait-ccw`
`labels`|{`id`: number, `name`: string}[]|Device label list array
`hardware.manufacturer`|string|Hardware manufacturer
`hardware.model`|string|Hardware model
`hardware.os`|string|Hardware operating system
`hardware.more`|Object?|An object with extra hardware information

### `signalive.getChannel()` -> `Promise<Object>`

Gets channel information. Channel object properties:

Property|Type|Description
----|----|-----------
`id`|number|Channel ID
`name`|string|Channel name

### `signalive.getOwner()` -> `Promise<Object>`

Gets owner account information. Owner object properties:

Property|Type|Description
----|----|-----------
`id`|number|Account ID
`name`|string|Account name

### `signalive.getShop()` -> `Promise<Object>`

Gets shop information. Shop object properties:

Property|Type|Description
----|----|-----------
`id`|number|Shop ID
`name`|string|Shop name
`location.latitude`|number|Shop location latitude
`location.longitude`|number|Shop location longitude
`labels`|{`id`: number, `name`: string}[]|Shop label list array
`orchestrator.id`|number|Shop orchestrator device id
`orchestrator.name`|string|Shop orchestrator device name
`orchestrator.ip`|string|Shop orchestrator ip address
`orchestrator.internalIp`|string|Shop orchestrator internal (local) ip address
`orchestrator.platform`|string|Shop orchestrator device platform
`orchestrator.appVersion`|string|Shop orchestrator device app version

### `signalive.getAllDatasources()` -> `Promise<Object>`

Gets all the binded datasources. Returning object's keys are datasource names and values are datasource objects. Datasource object properties:

Property|Type|Description
----|----|-----------
`id`|number|Datasource ID
`name`|string|Datasource Name
`period`|string|Datasource fetch period
`url`|string|Datasource URL
`data`|JSON|Data
`updatedAt`|string|Last update date string
`createdAt`|string|Creation date string

### `signalive.getDatasource(name)` -> `Promise<Object?>`

- name: `string` Datasource name

Gets a datasource with name. If specified datasource is not found, it resolves to null. Datasource object properties can be seen above.

### `signalive.on('datasource_updated', handler)`

- handler: `Function` Handler function.

Executes handler function when one of binded datasources is updated.

```js
signalive.on('datasource_updated', function(datasource) {
    // This is a global listener,
    // better to check which datasource is updated
    if (datasource.name == 'some datasource name') {
        // Do something, like:
        var element = document.querySelector('#some-element');
        if (element) {
            element.textContent = datasource.data['...'];
        }
    }
});
```

### `signalive.reload()` -> `Promise<null>`

Reloads the current content.

### `signalive.log(message[, payload])` -> `Promise<null>`

- message: `string` Log message
- payload: `any` Optional payload

Sends a `info` log.

### `signalive.log({ level, message, payload })` -> `Promise<null>`

- level: `string` Log level: `silly`, `debug`, `verbose`, `info`, `warn`, `error`
- message: `string` Log message
- payload: `any` Optional payload

Sends a log.
