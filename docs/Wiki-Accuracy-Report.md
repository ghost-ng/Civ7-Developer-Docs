# Civilization VII Modding Wiki - Accuracy Report

**Review Date:** 2026-02-01
**Review Method:** Parallel agent analysis cross-referencing wiki documentation against actual game files
**Game Files Location:** `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\Base\modules\`

---

## Executive Summary

Five specialized agents reviewed the entire wiki documentation against actual Civ VII game files. The documentation is **fundamentally accurate** in its core concepts but contains **several critical errors** that would prevent code from working.

| Category | Verified Correct | Errors Found | Accuracy |
|----------|------------------|--------------|----------|
| Database Documentation | 25+ items | 6 errors | ~85% |
| Modifiers Documentation | 20+ items | 2 errors | ~90% |
| JavaScript API | 15+ items | 5 errors | ~75% |
| Tutorials | 15+ items | 6 errors | ~70% |
| Technical Reference | 25+ items | 6 errors | ~80% |

---

## CRITICAL ERRORS (Must Fix Immediately)

### 1. JavaScript Event Subscription Pattern is WRONG ❌

**Location:** [docs/javascript/Events.md](docs/javascript/Events.md)

**Wiki Documents:**
```javascript
Events.TurnBegin.add((turn) => {
    console.log(`Turn ${turn} has begun`);
});
Events.EventName.remove(callbackFunction);
```

**Actual Pattern:**
```javascript
engine.on("TurnBegin", (turn) => {
    console.log(`Turn ${turn} has begun`);
});
engine.off("TurnBegin", callback, context);
```

**Impact:** ALL event subscription examples in the wiki will fail. The `Events.*.add()` API does not exist.

---

### 2. Localization XML Format is WRONG ❌

**Location:** [docs/technical-reference/Localization.md](docs/technical-reference/Localization.md)

**Wiki Documents:**
```xml
<LocalizedText>
    <Row Tag="LOC_MY_TEXT" Language="en_US">
        <Text>My text</Text>
    </Row>
</LocalizedText>
```

**Actual Format:**
```xml
<Database>
    <EnglishText>
        <Row Tag="LOC_MY_TEXT">
            <Text>My text</Text>
        </Row>
    </EnglishText>
</Database>
```

**Impact:** Mods using the documented format will fail to load localization.

---

### 3. UI Modding Uses HTML, Not BLP ❌

**Location:** [docs/technical-reference/UI-Modding.md](docs/technical-reference/UI-Modding.md)

**Wiki Documents:** BLP files with XML-like syntax
```xml
<Context Name="MyPanel">
    <Container ID="Main" Size="400,300">
        <Label ID="Title" String="LOC_TITLE"/>
    </Container>
</Context>
```

**Actual Format:** Standard HTML5 with custom web components
```html
<!doctype html>
<html>
<head>
    <script src="fs://game/core/ui/cohtml.js"></script>
</head>
<body>
    <fxs-slot name="top-center">
        <panel-yield-banner></panel-yield-banner>
    </fxs-slot>
</body>
</html>
```

**Impact:** Entire documented UI approach is fictional. Game uses Coherent Labs Cohtml for HTML/CSS/JS rendering.

---

### 4. Map Script Registration is WRONG ❌

**Location:** [docs/tutorials/Creating-a-Map-Generator.md](docs/tutorials/Creating-a-Map-Generator.md)

**Wiki Documents:**
```xml
<MapGenScripts>
    <Item>maps/my-map-script.js</Item>
</MapGenScripts>
```

**Actual Pattern:** Maps are registered via database table in config.xml:
```xml
<Maps>
    <Row File="{base-standard}maps/continents.js"
         Name="LOC_MAP_CONTINENTS_NAME"
         Description="LOC_MAP_CONTINENTS_DESCRIPTION"
         SortIndex="20"/>
