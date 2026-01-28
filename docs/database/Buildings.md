# Buildings & Constructibles Database Schema

This document describes the database tables used to define buildings, improvements, and wonders in Civilization VII.

## Overview

In Civ VII, buildings, improvements, and wonders are all "constructibles" sharing a common base structure with specialization tables.

## ConstructibleClasses

Three main classes of constructibles:

| Class | Description |
|-------|-------------|
| `BUILDING` | City buildings (Granary, Library, etc.) |
| `IMPROVEMENT` | Tile improvements (Farm, Mine, etc.) |
| `WONDER` | World wonders |

## Types Table

Register constructibles in the Types table:

```xml
<Types>
    <Row Type="BUILDING_GRANARY" Kind="KIND_CONSTRUCTIBLE"/>
    <Row Type="IMPROVEMENT_FARM" Kind="KIND_CONSTRUCTIBLE"/>
    <Row Type="WONDER_PYRAMIDS" Kind="KIND_CONSTRUCTIBLE"/>
</Types>
```

## Constructibles Table

The main constructible definition table:

```xml
<Constructibles>
    <Row
        ConstructibleType="BUILDING_GRANARY"
        Name="LOC_BUILDING_GRANARY_NAME"
        Description="LOC_BUILDING_GRANARY_DESCRIPTION"
        Cost="100"
        Population="1"
        ConstructibleClass="BUILDING"
    />
</Constructibles>
```

### Constructibles Table Columns

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `ConstructibleType` | TEXT | Primary key | `BUILDING_GRANARY` |
| `Name` | TEXT | Localization key | `LOC_BUILDING_GRANARY_NAME` |
| `Description` | TEXT | Description key | `LOC_BUILDING_GRANARY_DESCRIPTION` |
| `Cost` | INTEGER | Production cost | `100` |
| `Population` | INTEGER | Population provided | `1`, `0` |
| `ConstructibleClass` | TEXT | Class type | `BUILDING`, `IMPROVEMENT`, `WONDER` |
| `ImmuneDamage` | BOOLEAN | Cannot be damaged | `true` (Palace) |
| `CanBeHidden` | BOOLEAN | Hidden on map | `true` (Discoveries) |
| `Discovery` | BOOLEAN | Is a discovery site | `true` |
| `RequiresUnlock` | BOOLEAN | Needs explicit unlock | `true` |

## Buildings Table

Additional properties specific to buildings:

```xml
<Buildings>
    <Row
        ConstructibleType="BUILDING_PALACE"
        Capital="true"
        CityCenterPriority="100"
        BuildQueue="true"
        Movable="false"
        Town="True"
    />
</Buildings>
```

| Column | Type | Description |
|--------|------|-------------|
| `ConstructibleType` | TEXT | Foreign key to Constructibles |
| `Capital` | BOOLEAN | Is capital building |
| `CityCenterPriority` | INTEGER | Placement priority |
| `BuildQueue` | BOOLEAN | Uses build queue |
| `Movable` | BOOLEAN | Can be relocated |
| `Town` | BOOLEAN | Can be built in towns |

## Improvements Table

Register constructibles as improvements:

```xml
<Improvements>
    <Row ConstructibleType="IMPROVEMENT_FARM"/>
    <Row ConstructibleType="IMPROVEMENT_MINE"/>
    <Row ConstructibleType="IMPROVEMENT_FISHING_BOAT"/>
</Improvements>
```

## TypeTags for Constructibles

Classification tags for buildings:

```xml
<TypeTags>
    <Row Type="BUILDING_GRANARY" Tag="FOOD"/>
    <Row Type="BUILDING_LIBRARY" Tag="SCIENCE"/>
    <Row Type="BUILDING_MARKET" Tag="GOLD"/>
    <Row Type="BUILDING_PALACE" Tag="AGELESS"/>
    <Row Type="BUILDING_PALACE" Tag="PERSISTENT"/>
</TypeTags>
```

### Common Constructible Tags

