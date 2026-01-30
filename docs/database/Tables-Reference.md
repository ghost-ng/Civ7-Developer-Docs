# Database Tables Reference

This document provides a comprehensive reference for the **673 database tables** used in Civilization VII. Tables are organized by category with column definitions and examples.

## Quick Navigation

- [Core Type System](#core-type-system)
- [Civilization & Leadership](#civilization--leadership)
- [Unit System](#unit-system)
- [Building & District System](#building--district-system)
- [Map & Terrain](#map--terrain)
- [Resource System](#resource-system)
- [Modifier System](#modifier-system)
- [Progression System](#progression-system)
- [Diplomacy System](#diplomacy-system)
- [Religion System](#religion-system)
- [Narrative & Events](#narrative--events)
- [AI System](#ai-system)
- [Game Configuration](#game-configuration)
- [Victory & Scoring](#victory--scoring)
- [Worldbuilder Tables](#worldbuilder-tables)
- [Localization Tables](#localization-tables)
- [Modding Framework](#modding-framework)

---

## Core Type System

### Types

The foundation of all game entities.

| Column | Type | Description |
|--------|------|-------------|
| `Type` | TEXT | Primary key, unique identifier |
| `Hash` | INT | Hash value for fast lookup |
| `Kind` | TEXT | References Kinds table |

```xml
<Types>
    <Row Type="UNIT_WARRIOR" Kind="KIND_UNIT"/>
    <Row Type="BUILDING_GRANARY" Kind="KIND_CONSTRUCTIBLE"/>
</Types>
```

### Kinds

Defines entity categories.

| Column | Type | Description |
|--------|------|-------------|
| `Kind` | TEXT | Primary key |

### Tags

Tag definitions for categorization.

| Column | Type | Description |
|--------|------|-------------|
| `Tag` | TEXT | Primary key |

### TypeTags

Associates types with tags.

| Column | Type | Description |
|--------|------|-------------|
| `Type` | TEXT | Foreign key to Types |
| `Tag` | TEXT | Foreign key to Tags |

```xml
<TypeTags>
    <Row Type="UNIT_SCOUT" Tag="UNIT_CLASS_RECON"/>
    <Row Type="UNIT_SCOUT" Tag="UNIT_CLASS_NON_COMBAT"/>
</TypeTags>
```

### TypeProperties

Key-value properties for types.

| Column | Type | Description |
|--------|------|-------------|
| `Type` | TEXT | Foreign key to Types |
| `Name` | TEXT | Property name |
| `Value` | TEXT | Property value |

---

## Civilization & Leadership

### Civilizations

Core civilization definitions.

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Primary key |
| `Adjective` | TEXT | Localization key for adjective |
| `CapitalName` | TEXT | Default capital city name |
| `FullName` | TEXT | Localization key for full name |
| `Name` | TEXT | Localization key for short name |
| `AITargetCityPercentage` | REAL | AI city targeting preference |
| `RandomCityNameDepth` | INT | Random city name pool depth |
| `StartingCivilizationLevelType` | TEXT | Starting level reference |

```xml
<Civilizations>
    <Row CivilizationType="CIVILIZATION_ROME" Name="LOC_CIVILIZATION_ROME_NAME"
         CapitalName="LOC_CITY_NAME_ROME" Adjective="LOC_CIVILIZATION_ROME_ADJECTIVE"/>
</Civilizations>
```

### CivilizationInfo

Extended civilization information.

### CivilizationTraits

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Foreign key to Civilizations |
| `TraitType` | TEXT | Foreign key to Types (KIND_TRAIT) |

### CivilizationCitizenNames

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Foreign key |
| `CitizenName` | TEXT | Citizen name text |
| `Female` | BOOL | Is female name |
| `Modern` | BOOL | Is modern era name |

### Leaders

| Column | Type | Description |
|--------|------|-------------|
| `LeaderType` | TEXT | Primary key |
| `BasePersonaType` | TEXT | Base personality |
| `DiscountRate` | REAL | Trade discount |
| `IsBarbarianLeader` | BOOL | Barbarian flag |
| `IsIndependentLeader` | BOOL | City-state flag |
| `IsMajorLeader` | BOOL | Major civ flag |
| `OperationList` | TEXT | AI operation list |
| `AITargetCityPercentage` | REAL | AI targeting |

```xml
<Leaders>
    <Row LeaderType="LEADER_AUGUSTUS" IsMajorLeader="true"/>
</Leaders>
```

### LeaderTraits

| Column | Type | Description |
|--------|------|-------------|
| `LeaderType` | TEXT | Foreign key to Leaders |
| `TraitType` | TEXT | Foreign key to Types |

### LeaderCivPairings

Defines valid leader-civilization combinations.

| Column | Type | Description |
|--------|------|-------------|
| `LeaderType` | TEXT | Foreign key to Leaders |
| `CivilizationType` | TEXT | Foreign key to Civilizations |

---

## Unit System

### Units

Core unit definitions.

| Column | Type | Description |
|--------|------|-------------|
| `UnitType` | TEXT | Primary key |
| `BaseMoves` | INT | Base movement points |
| `BaseSightRange` | INT | Base sight range |
| `BuildCharges` | INT | Builder charges |
| `CanBeDamaged` | BOOL | Can be damaged |
| `CanCapture` | BOOL | Can capture |
| `CanEarnExperience` | BOOL | Can earn XP |
| `CanPurchase` | BOOL | Can be purchased |
| `CanTrain` | BOOL | Can be trained |
| `CoreClass` | TEXT | Core unit class |
| `CostProgressionModel` | TEXT | Cost scaling |
| `AirSlots` | INT | Air unit capacity |

```xml
<Units>
    <Row UnitType="UNIT_WARRIOR" BaseMoves="2" BaseSightRange="2"
         CanEarnExperience="true" CanTrain="true"/>
</Units>
```

### Unit_Stats

| Column | Type | Description |
|--------|------|-------------|
| `UnitType` | TEXT | Foreign key |
| `Combat` | INT | Combat strength |
| `RangedCombat` | INT | Ranged strength |
| `Range` | INT | Attack range |
| `AntiAirCombat` | INT | Anti-air strength |

### Unit_Abilities

| Column | Type | Description |
|--------|------|-------------|
| `UnitType` | TEXT | Foreign key |
| `AbilityType` | TEXT | Ability reference |

### Unit_Costs

| Column | Type | Description |
|--------|------|-------------|
| `UnitType` | TEXT | Foreign key |
| `YieldType` | TEXT | Cost yield type |
| `Amount` | INT | Cost amount |

### UnitAbilities

| Column | Type | Description |
|--------|------|-------------|
| `UnitAbilityType` | TEXT | Primary key |
| `Inactive` | BOOL | Default inactive |
| `ShowFloatText` | BOOL | Show combat text |

### UnitPromotions

| Column | Type | Description |
|--------|------|-------------|
| `UnitPromotionType` | TEXT | Primary key |
| `Level` | INT | Required level |
| `PromotionClass` | TEXT | Promotion class |

### UnitPromotionModifiers

| Column | Type | Description |
|--------|------|-------------|
| `UnitPromotionType` | TEXT | Foreign key |
| `ModifierId` | TEXT | Modifier reference |

---

## Building & District System

### Buildings

| Column | Type | Description |
|--------|------|-------------|
| `ConstructibleType` | TEXT | Primary key |
| `AllowsHolyCity` | BOOL | Can create holy city |
| `Capital` | BOOL | Is capital building |
| `CitizenSlots` | INT | Worker slots |
| `DefenseModifier` | INT | Defense bonus |
| `Housing` | INT | Housing provided |
| `MaxPlayerInstances` | INT | Max per player |
| `Movable` | BOOL | Can be moved |
| `Purchasable` | BOOL | Can purchase |
| `Town` | BOOL | Town-only |
| `TraitType` | TEXT | Required trait |

```xml
<Buildings>
    <Row ConstructibleType="BUILDING_GRANARY" Housing="2" CitizenSlots="1"/>
</Buildings>
```

### Constructibles

Generic constructible items.

| Column | Type | Description |
|--------|------|-------------|
| `ConstructibleType` | TEXT | Primary key |
| `ConstructibleClass` | TEXT | Classification |
| `Cost` | INT | Base cost |
| `Maintenance` | INT | Per-turn cost |

### Constructible_YieldChanges

| Column | Type | Description |
|--------|------|-------------|
| `ConstructibleType` | TEXT | Foreign key |
| `YieldType` | TEXT | Yield reference |
| `YieldChange` | INT | Yield amount |

### Constructible_ValidDistricts

| Column | Type | Description |
|--------|------|-------------|
| `ConstructibleType` | TEXT | Foreign key |
| `DistrictType` | TEXT | Valid district |

### Districts

| Column | Type | Description |
|--------|------|-------------|
| `DistrictType` | TEXT | Primary key |
| `DistrictClass` | TEXT | District category |
| `CitizenSlots` | INT | Worker capacity |
| `CityStrengthModifier` | INT | City strength bonus |
| `HitPoints` | INT | District HP |
| `Maintenance` | INT | Per-turn cost |

```xml
<Districts>
    <Row DistrictType="DISTRICT_CITY_CENTER" DistrictClass="CITYCENTER"
         CitizenSlots="3" CityStrengthModifier="3"/>
</Districts>
```

---

## Map & Terrain

### Terrains

| Column | Type | Description |
|--------|------|-------------|
| `TerrainType` | TEXT | Primary key |
| `Appeal` | INT | Appeal value |
| `DefenseModifier` | INT | Combat modifier |
| `Hills` | BOOL | Is hills |
| `Impassable` | BOOL | Cannot traverse |
| `Mountain` | BOOL | Is mountain |
| `MovementCost` | INT | Movement cost |
| `Water` | BOOL | Is water |
| `SightModifier` | INT | Sight bonus |

### Biomes

| Column | Type | Description |
|--------|------|-------------|
| `BiomeType` | TEXT | Primary key |
| `Name` | TEXT | Localization key |

### Biome_ValidTerrains

| Column | Type | Description |
|--------|------|-------------|
| `BiomeType` | TEXT | Foreign key |
| `TerrainType` | TEXT | Valid terrain |

### Features

| Column | Type | Description |
|--------|------|-------------|
| `FeatureType` | TEXT | Primary key |
| `AddsFreshWater` | BOOL | Provides fresh water |
| `AllowSettlement` | BOOL | Can settle on |
| `Appeal` | INT | Appeal modifier |
| `DefenseModifier` | INT | Combat modifier |
| `Impassable` | BOOL | Cannot traverse |
| `MovementChange` | INT | Movement modifier |

### NaturalWonders

| Column | Type | Description |
|--------|------|-------------|
| `FeatureType` | TEXT | Primary key |
| `IsUnique` | BOOL | Single instance |

---

## Resource System

### Resources

| Column | Type | Description |
|--------|------|-------------|
| `ResourceType` | TEXT | Primary key |
| `Name` | TEXT | Localization key |
| `ResourceClassType` | TEXT | Resource class |
| `Clumped` | BOOL | Spawns in groups |
| `HemisphereUnique` | BOOL | Hemisphere-specific |
| `LakeEligible` | BOOL | Can spawn in lakes |
| `Staple` | BOOL | Is staple resource |

### Resource_YieldChanges

| Column | Type | Description |
|--------|------|-------------|
| `ResourceType` | TEXT | Foreign key |
| `YieldType` | TEXT | Yield reference |
| `YieldChange` | INT | Yield amount |

### ResourceClasses

| Column | Type | Description |
|--------|------|-------------|
| `ResourceClassType` | TEXT | Primary key |

Values: `RESOURCECLASS_BONUS`, `RESOURCECLASS_LUXURY`, `RESOURCECLASS_STRATEGIC`

---

## Modifier System

### Modifiers

Core modifier definitions.

| Column | Type | Description |
|--------|------|-------------|
| `ModifierId` | TEXT | Primary key |
| `ModifierType` | TEXT | References DynamicModifiers |
| `NewOnly` | BOOL | New game only |
| `OwnerRequirementSetId` | TEXT | Owner requirements |
| `OwnerStackLimit` | INT | Stacking limit |
| `Permanent` | BOOL | Cannot be removed |
| `RunOnce` | BOOL | One-time effect |
| `SubjectRequirementSetId` | TEXT | Subject requirements |
| `SubjectStackLimit` | INT | Subject stacking |

```xml
<Modifiers>
    <Row ModifierId="TRAIT_ROME_PRODUCTION" ModifierType="MODIFIER_PLAYER_CITIES_ADJUST_YIELD"/>
</Modifiers>
```

### ModifierArguments

| Column | Type | Description |
|--------|------|-------------|
| `ModifierId` | TEXT | Foreign key |
| `Name` | TEXT | Argument name |
| `Type` | TEXT | Value type |
| `Value` | TEXT | Argument value |

```xml
<ModifierArguments>
    <Row ModifierId="TRAIT_ROME_PRODUCTION" Name="YieldType" Value="YIELD_PRODUCTION"/>
    <Row ModifierId="TRAIT_ROME_PRODUCTION" Name="Amount" Value="2"/>
</ModifierArguments>
```

### DynamicModifiers

| Column | Type | Description |
|--------|------|-------------|
| `ModifierType` | TEXT | Primary key |
| `CollectionType` | TEXT | Subject collection |
| `EffectType` | TEXT | Effect applied |

```xml
<DynamicModifiers>
    <Row ModifierType="MODIFIER_PLAYER_CITIES_ADJUST_YIELD"
         CollectionType="COLLECTION_PLAYER_CITIES"
         EffectType="EFFECT_CITY_ADJUST_YIELD"/>
</DynamicModifiers>
```

### TraitModifiers

| Column | Type | Description |
|--------|------|-------------|
| `TraitType` | TEXT | Foreign key to Types |
| `ModifierId` | TEXT | Foreign key to Modifiers |

---

## Progression System

### ProgressionTrees

| Column | Type | Description |
|--------|------|-------------|
| `ProgressionTreeType` | TEXT | Primary key |
| `Name` | TEXT | Localization key |

### ProgressionTreeNodes

| Column | Type | Description |
|--------|------|-------------|
| `ProgressionTreeNodeType` | TEXT | Primary key |
| `ProgressionTreeType` | TEXT | Tree reference |
| `Depth` | INT | Tree depth/tier |
| `Cost` | INT | Research cost |

### ProgressionTreePrereqs

| Column | Type | Description |
|--------|------|-------------|
| `ProgressionTreeNodeType` | TEXT | Foreign key |
| `PrereqNodeType` | TEXT | Required node |

### Traditions

| Column | Type | Description |
|--------|------|-------------|
| `TraditionType` | TEXT | Primary key |
| `Name` | TEXT | Localization key |
| `Description` | TEXT | Localization key |

### TraditionModifiers

| Column | Type | Description |
|--------|------|-------------|
| `TraditionType` | TEXT | Foreign key |
| `ModifierId` | TEXT | Modifier reference |

---

## Diplomacy System

### DiplomacyActions

| Column | Type | Description |
|--------|------|-------------|
| `DiplomacyActionType` | TEXT | Primary key |
| `ActionGroup` | TEXT | Action grouping |

### DiplomacyStatements

| Column | Type | Description |
|--------|------|-------------|
| `Type` | TEXT | Primary key |
| `StatementText` | TEXT | Localization key |

### DiplomaticActionStages

| Column | Type | Description |
|--------|------|-------------|
| `DiplomacyActionType` | TEXT | Foreign key |
| `Stage` | INT | Stage number |

---

## Religion System

### Religions

| Column | Type | Description |
|--------|------|-------------|
| `ReligionType` | TEXT | Primary key |
| `Name` | TEXT | Localization key |
| `IconString` | TEXT | Icon reference |

### Beliefs

| Column | Type | Description |
|--------|------|-------------|
| `BeliefType` | TEXT | Primary key |
| `BeliefClassType` | TEXT | Belief category |
| `Name` | TEXT | Localization key |

### BeliefModifiers

| Column | Type | Description |
|--------|------|-------------|
| `BeliefType` | TEXT | Foreign key |
| `ModifierId` | TEXT | Modifier reference |

---

## Narrative & Events

### NarrativeStories

| Column | Type | Description |
|--------|------|-------------|
| `NarrativeStoryType` | TEXT | Primary key |
| `Name` | TEXT | Localization key |

### NarrativeStory_Rewards

| Column | Type | Description |
|--------|------|-------------|
| `NarrativeStoryType` | TEXT | Foreign key |
| `RewardType` | TEXT | Reward reference |

### RandomEvents

| Column | Type | Description |
|--------|------|-------------|
| `RandomEventType` | TEXT | Primary key |
| `RandomEventClass` | TEXT | Event category |

---

## AI System

### AiOperationDefs

| Column | Type | Description |
|--------|------|-------------|
| `OperationType` | TEXT | Primary key |
| `Priority` | INT | Operation priority |

### AiFavoredItems

| Column | Type | Description |
|--------|------|-------------|
| `ListType` | TEXT | AI list type |
| `Item` | TEXT | Favored item |
| `Favored` | BOOL | Is favored |

### Strategies

| Column | Type | Description |
|--------|------|-------------|
| `StrategyType` | TEXT | Primary key |
| `NumConditions` | INT | Required conditions |

---

## Game Configuration

### Ages

| Column | Type | Description |
|--------|------|-------------|
| `AgeType` | TEXT | Primary key |
| `ChronologyIndex` | INT | Age order |
| `Name` | TEXT | Localization key |
| `StartingTraditionSlots` | INT | Initial tradition slots |

### GameSpeeds

| Column | Type | Description |
|--------|------|-------------|
| `GameSpeedType` | TEXT | Primary key |
| `CostMultiplier` | REAL | Cost scaling |
| `TechCostMultiplier` | REAL | Tech scaling |

### Difficulties

| Column | Type | Description |
|--------|------|-------------|
| `DifficultyType` | TEXT | Primary key |
| `Name` | TEXT | Localization key |

### GlobalParameters

| Column | Type | Description |
|--------|------|-------------|
| `Name` | TEXT | Primary key |
| `Value` | TEXT | Parameter value |

---

## Victory & Scoring

### Victories

| Column | Type | Description |
|--------|------|-------------|
| `VictoryType` | TEXT | Primary key |
| `Name` | TEXT | Localization key |

### VictoryScorings

| Column | Type | Description |
|--------|------|-------------|
| `VictoryType` | TEXT | Foreign key |
| `ScoringType` | TEXT | Scoring method |

---

## Worldbuilder Tables

These tables are used by the map editor.

### Map

| Column | Type | Description |
|--------|------|-------------|
| `ID` | INT | Primary key |
| `Width` | INT | Map width |
| `Height` | INT | Map height |
| `WrapX` | BOOL | Horizontal wrap |
| `WrapY` | BOOL | Vertical wrap |

### Plots

| Column | Type | Description |
|--------|------|-------------|
| `ID` | INT | Primary key |
| `TerrainType` | TEXT | Terrain reference |
| `BiomeType` | TEXT | Biome reference |
| `Elevation` | INT | Height value |
| `IsImpassable` | BOOL | Impassable flag |

### Cities (Worldbuilder)

| Column | Type | Description |
|--------|------|-------------|
| `Owner` | INT | Player ID |
| `Plot` | INT | Plot ID (primary key) |
| `Name` | TEXT | City name |
| `IsCapital` | BOOL | Capital flag |

### Players (Worldbuilder)

| Column | Type | Description |
|--------|------|-------------|
| `ID` | INT | Primary key |
| `CivilizationType` | TEXT | Civilization |
| `LeaderType` | TEXT | Leader |
| `TeamID` | INT | Team assignment |

---

## Localization Tables

### LocalizedText

| Column | Type | Description |
|--------|------|-------------|
| `Language` | TEXT | Language code |
| `Tag` | TEXT | Localization key |
| `Text` | TEXT | Translated text |

```xml
<LocalizedText>
    <Row Language="en_US" Tag="LOC_UNIT_WARRIOR_NAME" Text="Warrior"/>
</LocalizedText>
```

### Languages

| Column | Type | Description |
|--------|------|-------------|
| `LanguageCode` | TEXT | Primary key |
| `Name` | TEXT | Language name |

---

## Modding Framework

### Mods

| Column | Type | Description |
|--------|------|-------------|
| `ModId` | TEXT | Primary key |
| `Version` | TEXT | Mod version |
| `Disabled` | BOOL | Is disabled |

### ModProperties

| Column | Type | Description |
|--------|------|-------------|
| `ModId` | TEXT | Foreign key |
| `Name` | TEXT | Property name |
| `Value` | TEXT | Property value |

### Actions

| Column | Type | Description |
|--------|------|-------------|
| `ActionId` | TEXT | Primary key |
| `ActionType` | TEXT | Action type |
| `File` | TEXT | Target file |

### Criteria

| Column | Type | Description |
|--------|------|-------------|
| `CriteriaId` | TEXT | Primary key |
| `CriteriaType` | TEXT | Criteria type |

---

## Table Statistics

| Category | Table Count |
|----------|-------------|
| Core Type System | ~10 |
| Civilization & Leadership | ~25 |
| Unit System | ~40 |
| Building & District | ~35 |
| Map & Terrain | ~30 |
| Resource System | ~15 |
| Modifier System | ~20 |
| Progression System | ~25 |
| Diplomacy System | ~45 |
| Religion System | ~15 |
| Narrative & Events | ~25 |
| AI System | ~35 |
| Game Configuration | ~40 |
| Victory & Scoring | ~15 |
| Worldbuilder | ~50 |
| Localization | ~10 |
| Modding Framework | ~30 |
| Other/Utility | ~50+ |
| **Total** | **~673** |

---

## Finding Tables

To discover all tables in the game database:

```sql
SELECT name FROM sqlite_master WHERE type='table' ORDER BY name;
```

To see a table's schema:

```sql
PRAGMA table_info(TableName);
```

---

## See Also

- [Types and Kinds](Types-and-Kinds.md) - Type system overview
- [Effect Types](../modifiers/Effect-Types.md) - Modifier effects
- [Collection Types](../modifiers/Collection-Types.md) - Subject collections
- [Cross-Reference Index](../Index.md) - Quick lookup for all types
