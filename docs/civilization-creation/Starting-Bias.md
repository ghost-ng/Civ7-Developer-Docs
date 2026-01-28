# Starting Bias System

Starting bias controls where civilizations prefer to spawn on the map. The game uses multiple scoring tables to determine the best start positions for each civilization.

## Overview

Starting bias is calculated by scoring potential start locations based on:
- **Biomes** - Climate zones (desert, tropical, grassland, etc.)
- **Terrains** - Elevation and type (hills, flat, rivers, etc.)
- **Features** - Terrain features (forests, floodplains, etc.)
- **Resources** - Specific resource types nearby
- **Rivers** - Proximity to rivers
- **Coasts** - Proximity to coastal tiles

Higher scores indicate stronger preference. The game sums scores from all applicable tables.

## StartBiasBiomes Table

Preferences for specific climate zones:

```xml
<StartBiasBiomes>
    <Row CivilizationType="CIVILIZATION_EGYPT" BiomeType="BIOME_DESERT" Score="5"/>
    <Row CivilizationType="CIVILIZATION_GREECE" BiomeType="BIOME_GRASSLAND" Score="5"/>
    <Row CivilizationType="CIVILIZATION_KHMER" BiomeType="BIOME_TROPICAL" Score="5"/>
    <Row CivilizationType="CIVILIZATION_MAYA" BiomeType="BIOME_TROPICAL" Score="5"/>
    <Row CivilizationType="CIVILIZATION_PERSIA" BiomeType="BIOME_DESERT" Score="5"/>
</StartBiasBiomes>
```

### Columns

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Civilization reference |
| `BiomeType` | TEXT | Target biome |
| `Score` | INTEGER | Preference strength |

### Available Biomes

| BiomeType | Description |
|-----------|-------------|
| `BIOME_DESERT` | Arid desert regions |
| `BIOME_GRASSLAND` | Temperate grasslands |
| `BIOME_PLAINS` | Open plains |
| `BIOME_TROPICAL` | Tropical/jungle regions |
| `BIOME_TUNDRA` | Cold northern regions |
| `BIOME_MARINE` | Ocean/coastal biome |

## StartBiasTerrains Table

Preferences for specific terrain types:

```xml
<StartBiasTerrains>
    <Row CivilizationType="CIVILIZATION_AKSUM" TerrainType="TERRAIN_HILL" Score="5"/>
    <Row CivilizationType="CIVILIZATION_EGYPT" TerrainType="TERRAIN_NAVIGABLE_RIVER" Score="20"/>
    <Row CivilizationType="CIVILIZATION_GREECE" TerrainType="TERRAIN_HILL" Score="15"/>
    <Row CivilizationType="CIVILIZATION_INCA" TerrainType="TERRAIN_MOUNTAIN" Score="15"/>
    <Row CivilizationType="CIVILIZATION_MISSISSIPPIAN" TerrainType="TERRAIN_FLAT" Score="15"/>
</StartBiasTerrains>
```

### Columns

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Civilization reference |
| `TerrainType` | TEXT | Target terrain |
| `Score` | INTEGER | Preference strength |

### Available Terrains

| TerrainType | Description |
|-------------|-------------|
| `TERRAIN_FLAT` | Flat land |
| `TERRAIN_HILL` | Hilly terrain |
| `TERRAIN_MOUNTAIN` | Mountain tiles |
| `TERRAIN_COAST` | Coastal water |
| `TERRAIN_OCEAN` | Deep ocean |
| `TERRAIN_NAVIGABLE_RIVER` | Major rivers |

## StartBiasFeatureClasses Table

Preferences for terrain feature classes:

```xml
<StartBiasFeatureClasses>
    <Row CivilizationType="CIVILIZATION_KHMER" FeatureClassType="FEATURE_CLASS_FLOODPLAIN" Score="15"/>
    <Row CivilizationType="CIVILIZATION_MAURYA" FeatureClassType="FEATURE_CLASS_VEGETATED" Score="5"/>
    <Row CivilizationType="CIVILIZATION_MAYA" FeatureClassType="FEATURE_CLASS_VEGETATED" Score="15"/>
</StartBiasFeatureClasses>
```

### Columns

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Civilization reference |
| `FeatureClassType` | TEXT | Target feature class |
| `Score` | INTEGER | Preference strength |

### Available Feature Classes

| FeatureClassType | Description |
|------------------|-------------|
| `FEATURE_CLASS_FLOODPLAIN` | River floodplains |
| `FEATURE_CLASS_VEGETATED` | Forests, jungles |
| `FEATURE_CLASS_WET` | Marshes, wetlands |
| `FEATURE_CLASS_AQUATIC` | Water features |

## StartBiasResources Table

Preferences for specific resource types:

