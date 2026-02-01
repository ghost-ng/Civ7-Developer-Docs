# Tutorial: Creating a Custom Map Generator

This tutorial walks through creating a custom map generation script for Civilization VII using JavaScript. Map scripts control terrain generation, resource placement, and starting positions.

## What You'll Learn

- Understanding the map generation pipeline
- Using TerrainBuilder and ResourceBuilder APIs
- Creating terrain patterns and features
- Placing resources and natural wonders
- Registering your map script

## Example: Inland Sea Map

We'll create an "Inland Sea" map with a large central body of water surrounded by land, inspired by the Mediterranean.

## File Structure

```
my-inland-sea-map/
├── maps/
│   ├── inland-sea.js
│   └── map-utilities.js
├── config/
│   └── maps-config.xml
├── text/
│   └── text.xml
└── my-inland-sea-map.modinfo
```

## Step 1: Create the .modinfo File

**my-inland-sea-map.modinfo**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-inland-sea-map" version="1" xmlns="ModInfo">
    <Properties>
        <Name>Inland Sea Map</Name>
        <Description>A map with a large central sea surrounded by land.</Description>
        <Authors>Your Name</Authors>
        <AffectsSavedGames>0</AffectsSavedGames>
    </Properties>
    <Dependencies>
    </Dependencies>
    <ActionCriteria>
        <Criteria id="always">
            <AlwaysMet></AlwaysMet>
        </Criteria>
    </ActionCriteria>
    <ActionGroups>
        <!-- Shell scope to register the map in game setup -->
        <ActionGroup id="shell-config" scope="shell" criteria="always">
            <Actions>
                <UpdateDatabase>
                    <Item>config/maps-config.xml</Item>
                </UpdateDatabase>
                <UpdateText>
                    <Item>text/text.xml</Item>
                </UpdateText>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

> **Note:** Map scripts are loaded automatically when the map is selected based on the `Maps` table registration in Step 2. There is no `<MapGenScripts>` action - map scripts register event handlers that the game calls during generation.

## Step 2: Register the Map Script

**config/maps-config.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Maps>
        <Row
            File="{my-inland-sea-map}maps/inland-sea.js"
            Name="LOC_MAP_INLAND_SEA_NAME"
            Description="LOC_MAP_INLAND_SEA_DESCRIPTION"
            SortIndex="100"
        />
    </Maps>

    <!-- Register for standard map sizes -->
    <SupportedValuesByMap>
        <Row Map="{my-inland-sea-map}maps/inland-sea.js"
             Domain="StandardMapSizes"
             Value="MAPSIZE_TINY"/>
        <Row Map="{my-inland-sea-map}maps/inland-sea.js"
             Domain="StandardMapSizes"
             Value="MAPSIZE_SMALL"/>
        <Row Map="{my-inland-sea-map}maps/inland-sea.js"
             Domain="StandardMapSizes"
             Value="MAPSIZE_STANDARD"/>
        <Row Map="{my-inland-sea-map}maps/inland-sea.js"
             Domain="StandardMapSizes"
             Value="MAPSIZE_LARGE"/>
        <Row Map="{my-inland-sea-map}maps/inland-sea.js"
             Domain="StandardMapSizes"
             Value="MAPSIZE_HUGE"/>
    </SupportedValuesByMap>

    <!-- Register start positions -->
    <SupportedValuesByMap>
        <Row Map="{my-inland-sea-map}maps/inland-sea.js"
             Domain="StandardMapStartPositions"
             Value="START_POSITION_BALANCED"/>
        <Row Map="{my-inland-sea-map}maps/inland-sea.js"
             Domain="StandardMapStartPositions"
             Value="START_POSITION_STANDARD"/>
    </SupportedValuesByMap>
</Database>
```

## Step 3: Create the Map Generation Script

**maps/inland-sea.js**
```javascript
/**
 * Inland Sea Map Generator
 * Creates a map with a large central sea surrounded by land
 */

// ============================================
// CONFIGURATION
// ============================================

