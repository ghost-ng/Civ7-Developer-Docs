# Maps, Resources & World Generation

This document covers map configuration, resources, and world generation in Civilization VII.

## Overview

Civ VII uses JavaScript-based map scripts for world generation. Maps are configured through several interconnected tables that define sizes, resources, continents, and distribution rules.

## Core Tables

| Table | Purpose |
|-------|---------|
| `Maps` (config) | Available map scripts (JavaScript files) |
| `Maps` (data) | Map size configurations |
| `MapSizes` | Size domains for different game modes |
| `Continents` | Continent definitions |
| `Resources` | Resource definitions |
| `ResourceClasses` | Resource classification |
| `Resource_ValidBiomes` | Where resources can spawn |
| `Resource_ValidAges` | Which Ages resources appear in |
| `Resource_Distribution` | Age-based distribution rules |

## Map Scripts

Map scripts are JavaScript files that generate the world. They're registered in config.xml:

```xml
<Maps>
    <Row File="{base-standard}maps/continents-voronoi.js"
         Name="LOC_MAP_CONTINENTS_VORONOI_NAME"
         Description="LOC_MAP_CONTINENTS_VORONOI_DESCRIPTION"
         SortIndex="0"/>
    <Row File="{base-standard}maps/pangaea-voronoi.js"
         Name="LOC_MAP_PANGAEA_VORONOI_NAME"
         Description="LOC_MAP_PANGAEA_VORONOI_DESCRIPTION"
         SortIndex="1"/>
</Maps>
```

### Available Base Map Scripts

| Script | Description |
|--------|-------------|
| `continents-voronoi.js` | Multiple continents using Voronoi generation |
| `pangaea-voronoi.js` | Single supercontinent |
| `shattered-seas-voronoi.js` | Many small landmasses |
| `continents-plus.js` | Continents with islands |
| `continents.js` | Classic continent layout |
| `archipelago.js` | Island chains |
| `fractal.js` | Fractal-generated landmasses |
| `shuffle.js` | Random map type |
| `terra-incognita.js` | New World discovery setup |
| `pangaea-plus.js` | Pangaea with islands |

## Map Sizes

Map sizes are defined in the `Maps` table in `maps.xml`:

```xml
<Maps>
    <Row MapSizeType="MAPSIZE_TINY"
         Name="LOC_MAPSIZE_TINY_NAME"
         Description="LOC_MAPSIZE_TINY_DESCRIPTION"
         DefaultPlayers="4"
         PlayersLandmass1="3"
         PlayersLandmass2="1"
         GridWidth="60"
         GridHeight="38"
         NumNaturalWonders="3"
         OceanWidth="4"
         LakeSizeCutoff="6"
         LakeGenerationFrequency="25"
         Continents="2"
         StartSectorRows="3"
         StartSectorCols="2"/>
</Maps>
```

### Maps Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `MapSizeType` | TEXT | Size identifier (MAPSIZE_TINY, etc.) |
| `Name` | TEXT | Localization key |
| `Description` | TEXT | Localization key |
| `DefaultPlayers` | INTEGER | Default player count |
| `PlayersLandmass1` | INTEGER | Players on main landmass |
| `PlayersLandmass2` | INTEGER | Players on secondary landmass |
| `GridWidth` | INTEGER | Map width in tiles |
| `GridHeight` | INTEGER | Map height in tiles |
| `NumNaturalWonders` | INTEGER | Natural wonders to place |
| `OceanWidth` | INTEGER | Ocean buffer width |
| `LakeSizeCutoff` | INTEGER | Maximum lake size |
| `LakeGenerationFrequency` | INTEGER | Lake spawn frequency |
| `Continents` | INTEGER | Number of continents |
| `StartSectorRows` | INTEGER | Starting position grid rows |
| `StartSectorCols` | INTEGER | Starting position grid columns |

### Default Map Sizes

| Size | Grid | Players | Natural Wonders |
|------|------|---------|-----------------|
| Tiny | 60x38 | 4 | 3 |
| Small | 74x46 | 6 | 4 |
| Standard | 84x54 | 8 | 5 |
| Large | 96x60 | 10 | 6 |
| Huge | 106x66 | 10 | 7 |

## MapSizes Domain

The `MapSizes` table in config.xml defines player limits for different game modes:

