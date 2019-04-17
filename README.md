# Advanced Content
Omma provides a powerful content editor fulfilling most of your basic needs without any technical knowledge, such as:

- Displaying a series of videos, images, websites in a playlist-like fashion
- Customizing display rules for each item
  - Day and hour control: Display or skip some items on specific days or hours
  - Device label control: Display or skip some items on specific devices
  - Frequency control: Display some items more frequently than others
- Running sync content with other devices in the same shop

However you may have more advanced requirements, therefore Omma provides some helper methods and libraries accessible in the content:

- [Content helper](content-helper.md)
- [Player](player.md)
- [Orchestrator Adapter](orchestrator-adapter.md)
- [Overlay Renderer](overlay-renderer.md)


# FAQ
- [Making distiction with labels](#making-distiction-with-labels)

#### Making distiction with labels
On the fly, our codes define what goes on in the content on every single device and all lines run individually in all device. By doing so, we never know which logic runs on a supposed device. Thus we need to make a distinction with labels to run specific logic on the intended device. 

Go to `Devices` page. Choose your first device and give it a label name `left` and `right` to another one.

Create a content and open `Code Editor`. Replace all codes then hit the `Test` button.

```javascript
omma.ready(function(){ 
    const isLeft = omma.device.labels.indexOf('left') > -1;
    if (isLeft) {
        // That line will only run on a device labelled 'left'
        var rootEl = document.getElementById('root');
        rootEl.innerText = "This is lefty";
    }
});
```

This code block will run asynchronously on both devices but only labelled with `left` one will show `This is lefty` text.

