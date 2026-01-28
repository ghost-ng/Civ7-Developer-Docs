# JavaScript Scripting Overview

Civilization VII uses JavaScript (compiled from TypeScript) for scripting, replacing Lua from previous Civilization games. Scripts handle map generation, age transitions, and UI functionality.

## Script Types

### Map Generation Scripts
Located in `maps/` directories, these scripts generate terrain, place resources, and set up the map.

**Loaded via modinfo:**
```xml
<MapGenScripts>
    <Item>maps/my-map-script.js</Item>
</MapGenScripts>
```

### Scenario/Gameplay Scripts
Handle game events during age transitions and gameplay scenarios.

**Loaded via modinfo:**
```xml
<ScenarioScripts>
    <Item>scripts/my-gameplay-script.js</Item>
</ScenarioScripts>
```

### UI Scripts
Handle user interface, panels, and visual elements.

**Loaded via modinfo:**
```xml
<UIScripts>
    <Item>ui/my-panel.js</Item>
</UIScripts>
```

## Global Objects

Civ7 exposes many global objects for gameplay interaction.

### Core Game Objects

| Object | Description |
|--------|-------------|
| `Game` | Current game state, rules, turn info |
| `Players` | Collection of all players |
| `Units` | Unit management |
| `Cities` | City management |
| `Armies` | Army/commander management |
| `GameInfo` | Database table access |
| `GameplayMap` | Map data and queries |
| `GameContext` | Context info (local player, etc.) |

### Map Generation Objects

| Object | Description |
|--------|-------------|
| `TerrainBuilder` | Set terrain, features, elevations |
| `ResourceBuilder` | Place and manage resources |
| `MapCities` | City placement on map |

### Configuration Objects

| Object | Description |
|--------|-------------|
| `Configuration` | Game/map configuration values |
| `Database` | Direct database operations |

### UI Objects

| Object | Description |
|--------|-------------|
| `UI` | UI control and player interface |
| `Locale` | Localization/text composition |
| `engine` | Engine event system |
| `ContextManager` | UI context/screen management |

## Engine Event System

The `engine` object handles communication between scripts and the game engine.

### Subscribing to Events

```javascript
engine.on("EventName", callbackFunction);
```

**Common Events:**
- `RequestAgeInitializationParameters` - Age transition starting
- `GenerateAgeTransition` - Generate age transition content
- `UserRequestClose` - User requested game close
- `NetworkDisconnected` / `NetworkReconnected` - Network status

### Calling Engine Functions

```javascript
engine.call("FunctionName", parameters);
```

### Example: Age Transition Script

```javascript
function requestInitializationParameters(initParams) {
    console.log("Players: ", initParams.numMajorPlayers);
    console.log("Old Age: ", initParams.outgoingAge);
    console.log("New Age: ", initParams.incomingAge);
    engine.call("SetAgeInitializationParameters", initParams);
}

function generateTransition() {
    console.log("Generating age transition!");
    // ... transition logic
}

engine.on("RequestAgeInitializationParameters", requestInitializationParameters);
engine.on("GenerateAgeTransition", generateTransition);
```

## GameInfo Database Access

Access database tables directly through `GameInfo`:

```javascript
// Iterate all resources
for (const resource of GameInfo.Resources) {
    console.log(resource.ResourceType);
}

// Lookup by type
const unitInfo = GameInfo.Units.lookup("UNIT_WARRIOR");

// Lookup by hash
const hash = Database.makeHash("UNIT_WARRIOR");
const unitInfo = GameInfo.Units.lookup(hash);

// Get index
const resourceInfo = GameInfo.Resources.lookup(resourceType);
const index = resourceInfo.$index;
const typeHash = resourceInfo.$hash;
```

### Common GameInfo Tables

- `GameInfo.Units`
- `GameInfo.Resources`
- `GameInfo.Features`
- `GameInfo.Feature_NaturalWonders`
- `GameInfo.Civilizations`
- `GameInfo.Ages`
- `GameInfo.Resource_Distribution`
- `GameInfo.Unit_ShadowReplacements`
- `GameInfo.MapIslandBehavior`

## Players API

### Getting Players

```javascript
// Get specific player
const player = Players.get(playerId);

// Get all ever-alive players
const playerList = Players.getEverAlive();

// Get local player
const localPlayer = Players.get(GameContext.localObserverID);
```

### Player Properties

```javascript
const player = Players.get(playerId);

// Player info
player.id
player.civilizationType
player.isAlive

// Player components
player.Cities       // City management
player.Units        // Unit management
player.Treasury     // Gold balance
player.DiplomacyTreasury  // Influence balance
player.AdvancedStart      // Age transition cards

// Custom properties
player.getProperty("PROPERTY_NAME");
player.setProperty("PROPERTY_NAME", value);
```

### Player Cities

```javascript
const cities = player.Cities;

// Get city IDs
const cityIds = cities.getCityIds();

// Get cities collection
const cityList = cities.getCities();

// Get capital
const capital = cities.getCapital();

// Find closest city
const nearest = cities.findClosest(location);
```

### Player Units

```javascript
const units = player.Units;

// Get all unit IDs
const unitIds = units.getUnitIds();

// Get buildable unit type
const unitType = units.getBuildUnit("UNIT_WARRIOR");

// Unit shadows (for age transitions)
const shadows = units.getUnitShadows();
```

## Units API

```javascript
// Get unit by ID
const unit = Units.get(unitId);

// Create a unit
const result = Units.create(playerId, {
    Type: unitType,
    Location: location,
    Validate: true  // optional
});

if (result.Success && result.ID) {
    console.log("Created unit:", result.ID);
}

// Move unit
Units.setLocation(unitId, newLocation);
```