```xml
<MapSizes>
    <!-- Standard domain - all players can win -->
    <Row Domain="StandardMapSizes"
         MapSizeType="MAPSIZE_STANDARD"
         Name="LOC_MAPSIZE_STANDARD_NAME"
         Description="LOC_MAPSIZE_STANDARD_DESCRIPTION"
         MinPlayers="2"
         MaxPlayers="8"
         MaxHumans="8"
         DefaultPlayers="8"
         SortIndex="40"/>

    <!-- Distant Lands domain - only landmass1 can win -->
    <Row Domain="DistantLandsMapSizes"
         MapSizeType="MAPSIZE_STANDARD"
         Name="LOC_MAPSIZE_STANDARD_NAME"
         MinPlayers="2"
         MaxPlayers="8"
         MaxHumans="8"
         DefaultPlayers="8"
         SortIndex="40"/>
</MapSizes>
```

## Start Positions

Start positions determine how players are placed on the map:

```xml
<DomainValues>
    <Row Domain="StandardMapStartPositions"
         Value="START_POSITION_BALANCED"
         Name="LOC_MAP_START_POSITION_BALANCED_NAME"
         Description="LOC_MAP_START_POSITION_BALANCED_DESCRIPTION"
         SortIndex="10"/>
    <Row Domain="StandardMapStartPositions"
         Value="START_POSITION_STANDARD"
         Name="LOC_MAP_START_POSITION_STANDARD_NAME"
         Description="LOC_MAP_START_POSITION_STANDARD_DESCRIPTION"
         SortIndex="20"/>
</DomainValues>
```

### Start Position Types

| Type | Description |
|------|-------------|
| `START_POSITION_BALANCED` | Ensures fair resource distribution near starts |
| `START_POSITION_STANDARD` | Random placement within start sectors |

### Map-Specific Start Positions

```xml
<SupportedValuesByMap>
    <Row Map="{base-standard}maps/continents-voronoi.js"
         Domain="StandardMapStartPositions"
         Value="START_POSITION_STANDARD"/>
</SupportedValuesByMap>
```

## Continents

Continents are registered as types and defined in the `Continents` table:

```xml
<Types>
    <Row Type="CONTINENT_AFRICA" Kind="KIND_CONTINENT"/>
    <Row Type="CONTINENT_ASIA" Kind="KIND_CONTINENT"/>
    <Row Type="CONTINENT_EUROPE" Kind="KIND_CONTINENT"/>
    <Row Type="CONTINENT_PANGAEA" Kind="KIND_CONTINENT"/>
</Types>

<Continents>
    <Row ContinentType="CONTINENT_AFRICA"
         Description="LOC_CONTINENT_AFRICA_DESCRIPTION"/>
    <Row ContinentType="CONTINENT_ASIA"
         Description="LOC_CONTINENT_ASIA_DESCRIPTION"/>
</Continents>
```

## Resources

Resources are the primary strategic and luxury goods in the game.

### Resource Classes

```xml
<Types>
    <Row Type="RESOURCECLASS_BONUS" Kind="KIND_RESOURCECLASS"/>
    <Row Type="RESOURCECLASS_CITY" Kind="KIND_RESOURCECLASS"/>
    <Row Type="RESOURCECLASS_EMPIRE" Kind="KIND_RESOURCECLASS"/>
</Types>

<ResourceClasses>
    <Row ResourceClassType="RESOURCECLASS_BONUS"
         Name="LOC_RESOURCECLASS_BONUS_NAME"
         Description="LOC_RESOURCECLASS_BONUS_DESCRIPTION"
         Assignable="true"/>
    <Row ResourceClassType="RESOURCECLASS_CITY"
         Name="LOC_RESOURCECLASS_CITY_NAME"
         Description="LOC_RESOURCECLASS_CITY_DESCRIPTION"
         Assignable="true"/>
    <Row ResourceClassType="RESOURCECLASS_EMPIRE"
         Name="LOC_RESOURCECLASS_EMPIRE_NAME"
         Description="LOC_RESOURCECLASS_EMPIRE_DESCRIPTION"
         Assignable="false"/>
</ResourceClasses>
```

### Resource Class Types

| Class | Description | Assignable |
|-------|-------------|------------|
| `RESOURCECLASS_BONUS` | Basic resources (food, production) | Yes |
| `RESOURCECLASS_CITY` | City-level luxury resources | Yes |
| `RESOURCECLASS_EMPIRE` | Empire-wide strategic resources | No |

### Defining Resources

