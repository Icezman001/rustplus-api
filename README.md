# RustPlus API

This is an unofficial NodeJS library for controlling Smart Switches and various other things in the PC game [Rust](https://store.steampowered.com/app/252490/Rust/).

## Features

Below is a list of features that are implemented in this library.

- [x] Turn Smart Switch On
- [x] Turn Smart Switch Off
- [x] Send messages to Team Chat
- [x] Get Entity Info (Current state of Smart Switch / Smart Alarm)
- [ ] Get CCTV Camera Frames (Currently disabled by Facepunch)

There are other things that you can do, such as fetching an image of the map along with all of the player markers and player positions.

However, at the moment these aren't implemented in a helper method but you can still craft the requests manually like so:

```js
rustplus.sendRequest({
    getMap: {}
});
```

Have a look at the `AppRequest` message in the [rustplus.proto](./rustplus.proto) protobuf definition file for ideas on other requests you can send. 

## Example

### Connecting

The library will automatically connect to the server when you instantiate a `RustPlus` object. You will need to provide the follow details to be able to connect:

- Server Hostname/IP
- Server App Port
- Player Id (Your Steam ID)
- Player Token (Token from Server Pairing)

```js
const RustPlus = require('./rustplus');
var rustplus = new RustPlus('hostname', 28183, 'steamid', 1234567890);
```

### Turn Smart Switch On/Off

You can turn a Smart Switch on or off by calling `turnSmartSwitchOn` or `turnSmartSwitchOff` with the entity id of the Smart Switch.

An optional callback function can be provided as the second parameter which will be called when a response has been received from the server for this specific request.

```js
rustplus.turnSmartSwitchOn(1234567, (message) => {
  console.log("turnSmartSwitchOn response message: " + JSON.stringify(message));
  return true;
});
```

### Send messages to Team Chat

```js
rustplus.sendTeamMessage('message to team chat');
```

### Get Entity Info

You can check the current status of a Smart Switch or Smart Alarm by calling `getEntityInfo` with the entity id.

```js
rustplus.getEntityInfo(1234567, (message) => {
  console.log("getEntityInfo response message: " + JSON.stringify(message));
  return true;
});
```

## Handle Messages

A message is a payload of data sent to you from the Rust Server. This shouldn't be confused with Chat Messages.

When you send a request to the Rust Server, you can optionally provide a callback which will be invoked when a response is received from the server for that specific request.

If you return `true` from the callback, the response will be treated as handled.

When a message has been received and has not been handled by your callback, it will be emitted via the `message` event.

You can register a listener to catch all received messages like so:

```js
rustplus.on('message', (message) => {
    console.log("message received: " + JSON.stringify(message));
});
```

Here's a list of the emitted events:

- `connecting`: When we are connecting to the Rust Server.
- `connected`: When we are connected to the Rust Server.
- `disconnected`: When we are disconnected from the Rust Server.
- `error`: When something goes wrong.
- `message`: When an `AppMessage` has been received from the Rust Server.
- `request`: When an `AppRequest` has been sent to the Rust Server.

## Server Pairing

todo: document how to get player token