| Tag | Description |
|-----|-------------|
| `FOOD` | Food-producing building |
| `PRODUCTION` | Production building |
| `GOLD` | Gold/economic building |
| `SCIENCE` | Science building |
| `CULTURE` | Culture building |
| `HAPPINESS` | Happiness building |
| `DIPLOMACY` | Diplomacy building |
| `MILITARY` | Military building |
| `RELIGIOUS` | Religious building |
| `TRADE` | Trade-related |
| `WAREHOUSE` | Storage building |
| `FORTIFICATION` | Defensive structure |
| `GREATWORK` | Can hold great works |
| `UNIQUE` | Unique to a civilization |
| `UNIQUE_IMPROVEMENT` | Unique improvement |
| `AGELESS` | Persists across ages |
| `PERSISTENT` | Not removed on transition |

## Yield Tables

### Constructible_YieldChanges

Static yield bonuses from constructibles:

```xml
<Constructible_YieldChanges>
    <Row ConstructibleType="BUILDING_GRANARY" YieldType="YIELD_FOOD" YieldChange="2"/>
    <Row ConstructibleType="BUILDING_LIBRARY" YieldType="YIELD_SCIENCE" YieldChange="3"/>
</Constructible_YieldChanges>
```

### Constructible_Adjacencies

Adjacency bonuses:

```xml
<Constructible_Adjacencies>
    <Row ConstructibleType="BUILDING_LIBRARY" YieldChangeId="LIBRARY_SCIENCE_ADJACENCY"/>
</Constructible_Adjacencies>
```

### Constructible_WarehouseYields

Warehouse (storage) bonuses:

```xml
<Constructible_WarehouseYields>
    <Row ConstructibleType="BUILDING_GRANARY" YieldType="YIELD_FOOD" Amount="10"/>
</Constructible_WarehouseYields>
```

## Discovery Improvements

Discovery sites (goody huts) are special improvements:

```xml
<Constructibles>
    <Row
        ConstructibleType="IMPROVEMENT_CAMPFIRE"
        Name="LOC_IMPROVEMENT_CAMPFIRE_NAME"
        Description="LOC_IMPROVEMENT_CAMPFIRE_DESCRIPTION"
        ConstructibleClass="IMPROVEMENT"
        Cost="1"
        Population="0"
        CanBeHidden="true"
        Discovery="true"
    />
</Constructibles>
```

### Discovery Types

| Improvement | Description |
|-------------|-------------|
| `IMPROVEMENT_CAMPFIRE` | Basic discovery |
| `IMPROVEMENT_CAVE` | Cave exploration |
| `IMPROVEMENT_RUINS` | Ancient ruins |
| `IMPROVEMENT_SHIPWRECK` | Coastal discovery |
| `IMPROVEMENT_CAIRN` | Stone cairn |

## Age Transition

Buildings can be removed during age transitions:

```xml
<Constructible_TransitionRemoves>
    <Row ConstructibleType="BUILDING_PALACE"/>
    <Row ConstructibleType="BUILDING_CITY_HALL"/>
</Constructible_TransitionRemoves>
```

Use `AGELESS` and `PERSISTENT` tags to keep buildings across ages.

## Example: Creating a New Building

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Register the type -->
    <Types>
        <Row Type="BUILDING_MY_BUILDING" Kind="KIND_CONSTRUCTIBLE"/>
    </Types>

    <!-- Define the constructible -->
    <Constructibles>
        <Row
            ConstructibleType="BUILDING_MY_BUILDING"
            Name="LOC_BUILDING_MY_BUILDING_NAME"
            Description="LOC_BUILDING_MY_BUILDING_DESCRIPTION"
            Cost="150"
            Population="1"
            ConstructibleClass="BUILDING"
        />
    </Constructibles>

    <!-- Building-specific properties -->
    <Buildings>
        <Row ConstructibleType="BUILDING_MY_BUILDING"/>
    </Buildings>

    <!-- Classification -->
    <TypeTags>
        <Row Type="BUILDING_MY_BUILDING" Tag="CULTURE"/>
    </TypeTags>

    <!-- Yield bonuses -->
    <Constructible_YieldChanges>
        <Row ConstructibleType="BUILDING_MY_BUILDING" YieldType="YIELD_CULTURE" YieldChange="3"/>
    </Constructible_YieldChanges>
</Database>
```

## Related Tables

- `Constructible_Prerequisites` - Tech/civic requirements
- `Constructible_ValidTerrains` - Terrain restrictions
- `Constructible_ValidBiomes` - Biome restrictions
- `Constructible_ValidFeatures` - Feature requirements
- `Constructible_ValidResources` - Resource requirements
