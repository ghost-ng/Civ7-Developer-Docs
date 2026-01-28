# Effect Types Reference

This document lists the available effect types for modifiers in Civilization VII.

## Yield Effects

### EFFECT_CITY_ADJUST_YIELD

Adjusts a city's yield output.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | `YIELD_FOOD`, `YIELD_PRODUCTION`, etc. |
| `Amount` | INTEGER | Flat bonus |
| `Percent` | INTEGER | Percentage bonus |
| `Tooltip` | TEXT | Tooltip localization key |

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_FOOD</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

### EFFECT_PLOT_ADJUST_YIELD

Adjusts yield from a specific plot.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Flat bonus |

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_PLOT_YIELDS" effect="EFFECT_PLOT_ADJUST_YIELD">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLOT_BIOME_TYPE_MATCHES">
            <Argument name="BiomeType">BIOME_DESERT</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_PRODUCTION</Argument>
    <Argument name="Amount">1</Argument>
</Modifier>
```

### EFFECT_PLAYER_GRANT_YIELD

Grants a one-time yield to the player.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Amount to grant |

### EFFECT_PLAYER_ADJUST_YIELD_PER_NUM_TRADE_ROUTES

Adjusts yield based on trade route count.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Per trade route |
| `Tooltip` | TEXT | Tooltip key |

### EFFECT_CITY_ADJUST_WORKER_YIELD

Adjusts yield per worker in cities.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Per worker |
| `Tooltip` | TEXT | Tooltip key |

## Production Effects

### EFFECT_CITY_ADJUST_WONDER_PRODUCTION

Adjusts wonder production speed.

| Argument | Type | Description |
|----------|------|-------------|
| `Percent` | INTEGER | Percentage bonus |

### EFFECT_CITY_ADJUST_CONSTRUCTIBLE_PRODUCTION

Adjusts constructible production speed.

| Argument | Type | Description |
|----------|------|-------------|
| `ConstructibleType` | TEXT | Specific constructible (optional) |
| `Percent` | INTEGER | Percentage bonus |

### EFFECT_CITY_ADJUST_UNIT_PRODUCTION

Adjusts unit production speed.

| Argument | Type | Description |
|----------|------|-------------|
| `UnitType` | TEXT | Specific unit (optional) |
| `Percent` | INTEGER | Percentage bonus |

## Constructible Effects

### EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD

Adjusts yields from constructibles.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Bonus amount |
| `ConstructibleType` | TEXT | Specific type (optional) |
| `Tag` | TEXT | Constructible tag (optional) |

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_OWNER" effect="EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD">
    <Argument name="YieldType">YIELD_SCIENCE</Argument>
    <Argument name="Amount">1</Argument>
    <Argument name="Tag">SCIENCE</Argument>
</Modifier>
```

## Unit Effects

### EFFECT_UNIT_ADJUST_COMBAT_STRENGTH

Adjusts unit combat strength.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Strength modifier |

### EFFECT_ADJUST_UNIT_STRENGTH_MODIFIER

Adjusts unit strength as a percentage.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Percentage modifier |

### EFFECT_UNIT_ADJUST_HEAL_PER_TURN

Adjusts unit healing rate.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Healing per turn |

### EFFECT_UNIT_ADJUST_ABILITY

Grants an ability to units.

| Argument | Type | Description |
|----------|------|-------------|
| `AbilityType` | TEXT | Ability to grant |

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_UNITS" effect="EFFECT_UNIT_ADJUST_ABILITY">
    <Argument name="AbilityType">ABILITY_AMPHIBIOUS</Argument>
</Modifier>
```

### EFFECT_ARMY_ADJUST_EXPERIENCE_RATE

Adjusts XP gain rate for armies.

| Argument | Type | Description |
|----------|------|-------------|
| `Percent` | INTEGER | XP rate modifier |

### EFFECT_ADJUST_UNIT_VALID_TERRAIN

Allows units to traverse terrain.

| Argument | Type | Description |
|----------|------|-------------|
| `TerrainType` | TEXT | Terrain type |

### EFFECT_ADJUST_UNIT_WATER_DAMAGE_PROTECTION

Protects units from water damage.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Protection level |

## Grant Effects

### EFFECT_CITY_GRANT_UNIT

Grants a unit to a city.

| Argument | Type | Description |
|----------|------|-------------|
| `UnitType` | TEXT | Unit to grant |

### EFFECT_GRANT_GREAT_WORK

Grants a great work.

| Argument | Type | Description |
|----------|------|-------------|
| `GreatWorkType` | TEXT | Great work type |

### EFFECT_PLAYER_GRANT_PROGRESSION

Grants progression in a tech/civic tree.

| Argument | Type | Description |
|----------|------|-------------|
| `ProgressionTreeType` | TEXT | Tree type |
| `Amount` | INTEGER | Progress amount |

### EFFECT_GRANT_WALLS

Grants walls to cities.

| Argument | Type | Description |
|----------|------|-------------|
| `WallLevel` | INTEGER | Wall level |

## Diplomacy Effects

### EFFECT_DIPLOMACY_ADJUST_DIPLOMATIC_ACTION_TYPE_EFFICIENCY

Adjusts diplomacy action efficiency.

| Argument | Type | Description |
|----------|------|-------------|
| `ActionType` | TEXT | Specific action (optional) |
| `ActionGroupType` | TEXT | Action group (optional) |
| `Percent` | INTEGER | Efficiency modifier |

## Scoring Effects

### EFFECT_PLAYER_ADJUST_LEGACY_PATH_HIGH_YIELD_DISTRICT_SCORING

Adjusts scoring for high-yield districts.

### EFFECT_PLAYER_ADJUST_LEGACY_PATH_GREAT_WORK_SCORING

Adjusts great work scoring.

### EFFECT_PLAYER_ADJUST_LEGACY_PATH_RESOURCE_SCORING

Adjusts resource scoring.

### EFFECT_PLAYER_ADJUST_LEGACY_PATH_SETTLEMENT_SCORING

Adjusts settlement scoring.

### EFFECT_PLAYER_ADJUST_LEGACY_PATH_WONDER_SCORING

Adjusts wonder scoring.

## Progression Effects

### EFFECT_PLAYER_ADJUST_PROGRESSION_TREE_EFFICIENCY

Adjusts research/civic speed.

| Argument | Type | Description |
|----------|------|-------------|
| `ProgressionTreeType` | TEXT | Tree type |
| `Percent` | INTEGER | Speed modifier |

## Maintenance Effects

### EFFECT_CITY_ADJUST_WORKER_MAINTENANCE_EFFICIENCY

Adjusts worker maintenance costs.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type(s) |
| `Percent` | INTEGER | Efficiency bonus |

## Finding More Effects

To discover all available effects:

1. Search game files for `EffectType="EFFECT_`
2. Check `gameplay-copy.sqlite` in the debug folder
3. Look at `*-gameeffects.xml` files in `modules/base-standard/data/`

## See Also

- [Collection Types](Collection-Types.md)
- [Requirement Types](Requirement-Types.md)
- [Modifier System Overview](Overview.md)
