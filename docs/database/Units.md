# Units Database Schema

This document describes the database tables used to define units in Civilization VII.

## Overview

Units are defined across multiple tables:
- `Types` - Registers the unit type
- `Units` - Core unit properties
- `Unit_Stats` - Combat statistics
- `TypeTags` - Unit classification tags
- Related ability tables

## Types Table

Every unit must first be registered in the `Types` table:

```xml
<Types>
    <Row Type="UNIT_WARRIOR" Kind="KIND_UNIT"/>
    <Row Type="UNIT_SCOUT" Kind="KIND_UNIT"/>
</Types>
```

| Column | Type | Description |
|--------|------|-------------|
| `Type` | TEXT | Unique identifier (e.g., `UNIT_WARRIOR`) |
| `Kind` | TEXT | Must be `KIND_UNIT` |

## Units Table

The main unit definition table:

```xml
<Units>
    <Row
        UnitType="UNIT_SCOUT"
        Name="LOC_UNIT_SCOUT_NAME"
        Description="LOC_UNIT_SCOUT_DESCRIPTION"
        BaseSightRange="2"
        BaseMoves="2"
        UnitMovementClass="UNIT_MOVEMENT_CLASS_RECON"
        Domain="DOMAIN_LAND"
        CoreClass="CORE_CLASS_RECON"
        FormationClass="FORMATION_CLASS_CIVILIAN"
        ZoneOfControl="false"
    />
</Units>
```

### Units Table Columns

| Column | Type | Description | Example Values |
|--------|------|-------------|----------------|
| `UnitType` | TEXT | Primary key, matches Type | `UNIT_WARRIOR` |
| `Name` | TEXT | Localization key for name | `LOC_UNIT_WARRIOR_NAME` |
| `Description` | TEXT | Localization key for description | `LOC_UNIT_WARRIOR_DESCRIPTION` |
| `BaseSightRange` | INTEGER | Vision range in tiles | `2` |
| `BaseMoves` | INTEGER | Movement points per turn | `2`, `3`, `4` |
| `UnitMovementClass` | TEXT | Movement type | See below |
| `Domain` | TEXT | Unit domain | `DOMAIN_LAND`, `DOMAIN_SEA`, `DOMAIN_AIR` |
| `CoreClass` | TEXT | Core classification | See below |
| `FormationClass` | TEXT | Formation type | See below |
| `ZoneOfControl` | BOOLEAN | Exerts ZOC | `true`, `false` |
| `CanTrain` | BOOLEAN | Can be trained normally | `true`, `false` |
| `CanPurchase` | BOOLEAN | Can be purchased with gold | `true`, `false` |
| `FoundCity` | BOOLEAN | Can found settlements | `true` (Settlers) |
| `MakeTradeRoute` | BOOLEAN | Creates trade routes | `true` (Merchants) |
| `CostProgressionModel` | TEXT | How cost scales | See below |
| `CostProgressionParam1` | INTEGER | Cost scaling parameter | Varies |
| `PrereqPopulation` | INTEGER | Population required to train | `5` (Settlers) |
| `CanEarnExperience` | BOOLEAN | Gains XP | `true`, `false` |
| `PromotionClass` | TEXT | Promotion tree | See below |
| `CanTriggerDiscovery` | BOOLEAN | Triggers discovery sites | `true`, `false` |
| `CanBeDamaged` | BOOLEAN | Can take damage | `true`, `false` |
| `ManualDelete` | BOOLEAN | Can be manually deleted | `true`, `false` |

### UnitMovementClass Values

| Value | Description |
|-------|-------------|
| `UNIT_MOVEMENT_CLASS_FOOT` | Standard land movement |
| `UNIT_MOVEMENT_CLASS_RECON` | Scout/reconnaissance |
| `UNIT_MOVEMENT_CLASS_NAVAL` | Naval vessels |
| `UNIT_MOVEMENT_CLASS_MOUNTED` | Cavalry |
| `UNIT_MOVEMENT_CLASS_WHEELED` | Wheeled vehicles |
| `NO_UNIT_MOVEMENT_CLASS` | Stationary units |

### Domain Values

| Value | Description |
|-------|-------------|
| `DOMAIN_LAND` | Ground units |
| `DOMAIN_SEA` | Naval units |
| `DOMAIN_AIR` | Aircraft |

### CoreClass Values

| Value | Description |
|-------|-------------|
| `CORE_CLASS_MILITARY` | Combat units |
| `CORE_CLASS_CIVILIAN` | Non-combat civilians |
| `CORE_CLASS_RECON` | Scouts and explorers |
| `CORE_CLASS_SUPPORT` | Support units (Commanders, Settlers) |

### FormationClass Values

| Value | Description |
|-------|-------------|
| `FORMATION_CLASS_LAND_COMBAT` | Land combat formation |
| `FORMATION_CLASS_NAVAL_COMBAT` | Naval combat formation |
| `FORMATION_CLASS_CIVILIAN` | Civilian formation |
| `FORMATION_CLASS_COMMAND` | Commander formation |

### CostProgressionModel Values

| Value | Description |
|-------|-------------|
| `COST_PROGRESSION_PREVIOUS_COPIES` | Cost increases with each copy |
| `COST_PROGRESSION_NUM_SETTLEMENTS` | Cost scales with settlement count |

### PromotionClass Values