```xml
<Types>
    <Row Type="RESOURCE_GOLD" Kind="KIND_RESOURCE"/>
    <Row Type="RESOURCE_IRON" Kind="KIND_RESOURCE"/>
</Types>

<Resources>
    <Row ResourceType="RESOURCE_GOLD"
         Name="LOC_RESOURCE_GOLD_NAME"
         Tooltip="LOC_ANT_RESOURCE_GOLD_TOOLTIP"
         ResourceClassType="RESOURCECLASS_EMPIRE"
         Weight="20"
         Staple="true"
         MinimumPerHemisphere="8"/>
    <Row ResourceType="RESOURCE_IRON"
         Name="LOC_RESOURCE_IRON_NAME"
         Tooltip="LOC_ANT_RESOURCE_IRON_TOOLTIP"
         ResourceClassType="RESOURCECLASS_EMPIRE"
         Weight="10"
         UnlocksCiv="true"
         Staple="true"/>
</Resources>
```

### Resources Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `ResourceType` | TEXT | Unique identifier |
| `Name` | TEXT | Localization key |
| `Tooltip` | TEXT | Localization key for tooltip |
| `ResourceClassType` | TEXT | Resource class reference |
| `Weight` | INTEGER | Spawn weight (higher = more common) |
| `Staple` | BOOLEAN | Always appears in Age |
| `MinimumPerHemisphere` | INTEGER | Minimum spawn count per hemisphere |
| `UnlocksCiv` | BOOLEAN | Can unlock a civilization |
| `Tradeable` | BOOLEAN | Can be traded (default true) |
| `AdjacentToLand` | BOOLEAN | Must spawn adjacent to land |
| `LakeEligible` | BOOLEAN | Can spawn in lakes |
| `HemisphereUnique` | BOOLEAN | Only spawns in one hemisphere |
| `BonusResourceSlots` | INTEGER | Extra resource slots provided |

## Resource Yields

Define what yields a resource provides:

```xml
<Resource_YieldChanges>
    <Row ResourceType="RESOURCE_GOLD" YieldType="YIELD_GOLD" YieldChange="1"/>
    <Row ResourceType="RESOURCE_IRON" YieldType="YIELD_PRODUCTION" YieldChange="1"/>
    <Row ResourceType="RESOURCE_WINE" YieldType="YIELD_HAPPINESS" YieldChange="1"/>
    <Row ResourceType="RESOURCE_FISH" YieldType="YIELD_FOOD" YieldChange="1"/>
</Resource_YieldChanges>
```

## Resource Age Validity

Resources appear only in specific Ages:

```xml
<Resource_ValidAges>
    <!-- Gold appears in all ages -->
    <Row ResourceType="RESOURCE_GOLD" AgeType="AGE_ANTIQUITY"/>
    <Row ResourceType="RESOURCE_GOLD" AgeType="AGE_EXPLORATION"/>
    <Row ResourceType="RESOURCE_GOLD" AgeType="AGE_MODERN"/>

    <!-- Cocoa only in Exploration and Modern -->
    <Row ResourceType="RESOURCE_COCOA" AgeType="AGE_EXPLORATION"/>
    <Row ResourceType="RESOURCE_COCOA" AgeType="AGE_MODERN"/>

    <!-- Oil only in Modern -->
    <Row ResourceType="RESOURCE_OIL" AgeType="AGE_MODERN"/>
</Resource_ValidAges>
```

## Resource Biome Placement

Define where resources can spawn based on terrain:

```xml
<Resource_ValidBiomes>
    <Row ResourceType="RESOURCE_GOLD"
         BiomeType="BIOME_GRASSLAND"
         TerrainType="TERRAIN_HILL"/>
    <Row ResourceType="RESOURCE_GOLD"
         BiomeType="BIOME_PLAINS"
         TerrainType="TERRAIN_HILL"/>
    <Row ResourceType="RESOURCE_FISH"
         BiomeType="BIOME_MARINE"
         TerrainType="TERRAIN_COAST"/>
    <Row ResourceType="RESOURCE_COTTON"
         BiomeType="BIOME_GRASSLAND"
         TerrainType="TERRAIN_FLAT"
         FeatureType="FEATURE_GRASSLAND_FLOODPLAIN_MINOR"/>
</Resource_ValidBiomes>
```

### Available Biomes

