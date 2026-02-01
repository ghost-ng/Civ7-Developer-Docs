# Map and Terrain Database Reference

This document covers the database tables related to maps, terrain, biomes, features, and plot-level data in Civilization VII.

## Terrain Types

Terrain defines the base elevation/type of a tile.

### Terrains Table

```xml
<Terrains>
    <Row TerrainType="TERRAIN_MOUNTAIN" Name="LOC_TERRAIN_MOUNTAIN_NAME"
         InfluenceCost="3" MovementCost="1" Appeal="1"
         Impassable="true" SightThroughModifier="2"/>
    <Row TerrainType="TERRAIN_HILL" Name="LOC_TERRAIN_HILL_NAME"
         InfluenceCost="2" MovementCost="1" Hills="true"/>
    <Row TerrainType="TERRAIN_FLAT" Name="LOC_TERRAIN_FLAT_NAME"
         InfluenceCost="0" MovementCost="1"/>
    <Row TerrainType="TERRAIN_COAST" Name="LOC_TERRAIN_COAST_NAME"
         InfluenceCost="1" MovementCost="1" Appeal="1" Water="true"/>
    <Row TerrainType="TERRAIN_OCEAN" Name="LOC_TERRAIN_OCEAN_NAME"
         InfluenceCost="3" MovementCost="1" Water="true"/>
    <Row TerrainType="TERRAIN_NAVIGABLE_RIVER" Name="LOC_TERRAIN_NAVIGABLE_RIVER_NAME"
         InfluenceCost="0" MovementCost="1" Appeal="1"/>
</Terrains>
```

### Terrain Columns

| Column | Type | Description |
|--------|------|-------------|
| `TerrainType` | Text | Unique identifier (PK) |
| `Name` | LocalizedText | Display name |
| `InfluenceCost` | Integer | Cost to acquire tile |
| `MovementCost` | Integer | Base movement cost |
| `Appeal` | Integer | Appeal bonus/penalty |
| `Impassable` | Boolean | Units cannot enter |
| `Hills` | Boolean | Is hill terrain |
| `Water` | Boolean | Is water terrain |
| `SightThroughModifier` | Integer | Visibility modifier |

### Base Terrain Types

| Type | Description |
|------|-------------|
| `TERRAIN_MOUNTAIN` | Impassable mountains |
| `TERRAIN_HILL` | Elevated land |
| `TERRAIN_FLAT` | Standard flat land |
| `TERRAIN_COAST` | Shallow water |
| `TERRAIN_OCEAN` | Deep water |
| `TERRAIN_NAVIGABLE_RIVER` | Major rivers |

## Biomes

Biomes define the climate/vegetation type of an area.

### Biomes Table

```xml
<Biomes>
    <Row BiomeType="BIOME_TUNDRA" Name="LOC_BIOME_TUNDRA_NAME"/>
    <Row BiomeType="BIOME_GRASSLAND" Name="LOC_BIOME_GRASSLAND_NAME" MaxLatitude="58"/>
    <Row BiomeType="BIOME_PLAINS" Name="LOC_BIOME_PLAINS_NAME" MaxLatitude="33"/>
    <Row BiomeType="BIOME_TROPICAL" Name="LOC_BIOME_TROPICAL_NAME" MaxLatitude="17"/>
    <Row BiomeType="BIOME_DESERT" Name="LOC_BIOME_DESERT_NAME" MaxLatitude="44"/>
    <Row BiomeType="BIOME_MARINE" Name="LOC_BIOME_MARINE_NAME"/>
</Biomes>
```

### Biome Columns

| Column | Type | Description |
|--------|------|-------------|
| `BiomeType` | Text | Unique identifier |
| `Name` | LocalizedText | Display name |
| `MaxLatitude` | Integer | Maximum latitude where biome appears |

### Biome_ValidTerrains

Links biomes to terrain types they can appear on:

```xml
<Biome_ValidTerrains>
    <Row BiomeType="BIOME_DESERT" TerrainType="TERRAIN_MOUNTAIN"/>
    <Row BiomeType="BIOME_DESERT" TerrainType="TERRAIN_HILL"/>
    <Row BiomeType="BIOME_DESERT" TerrainType="TERRAIN_FLAT"/>
    <Row BiomeType="BIOME_MARINE" TerrainType="TERRAIN_COAST"/>
    <Row BiomeType="BIOME_MARINE" TerrainType="TERRAIN_OCEAN"/>
</Biome_ValidTerrains>
```

## Features

Features are overlays on terrain (forests, marshes, natural wonders, etc.).

### Features Table