</Maps>
```

And scripts use event handlers:
```javascript
engine.on("RequestMapInitData", requestMapData);
engine.on("GenerateMap", generateMap);
```

---

## HIGH PRIORITY ERRORS

### 5. Unit Cost Tables (Creating-a-Unit.md)

| Wiki Claims | Actual |
|-------------|--------|
| `Unit_ProductionCosts`, `Unit_PurchaseCosts`, `Unit_MaintenanceCosts` | Single `Unit_Costs` table with columns: `UnitType`, `YieldType`, `Cost` |

### 6. Starting Bias Tables (Creating-a-Civilization.md)

| Wiki Claims | Actual |
|-------------|--------|
| Single `StartBiases` table with `Tier` column | Multiple tables: `StartBiasBiomes`, `StartBiasTerrains`, `StartBiasFeatureClasses`, `StartBiasRivers`, etc. with `Score` column |

### 7. GameContext Property Name (Overview.md)

| Wiki Claims | Actual |
|-------------|--------|
| `GameContext.localObserverID` | `GameContext.localPlayerID` |

### 8. Icon Definition Structure (Art-Assets.md)

| Wiki Claims | Actual |
|-------------|--------|
| `IconTextureAtlases` with atlas metadata | `IconDefinitions` with simple `ID`/`Path` format |

### 9. Warrior Combat Stat (Units.md)

| Wiki Claims | Actual |
|-------------|--------|
| `Combat="25"` | `Combat="20"` |

### 10. Feature_NaturalWonders Columns (Map-Terrain.md)

| Wiki Claims | Actual |
|-------------|--------|
| `Size` column | `Tiles` column |
| `Direction` column | Does not exist; `NoRiver` exists instead |

---

## MISSING DOCUMENTATION (High Impact)

### Requirement Types Gap
- **Wiki documents:** ~20 requirement types
- **Game contains:** **236 requirement types**
- **Gap:** ~216 undocumented requirement types including combat, commander, city, and game state requirements

### Undocumented Tables
| Table | Purpose |
|-------|---------|
| `Unit_Costs` | Single table for all unit costs |
| `UnitUpgrades` | Unit upgrade paths |
| `UnitReplaces` | Civ-unique unit replacements |
| `CityNames` | Per-civilization city names (not `CivilizationCityNames`) |
| `StartBias*` (6 tables) | Specialized starting bias tables |
| `Feature_ValidTerrains` | Feature placement rules |
| `Feature_ValidBiomes` | Feature biome rules |

### Undocumented Modinfo Attributes
| Attribute | Purpose |
|-----------|---------|
| `locale="de_DE"` on Item elements | Load localized files |
| `platform="Switch"` | Platform-specific content |
| `module="false"` | Non-module script loading |
| `LoadOrder` in Properties | Control load ordering |
| `inverse="1"` on criteria | Inverse age matching |

### Undocumented JavaScript APIs
| API | Purpose |
|-----|---------|
| `FractalBuilder` | Procedural terrain generation |
| `AreaBuilder` | Landmass calculations |
| `MapCities`, `MapRivers` | Map-level operations |
| `engine.on/off` | Event subscription (replaces `Events.*`) |

---

## VERIFIED CORRECT ✓

### Core Concepts
- ✓ JavaScript instead of Lua (confirmed)
- ✓ HTML/CSS/JavaScript for UI (confirmed, but not BLP)
- ✓ Two database scopes: Shell and Game
- ✓ Age-based content system
- ✓ GameEffects XML format structure
- ✓ Modifier system Owner/Subject/Effect pattern

### Modinfo Structure
- ✓ XML namespace `xmlns="ModInfo"`
- ✓ Properties elements (Name, Description, Authors, etc.)
- ✓ ActionCriteria patterns (AgeInUse, AlwaysMet, etc.)
- ✓ Scope values (game, shell)
- ✓ Default module dependencies

### Database Tables
- ✓ Types table with Kind column
- ✓ Units table structure (most columns)
- ✓ Unit_Stats table
- ✓ Civilizations table
- ✓ Leaders table
- ✓ Traits, TraitModifiers, CivilizationTraits
- ✓ Modifiers, ModifierArguments, DynamicModifiers
- ✓ RequirementSets (TEST_ALL, TEST_ANY)

### JavaScript APIs
- ✓ `engine.call()` returns Promise
- ✓ `GameInfo.*.lookup()` pattern
- ✓ `GameplayMap` methods (getTerrainType, getResourceType, etc.)
- ✓ `TerrainBuilder` methods
- ✓ `ResourceBuilder` methods
- ✓ `Players.get()`, `Units.get()`, `Cities.get()`
- ✓ `InterfaceMode.switchTo()`, `addHandler()`

---

## RECOMMENDATIONS BY PRIORITY

### Priority 1: Critical Rewrites
1. **Events.md** - Replace all `Events.*.add()` with `engine.on()` pattern
2. **Localization.md** - Use `<EnglishText>` table structure
3. **UI-Modding.md** - Document HTML/web components, not BLP
4. **Creating-a-Map-Generator.md** - Show Maps database table registration

### Priority 2: Table/Column Fixes
5. Fix `Unit_Costs` table documentation (single table, not three)
6. Add all `StartBias*` specialized tables
7. Rename `CivilizationCityNames` to `CityNames`
8. Fix `Feature_NaturalWonders` column names
9. Correct UNIT_WARRIOR combat stat to 20

### Priority 3: Expand Coverage
10. Document 216 missing requirement types (organize by category)
11. Add missing tables: `UnitUpgrades`, `UnitReplaces`, `Feature_Valid*`
12. Document modinfo attributes: `locale`, `platform`, `LoadOrder`, `inverse`
13. Add JavaScript API documentation for `engine.on/off`, `FractalBuilder`, etc.

### Priority 4: Enhancements
14. Update effect type count from 376 to ~257
15. Document `ScaleByGameAge` dynamic argument scaling
16. Add ModifierCategories table documentation
17. Document Coherent Cohtml UI framework context

---

## Source Files Examined

| Module | Files | Purpose |
|--------|-------|---------|
| core | `core.modinfo`, `cohtml.js`, `root-shell.html` | Core systems |
| base-standard | `units.xml`, `leaders.xml`, `civilizations.xml`, `terrain.xml`, `modifiers.xml`, `config.xml` | Base game data |
| age-antiquity | `units.xml`, `civilizations.xml`, `constructibles.xml` | Antiquity content |
| age-exploration | `golden-ages-gameeffects.xml` | Exploration content |
| age-modern | `tutorial-quest-items-modern.js` | Modern content |
| Dev Tools | `modinfo Files.md`, example mods | Official documentation |

---

## Conclusion

The Civilization VII modding wiki provides a solid conceptual foundation but requires significant corrections before it can serve as reliable reference documentation. The most critical issues are:

1. **Event subscription pattern** - Would cause all event-handling code to fail
2. **Localization format** - Would cause all text loading to fail
3. **UI modding approach** - Documents a non-existent system
4. **Map registration** - Documents a non-existent action type

Addressing these four critical issues should be the immediate priority, followed by the table/column corrections and coverage expansion.
