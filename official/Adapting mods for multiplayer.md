# Adaptation of mods for network play

## Declaration
To declare a mod as supporting the game in multiplayer, in the `launcher.js` launch script, before calling `Launch(...)`, you need to call the `ConfigureMultiplayer(configuration)` method. When calling this method, you can specify the name and version parameters, which will be used to identify the mod in the network game. If any of these parameters is not specified, then it will be taken from `mod.info`. In addition, if the `isClientOnly: true` parameter is present, the mod will be declared as purely client-side, client-side mods should not affect the world and will not be taken into account when mods are synchronized when connected(examples: MiniMap, Recipe Viewer, WAILA).

```js
ConfigureMultiplayer({
    name: "readable unique name", // unique network name of the mod
    version: "version", // version
    isClientOnly: true / false // if true, the mod is considered purely client-side
});
```

## General concepts
In order to work correctly in a network game, the mod code must be divided into client and server parts, which do not interact with each other in any way, except for network packets and events of network entities. This means that the client side of the mod cannot use variables, functions, etc. server side and vice versa.

The server side should be responsible for all the logic and work with the world, while the client side is responsible for the visual part based on data from the server and sending custom actions to the server.

However, many standard objects such as Tile Entities, containers, armor, etc. already adapted for multiplayer if you use the new API.

If your world is loaded, the server and the client connected to it are launched on the same device. When connecting to the world, a client is launched, connected to the server running on the host.

## Network packet transmission and processing
The basic network functionality is based on the transfer of packets - named data, from clients to the server and vice versa. To accept packets with a specific name for a server or client, you can register an event that will be triggered every time a packet with the corresponding name arrives.

```js
// the event will be triggered when the testMod.serverMessage package arrives on the client
Network.addClientPacket("testMod.serverMessage", function(packetData) {
    // we know that an object of the form {sender: ..., text: ...} is coming
    Game.message("[" + packetData.sender + "]" + packetData.text);
});

// function that sends a message to all players
function sendMessageToAll(sender, text) {
    // send a packet to all clients testMod.serverMessage
    Network.sendToAllClients("testMod.serverMessage", {
        sender: sender,
        text: text
    });
}
```
Similarly, you can send packets from the client to the server.
```js
// event the server receives the testMod.clientMessage package from the client
Network.addServerPacket("testMod.clientMessage", function(client, data)) {
    // send a message to all clients, by the sender indicate the player from whom the message came
    sendMessageToAll(client.getPlayerUid(), data.text);
});

// client function that sends a message to the player to the server, which will then send it to all players on his behalf
function sendMessageFromClient(message) {
    // send a packet from client to server
    Network.sendToServer("testMod.clientMessage", {text: message});
}
```

In the case of the last example, we can see that the client parameter is received in the event of receiving a packet by the server, which is the server interface of the client from which this packet was received. It can be used to get the player entity (`client.getPlayerUid()`). In addition, it allows you to send packets to a specific client, and not all at once. To get a client, knowing the identity of the player, use `Network.getClientForPlayer(playerUid)`.

```js
// register a use function for the stick
Item.registerUseFunctionForID(280, function(coords, item, block, player) {
    // get the client interface for the using player
    var client = Network.getClientForPlayer(player);
    // make sure to check that it exists
    if(client! = null) {
        // send a message specifically to this client
        client.send("testMod.serverMessage", {
            sender: "server debug", // sender - server debug
            text: "you just used stick!" // just text
        });
    }
});
```

## Convert block and item IDs
The numeric IDs of the same blocks and items will most likely be different on the client and on the server. Inner Core automatically syncs IDs so that the same item will have different IDs on different devices. However, if they are transmitted as numbers using packets, they will be different and will have to be converted manually.

Since there are many clients, and there is only one server, the conversion occurs on the client side, for this there are the functions `Network.serverToLocalId(id)` and `Network.localToServerId(id)`, converting the server ID to the client ID and vice versa.

Consider an example to demonstrate this:

```js
// a packet that comes to the client from the server with information about the use of the item(for example, we pass only id and data)
Network.addClientPacket("testMod.useItemInfo", function() {
    // display information: first the id that the server sent us, then the id that this item has on the client, to confirm that this is the same item on the client, we get its name by the client id
    var localId = Network.serverToLocalId(packetData.id);
    Game.message("you just used item: server =" + packetData.id + "client =" + localId + "name =" + Item.getName(localId, packetData.data));
});

// server event of item use
Callback.addCallback("ItemUse", function(coords, item, block, isExternal, player) {
    var client = Network.getClientForPlayer(player);
    if(client! = null) {
        // send a packet with the item data, since the event is purely server-side, this will be the server-side id of the item
        client.send("testMod.useItemInfo", {id: item.id, data: item.data});
    }
});
```

In order to test this example in practice, you need two devices, because a pair of client and server that run on the same device will have the same id. To force different ids on two devices, you can try changing the innercore / mods / .staticid file

## Tile Entity adaptation
The main logic of Tile Entity runs on the server side. However, you can declare a separate prototype for the client. According to this prototype, an instance will be created for clients who have this Tile Entity nearby, have events of appearance, tick and destruction, which allows you to add a visual component.

Since the old container, which was used by Tile Entity earlier, does not support multiplayer, in order for Tile Entity to support network play, it needs to be transferred to a new container type - ItemContainer. The new container type is compatible with old saves.

The following is the documentation for the Tile Entity network prototype, the events that are omitted here are server side.

```js
{
// server side prototype

useNetworkItemContainer: true, // allow the use of a container with multiplayer support

defaultValues: {
        // stored values
},

    // initialization
init: function() {
// The following fields are available inside the server Tile Entity:
// this.x, this.y, this.z, this.dimension - coordinates and dimension
// this.data - all stored values
// this.container is the ItemContainer object
// this.blockSource - BlockSource object for accessing the dimension in which this Tile Entity is located
// this.networkData - SyncedNetworkData object synchronized data available to all clients
// this.networkEntity is a NetworkEntity object representing the network entity that this Tile Entity is based on, only needed for advanced operations, usually not required

// Network methods
// this.sendPacket("eventName", data) - Sends a packet to all client instances of this Tile Entity
},
    
    // this event is called on the server side and returns the name of the interface to open on click
getScreenName: function(player, coords) {
        return "someName";
    },

    // this event is called on the client side, this is not defined in this case, by the passed name returned by the getScreenName method, it returns the window that needs to be opened
    getScreenByName: function(screenName) {
        return someGuiWindow;
    },

client: {
// client Tile Entity instance prototype, an instance is created for each client, every time the Tile Entity becomes visible on the client side and is destroyed every time it becomes invisible.

// called when the instance is created
load: function() {
    // The following fields are available inside the client Tile Entity:
    // this.x, this.y, this.z, this.dimension - coordinates and dimension
    // this.networkData - SyncedNetworkData object synchronized data available to all clients
    // this.networkEntity is a NetworkEntity object representing the network entity that this Tile Entity is based on, only needed for advanced operations, usually not required

// container and world access are not available on the client side

// Network methods
// this.sendPacket("eventName", data) - Sends a packet to the server Tile Entity instance
},

        // called when the instance is destroyed
unload: function() {

},

        // every tick is called on the local thread, it is worth considering, the loads on the local thread have a greater impact on performance
tick: function() {

},

events: {
// events that receive packages on the client side, in this case this will be the client instance that received this package
packetName: function(packetData, packetExtra) {

}
},

containerEvents: {
// events of the client container instance, this is undefined
// these events are for editing the contents of the window
eventName: function(container, window, windowContent, eventData) {
// window and windowContent can be null
// To send data to the server container.sendEvent("eventName", someData)
}
}
},

events: {
// events that receive packets on the server side, in this case this is the server instance that received the packet
packetName: function(packetData, packetExtra, connectedClient) {
// method available only here, sends a packet to a specific client: this.sendResponse("packetName", someData)
}
},

containerEvents: {
// container events on the server side, in this case this is the server instance that received the package

eventName: function(eventData, connectedClient) {
// method available only here: this.container.sendResponseEvent("eventName", someData)
}
}
}
```
Let's look at a few examples, starting with the simplest. In the examples, the interface window declaration will be missing, since no changes are required in this scope, the variable containing it will be named `guiExample`.