```xml
<Features>
    <Row FeatureType="FEATURE_FOREST" Name="LOC_FEATURE_FOREST_NAME"
         FeatureClassType="FEATURE_CLASS_VEGETATED"
         MovementChange="1" SightThroughModifier="-1"
         Removable="true" Appeal="-1"/>
    <Row FeatureType="FEATURE_MARSH" Name="LOC_FEATURE_MARSH_NAME"
         FeatureClassType="FEATURE_CLASS_WET"
         MovementChange="2" DefenseModifier="-10"
         Removable="true" Appeal="-2"/>
</Features>
```

### Feature Columns

| Column | Type | Description |
|--------|------|-------------|
| `FeatureType` | Text | Unique identifier |
| `Name` | LocalizedText | Display name |
| `FeatureClassType` | Text | Feature class FK |
| `MovementChange` | Integer | Additional movement cost |
| `SightThroughModifier` | Integer | Vision modifier |
| `DefenseModifier` | Integer | Combat defense bonus |
| `Removable` | Boolean | Can be cleared |
| `Appeal` | Integer | Appeal modifier |
| `NaturalWonder` | Boolean | Is a natural wonder |

### Base Features

| Type | Description |
|------|-------------|
| `FEATURE_FOREST` | Standard forest |
| `FEATURE_RAINFOREST` | Tropical rainforest |
| `FEATURE_MARSH` | Wetland marsh |
| `FEATURE_OASIS` | Desert oasis |
| `FEATURE_REEF` | Ocean reef |
| `FEATURE_ICE` | Polar ice |
| `FEATURE_VOLCANO` | Active volcano |

### Feature Classes

```xml
<FeatureClasses>
    <Row FeatureClassType="FEATURE_CLASS_AQUATIC"/>
    <Row FeatureClassType="FEATURE_CLASS_FLOODPLAIN"/>
    <Row FeatureClassType="FEATURE_CLASS_VEGETATED"/>
    <Row FeatureClassType="FEATURE_CLASS_WET"/>
</FeatureClasses>
```

## Natural Wonders

Natural wonders are special features with unique bonuses.

### Feature_NaturalWonders Table

```xml
<Feature_NaturalWonders>
    <Row FeatureType="FEATURE_GRAND_CANYON" PlacementPercentage="100"
         PlaceFirst="false" Tiles="4" NoRiver="true"/>
    <Row FeatureType="FEATURE_KILIMANJARO" PlacementPercentage="100"
         PlaceFirst="true" Tiles="1" NoRiver="false"/>
</Feature_NaturalWonders>
```

### Natural Wonder Columns

| Column | Type | Description |
|--------|------|-------------|
| `FeatureType` | Text | Feature type FK |
| `PlacementPercentage` | Integer | Chance to place (0-100) |
| `PlaceFirst` | Boolean | Priority placement |
| `Tiles` | Integer | Number of tiles the wonder occupies |
| `NoRiver` | Boolean | Prevents placement adjacent to rivers |

### Base Natural Wonders

| Type | Description |
|------|-------------|
| `FEATURE_GRAND_CANYON` | Grand Canyon |
| `FEATURE_KILIMANJARO` | Mount Kilimanjaro |
| `FEATURE_BARRIER_REEF` | Great Barrier Reef |
| `FEATURE_ULURU` | Uluru |
| `FEATURE_IGUAZU_FALLS` | Iguazu Falls |
| `FEATURE_VALLEY_OF_FLOWERS` | Valley of Flowers |
| `FEATURE_TORRES_DEL_PAINE` | Torres del Paine |
| `FEATURE_REDWOOD_FOREST` | Redwood Forest |
| `FEATURE_GULLFOSS` | Gullfoss |
| `FEATURE_ZHANGJIAJIE` | Zhangjiajie |

## Fertility

Fertility defines tile food potential.

```xml
<Types>
    <Row Type="FERTILITY_STANDARD" Kind="KIND_FERTILITY"/>
    <Row Type="FERTILITY_DRY" Kind="KIND_FERTILITY"/>
    <Row Type="FERTILITY_FERTILE" Kind="KIND_FERTILITY"/>
</Types>
```

## Feature Attribute Tags

Tags define placement rules for features:

```xml
<Tags>
    <Row Tag="ADJACENTTOLAND" Category="FEATURE_ATTRIBUTE"/>
    <Row Tag="ADJACENTTOCOAST" Category="FEATURE_ATTRIBUTE"/>
    <Row Tag="ADJACENTMOUNTAIN" Category="FEATURE_ATTRIBUTE"/>
    <Row Tag="NEARCOAST" Category="FEATURE_ATTRIBUTE"/>
    <Row Tag="NOTNEARCOAST" Category="FEATURE_ATTRIBUTE"/>
    <Row Tag="VOLCANO" Category="FEATURE_ATTRIBUTE"/>
    <Row Tag="WATERFALL" Category="FEATURE_ATTRIBUTE"/>
    <Row Tag="LAKE" Category="FEATURE_ATTRIBUTE"/>
</Tags>
```

### TypeTags for Features