| Biome | Description |
|-------|-------------|
| `BIOME_GRASSLAND` | Temperate grasslands |
| `BIOME_PLAINS` | Savanna and steppes |
| `BIOME_DESERT` | Arid regions |
| `BIOME_TUNDRA` | Cold northern regions |
| `BIOME_TROPICAL` | Rainforest and jungle |
| `BIOME_MARINE` | Ocean and coastal |

### Terrain Types

| Terrain | Description |
|---------|-------------|
| `TERRAIN_FLAT` | Flatland tiles |
| `TERRAIN_HILL` | Hill tiles |
| `TERRAIN_COAST` | Coastal water |
| `TERRAIN_OCEAN` | Deep ocean |

## Resource Distribution

Control how resources are distributed per Age:

```xml
<Resource_Distribution>
    <Row AgeType="AGE_ANTIQUITY"
         MaxResourceTypes="25"
         AdditionalCutAmount="0"
         ResourceTypeMaxPerHemisphere="8"/>
    <Row AgeType="AGE_EXPLORATION"
         MaxResourceTypes="25"
         AdditionalCutAmount="2"
         ResourceTypeMaxPerHemisphere="10"/>
    <Row AgeType="AGE_MODERN"
         MaxResourceTypes="27"
         AdditionalCutAmount="0"
         ResourceTypeMaxPerHemisphere="10"/>
</Resource_Distribution>
```

## Map Resource Modifiers

Adjust resource amounts per map type and size:

```xml
<MapResourceMinimumAmountModifier>
    <Row MapType="DEFAULT" MapSizeType="MAPSIZE_TINY" Amount="-4"/>
    <Row MapType="DEFAULT" MapSizeType="MAPSIZE_SMALL" Amount="-2"/>
    <Row MapType="DEFAULT" MapSizeType="MAPSIZE_LARGE" Amount="0"/>
    <Row MapType="DEFAULT" MapSizeType="MAPSIZE_HUGE" Amount="4"/>

    <Row MapType="LOC_MAP_ARCHIPELAGO_NAME"
         MapSizeType="MAPSIZE_TINY"
         Amount="-4"/>
</MapResourceMinimumAmountModifier>
```

## Island Behavior

Configure how islands behave during specific Ages:

```xml
<MapIslandBehavior>
    <Row MapType="LOC_MAP_CONTINENTS_NAME"
         AgeType="AGE_EXPLORATION"
         ResourceClassType="RESOURCECLASS_TREASURE"/>
    <Row MapType="LOC_MAP_ARCHIPELAGO_NAME"
         AgeType="AGE_EXPLORATION"
         ResourceClassType="RESOURCECLASS_TREASURE"/>
</MapIslandBehavior>
```

## Leader Resource Requirements

Ensure specific resources spawn for certain leaders:

```xml
<Resource_RequiredLeaders>
    <Row ResourceType="RESOURCE_COCOA" LeaderType="LEADER_ASHOKA"/>
    <Row ResourceType="RESOURCE_TEA" LeaderType="LEADER_ASHOKA"/>
    <Row ResourceType="RESOURCE_HORSES" LeaderType="LEADER_HATSHEPSUT"/>
    <Row ResourceType="RESOURCE_JADE" LeaderType="LEADER_HIMIKO"/>
</Resource_RequiredLeaders>
```

## Complete Example: Custom Resource

### Database XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register type -->
    <Types>
        <Row Type="RESOURCE_OBSIDIAN" Kind="KIND_RESOURCE"/>
    </Types>

    <!-- 2. Define resource -->
    <Resources>
        <Row ResourceType="RESOURCE_OBSIDIAN"
             Name="LOC_RESOURCE_OBSIDIAN_NAME"
             Tooltip="LOC_RESOURCE_OBSIDIAN_TOOLTIP"
             ResourceClassType="RESOURCECLASS_CITY"
             Weight="10"
             UnlocksCiv="true"/>
    </Resources>

    <!-- 3. Define yields -->
    <Resource_YieldChanges>
        <Row ResourceType="RESOURCE_OBSIDIAN"
             YieldType="YIELD_PRODUCTION"
             YieldChange="1"/>
    </Resource_YieldChanges>

    <!-- 4. Define valid Ages -->
    <Resource_ValidAges>
        <Row ResourceType="RESOURCE_OBSIDIAN" AgeType="AGE_ANTIQUITY"/>
        <Row ResourceType="RESOURCE_OBSIDIAN" AgeType="AGE_EXPLORATION"/>
    </Resource_ValidAges>

    <!-- 5. Define valid biomes -->
    <Resource_ValidBiomes>
        <Row ResourceType="RESOURCE_OBSIDIAN"
             BiomeType="BIOME_GRASSLAND"
             TerrainType="TERRAIN_HILL"/>
        <Row ResourceType="RESOURCE_OBSIDIAN"
             BiomeType="BIOME_PLAINS"
             TerrainType="TERRAIN_HILL"/>
        <Row ResourceType="RESOURCE_OBSIDIAN"
             BiomeType="BIOME_TROPICAL"
             TerrainType="TERRAIN_HILL"/>
    </Resource_ValidBiomes>
