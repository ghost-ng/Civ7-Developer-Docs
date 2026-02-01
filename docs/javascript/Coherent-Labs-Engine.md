# Coherent Labs Engine API

Civilization VII's UI is built on **Coherent Labs Gameface** (also known as Cohtml), a commercial HTML5/JavaScript UI framework designed for game engines. This document covers the `engine` API that provides communication between JavaScript UI code and the game.

## Official Documentation

- [Coherent Labs Gameface Documentation](https://docs.coherent-labs.com/unreal-gameface/api_reference/)
- [JavaScript Interactions Guide](https://docs.coherent-labs.com/unreal-gameface/integration/ui-scripting/jsinteractions/)
- [Data Binding Guide](https://docs.coherent-labs.com/unreal-gameface/integration/ui-scripting/databinding/)

## The `engine` Object

The `engine` object is the primary interface for communication between JavaScript and the game. It's defined in `cohtml.js` and provides event subscription, function calls, and data exchange.

### Source File
```
Base/modules/core/ui/cohtml.js
```

## Event Subscription

### engine.on(name, callback, [context])

Subscribe to events triggered by the game.

```javascript
// Basic subscription
engine.on("TurnBegin", (turn) => {
    console.log(`Turn ${turn} started`);
});

// With context (for class methods)
engine.on("CitySelectionChanged", this.onCitySelected, this);

// Store handle for later cleanup
const handle = engine.on("UnitMoved", this.onUnitMoved, this);
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | string | Event name to subscribe to |
| `callback` | function | Function to call when event fires |
| `context` | object | (Optional) `this` context for callback |

**Returns:** Connection object with `clear()` method for unsubscription.

### engine.off(name, handler, [context])

Unsubscribe from an event.

```javascript
// Unsubscribe by handler reference
engine.off("TurnBegin", myCallback);

// Unsubscribe with context
engine.off("TurnBegin", this.onTurnBegin, this);
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | string | Event name to unsubscribe from |
| `handler` | function | The callback function to remove |
| `context` | object | (Optional) Context that was used during subscription |

### Handle-based Cleanup

The preferred method for cleanup is using the handle returned by `engine.on()`:

```javascript
class MyPanel {
    constructor() {
        // Store handles
        this.handles = [];
        this.handles.push(engine.on("TurnBegin", this.onTurn, this));
        this.handles.push(engine.on("CitySelectionChanged", this.onCity, this));
    }

    destroy() {
        // Clean up all subscriptions
        this.handles.forEach(h => h.clear());
        this.handles = [];
    }
}
```

## Calling Game Functions

### engine.call(name, ...args)

Call a function exposed by the game and receive a result.

```javascript
// Simple call
engine.call("PlaySound", "UI_Click");

// Call with parameters
engine.call("SetAgeInitializationParameters", initParams);

// Call that returns a Promise
engine.call("GetPlayerData", playerId).then((data) => {
    console.log("Player data:", data);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | string | Function name to call |
| `...args` | any | Arguments to pass to the function |

**Returns:** Promise that resolves with the function result.

### engine.trigger(name, data)

Send structured data to the game (alternative to `call` for complex objects).

```javascript
const mapConfig = {
    width: 100,
    height: 80,
    wrapX: true,
    wrapY: false
};
engine.trigger("SetMapConfiguration", mapConfig);
```

## Common Event Names

These events are commonly used in Civ VII UI scripts:

### Turn Events
| Event | Parameters | Description |
|-------|------------|-------------|
| `TurnBegin` | turn | New turn starts |
| `LocalPlayerTurnBegin` | data | Local player's turn begins |
| `RemotePlayerTurnBegin` | data | Remote player's turn begins |
| `PlayerTurnActivated` | playerID | A player's turn activates |

### Selection Events
| Event | Parameters | Description |
|-------|------------|-------------|
| `CitySelectionChanged` | playerID, cityID | City selection changes |
| `UnitSelectionChanged` | playerID, unitID | Unit selection changes |

### Map Generation Events
| Event | Parameters | Description |
|-------|------------|-------------|
| `RequestMapInitData` | initParams | Map needs initialization data |
| `GenerateMap` | - | Trigger map generation |

### Age Transition Events
| Event | Parameters | Description |
|-------|------------|-------------|
| `RequestAgeInitializationParameters` | data | Age transition starting |
| `GenerateAgeTransition` | - | Generate age content |

### UI Events
| Event | Parameters | Description |
|-------|------------|-------------|
| `InputAction` | action | Input from keyboard/controller |
| `input-source-changed` | deviceType, deviceLayout | Input device changed |
| `UIGameLoadingProgressChanged` | progress | Loading progress update |
| `CameraChanged` | data | Camera position/zoom changed |

## Example: Map Script Registration

Map scripts use the engine API to register for generation events:

```javascript
// From Base/modules/base-standard/maps/continents.js

function requestMapData(initParams) {
    // Configure map parameters
    const mapData = {
        width: initParams.width,
        height: initParams.height,
        // ... other configuration
    };
    engine.call("SetMapInitData", mapData);
}

function generateMap() {
    // Generate terrain, resources, etc.
    TerrainBuilder.setTerrainType(x, y, terrainType);
    ResourceBuilder.setResourceType(x, y, resourceType);
}

// Register event handlers
engine.on("RequestMapInitData", requestMapData);
engine.on("GenerateMap", generateMap);
```

## Example: UI Panel with Events

```javascript
class InfoPanel {
    constructor() {
        this.handles = [];
        this.container = document.getElementById("info-panel");

        // Subscribe to events
        this.handles.push(
            engine.on("TurnBegin", this.onTurnBegin, this)
        );
        this.handles.push(
            engine.on("CitySelectionChanged", this.onCitySelected, this)
        );
    }

    onTurnBegin(turn) {
        this.updateTurnDisplay(turn);
    }

    onCitySelected(playerID, cityID) {
        if (cityID !== -1) {
            const city = Cities.get(cityID);
            this.showCityInfo(city);
        }
    }

    destroy() {
        // Clean up all event subscriptions
        this.handles.forEach(h => h.clear());
        this.handles = [];
    }
}
```

## Data Types

The engine API supports these data types for parameters:

| JavaScript Type | Notes |
|-----------------|-------|
| `number` | Integers and floats |
| `string` | Text data |
| `boolean` | true/false |
| `object` | Plain objects (converted to game structs) |
| `array` | Arrays of supported types |

## Best Practices

1. **Always clean up subscriptions** - Use `handle.clear()` or `engine.off()` when components are destroyed

2. **Bind context for class methods** - Pass `this` as the third argument or use `.bind()`

3. **Check for null/undefined** - Event data may be missing expected properties

4. **Use descriptive event names** - Follow the game's naming conventions

5. **Don't block in callbacks** - Keep event handlers fast and non-blocking

## See Also

- [Events Reference](Events.md) - Complete list of game events
- [JavaScript Overview](Overview.md) - JavaScript API reference
- [UI Modding](../technical-reference/UI-Modding.md) - UI modification guide