```xml
<TypeTags>
    <Row Type="FEATURE_GRAND_CANYON" Tag="NOTNEARCOAST"/>
    <Row Type="FEATURE_GRAND_CANYON" Tag="NOTADJACENTTORIVER"/>
    <Row Type="FEATURE_KILIMANJARO" Tag="VOLCANO"/>
    <Row Type="FEATURE_BARRIER_REEF" Tag="ADJACENTTOLAND"/>
    <Row Type="FEATURE_BARRIER_REEF" Tag="SHALLOWWATER"/>
</TypeTags>
```

## City Expansion

Defines what terrain cities can expand into.

### CityExpansionTypes

```xml
<CityExpansionTypes>
    <Row CityExpansionType="CITY_EXPANSION_EXPAND" Name="LOC_CITY_EXPANSION_EXPAND_NAME"/>
    <Row CityExpansionType="CITY_EXPANSION_DEVELOP" Name="LOC_CITY_EXPANSION_DEVELOP_NAME"/>
</CityExpansionTypes>
```

### CityExpansionValidTerrains

```xml
<CityExpansionValidTerrains>
    <Row CityExpansionType="CITY_EXPANSION_EXPAND" TerrainType="TERRAIN_MOUNTAIN"/>
    <Row CityExpansionType="CITY_EXPANSION_EXPAND" TerrainType="TERRAIN_HILL"/>
    <Row CityExpansionType="CITY_EXPANSION_EXPAND" TerrainType="TERRAIN_FLAT"/>
    <Row CityExpansionType="CITY_EXPANSION_EXPAND" TerrainType="TERRAIN_COAST"/>
    <Row CityExpansionType="CITY_EXPANSION_EXPAND" TerrainType="TERRAIN_OCEAN"/>
    <Row CityExpansionType="CITY_EXPANSION_DEVELOP" TerrainType="TERRAIN_HILL"/>
    <Row CityExpansionType="CITY_EXPANSION_DEVELOP" TerrainType="TERRAIN_FLAT"/>
</CityExpansionValidTerrains>
```

## JavaScript Map API

For runtime map access, use the `GameplayMap` global object:

```javascript
// Map dimensions
const width = GameplayMap.getGridWidth();
const height = GameplayMap.getGridHeight();

// Tile queries
const terrain = GameplayMap.getTerrainType(x, y);
const resource = GameplayMap.getResourceType(x, y);
const elevation = GameplayMap.getElevation(x, y);

// Location helpers
const hemisphere = GameplayMap.getHemisphere(x);
const landmassId = GameplayMap.getLandmassRegionId(x, y);

// Index conversions
const index = GameplayMap.getIndexFromXY(x, y);
const location = GameplayMap.getLocationFromIndex(index);
```

### TerrainBuilder (Map Generation)

```javascript
// Set features
const featureParam = {
    Feature: featureHash,
    Direction: 0,
    Elevation: elevation
};
TerrainBuilder.canHaveFeatureParam(x, y, featureParam);
TerrainBuilder.setFeatureType(x, y, featureParam);

// Random numbers for map gen
const roll = TerrainBuilder.getRandomNumber(100, "description");

// Generate patterns
const poisson = TerrainBuilder.generatePoissonMap(seed, distance, smoothing);
```

## Modifying Terrain with Modifiers

### Adjust Terrain Yields

```xml
<Modifier id="MY_TERRAIN_BONUS" collection="COLLECTION_PLAYER_PLOT_YIELDS"
          effect="EFFECT_ADJUST_PLOT_YIELD">
    <SubjectRequirementSet>
        <Requirement type="REQUIREMENT_PLOT_TERRAIN_TYPE_MATCHES">
            <Argument name="TerrainType">TERRAIN_HILL</Argument>
        </Requirement>
    </SubjectRequirementSet>
    <Argument name="YieldType">YIELD_PRODUCTION</Argument>
    <Argument name="Amount">1</Argument>
</Modifier>
```

### Feature-based Bonuses

```xml
<Modifier id="MY_FOREST_BONUS" collection="COLLECTION_PLAYER_PLOT_YIELDS"
          effect="EFFECT_ADJUST_PLOT_YIELD">
    <SubjectRequirementSet>
        <Requirement type="REQUIREMENT_PLOT_FEATURE_TYPE_MATCHES">
            <Argument name="FeatureType">FEATURE_FOREST</Argument>
        </Requirement>
    </SubjectRequirementSet>
    <Argument name="YieldType">YIELD_PRODUCTION</Argument>
    <Argument name="Amount">1</Argument>
</Modifier>
```

## See Also

- [JavaScript Overview](../javascript/Overview.md) - Map API details
- [Types and Kinds](Types-and-Kinds.md) - Type system
- [Modifier System](../modifiers/Overview.md) - Creating bonuses
- [Resources](Resources.md) - Resource placement