const MAP_CONFIG = {
    // Sea size as percentage of map
    seaWidthPercent: 0.4,
    seaHeightPercent: 0.5,

    // Land border around sea
    minLandBorder: 6,

    // Terrain distribution
    hillChance: 0.15,
    mountainChance: 0.05,
    forestChance: 0.25,

    // Resource settings
    resourceDensity: 1.0
};

// ============================================
// MAP INIT DATA
// ============================================

/**
 * Called when the game needs map initialization parameters
 */
function requestMapData(initParams) {
    // Configure map parameters based on settings
    const mapData = {
        width: initParams.width,
        height: initParams.height,
        wrapX: true,
        wrapY: false
    };

    engine.call("SetMapInitData", mapData);
}

// ============================================
// MAIN GENERATION FUNCTION
// ============================================

/**
 * Main entry point called by the game engine via engine event
 */
function generateMap() {
    console.log("Generating Inland Sea map...");

    const width = GameplayMap.getGridWidth();
    const height = GameplayMap.getGridHeight();
    const seed = GameplayMap.getRandomSeed();

    console.log(`Map size: ${width}x${height}, seed: ${seed}`);

    // Phase 1: Generate base terrain (land/water)
    generateBaseTerrain(width, height);

    // Phase 2: Add elevation (hills/mountains)
    generateElevation(width, height);

    // Phase 3: Add terrain features (forests, rivers)
    generateFeatures(width, height);

    // Phase 4: Place resources
    placeResources(width, height);

    // Phase 5: Place natural wonders
    placeNaturalWonders(width, height);

    // Store water data for pathfinding
    TerrainBuilder.storeWaterData();

    console.log("Map generation complete!");
}

// ============================================
// TERRAIN GENERATION
// ============================================

function generateBaseTerrain(width, height) {
    // Calculate sea boundaries
    const seaWidth = Math.floor(width * MAP_CONFIG.seaWidthPercent);
    const seaHeight = Math.floor(height * MAP_CONFIG.seaHeightPercent);

    const seaLeft = Math.floor((width - seaWidth) / 2);
    const seaRight = seaLeft + seaWidth;
    const seaTop = Math.floor((height - seaHeight) / 2);
    const seaBottom = seaTop + seaHeight;

    // Generate each tile
    for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
            let terrainType;
            let biomeType;

            // Check if this tile is in the central sea
            if (isInSea(x, y, seaLeft, seaRight, seaTop, seaBottom)) {
                // Determine if coast or ocean based on distance from shore
                const distFromShore = getDistanceFromShore(
                    x, y, seaLeft, seaRight, seaTop, seaBottom
                );

                if (distFromShore <= 2) {
                    terrainType = "TERRAIN_COAST";
                } else {
                    terrainType = "TERRAIN_OCEAN";
                }
                biomeType = "BIOME_MARINE";
            } else {
                // Land tile - determine biome based on latitude
                terrainType = "TERRAIN_FLAT";
                biomeType = getBiomeByLatitude(y, height);
            }

            // Set the terrain
            setTerrain(x, y, terrainType, biomeType);
        }
    }
}

function isInSea(x, y, seaLeft, seaRight, seaTop, seaBottom) {
    // Add some noise to the sea edges for organic shape
    const noise = TerrainBuilder.getRandomNumber(3, "sea_edge") - 1;

    return x >= seaLeft + noise &&
           x < seaRight - noise &&
           y >= seaTop + noise &&
           y < seaBottom - noise;
}

function getDistanceFromShore(x, y, seaLeft, seaRight, seaTop, seaBottom) {
    const distLeft = x - seaLeft;
    const distRight = seaRight - x;
    const distTop = y - seaTop;
    const distBottom = seaBottom - y;

    return Math.min(distLeft, distRight, distTop, distBottom);
}

function getBiomeByLatitude(y, height) {
    const latitude = y / height;

    // Polar regions at top and bottom
    if (latitude < 0.1 || latitude > 0.9) {
        return "BIOME_TUNDRA";
    }
    // Temperate regions
    if (latitude < 0.25 || latitude > 0.75) {
        return "BIOME_GRASSLAND";
    }
    // Arid regions near equator edges
    if (latitude < 0.35 || latitude > 0.65) {
        return "BIOME_PLAINS";
    }
    // Tropical near center
    return "BIOME_TROPICAL";
}