</Database>
```

### Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_RESOURCE_OBSIDIAN_NAME" Language="en_US">
            <Text>Obsidian</Text>
        </Row>
        <Row Tag="LOC_RESOURCE_OBSIDIAN_TOOLTIP" Language="en_US">
            <Text>Volcanic glass used for tools and weapons.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Complete Example: Custom Map Size

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Register the type -->
    <Types>
        <Row Type="MAPSIZE_MASSIVE" Kind="KIND_MAPSIZE"/>
    </Types>

    <!-- Define the size -->
    <Maps>
        <Row MapSizeType="MAPSIZE_MASSIVE"
             Name="LOC_MAPSIZE_MASSIVE_NAME"
             Description="LOC_MAPSIZE_MASSIVE_DESCRIPTION"
             DefaultPlayers="14"
             PlayersLandmass1="8"
             PlayersLandmass2="6"
             GridWidth="120"
             GridHeight="76"
             NumNaturalWonders="9"
             OceanWidth="10"
             LakeSizeCutoff="12"
             LakeGenerationFrequency="25"
             Continents="8"
             StartSectorRows="5"
             StartSectorCols="4"/>
    </Maps>

    <!-- Add to domains -->
    <MapSizes>
        <Row Domain="StandardMapSizes"
             MapSizeType="MAPSIZE_MASSIVE"
             Name="LOC_MAPSIZE_MASSIVE_NAME"
             Description="LOC_MAPSIZE_MASSIVE_DESCRIPTION"
             MinPlayers="2"
             MaxPlayers="14"
             MaxHumans="12"
             DefaultPlayers="14"
             SortIndex="70"/>
    </MapSizes>
</Database>
```

## Resources by Age

### Antiquity Age Resources

| Resource | Class | Yield |
|----------|-------|-------|
| Cotton | Bonus | +1 Gold |
| Dates | Bonus | +1 Food |
| Fish | Bonus | +1 Food |
| Gold | Empire | +1 Gold |
| Gypsum | City | +1 Science |
| Horses | Empire | +1 Production |
| Iron | Empire | +1 Production |
| Ivory | Empire | +1 Culture |
| Jade | City | +1 Culture |
| Marble | Empire | +1 Culture |
| Pearls | City | +1 Gold |
| Silk | City | +1 Culture |
| Silver | Empire | +1 Gold |
| Wine | Empire | +1 Happiness |

### Exploration Age Resources

| Resource | Class | Yield |
|----------|-------|-------|
| Cocoa | Empire | +1 Gold |
| Furs | Empire | +1 Gold |
| Niter | Empire | +1 Production |
| Spices | Empire | +1 Gold |
| Sugar | Empire | +1 Food |
| Tea | Empire | +1 Science |
| Truffles | City | +1 Gold |

### Modern Age Resources

| Resource | Class | Yield |
|----------|-------|-------|
| Coal | Empire | +1 Production |
| Coffee | Bonus | +1 Science |
| Citrus | Bonus | +1 Food |
| Oil | Empire | +1 Production |
| Rubber | Empire | +1 Production |
| Tobacco | Bonus | +1 Gold |

## Best Practices

1. **Balance Spawn Weights** - Higher weight = more common spawning
2. **Use Staple Resources** - Mark essential resources with `Staple="true"`
3. **Hemisphere Diversity** - Use `HemisphereUnique` for trade opportunities
4. **Biome Consistency** - Place resources in thematically appropriate biomes
5. **Age Progression** - Introduce new resources each Age
6. **Leader Requirements** - Ensure leaders have access to needed resources
7. **Map Size Scaling** - Adjust resource amounts for different map sizes

## Related Documentation

- [Map & Terrain](../database/Map-Terrain.md)
- [Civilizations](../database/Civilizations.md)
- [Starting Bias](../civilization-creation/Starting-Bias.md)
- [JavaScript API](../javascript/Overview.md)