An example of the simplest Tile Entity with an interface and basic container logic.

```js
{
    useNetworkItemContainer: true, // use a network container
    
    getScreenName: function(player, coords) {
        // we only have one version of the interface, let's call it "main"
        return "main";
    },

    getScreenByName: function(screenName) {
        // we only have an interface named "main"
        return screenName == "main"? guiExample: null;
        // in addition, it is possible without checking the name:
        return guiExample;
    },
    
    tick: function() {
        // in the server tick, we will perform the basic logic on the container:
        if(World.getThreadTime()% 20 == 0) {
            var slot = this.container.getSlot("someSlot");
            if(slot.id == 264) {
                // you need to apply slot changes through setSlot so that the container registers them, just changing the object's fields(slot.count--) will not be counted
                this.container.setSlot("someSlot", slot.id, slot.count - 1, slot.data, slot.extra);
                // slot validation: if the count is 0, clear the slot
                this.container.validateSlot("someSlot");
                // here you can do some other server logic
            }
        }
        // send all changes to the container to clients so that they display the current content, this function sends only the changed slots and values, which happens quickly, so it can be called at the end of each tick if changes could occur on the server side
        this.container.sendChanges();
    }
}
```

Now let's look at using the Tile Entity network events. The following example demonstrates the use of a client prototype and network events to create particle animations and is a continuation of the past(only the places to be shown will be shown).

```js
{
    ...
    client: {
        // add a client prototype that will be responsible for animations
        events: {
            // event that receives the animateParticles package
            animateParticles: function(packetData) {
                // create fire particles packetData.power
                for(var i = 0; i <packetData.power; i ++) {
                    Particles.addParticle(7, this.x + .5, this.y + .5, this.z + .5, Math.random() - .5, Math.random() - .5, Math.random() - .50);
                }
            }
        }
    }
    
    tick: function() {
        ...
        if(slot.id == 264) {
            ...
            // on successful operation, send an animation event to all clients
            this.sendPacket("animateParticles", {power: 100});
        }
        ...
    }
}
```

## Player Tick
Since many operations are required to be performed for each player separately, an additional `ServerPlayerTick` event has been introduced. This event is called on the server side every tick for every player. The following is an example of adding an event:

```js
Callback.addCallback("ServerPlayerTick", function(playerUid, isPlayerDead) {
    // playerUid - player entity
    // isPlayerDead - additional parameter - whether this player is dead
    var client = Network.getClientForPlayer(playerUid);
    if(client! = null) {
        // do something
    }
});
```

In addition, there is a client tick event, which is called every tick of the client stream - `LocalTick`. All client actions of the tick must take place in it, in particular, by the player's custom movement.

```js
Callback.addCallback("LocalTick", function() {
    // local player can be obtained via Player.get()
});
```

## Armor
Since the old methods of registering armor events were not suitable for multiplayer, new registration functions have been added, which are listed below.

```js
Armor.registerOnTickListener(id, function(item, slot, player) {
    // this event is triggered every tick for every player wearing this armor
    // this event can return an object of the type {id:, count:, data:, extra:} to change the item of armor, if nothing is returned, then the armor will not be changed
});
```

```js
Armor.registerOnHurtListener(id, function(item, slot, player, value, type, attacker, bool1, bool2) {
    // this event is triggered when damage is dealt to a player wearing armor
    // this event can return an object of the form {id:, count:, data:, extra:} to change the item of armor, if nothing is returned, then the armor will be damaged in the standard way
});
```

```js
Armor.registerOnTakeOnListener(id, function(item, slot, player) {
    // this event is triggered when the player puts on armor or spawns with it
});
```

```js
Armor.registerOnTakeOffListener(id, function(item, slot, player) {
    // this event is triggered when the player removes or replaces this piece of armor
});
```