function setTerrain(x, y, terrainType, biomeType) {
    const terrainHash = Database.makeHash(terrainType);
    const biomeHash = Database.makeHash(biomeType);

    TerrainBuilder.setTerrainType(x, y, terrainHash);
    TerrainBuilder.setBiomeType(x, y, biomeHash);
}

// ============================================
// ELEVATION GENERATION
// ============================================

function generateElevation(width, height) {
    for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
            // Skip water tiles
            const terrain = GameplayMap.getTerrainType(x, y);
            if (isWater(terrain)) continue;

            // Random chance for hills
            const roll = TerrainBuilder.getRandomNumber(100, "elevation");

            if (roll < MAP_CONFIG.mountainChance * 100) {
                // Mountain
                TerrainBuilder.setElevation(x, y, 2);
            } else if (roll < (MAP_CONFIG.hillChance + MAP_CONFIG.mountainChance) * 100) {
                // Hill
                TerrainBuilder.setElevation(x, y, 1);
            }
            // else flat (elevation 0)
        }
    }
}

function isWater(terrainType) {
    const coastHash = Database.makeHash("TERRAIN_COAST");
    const oceanHash = Database.makeHash("TERRAIN_OCEAN");
    return terrainType === coastHash || terrainType === oceanHash;
}

// ============================================
// FEATURE GENERATION
// ============================================

function generateFeatures(width, height) {
    for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
            const terrain = GameplayMap.getTerrainType(x, y);
            if (isWater(terrain)) continue;

            const biome = GameplayMap.getBiomeType(x, y);
            const elevation = GameplayMap.getElevation(x, y);

            // Skip mountains
            if (elevation >= 2) continue;

            // Determine appropriate feature based on biome
            const featureType = getFeatureForBiome(biome, elevation);

            if (featureType && canPlaceFeature(x, y, featureType)) {
                const roll = TerrainBuilder.getRandomNumber(100, "feature");
                if (roll < MAP_CONFIG.forestChance * 100) {
                    placeFeature(x, y, featureType);
                }
            }
        }
    }
}

function getFeatureForBiome(biomeHash, elevation) {
    const grasslandHash = Database.makeHash("BIOME_GRASSLAND");
    const plainsHash = Database.makeHash("BIOME_PLAINS");
    const tropicalHash = Database.makeHash("BIOME_TROPICAL");
    const tundraHash = Database.makeHash("BIOME_TUNDRA");

    if (biomeHash === grasslandHash) {
        return elevation === 0 ? "FEATURE_FOREST" : "FEATURE_FOREST_HILL";
    }
    if (biomeHash === plainsHash) {
        return elevation === 0 ? "FEATURE_SAVANNA" : null;
    }
    if (biomeHash === tropicalHash) {
        return elevation === 0 ? "FEATURE_JUNGLE" : "FEATURE_JUNGLE_HILL";
    }
    if (biomeHash === tundraHash) {
        return "FEATURE_TUNDRA_FOREST";
    }

    return null;
}

function canPlaceFeature(x, y, featureType) {
    const featureHash = Database.makeHash(featureType);
    return TerrainBuilder.canHaveFeatureParam(x, y, featureHash);
}

function placeFeature(x, y, featureType) {
    const featureHash = Database.makeHash(featureType);
    TerrainBuilder.setFeatureType(x, y, featureHash);
}

// ============================================
// RESOURCE PLACEMENT
// ============================================

function placeResources(width, height) {
    console.log("Placing resources...");

    // Get valid resources for current age
    const validResources = [];
    for (const resource of GameInfo.Resources) {
        if (ResourceBuilder.isResourceValidForAge(resource, Game.age)) {
            validResources.push(resource);
        }
    }

    // Place each resource type
    for (const resource of validResources) {
        const count = calculateResourceCount(resource, width, height);
        placeResourceType(resource, count, width, height);
    }
}

