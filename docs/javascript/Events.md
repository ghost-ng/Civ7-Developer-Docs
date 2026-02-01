# JavaScript Events Reference

This document lists all JavaScript events that fire during Civilization VII gameplay. Events are the primary mechanism for scripts to respond to game state changes.

## Quick Navigation

- [Engine Events](#engine-events) - All gameplay and engine events
- [DOM/Window Events](#domwindow-events) - UI and browser events
- [Custom Events](#custom-events) - Creating your own events
- [Event Subscription](#event-subscription) - How to subscribe to events

---

## Engine Events

All game events in Civ VII are subscribed using `engine.on()`. The `engine` object is provided by the Coherent Labs Cohtml framework.

> **Important:** Unlike Civ VI which used a Lua `Events` object, Civ VII uses JavaScript with `engine.on("EventName", callback)` for all event subscriptions.

### Event Subscription Pattern

```javascript
// Subscribe to an event
engine.on("EventName", (data) => {
    // Handle event
});

// Subscribe with context (for class methods)
engine.on("EventName", this.handleEvent, this);

// Store handle for later unsubscription
const handle = engine.on("EventName", callback);
// Later:
handle.clear();

// Unsubscribe directly
engine.off("EventName", callback, context);
```

### Age Transition Events

#### RequestAgeInitializationParameters

Fired when an age transition is starting and the game needs initialization parameters.

```javascript
engine.on("RequestAgeInitializationParameters", (data) => {
    // data contains:
    // - numMajorPlayers: number of major civilizations
    // - outgoingAge: the age being left
    // - incomingAge: the age being entered

    // Set parameters with engine.call()
    engine.call("SetAgeInitializationParameters", initParams);
});
```

**Parameters:**
| Property | Type | Description |
|----------|------|-------------|
| `numMajorPlayers` | number | Count of major civilizations |
| `outgoingAge` | string | Current age type |
| `incomingAge` | string | New age type |

#### GenerateAgeTransition

Fired during age transition content generation.

```javascript
engine.on("GenerateAgeTransition", () => {
    // Generate age transition content
});
```

### Map Generation Events

#### RequestMapInitData

Fired when the game needs map initialization data. Used by map scripts.

```javascript
engine.on("RequestMapInitData", (initParams) => {
    // Configure map parameters
    requestMapData(initParams);
});
```

#### GenerateMap

Fired to trigger map generation. Used by map scripts.

```javascript
engine.on("GenerateMap", () => {
    // Run map generation logic
    generateMap();
});
```

### Turn Events

#### TurnBegin

Fired at the start of each new turn.

```javascript
engine.on("TurnBegin", (turn) => {
    console.log(`Turn ${turn} has begun`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `turn` | number | Current turn number |

#### LocalPlayerTurnBegin

Fired when the local player's turn begins.

```javascript
engine.on("LocalPlayerTurnBegin", (data) => {
    // Handle local player turn start
});
```

#### RemotePlayerTurnBegin

Fired when a remote player's turn begins (multiplayer).

```javascript
engine.on("RemotePlayerTurnBegin", (data) => {
    // Handle remote player turn start
});
```

#### PlayerTurnActivated

Fired when any player's turn is activated.

```javascript
engine.on("PlayerTurnActivated", (playerID) => {
    console.log(`Player ${playerID} turn activated`);
});
```

### City Events

#### CitySelectionChanged

Fired when the player selects or deselects a city.

```javascript
engine.on("CitySelectionChanged", (playerID, cityID) => {
    if (cityID !== -1) {
        console.log(`Player ${playerID} selected city ${cityID}`);
    }
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Owner player ID |
| `cityID` | number | Selected city ID (-1 if deselected) |

#### CityProductionCompleted

Fired when a city completes production of an item.

```javascript
engine.on("CityProductionCompleted", (playerID, cityID, itemType) => {
    console.log(`City ${cityID} completed ${itemType}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Owner player ID |
| `cityID` | number | City ID |
| `itemType` | string | Completed item type |

#### CityPopulationChanged

Fired when a city's population changes.

```javascript
engine.on("CityPopulationChanged", (data) => {
    // Handle population change
});
```

#### CityInitialized

Fired when a new city is created and initialized.

```javascript
engine.on("CityInitialized", (playerID, cityID) => {
    // Handle new city
});
```

### Unit Events

#### UnitSelectionChanged

Fired when the player selects or deselects a unit.

```javascript
engine.on("UnitSelectionChanged", (playerID, unitID) => {
    if (unitID !== -1) {
        console.log(`Player ${playerID} selected unit ${unitID}`);
    }
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Owner player ID |
| `unitID` | number | Selected unit ID (-1 if deselected) |

#### UnitMoved

Fired when a unit completes movement.

```javascript
engine.on("UnitMoved", (playerID, unitID) => {
    console.log(`Unit ${unitID} has moved`);
});
```

#### UnitMovementPointsChanged

Fired when a unit's movement points change.

```javascript
engine.on("UnitMovementPointsChanged", (data) => {
    // Handle movement points update
});
```

#### UnitCreated

Fired when a new unit is created.

```javascript
engine.on("UnitCreated", (data) => {
    // Handle unit creation
});
```

#### UnitKilled

Fired when a unit is destroyed.

```javascript
engine.on("UnitKilled", (data) => {
    // Handle unit death
});
```

### Technology & Civic Events

#### TechnologyResearched

Fired when a player completes a technology.

```javascript
engine.on("TechnologyResearched", (playerID, techType) => {
    console.log(`Player ${playerID} researched ${techType}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Researching player ID |
| `techType` | string | Technology type completed |

#### CivicCompleted

Fired when a player completes a civic.

```javascript
engine.on("CivicCompleted", (playerID, civicType) => {
    console.log(`Player ${playerID} completed civic ${civicType}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Player ID |
| `civicType` | string | Civic type completed |

### Diplomacy Events

#### WarDeclared

Fired when war is declared between players.

```javascript
engine.on("WarDeclared", (attackerID, defenderID) => {
    console.log(`Player ${attackerID} declared war on ${defenderID}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `attackerID` | number | Attacking player ID |
| `defenderID` | number | Defending player ID |

#### PeaceMade

Fired when peace is established between players.

```javascript
engine.on("PeaceMade", (player1ID, player2ID) => {
    console.log(`Peace between ${player1ID} and ${player2ID}`);
});
```

### Player Events

#### PlayerYieldChanged

Fired when a player's yield production changes.

```javascript
engine.on("PlayerYieldChanged", (data) => {
    // Handle yield change
});
```

### Network Events

#### NetworkDisconnected

Fired when the network connection is lost.

```javascript
engine.on("NetworkDisconnected", () => {
    // Handle disconnection
});
```

#### NetworkReconnected

Fired when the network connection is restored.

```javascript
engine.on("NetworkReconnected", () => {
    // Handle reconnection
});
```

### Application Events

#### UserRequestClose

Fired when the user requests to close the game.

```javascript
engine.on("UserRequestClose", () => {
    // Handle close request (save, cleanup, etc.)
});
```

### Input Events

#### InputAction

Fired for input events from keyboard/controller.

```javascript
engine.on("InputAction", (action) => {
    // Handle input action
});
```

#### input-source-changed

Fired when input source changes (keyboard to controller, etc.).

```javascript
engine.on("input-source-changed", (deviceType, deviceLayout) => {
    // Handle input source change
});
```

### UI Events

#### UIGameLoadingProgressChanged

Fired when game loading progress updates.

```javascript
engine.on("UIGameLoadingProgressChanged", (progress) => {
    // Update loading UI
});
```

#### CameraChanged

Fired when the camera position or zoom changes.

```javascript
engine.on("CameraChanged", (data) => {
    // Handle camera update
});
```

---

## DOM/Window Events

Standard DOM events used for UI interaction.

### Lifecycle Events

#### DOMContentLoaded

Fired when the DOM is fully loaded.

```javascript
document.addEventListener("DOMContentLoaded", () => {
    // Initialize UI components
});
```

### Custom Engine Events

#### engine-input

Fired for engine input events.

```javascript
window.addEventListener("engine-input", (event) => {
    // Handle engine input
});
```

#### user-interface-loaded-and-ready

Fired when the UI is fully loaded and ready.

```javascript
window.addEventListener("user-interface-loaded-and-ready", () => {
    // UI is ready for interaction
});
```

### Element Events

#### click

Standard click event for buttons and interactive elements.

```javascript
element.addEventListener("click", (event) => {
    // Handle click
});
```

#### keydown

Keyboard input for hotkeys.

```javascript
window.addEventListener("keydown", (event) => {
    if (event.key === "F8") {
        // Toggle custom panel
    }
});
```

---

## Custom Events

You can create and dispatch custom events for communication between UI components.

### Creating Custom Events

```javascript
const myEvent = new CustomEvent("my-custom-event", {
    bubbles: true,
    cancelable: true,
    detail: {
        customData: "value",
        moreData: 123
    }
});
```

### Dispatching Custom Events

```javascript
window.dispatchEvent(myEvent);
// or on a specific element
element.dispatchEvent(myEvent);
```

### Listening for Custom Events

```javascript
window.addEventListener("my-custom-event", (event) => {
    console.log(event.detail.customData);
});
```

---

## Event Subscription

### For Engine Events (All Gameplay Events)

```javascript
// Subscribe to an event
engine.on("EventName", callbackFunction);

// Subscribe with context (for class methods)
engine.on("EventName", callbackFunction, this);

// Store handle for unsubscription
const handle = engine.on("EventName", callback);
handle.clear(); // Unsubscribe later

// Direct unsubscription
engine.off("EventName", callbackFunction, context);
```

**Example:**

```javascript
function onTurnBegin(turn) {
    console.log(`Turn ${turn} started`);
}

// Subscribe
engine.on("TurnBegin", onTurnBegin);

// Later, unsubscribe
engine.off("TurnBegin", onTurnBegin);
```

### For Engine Calls

```javascript
// Call engine functions (returns Promise)
engine.call("FunctionName", parameters).then((result) => {
    // Handle result
});
```

### For window/DOM Events

```javascript
// Subscribe
window.addEventListener("event-name", handler);

// Unsubscribe
window.removeEventListener("event-name", handler);

// Dispatch
window.dispatchEvent(new CustomEvent("event-name", options));
```

---

## Event Categories Summary

| Category | Subscription Method |
|----------|-------------------|
| All Gameplay Events | `engine.on()` |
| DOM Events | `addEventListener()` |
| Custom Events | `dispatchEvent()` |

---

## Best Practices

### 1. Always Clean Up

Remove event listeners when your UI component is destroyed:

```javascript
class MyPanel {
    constructor() {
        this.onTurnBegin = this.onTurnBegin.bind(this);
        this.turnBeginHandle = engine.on("TurnBegin", this.onTurnBegin);
    }

    destroy() {
        this.turnBeginHandle.clear();
    }

    onTurnBegin(turn) {
        // Handle turn begin
    }
}
```

### 2. Bind Context

Always bind `this` when using class methods as callbacks:

```javascript
// Option 1: Use bind
this.handleClick = this.handleClick.bind(this);
engine.on("CitySelectionChanged", this.handleClick);

// Option 2: Pass context as third argument
engine.on("CitySelectionChanged", this.handleClick, this);
```

### 3. Check Event Data

Always validate event data before using it:

```javascript
engine.on("CitySelectionChanged", (playerID, cityID) => {
    if (cityID === -1) return; // No city selected
    // Process selection
});
```

### 4. Use Event Delegation

For many similar elements, use event delegation:

```javascript
container.addEventListener("click", (event) => {
    if (event.target.matches(".city-button")) {
        // Handle city button click
    }
});
```

---

## See Also

- [JavaScript Overview](Overview.md) - Complete JavaScript API reference
- [UI Modding](../technical-reference/UI-Modding.md) - UI modification guide
- [Creating UI Extensions](../tutorials/Creating-UI-Extensions.md) - UI tutorial
- [Cross-Reference Index](../Index.md) - Quick lookup for all types