### Unit Properties

```javascript
const unit = Units.get(unitId);

unit.name
unit.location
unit.armyId
unit.isCommanderUnit
unit.isArmyCommander
unit.isFleetCommander
unit.Experience  // Experience component

// Custom properties
unit.setProperty("PROPERTY_NAME", value);
```

## Cities API

```javascript
// Get city by ID
const city = Cities.get(cityId);

city.id
city.name
city.location
city.population
city.isCapital
city.isTown
city.Trade  // Trade component

// Check trade connection
city.Trade?.isConnectedToOwnersCapitalByLand();

// Change build queue
city.changeHasBuildQueue(-1);  // Regress to town
```

## GameplayMap API

```javascript
// Map dimensions
const width = GameplayMap.getGridWidth();
const height = GameplayMap.getGridHeight();

// Plot queries
const terrain = GameplayMap.getTerrainType(x, y);
const resource = GameplayMap.getResourceType(x, y);
const elevation = GameplayMap.getElevation(x, y);
const hemisphere = GameplayMap.getHemisphere(x);
const primaryHemisphere = GameplayMap.getPrimaryHemisphere();
const landmassRegionId = GameplayMap.getLandmassRegionId(x, y);

// Index conversions
const index = GameplayMap.getIndexFromXY(x, y);
const location = GameplayMap.getLocationFromIndex(index);

// Random seed
const seed = GameplayMap.getRandomSeed();
```

## TerrainBuilder API

For map generation scripts:

```javascript
// Set terrain features
TerrainBuilder.setFeatureType(x, y, featureParam);
TerrainBuilder.canHaveFeatureParam(x, y, featureParam);

// Generate maps
TerrainBuilder.generatePoissonMap(seed, avgDistance, smoothing);

// Random numbers
const roll = TerrainBuilder.getRandomNumber(max, "description");

// Water data
TerrainBuilder.storeWaterData();
```

## ResourceBuilder API

```javascript
// Set resources
ResourceBuilder.setResourceType(x, y, resourceType);
ResourceBuilder.canHaveResource(x, y, resourceType, strict);

// Query resources
const counts = ResourceBuilder.getResourceCounts(playerId);  // -1 for all
const generated = ResourceBuilder.getGeneratedMapResources();

// Age validation
ResourceBuilder.isResourceValidForAge(resource, age);
ResourceBuilder.isResourceRequiredForAge(resource, age);
ResourceBuilder.isResourceClassRequiredForLegacyPath(resource);

// Landmass assignment
ResourceBuilder.getResourceLandmass(resourceIdx);

// Cutting resources
ResourceBuilder.getBestMapResourceCuts(newResources, numToCut);
```

## Database Utilities

```javascript
// Create hash from string
const hash = Database.makeHash("TYPE_STRING");

// Compare with hash
if (Game.age == Database.makeHash("AGE_EXPLORATION")) {
    // ...
}
```

## Configuration

```javascript
// Game configuration
const setting = Configuration.getGameValue("SettingName");

// Map configuration
const mapValue = Configuration.getMapValue("Name");
const requests = Configuration.getMapValue("RequestedNaturalWonders");

// User configuration
const userConfig = Configuration.getUser();
userConfig.firstTimeTutorialEnabled;
userConfig.setFirstTimeTutorialEnabled(false);
userConfig.saveCheckpoint();
```

## Locale/Localization

```javascript
// Compose localized text
const text = Locale.compose(locKey);  // "LOC_UNIT_WARRIOR_NAME" -> "Warrior"
```

## Logging

```javascript
console.log("Message");
console.warn("Warning message");
console.error("Error message");
```

Logs appear in the game's console/log files.

## Module System

Scripts use ES6 module imports:

```javascript
import { functionName } from '../path/to/module.js';
import { shuffle } from './map-utilities.js';
```

Export functions for use by other scripts:

```javascript
export { myFunction, myVariable };
```

## UI Custom Events

UI scripts use custom DOM events:

```javascript
// Creating custom events
class MyEvent extends CustomEvent {
    constructor(data) {
        super("my-event-name", {
            bubbles: true,
            cancelable: true,
            detail: { data }
        });
    }
}

// Listening for events
window.addEventListener("engine-input", handleInput);
window.dispatchEvent(new CustomEvent("user-interface-loaded-and-ready"));
```

## Interface Modes

```javascript
import { InterfaceMode } from '../interface-modes/interface-modes.js';

// Switch interface mode
InterfaceMode.switchTo("INTERFACEMODE_DEFAULT");
InterfaceMode.switchTo("INTERFACEMODE_PAUSE_MENU");
InterfaceMode.switchTo("INTERFACEMODE_SCREENSHOT");

// Check current mode
InterfaceMode.isInInterfaceMode("INTERFACEMODE_PAUSE_MENU");

// Register custom mode handler
class MyInterfaceMode {
    transitionTo(oldMode, newMode, context) {
        // Called when switching to this mode
    }
    transitionFrom(oldMode, newMode) {
        // Called when leaving this mode
    }
    allowsHotKeys() {
        return true;
    }
}
InterfaceMode.addHandler("INTERFACEMODE_MY_MODE", new MyInterfaceMode());
```

## See Also

- [UI Scripting](UI-Scripting.md)
- [Gameplay Scripting](Gameplay-Scripting.md)
- [modinfo Files](../Modinfo-Files.md)
- [Map Scripts Reference](Map-Scripts.md)
