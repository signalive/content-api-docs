## Overlay Renderer
It's a built-in library to create one virtual scene and disrupt into connected displays. 

#### Creating a virtual scene using with Overlay Renderer
Even if you only need to run a single scene in one display, you might run one scene within several devices.
At that point, Overlay Renderer comes out. We can create a virtual screen and arrange our physical device positions into it. When we create an element on the overlay, it will be seen on a device which can involve the position of the element. If you don't know how to differentiate with labels start to read [here](../faq.md).

```javascript
// Screen positions for each device
var SCREENS = {
    left: { x: 0, y: 0, width: 1000, height: 1000 },
    right: { x: 1000, y: 0, width: 1000, height: 1000 }
};
var leftPosition = 0;

var isLeft = device.labels.some(function(label) { return label.name === 'left' });
var screen = isLeft ? SCREENS.left : SCREENS.right;

// Create a scene and arrange devices
var overlayRenderer = new OverlayRenderer({
    baseElement: document.body,
    className: 'overlay',
    viewport: {
        // Total size of virtual scene
        width: 2000,
        height: 1000,
        // Screen positions for current device
        screenWidth: screen.width,
        screenHeight: screen.height,
        screenX: screen.x,
        screenY: screen.y
    }
});

overlayRenderer.setScene([
    {
        id: 'box',
        type: 'text',
        styles: {
            position: 'absolute',
            width: 200,
            height: 200,
            background: '#000'
        },
        value: 'OMMA'
    }
]);

setInterval(function() {
    var boxEl = overlayRenderer.scene.getElementById('box');
    leftPosition += 10;
    boxEl.addStyles({
        left: leftPosition
    });
}, 60);
```

Now we can share one scene into two devices. But scenes are run asynchronously and might occur a difference on element positions. To make a concurrent we can move our trigger into Orchestration Adapter. 

#### Overlay Renderer with Orchestration Adapter

```javascript
var device = omma.device;

// Screen positions for each device
var SCREENS = {
    left: { x: 0, y: 0, width: 1000, height: 1000 },
    right: { x: 1000, y: 0, width: 1000, height: 1000 }
};
var leftPosition = 0;

var isLeft = device.labels.some(function(label) { return label.name === 'left' });
var screen = isLeft ? SCREENS.left : SCREENS.right;

// Create a scene and arrange devices
var overlayRenderer = new OverlayRenderer({
    baseElement: document.body,
    className: 'overlay',
    viewport: {
        // Total size of virtual scene
        width: 2000,
        height: 1000,
        // Screen positions for current device
        screenWidth: screen.width,
        screenHeight: screen.height,
        screenX: screen.x,
        screenY: screen.y
    }
});

overlayRenderer.setScene([
    {
        id: 'box',
        type: 'text',
        styles: {
            position: 'absolute',
            width: 200,
            height: 200,
            background: '#000'
        },
        value: 'OMMA'
    }
]);

setInterval(function() {
    var boxEl = overlayRenderer.scene.getElementById('box');
    leftPosition += 10;
    boxEl.addStyles({
        left: leftPosition
    });
}, 60);
```