| Value | Description |
|-------|-------------|
| `PROMOTION_CLASS_LAND_COMMANDER` | Army commander promotions |
| `PROMOTION_CLASS_NAVAL_COMMANDER` | Fleet commander promotions |
| `PROMOTION_CLASS_AIR_COMMANDER` | Squadron commander promotions |
| `PROMOTION_CLASS_CARRIER_COMMANDER` | Carrier commander promotions |
| `PROMOTION_CLASS_AERODROME_COMMANDER` | Aerodrome commander promotions |

## Unit_Stats Table

Combat statistics for units:

```xml
<Unit_Stats>
    <Row UnitType="UNIT_WARRIOR" Combat="25"/>
    <Row UnitType="UNIT_ARCHER" Combat="15" RangedCombat="25" Range="2"/>
</Unit_Stats>
```

| Column | Type | Description |
|--------|------|-------------|
| `UnitType` | TEXT | Foreign key to Units |
| `Combat` | INTEGER | Melee combat strength |
| `RangedCombat` | INTEGER | Ranged combat strength |
| `Range` | INTEGER | Attack range in tiles |

## TypeTags Table

Unit classification tags for game logic:

```xml
<TypeTags>
    <Row Type="UNIT_SCOUT" Tag="UNIT_CLASS_NON_COMBAT"/>
    <Row Type="UNIT_SCOUT" Tag="UNIT_CLASS_AUTOEXPLORE"/>
    <Row Type="UNIT_SCOUT" Tag="UNIT_CLASS_RECON"/>
    <Row Type="UNIT_WARRIOR" Tag="UNIT_CLASS_COMBAT"/>
    <Row Type="UNIT_WARRIOR" Tag="UNIT_CLASS_MELEE"/>
    <Row Type="UNIT_WARRIOR" Tag="UNIT_CLASS_INFANTRY"/>
</TypeTags>
```

### Common Unit Class Tags

| Tag | Description |
|-----|-------------|
| `UNIT_CLASS_COMBAT` | Can engage in combat |
| `UNIT_CLASS_NON_COMBAT` | Cannot attack |
| `UNIT_CLASS_MELEE` | Melee attacker |
| `UNIT_CLASS_RANGED` | Ranged attacker |
| `UNIT_CLASS_INFANTRY` | Infantry unit |
| `UNIT_CLASS_CAVALRY` | Cavalry unit |
| `UNIT_CLASS_MOUNTED` | Mounted unit |
| `UNIT_CLASS_SIEGE` | Siege weapon |
| `UNIT_CLASS_NAVAL` | Naval unit |
| `UNIT_CLASS_AIRCRAFT` | Air unit |
| `UNIT_CLASS_RECON` | Reconnaissance |
| `UNIT_CLASS_COMMAND` | Commander unit |
| `UNIT_CLASS_HEAVY` | Heavy unit |
| `UNIT_CLASS_LIGHT` | Light unit |
| `UNIT_CLASS_STEALTH` | Stealth capable |
| `AGELESS` | Persists across ages |

## Unit Abilities

Abilities are defined in `Types` with `KIND_ABILITY`:

```xml
<Types>
    <Row Type="ABILITY_AMPHIBIOUS" Kind="KIND_ABILITY"/>
    <Row Type="ABILITY_FIRST_STRIKE" Kind="KIND_ABILITY"/>
</Types>
```

### Common Abilities

| Ability | Description |
|---------|-------------|
| `ABILITY_AMPHIBIOUS` | No penalty attacking from water |
| `ABILITY_FIRST_STRIKE` | Attacks first in combat |
| `ABILITY_FEAR` | Causes fear in enemies |
| `ABILITY_POISON` | Applies poison damage |
| `ABILITY_SKIRMISH` | Can move after attacking |
| `ABILITY_SWIFT` | Increased movement |
| `ABILITY_STEALTH` | Can be hidden |

## Example: Creating a New Unit

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Register the type -->
    <Types>
        <Row Type="UNIT_MY_WARRIOR" Kind="KIND_UNIT"/>
    </Types>

    <!-- Define the unit -->
    <Units>
        <Row
            UnitType="UNIT_MY_WARRIOR"
            Name="LOC_UNIT_MY_WARRIOR_NAME"
            Description="LOC_UNIT_MY_WARRIOR_DESCRIPTION"
            BaseSightRange="2"
            BaseMoves="2"
            UnitMovementClass="UNIT_MOVEMENT_CLASS_FOOT"
            Domain="DOMAIN_LAND"
            CoreClass="CORE_CLASS_MILITARY"
            FormationClass="FORMATION_CLASS_LAND_COMBAT"
            ZoneOfControl="true"
        />
    </Units>

    <!-- Combat stats -->
    <Unit_Stats>
        <Row UnitType="UNIT_MY_WARRIOR" Combat="30"/>
    </Unit_Stats>

    <!-- Classification -->
    <TypeTags>
        <Row Type="UNIT_MY_WARRIOR" Tag="UNIT_CLASS_COMBAT"/>
        <Row Type="UNIT_MY_WARRIOR" Tag="UNIT_CLASS_MELEE"/>
        <Row Type="UNIT_MY_WARRIOR" Tag="UNIT_CLASS_INFANTRY"/>
    </TypeTags>
</Database>
```

## Related Tables

- `UnitPromotionClasses` - Promotion class definitions
- `UnitAbilities` - Unit ability definitions
- `Unit_BuildingPrereqs` - Building prerequisites
- `Unit_ResourceCosts` - Strategic resource costs