```xml
<StartBiasResources>
    <Row CivilizationType="CIVILIZATION_ABBASID" ResourceType="RESOURCE_CAMELS" Score="5"/>
    <Row CivilizationType="CIVILIZATION_MAJAPAHIT" ResourceType="RESOURCE_SPICES" Score="5"/>
    <Row CivilizationType="CIVILIZATION_MING" ResourceType="RESOURCE_SILK" Score="5"/>
    <Row CivilizationType="CIVILIZATION_MONGOLIA" ResourceType="RESOURCE_HORSES" Score="5"/>
</StartBiasResources>
```

### Columns

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Civilization reference |
| `ResourceType` | TEXT | Target resource |
| `Score` | INTEGER | Preference strength |

## StartBiasRivers Table

Preference for spawning near any river:

```xml
<StartBiasRivers>
    <Row CivilizationType="CIVILIZATION_MISSISSIPPIAN" Score="5"/>
</StartBiasRivers>
```

### Columns

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Civilization reference |
| `Score` | INTEGER | River proximity preference |

## StartBiasAdjacentToCoasts Table

Preference for spawning near coastal tiles:

```xml
<StartBiasAdjacentToCoasts>
    <Row CivilizationType="CIVILIZATION_AKSUM" Score="150"/>
    <Row CivilizationType="CIVILIZATION_CHOLA" Score="150"/>
    <Row CivilizationType="CIVILIZATION_HAWAII" Score="25"/>
    <Row CivilizationType="CIVILIZATION_MAJAPAHIT" Score="150"/>
    <Row CivilizationType="CIVILIZATION_NORMAN" Score="75"/>
    <Row CivilizationType="CIVILIZATION_SPAIN" Score="150"/>
</StartBiasAdjacentToCoasts>
```

### Columns

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Civilization reference |
| `Score` | INTEGER | Coastal proximity preference |

## Score Guidelines

| Score Range | Meaning |
|-------------|---------|
| 1-5 | Weak preference |
| 10-20 | Moderate preference |
| 25-75 | Strong preference |
| 100-150 | Very strong preference |

Higher scores significantly influence spawn location. Use scores of 100+ for mandatory features (like Hawaii needing coastal access).

## Example: Complete Starting Bias Setup

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Desert river civ with coastal access -->
    <StartBiasBiomes>
        <Row CivilizationType="CIVILIZATION_MY_CIV" BiomeType="BIOME_DESERT" Score="10"/>
    </StartBiasBiomes>

    <StartBiasTerrains>
        <Row CivilizationType="CIVILIZATION_MY_CIV" TerrainType="TERRAIN_NAVIGABLE_RIVER" Score="20"/>
    </StartBiasTerrains>

    <StartBiasFeatureClasses>
        <Row CivilizationType="CIVILIZATION_MY_CIV" FeatureClassType="FEATURE_CLASS_FLOODPLAIN" Score="15"/>
    </StartBiasFeatureClasses>

    <StartBiasAdjacentToCoasts>
        <Row CivilizationType="CIVILIZATION_MY_CIV" Score="50"/>
    </StartBiasAdjacentToCoasts>
</Database>
```

## Antiquity Civilization Start Biases

| Civilization | Primary Bias | Score | Secondary Bias |
|--------------|--------------|-------|----------------|
| Aksum | Hills, Coast | 5, 150 | - |
| Egypt | Navigable River | 20 | Desert biome |
| Greece | Hills | 15 | Grassland biome |
| Han | Grassland | 5 | - |
| Khmer | Tropical, Floodplains | 5, 15 | - |
| Maurya | Vegetated | 5 | - |
| Maya | Tropical, Vegetated | 5, 15 | - |
| Mississippian | Flat, Rivers | 15, 5 | - |
| Persia | Desert | 5 | - |
| Rome | Grassland | 5 | - |

## Exploration Civilization Start Biases

| Civilization | Primary Bias | Score | Secondary Bias |
|--------------|--------------|-------|----------------|
| Abbasid | Coast, Camels | 25, 5 | - |
| Chola | Tropical, Coast | 5, 150 | - |
| Hawaii | Marine, Coast | 15, 25 | - |
| Inca | Plains, Mountains | 5, 15 | - |
| Majapahit | Coast, Spices | 150, 5 | - |
| Ming | Silk | 5 | - |
| Mongolia | Horses | 5 | - |
| Norman | Coast | 75 | - |
| Songhai | Navigable River | 20 | - |
| Spain | Coast | 150 | - |

## Best Practices

1. **Balance scores** - Don't make all biases too strong or spawn will fail
2. **Consider map types** - Island maps need coastal bias, Pangaea doesn't
3. **Match civ abilities** - River civs should have river bias
4. **Test on multiple maps** - Verify spawns work on different map types
5. **Combine multiple biases** - Use 2-3 bias types for natural variety

## Related Documentation

- [Civilizations](../database/Civilizations.md)
- [Map & Terrain](../database/Map-Terrain.md)
- [Resources](../resources/Resources.md)