function calculateResourceCount(resource, width, height) {
    const baseCount = Math.floor((width * height) / 500);
    const weight = resource.Weight || 10;
    return Math.max(1, Math.floor(baseCount * weight / 10 * MAP_CONFIG.resourceDensity));
}

function placeResourceType(resource, targetCount, width, height) {
    let placed = 0;
    let attempts = 0;
    const maxAttempts = targetCount * 10;

    while (placed < targetCount && attempts < maxAttempts) {
        attempts++;

        const x = TerrainBuilder.getRandomNumber(width, "resource_x");
        const y = TerrainBuilder.getRandomNumber(height, "resource_y");

        if (ResourceBuilder.canHaveResource(x, y, resource.ResourceType, true)) {
            ResourceBuilder.setResourceType(x, y, resource.$hash);
            placed++;
        }
    }

    console.log(`Placed ${placed}/${targetCount} ${resource.ResourceType}`);
}

// ============================================
// NATURAL WONDERS
// ============================================

function placeNaturalWonders(width, height) {
    const requestedWonders = Configuration.getMapValue("RequestedNaturalWonders") || 5;

    console.log(`Placing ${requestedWonders} natural wonders...`);

    // Get available natural wonders
    const wonders = [];
    for (const wonder of GameInfo.Feature_NaturalWonders) {
        wonders.push(wonder);
    }

    // Shuffle for randomness
    const shuffledWonders = shuffle([...wonders]);

    let placed = 0;
    for (const wonder of shuffledWonders) {
        if (placed >= requestedWonders) break;

        if (placeNaturalWonder(wonder, width, height)) {
            placed++;
            console.log(`Placed natural wonder: ${wonder.FeatureType}`);
        }
    }
}

function placeNaturalWonder(wonder, width, height) {
    const maxAttempts = 100;

    for (let i = 0; i < maxAttempts; i++) {
        const x = TerrainBuilder.getRandomNumber(width, "wonder_x");
        const y = TerrainBuilder.getRandomNumber(height, "wonder_y");

        // Check if valid placement
        if (canPlaceNaturalWonder(x, y, wonder)) {
            const wonderHash = Database.makeHash(wonder.FeatureType);
            TerrainBuilder.setFeatureType(x, y, wonderHash);
            return true;
        }
    }

    return false;
}

function canPlaceNaturalWonder(x, y, wonder) {
    // Must be on land
    const terrain = GameplayMap.getTerrainType(x, y);
    if (isWater(terrain)) return false;

    // Must not have existing feature
    const existingFeature = GameplayMap.getFeatureType(x, y);
    if (existingFeature !== 0) return false;

    // Check if the wonder can be placed here
    const wonderHash = Database.makeHash(wonder.FeatureType);
    return TerrainBuilder.canHaveFeatureParam(x, y, wonderHash);
}

// ============================================
// UTILITY FUNCTIONS
// ============================================

/**
 * Fisher-Yates shuffle
 */
function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = TerrainBuilder.getRandomNumber(i + 1, "shuffle");
        [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
}

// ============================================
// REGISTER EVENT HANDLERS
// ============================================

// Register for map generation events
// The game calls these when this map type is selected
engine.on("RequestMapInitData", requestMapData);
engine.on("GenerateMap", generateMap);
```

> **Important:** Map scripts use `engine.on()` to register for generation events. The game calls `RequestMapInitData` first to get map parameters, then `GenerateMap` to generate the terrain. Do NOT use `export function` - the game does not use ES module exports for map scripts.

## Step 4: Add Localization

**text/text.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <EnglishText>
        <Row Tag="LOC_MAP_INLAND_SEA_NAME">
            <Text>Inland Sea</Text>
        </Row>
        <Row Tag="LOC_MAP_INLAND_SEA_DESCRIPTION">
            <Text>A large central sea surrounded by land on all sides, perfect for naval trade and coastal empires.</Text>
        </Row>
    </EnglishText>
</Database>
```

> **Note:** Use language-specific table names like `EnglishText`, `FrenchText`, etc. Do NOT use a `Language` attribute on rows.

## Key JavaScript APIs

### TerrainBuilder

