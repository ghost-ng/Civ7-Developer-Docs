# JavaScript Events Reference

This document lists all JavaScript events that fire during Civilization VII gameplay. Events are the primary mechanism for scripts to respond to game state changes.

## Quick Navigation

- [Engine Events](#engine-events) - Core engine-level events
- [Gameplay Events](#gameplay-events) - Turn, city, unit, tech events
- [DOM/Window Events](#domwindow-events) - UI and browser events
- [Custom Events](#custom-events) - Creating your own events
- [Event Subscription](#event-subscription) - How to subscribe to events

---

## Engine Events

Engine events are triggered by the core game engine and are subscribed using `engine.on()`.

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

#### SetAgeInitializationParameters

Called by scripts to set age initialization parameters.

```javascript
engine.call("SetAgeInitializationParameters", {
    // initialization parameters
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

---

## Gameplay Events

Gameplay events are accessed through the `Events` object and subscribed using `.add()`.

### Turn Events

#### Events.TurnBegin

Fired at the start of each new turn.

```javascript
Events.TurnBegin.add((turn) => {
    console.log(`Turn ${turn} has begun`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `turn` | number | Current turn number |

#### Events.TurnEnd

Fired at the end of each turn.

```javascript
Events.TurnEnd.add((turn) => {
    console.log(`Turn ${turn} has ended`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `turn` | number | Ending turn number |

### City Events

#### Events.CitySelectionChanged

Fired when the player selects or deselects a city.

```javascript
Events.CitySelectionChanged.add((playerID, cityID) => {
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

#### Events.CityProductionCompleted

Fired when a city completes production of an item.

```javascript
Events.CityProductionCompleted.add((playerID, cityID, itemType) => {
    console.log(`City ${cityID} completed ${itemType}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Owner player ID |
| `cityID` | number | City ID |
| `itemType` | string | Completed item type |

#### Events.CityPopulationChanged

Fired when a city's population changes.

```javascript
Events.CityPopulationChanged.add((data) => {
    // Handle population change
});
```

#### Events.PlayerYieldChanged

Fired when a player's yield production changes.

```javascript
Events.PlayerYieldChanged.add((data) => {
    // Handle yield change
});
```

### Unit Events

#### Events.UnitSelectionChanged

Fired when the player selects or deselects a unit.

```javascript
Events.UnitSelectionChanged.add((playerID, unitID) => {
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

#### Events.UnitMoved

Fired when a unit completes movement.

```javascript
Events.UnitMoved.add((playerID, unitID) => {
    console.log(`Unit ${unitID} has moved`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Owner player ID |
| `unitID` | number | Moving unit ID |

#### Events.UnitCreated

Fired when a new unit is created.

```javascript
Events.UnitCreated.add((data) => {
    // Handle unit creation
});
```

#### Events.UnitKilled

Fired when a unit is destroyed.

```javascript
Events.UnitKilled.add((data) => {
    // Handle unit death
});
```

### Technology & Civic Events

#### Events.TechnologyResearched

Fired when a player completes a technology.

```javascript
Events.TechnologyResearched.add((playerID, techType) => {
    console.log(`Player ${playerID} researched ${techType}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Researching player ID |
| `techType` | string | Technology type completed |

#### Events.CivicCompleted

Fired when a player completes a civic.

```javascript
Events.CivicCompleted.add((playerID, civicType) => {
    console.log(`Player ${playerID} completed civic ${civicType}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `playerID` | number | Player ID |
| `civicType` | string | Civic type completed |

### Diplomacy Events

#### Events.WarDeclared

Fired when war is declared between players.

```javascript
Events.WarDeclared.add((attackerID, defenderID) => {
    console.log(`Player ${attackerID} declared war on ${defenderID}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `attackerID` | number | Attacking player ID |
| `defenderID` | number | Defending player ID |

#### Events.PeaceMade

Fired when peace is established between players.

```javascript
Events.PeaceMade.add((player1ID, player2ID) => {
    console.log(`Peace between ${player1ID} and ${player2ID}`);
});
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `player1ID` | number | First player ID |
| `player2ID` | number | Second player ID |

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

### For Events.* (Gameplay Events)

```javascript
// Subscribe to an event
Events.EventName.add(callbackFunction);

// Unsubscribe from an event
Events.EventName.remove(callbackFunction);
```

**Example:**

```javascript
function onTurnBegin(turn) {
    console.log(`Turn ${turn} started`);
}

// Subscribe
Events.TurnBegin.add(onTurnBegin);

// Later, unsubscribe
Events.TurnBegin.remove(onTurnBegin);
```

### For engine Events

```javascript
// Subscribe to engine event
engine.on("EventName", callbackFunction);

// Call engine functions
engine.call("FunctionName", parameters);
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

| Category | Count | Subscription Method |
|----------|-------|-------------------|
| Engine Events | 6 | `engine.on()` |
| Gameplay Events | 14+ | `Events.*.add()` |
| DOM Events | 5+ | `addEventListener()` |
| Custom Events | Unlimited | `dispatchEvent()` |

---

## Best Practices

### 1. Always Clean Up

Remove event listeners when your UI component is destroyed:

```javascript
class MyPanel {
    constructor() {
        this.onTurnBegin = this.onTurnBegin.bind(this);
        Events.TurnBegin.add(this.onTurnBegin);
    }

    destroy() {
        Events.TurnBegin.remove(this.onTurnBegin);
    }

    onTurnBegin(turn) {
        // Handle turn begin
    }
}
```

### 2. Bind Context

Always bind `this` when using class methods as callbacks:

```javascript
this.handleClick = this.handleClick.bind(this);
button.addEventListener("click", this.handleClick);
```

### 3. Check Event Data

Always validate event data before using it:

```javascript
Events.CitySelectionChanged.add((playerID, cityID) => {
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
