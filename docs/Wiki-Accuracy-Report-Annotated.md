# Wiki Accuracy Report - Annotated with Sources

**Review Date:** 2026-02-01
**Purpose:** Document WHY each correction is needed with specific file sources

---

## Critical Error #1: JavaScript Event Pattern

### Wiki Claims (Events.md lines 102-114, 143-148, 420-428)

```javascript
// Wiki shows this pattern:
Events.TurnBegin.add((turn) => {
    console.log(`Turn ${turn} has begun`);
});
Events.EventName.remove(callbackFunction);
```

### Why This Is Wrong

**Evidence 1 - Engine API Definition:**
- **File:** `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\Base\modules\core\ui\cohtml.js`
- **Lines 83-90:** Define `Emitter.prototype.on = function (name, callback, context)`
- **Lines 101-124:** Define `Emitter.prototype.off = function (name, handler, context)`
- **Conclusion:** The engine provides `engine.on()` and `engine.off()`, not `Events.*.add()`

**Evidence 2 - Actual Game Usage (30+ occurrences found):**
```
engine.on("TurnBegin", ...) - automation-base-play-game.chunk.js
engine.on("UnitMovementPointsChanged", ...) - tutorial-items-antiquity.js:76
engine.on("RequestMapInitData", ...) - continents.js:197
engine.on("GenerateMap", ...) - continents.js:198
engine.on("InputAction", ...) - sandbox-navigation.chunk.js:7
```

**Evidence 3 - `Events.*.add()` search:**
- **Search performed:** `Events\.[A-Za-z]+\.add\(` across all .js files
- **Result:** 0 matches found
- **Conclusion:** The `Events.*.add()` pattern does not exist in the game

### Correct Pattern

```javascript
// Subscribe
engine.on("TurnBegin", (turn) => {
    console.log(`Turn ${turn} has begun`);
});

// Unsubscribe
engine.off("TurnBegin", callback, context);
// OR use the handle:
const handle = engine.on("TurnBegin", callback);
handle.clear();
```

---

## Critical Error #2: Localization XML Format

### Wiki Claims (Localization.md lines 28-40)

```xml
<LocalizedText>
    <Row Tag="LOC_MY_TEXT_KEY" Language="en_US">
        <Text>This is my English text.</Text>
    </Row>
</LocalizedText>
```

### Why This Is Wrong

**Evidence - Actual Game Localization File:**
- **File:** `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\Base\modules\core\text\en_us\CoreText.xml`
- **Lines 1-21:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
	<EnglishText>
		<!-- Internal Modding Errors -->
		<Row Tag="LOC_MODDING_ERROR_UNKNOWN">
			<Text>An unknown error occurred while trying to start the game.</Text>
		</Row>
		<Row Tag="LOC_MODDING_ERROR_CONTENT">
			<Text>One or more errors occurred while trying to configure modding content.</Text>
		</Row>
	</EnglishText>
</Database>
```

### Key Differences

| Aspect | Wiki Claims | Actual |
|--------|-------------|--------|
| Root element | `<LocalizedText>` | `<Database><EnglishText>` |
| Language attribute | `Language="en_US"` on each Row | No attribute; table name `EnglishText` implies language |
| Wrapper | None | `<Database>` wraps all content |

### Correct Pattern

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <EnglishText>
        <Row Tag="LOC_MY_TEXT_KEY">
            <Text>This is my English text.</Text>
        </Row>
    </EnglishText>
</Database>
```

---

## Critical Error #3: UI Uses HTML, Not BLP

### Wiki Claims (UI-Modding.md lines 11, 43-71)

Claims UI uses `.blp` files with XML-like syntax:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context Name="MyCustomPanel">
    <Container ID="MainContainer" Size="400,300" Anchor="C,C">
        <Grid ID="Background" Size="parent" Style="Panel_Background"/>
        <Label ID="TitleLabel" String="LOC_MY_PANEL_TITLE" Style="HeaderText"/>
    </Container>
</Context>
```

### Why This Is Wrong

**Evidence - Actual Game UI File:**
- **File:** `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\Base\modules\core\ui\shell\root-shell.html`
- **Lines 1-61:** Standard HTML5 file

```html
<!doctype html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Root - Shell (Menus)</title>
		<script src="fs://game/core/ui/cohtml.js"></script>
		<script type="module" crossorigin src="fs://game/core/ui/shell/root-shell.js"></script>
		<link rel="stylesheet" crossorigin href="fs://game/core/ui/shell/root-shell.css">
	</head>
	<body class="font-body text-base text-accent-2">
		<div id="roots" class="fullscreen"></div>
		<nav-tray></nav-tray>
		<div id="tooltips" class="absolute pointer-events-none"></div>
	</body>
</html>
```

### Key Differences

| Aspect | Wiki Claims | Actual |
|--------|-------------|--------|
| File format | `.blp` files | Standard `.html` files |
| Syntax | Custom XML (`<Container>`, `<Label>`) | Standard HTML5 (`<div>`, `<body>`) |
| Components | `Size="400,300" Anchor="C,C"` | CSS classes and custom web components (`<nav-tray>`) |
| Engine | Not specified | Coherent Labs Cohtml (see cohtml.js line 6) |

### Correct Information

- Civ VII uses **Coherent Labs Cohtml** for UI rendering
- UI is built with standard **HTML5/CSS/JavaScript**
- Custom functionality via **web components** (e.g., `<nav-tray>`, `<panel-yield-banner>`)
- No `.blp` files exist for UI - this format does not exist in the game

---

## Critical Error #4: MapGenScripts Action Does Not Exist

### Wiki Claims (Creating-a-Map-Generator.md lines 64-70)

```xml
<ActionGroup id="map-scripts" scope="game" criteria="always">
    <Actions>
        <MapGenScripts>
            <Item>maps/inland-sea.js</Item>
        </MapGenScripts>
    </Actions>
