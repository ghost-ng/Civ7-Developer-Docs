# Requirement Types Reference

Requirements are conditions that control when modifiers activate. This document lists available requirement types.

## Overview

Requirements are combined into RequirementSets:
- `REQUIREMENTSET_TEST_ALL` - All requirements must pass
- `REQUIREMENTSET_TEST_ANY` - Any requirement can pass

Requirements can be inverted with `Inverse="true"`.

## City Requirements

### REQUIREMENT_CITY_HAS_BUILDING

Checks if city has a specific building.

| Argument | Type | Description |
|----------|------|-------------|
| `BuildingType` | TEXT | Building to check for |

```xml
<Requirement type="REQUIREMENT_CITY_HAS_BUILDING">
    <Argument name="BuildingType">BUILDING_GRANARY</Argument>
</Requirement>
```

### REQUIREMENT_CITY_HAS_BUILD_QUEUE

Checks if city has a build queue (is a full city, not a town).

```xml
<Requirement type="REQUIREMENT_CITY_HAS_BUILD_QUEUE"/>
```

Use `Inverse="true"` to check for towns:
```xml
<Requirement type="REQUIREMENT_CITY_HAS_BUILD_QUEUE" Inverse="true"/>
```

### REQUIREMENT_CITY_IS_CITY

Checks if settlement is a city (not town).

### REQUIREMENT_CITY_FOUNDED_WITH_X_BIOME_TILES

Checks biome tiles at city founding.

| Argument | Type | Description |
|----------|------|-------------|
| `BiomeType` | TEXT | Biome type |
| `Amount` | INTEGER | Minimum tiles |

## Plot Requirements

### REQUIREMENT_PLOT_BIOME_TYPE_MATCHES

Checks plot biome.

| Argument | Type | Description |
|----------|------|-------------|
| `BiomeType` | TEXT | `BIOME_DESERT`, `BIOME_TUNDRA`, etc. |

### REQUIREMENT_PLOT_TERRAIN_TYPE_MATCHES

Checks terrain type.

| Argument | Type | Description |
|----------|------|-------------|
| `TerrainType` | TEXT | Terrain type |

### REQUIREMENT_PLOT_DISTRICT_CLASS

Checks district class on the plot.

| Argument | Type | Description |
|----------|------|-------------|
| `DistrictClass` | TEXT | `CITYCENTER`, `URBAN`, `RURAL`, `WONDER` |

Can check multiple:
```xml
<Requirement type="REQUIREMENT_PLOT_DISTRICT_CLASS">
    <Argument name="DistrictClass">RURAL, URBAN, CITYCENTER</Argument>
</Requirement>
```

### REQUIREMENT_PLOT_IS_RIVER

Checks if plot is on a river.

| Argument | Type | Description |
|----------|------|-------------|
| `Navigable` | BOOLEAN | Require navigable river |

### REQUIREMENT_PLOT_IS_LAKE

Checks if plot is a lake.

### REQUIREMENT_PLOT_ADJACENT_TO_COAST

Checks if plot is adjacent to coast.

### REQUIREMENT_PLOT_ADJACENT_TO_LAKE

Checks if plot is adjacent to a lake.

### REQUIREMENT_PLOT_ADJACENT_TO_RIVER

Checks if plot is adjacent to a river.

### REQUIREMENT_PLOT_RESOURCE_VISIBLE

Checks if plot has a visible resource.

### REQUIREMENT_PLOT_IS_HOMELANDS

Checks if plot is in player's homelands (near capital hemisphere).

### REQUIREMENT_PLOT_ADJACENT_TO_OWNER

Checks if plot is adjacent to owner's territory.

## Unit Requirements

### REQUIREMENT_UNIT_TAG_MATCHES

Checks if unit has a specific tag.

| Argument | Type | Description |
|----------|------|-------------|
| `Tag` | TEXT | Unit class tag |

```xml
<Requirement type="REQUIREMENT_UNIT_TAG_MATCHES">
    <Argument name="Tag">UNIT_CLASS_INFANTRY</Argument>
</Requirement>
```