| Method | Description |
|--------|-------------|
| `setTerrainType(x, y, terrainHash)` | Set terrain at coordinates |
| `setBiomeType(x, y, biomeHash)` | Set biome at coordinates |
| `setElevation(x, y, level)` | Set elevation (0=flat, 1=hill, 2=mountain) |
| `setFeatureType(x, y, featureHash)` | Place a feature |
| `canHaveFeatureParam(x, y, featureHash)` | Check if feature can be placed |
| `getRandomNumber(max, description)` | Seeded random number |
| `storeWaterData()` | Finalize water for pathfinding |

### GameplayMap

| Method | Description |
|--------|-------------|
| `getGridWidth()` | Map width in tiles |
| `getGridHeight()` | Map height in tiles |
| `getRandomSeed()` | Map generation seed |
| `getTerrainType(x, y)` | Get terrain at coordinates |
| `getBiomeType(x, y)` | Get biome at coordinates |
| `getFeatureType(x, y)` | Get feature at coordinates |
| `getElevation(x, y)` | Get elevation at coordinates |

### ResourceBuilder

| Method | Description |
|--------|-------------|
| `setResourceType(x, y, resourceHash)` | Place resource |
| `canHaveResource(x, y, type, strict)` | Check if resource can be placed |
| `isResourceValidForAge(resource, age)` | Check age validity |

### Database

| Method | Description |
|--------|-------------|
| `makeHash(string)` | Convert type string to hash |

### GameInfo Tables

| Table | Contents |
|-------|----------|
| `GameInfo.Resources` | All resource definitions |
| `GameInfo.Features` | All feature definitions |
| `GameInfo.Feature_NaturalWonders` | Natural wonder list |
| `GameInfo.Terrains` | Terrain types |

## Terrain Types

| Type | Description |
|------|-------------|
| `TERRAIN_FLAT` | Flat land |
| `TERRAIN_HILL` | Hills (use elevation instead) |
| `TERRAIN_COAST` | Shallow water |
| `TERRAIN_OCEAN` | Deep water |

## Biome Types

| Type | Climate |
|------|---------|
| `BIOME_GRASSLAND` | Temperate |
| `BIOME_PLAINS` | Savanna/Steppe |
| `BIOME_DESERT` | Arid |
| `BIOME_TUNDRA` | Cold |
| `BIOME_TROPICAL` | Jungle |
| `BIOME_MARINE` | Water |

## Step 5: Install and Test

1. Copy mod folder to:
   - **Windows**: `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Mods\`

2. Enable the mod and start a new game

3. In Advanced Setup, find "Inland Sea" in the map type dropdown

4. Start the game and verify the map generates correctly

## Debugging Tips

1. **Use console.log** - Messages appear in the game log
2. **Check Database.log** - Shows script loading errors
3. **Enable debug panels** - Press backtick (`) in-game with debug enabled
4. **Test with small maps** - Faster iteration on Tiny size

## Advanced: Custom Map Options

You can add custom configuration options:

**config/maps-config.xml**
```xml
<DomainValues>
    <Row Domain="InlandSeaOptions"
         Value="SEA_SIZE_SMALL"
         Name="LOC_SEA_SIZE_SMALL"
         SortIndex="1"/>
    <Row Domain="InlandSeaOptions"
         Value="SEA_SIZE_MEDIUM"
         Name="LOC_SEA_SIZE_MEDIUM"
         SortIndex="2"/>
    <Row Domain="InlandSeaOptions"
         Value="SEA_SIZE_LARGE"
         Name="LOC_SEA_SIZE_LARGE"
         SortIndex="3"/>
</DomainValues>
```

Then in your script:

```javascript
const seaSize = Configuration.getMapValue("InlandSeaOptions");
if (seaSize === "SEA_SIZE_LARGE") {
    MAP_CONFIG.seaWidthPercent = 0.6;
}
```

## Next Steps

- [JavaScript API Overview](../javascript/Overview.md)
- [Maps & Resources Reference](../maps-resources/Overview.md)
- [Map-Terrain Database](../database/Map-Terrain.md)