</ActionGroup>
```

### Why This Is Wrong

**Evidence 1 - Search for MapGenScripts:**
- **Search performed:** `MapGenScripts` across all files in game modules
- **Result:** 0 matches found
- **Conclusion:** This action type does not exist

**Evidence 2 - How Maps Are Actually Registered:**
- **File:** `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\Base\modules\base-standard\config\config.xml`
- **Lines 197-202:**

```xml
<Maps>
    <Row File="{base-standard}maps/continents-voronoi.js" Name="LOC_MAP_CONTINENTS_VORONOI_NAME" Description="LOC_MAP_CONTINENTS_VORONOI_DESCRIPTION" SortIndex="0" />
    <Row File="{base-standard}maps/pangaea-voronoi.js" Name="LOC_MAP_PANGAEA_VORONOI_NAME" Description="LOC_MAP_PANGAEA_VORONOI_DESCRIPTION" SortIndex="1" />
    <Row File="{base-standard}maps/continents.js" Name="LOC_MAP_CONTINENTS_NAME" Description="LOC_MAP_CONTINENTS_DESCRIPTION" SortIndex="20"/>
</Maps>
```

**Evidence 3 - How Map Scripts Receive Events:**
- **File:** `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\Base\modules\base-standard\maps\continents.js`
- **Lines 197-198:**

```javascript
engine.on("RequestMapInitData", requestMapData);
engine.on("GenerateMap", generateMap);
```

### Correct Pattern

Maps are registered via the `Maps` database table (not a modinfo action), and scripts subscribe to generation events via `engine.on()`.

---

## High Priority Error #5: Unit Cost Tables

### Wiki Claims (Creating-a-Unit.md)

Documents three separate tables:
- `Unit_ProductionCosts`
- `Unit_PurchaseCosts`
- `Unit_MaintenanceCosts`

### Why This Is Wrong

**Evidence - Actual Game File:**
- **File:** `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\Base\modules\age-antiquity\data\units.xml`
- **Lines 267-300:** Show a single `Unit_Costs` table

```xml
<Unit_Costs>
    <Row UnitType="UNIT_WARRIOR" YieldType="YIELD_PRODUCTION" Cost="30"/>
    <Row UnitType="UNIT_SCOUT" YieldType="YIELD_PRODUCTION" Cost="25"/>
    <!-- etc -->
</Unit_Costs>
```

### Correct Pattern

Single `Unit_Costs` table with columns: `UnitType`, `YieldType`, `Cost`

---

## High Priority Error #6: Starting Bias Tables

### Wiki Claims (Creating-a-Civilization.md)

Documents a single `StartBiases` table with `Tier` column (1-5 scale).

### Why This Is Wrong

**Evidence - Actual Game File:**
- **File:** `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\Base\modules\age-antiquity\data\civilizations.xml`
- **Lines 540-565:** Show multiple specialized tables

```xml
<StartBiasBiomes>
    <Row CivilizationType="CIVILIZATION_AKSUM" BiomeType="BIOME_DESERT" Score="3"/>
</StartBiasBiomes>
<StartBiasTerrains>
    <Row CivilizationType="CIVILIZATION_AKSUM" TerrainType="TERRAIN_GRASS" Score="2"/>
</StartBiasTerrains>
<StartBiasRivers>
    <Row CivilizationType="CIVILIZATION_AKSUM" Score="5"/>
</StartBiasRivers>
```

### Key Differences

| Aspect | Wiki Claims | Actual |
|--------|-------------|--------|
| Structure | Single `StartBiases` table | 6+ specialized tables |
| Column name | `Tier` | `Score` |
| Tables | 1 | `StartBiasBiomes`, `StartBiasTerrains`, `StartBiasFeatureClasses`, `StartBiasRivers`, `StartBiasAdjacentToCoasts`, `StartBiasResources` |

---

## Summary of Sources

| Error | Primary Source File | Line Numbers |
|-------|---------------------|--------------|
| Event pattern | `core\ui\cohtml.js` | 83-90, 101-124 |
| Localization format | `core\text\en_us\CoreText.xml` | 1-21 |
| UI format | `core\ui\shell\root-shell.html` | 1-61 |
| Map registration | `base-standard\config\config.xml` | 197-202 |
| Unit costs | `age-antiquity\data\units.xml` | 267-300 |
| Start biases | `age-antiquity\data\civilizations.xml` | 540-565 |

---

## Verification Commands Used

```bash
# Event pattern verification
grep -r "engine\.on\(" *.js           # 30+ matches
grep -r "Events\.[A-Za-z]+\.add\(" *.js  # 0 matches

# MapGenScripts verification
grep -r "MapGenScripts" *             # 0 matches

# Maps table verification
grep -r "<Maps>" *.xml                # Found in config.xml
```

---

## Recommendation

These corrections are necessary because:

1. **Event pattern** - Modders using `Events.*.add()` will get JavaScript errors; their event handling won't work at all
2. **Localization format** - Mods using `<LocalizedText>` won't load any translated strings
3. **UI format** - Modders creating `.blp` files are wasting time; the format doesn't exist
4. **Map registration** - `<MapGenScripts>` action doesn't exist; maps won't be registered
5. **Unit costs** - Using three separate tables will cause database errors
6. **Start biases** - Using `Tier` column will cause database errors; civs won't have starting preferences