### REQUIREMENT_UNIT_DOMAIN_MATCHES

Checks unit domain.

| Argument | Type | Description |
|----------|------|-------------|
| `UnitDomain` | TEXT | `DOMAIN_LAND`, `DOMAIN_SEA`, `DOMAIN_AIR` |

### REQUIREMENT_UNIT_CLASS_MATCHES

Checks unit class.

| Argument | Type | Description |
|----------|------|-------------|
| `UnitClass` | TEXT | Unit class |

## Player Requirements

### REQUIREMENT_PLAYER_IS_HUMAN

Checks if player is human (not AI).

Use `Inverse="true"` to check for AI:
```xml
<Requirement type="REQUIREMENT_PLAYER_IS_HUMAN" Inverse="true"/>
```

### REQUIREMENT_PLAYER_IS_ATTACKING

Checks if player is the attacker in combat.

### REQUIREMENT_PLAYER_IS_MAJOR

Checks if player is a major civilization.

### REQUIREMENT_PLAYER_IS_MINOR

Checks if player is a minor civilization (city-state).

### REQUIREMENT_PLAYER_HAS_COMPLETED_PROGRESSION_TREE_NODE

Checks if player has researched a tech/civic.

| Argument | Type | Description |
|----------|------|-------------|
| `ProgressionTreeNodeType` | TEXT | Tech/civic node |
| `MinDepth` | INTEGER | Required completion depth |

```xml
<Requirement type="REQUIREMENT_PLAYER_HAS_COMPLETED_PROGRESSION_TREE_NODE">
    <Argument name="ProgressionTreeNodeType">NODE_TECH_AQ_MASONRY</Argument>
    <Argument name="MinDepth">1</Argument>
</Requirement>
```

## Meta Requirements

### REQUIREMENT_REQUIREMENTSET_IS_MET

Checks if another requirement set is met.

| Argument | Type | Description |
|----------|------|-------------|
| `RequirementSetId` | TEXT | Requirement set to check |

```xml
<Requirement type="REQUIREMENT_REQUIREMENTSET_IS_MET">
    <Argument name="RequirementSetId">REQSET_UNIT_CLASS_INFANTRY</Argument>
</Requirement>
```

## Database Definition

In traditional database format:

```xml
<Requirements>
    <Row RequirementId="REQ_MY_CHECK" RequirementType="REQUIREMENT_CITY_HAS_BUILDING"/>
</Requirements>
<RequirementArguments>
    <Row RequirementId="REQ_MY_CHECK" Name="BuildingType" Value="BUILDING_GRANARY"/>
</RequirementArguments>
<RequirementSetRequirements>
    <Row RequirementSetId="MY_REQUIREMENTS" RequirementId="REQ_MY_CHECK"/>
</RequirementSetRequirements>
```

## GameEffects XML Format

In GameEffects format:

```xml
<Modifier id="MY_MODIFIER" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
    <SubjectRequirements type="REQUIREMENTSET_TEST_ALL">
        <Requirement type="REQUIREMENT_CITY_HAS_BUILDING">
            <Argument name="BuildingType">BUILDING_GRANARY</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_FOOD</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

## Common Predefined RequirementSets

| RequirementSetId | Description |
|------------------|-------------|
| `REQSET_URBAN_PLOTS` | Urban or city center plots |
| `REQSET_RURAL_PLOTS` | Rural plots |
| `REQSET_ONLY_CITIES` | Full cities only |
| `REQSET_ONLY_TOWNS` | Towns only |
| `REQSET_BIOME_TUNDRA` | Tundra biome |
| `REQSET_UNIT_CLASS_INFANTRY` | Infantry units |
| `REQSET_UNIT_CLASS_MOUNTED` | Mounted units |
| `REQSET_UNIT_CLASS_NAVAL` | Naval units |
| `REQSET_PLAYER_IS_AI` | AI players |
| `REQSET_PLAYER_IS_MAJOR` | Major civilizations |

## See Also

- [Effect Types](Effect-Types.md)
- [Collection Types](Collection-Types.md)
- [Modifier System Overview](Overview.md)
