# Independents System (City-States & Tribes)

This document covers the independents system in Civilization VII, including city-states, independent tribes, and how to create custom content for both.

## Overview

Civ VII's independents system includes:
- **City-States** - Minor civilizations that provide suzerain bonuses
- **Independent Tribes** - Neutral or hostile groups (barbarian equivalents)
- **City-State Bonuses** - Unique effects when you become suzerain
- **Affinities** - Determines initial disposition (Friendly, Neutral, Hostile)

## Core Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register independents and bonuses as kinds |
| `CityStateTypes` | Define city-state categories |
| `Independents` | Define individual city-states/tribes |
| `Affinities` | Define disposition types |
| `CityStateBonuses` | Define suzerain bonuses |
| `CityStateBonusModifiers` | Link bonuses to effects |
| `IndependentTribeTypes` | Define tribe behavior patterns |
| `TribeTagSets` | Define unit composition for tribes |

## Type Kinds

```xml
<Types>
    <!-- Independent (city-state or tribe) -->
    <Row Type="INDEPENDENT_MY_CITYSTATE" Kind="KIND_INDEPENDENT"/>

    <!-- City-State Bonus -->
    <Row Type="CITY_STATE_BONUS_MY_BONUS" Kind="KIND_CITY_STATE_BONUS"/>
</Types>
```

## City-State Types

City-states are categorized by their focus/bonus type.

### Built-in Types

| Type | Yield | Description |
|------|-------|-------------|
| `CULTURAL` | Culture | Art and tradition focused |
| `ECONOMIC` | Gold | Trade and commerce focused |
| `MILITARISTIC` | Production | Military and defense focused |
| `SCIENTIFIC` | Science | Research and knowledge focused |
| `EXPANSIONIST` | Food | Growth and settlement focused |
| `DIPLOMATIC` | Diplomacy | Influence and relations focused |

### CityStateTypes Table

```xml
<CityStateTypes>
    <Row CityStateType="CULTURAL"
         Name="LOC_CULTURAL_NAME"
         Weight="100"
         YieldType="YIELD_CULTURE"
         DisperseRewardAmount="100"/>
</CityStateTypes>
```

| Column | Type | Description |
|--------|------|-------------|
| `CityStateType` | TEXT | Unique type identifier |
| `Name` | TEXT | Localization key for name |
| `Weight` | INTEGER | Spawn weight relative to others |
| `YieldType` | TEXT | Associated yield type |
| `DisperseRewardAmount` | INTEGER | Base reward when dispersed |

## Affinities

Affinities determine the starting relationship with an independent.

| Affinity | Hostility Chance | Neutrality Chance | Description |
|----------|------------------|-------------------|-------------|
| `AFFINITY_FRIENDLY` | 0% | 0% | Starts friendly, won't attack |
| `AFFINITY_NEUTRAL` | 80% | 100% | Can become hostile |
| `AFFINITY_HOSTILE` | 80% | 100% | Starts hostile, may raid |

```xml
<Affinities>
    <Row Affinity="AFFINITY_FRIENDLY" HostilityChance="0" NeutralityChance="0"/>
    <Row Affinity="AFFINITY_NEUTRAL" HostilityChance="80" NeutralityChance="100"/>
    <Row Affinity="AFFINITY_HOSTILE" HostilityChance="80" NeutralityChance="100"/>
</Affinities>
```

## Defining Independents

### City-States

```xml
<Independents>
    <Row IndependentType="INDEPENDENT_MY_CITYSTATE"
         Name="LOC_INDEPENDENT_MY_CITYSTATE_MINOR_NAME"
         CityStateName="LOC_INDEPENDENT_MY_CITYSTATE_MAJOR_NAME"
         Affinity="AFFINITY_FRIENDLY"
         BiomeType="BIOME_GRASSLAND"
         CityStateType="SCIENTIFIC"/>
</Independents>
```

### Independents Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `IndependentType` | TEXT | Unique independent identifier |
| `Name` | TEXT | Name when minor (tribe) |
| `CityStateName` | TEXT | Name when promoted to city-state |
| `Affinity` | TEXT | Starting disposition |
| `BiomeType` | TEXT | Preferred biome for spawning |
| `CityStateType` | TEXT | Category (CULTURAL, ECONOMIC, etc.) |

## City-State Bonuses

Bonuses are granted to the suzerain (player with highest influence).

### Defining a Bonus

```xml
<CityStateBonuses>
    <Row CityStateBonusType="CITY_STATE_BONUS_MY_BONUS"
         Name="LOC_CITY_STATE_BONUS_MY_BONUS_NAME"
         Description="LOC_CITY_STATE_BONUS_MY_BONUS_DESCRIPTION"
         CityStateType="SCIENTIFIC"
         Shareable="false"/>
</CityStateBonuses>
```

### CityStateBonuses Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `CityStateBonusType` | TEXT | Unique bonus identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `CityStateType` | TEXT | Which city-state type uses this |
| `Shareable` | BOOLEAN | Can multiple players benefit |

### Linking Bonuses to Modifiers

```xml
<!-- For AI evaluation -->
<CityStateBonusModifiers>
    <Row CityStateBonusType="CITY_STATE_BONUS_MY_BONUS" ModifierID="MOD_MY_CS_BONUS"/>
</CityStateBonusModifiers>

<!-- Actually applies the modifier in-game -->
<GameModifiers>
    <Row ModifierId="MOD_MY_CS_BONUS"/>
</GameModifiers>
```

Note: Bonuses require entries in both `CityStateBonusModifiers` (for AI evaluation) and `GameModifiers` (for actual application).

## Independent Tribe Types

Tribes define the behavior of hostile independents.

### Built-in Tribe Types

| Type | Description |
|------|-------------|
| `TRIBE_DEFAULT` | Standard land-based tribe |
| `TRIBE_NAVAL` | Naval-focused tribe |
| `TRIBE_CRISIS` | Special crisis event tribes |

### IndependentTribeTypes Table

```xml
<IndependentTribeTypes>
    <Row TribeType="TRIBE_MY_TRIBE"
         DefenderTagSet="DEFAULT_DEFENDER_TAGS"
         RaidOperation="Independent Large Raid"
         AdvancedRaidOperation="Independent Large Raid"
         RespawnTime="6"
         IgnoreSpacing="false"
         CanBeBefriended="true"
         CampConstructibleType="IMPROVEMENT_ENCAMPMENT"/>
</IndependentTribeTypes>
```

| Column | Type | Description |
|--------|------|-------------|
| `TribeType` | TEXT | Unique tribe type identifier |
| `DefenderTagSet` | TEXT | Tags for defender units |
| `RaidOperation` | TEXT | AI operation for raids |
| `AdvancedRaidOperation` | TEXT | AI operation for advanced raids |
| `RespawnTime` | INTEGER | Turns to respawn after defeat |
| `IgnoreSpacing` | BOOLEAN | Can spawn near other independents |
| `CanBeBefriended` | BOOLEAN | Can become friendly |
| `CampConstructibleType` | TEXT | Building type for camps |

## Tribe Unit Composition

### TribeTagSets

Define how many units of each type a tribe can have:

```xml
<TribeTagSets>
    <Row TribeTagName="DEFAULT_SCOUT_TAGS" InitialUnitAmount="1" MaxUnitAmount="1"/>
    <Row TribeTagName="DEFAULT_DEFENDER_TAGS" InitialUnitAmount="1" MaxUnitAmount="1"/>
    <Row TribeTagName="DEFAULT_COMBAT_TAGS" InitialUnitAmount="0" MaxUnitAmount="5"/>
    <Row TribeTagName="DEFAULT_COMMANDER_TAGS" InitialUnitAmount="0" MaxUnitAmount="0"/>
</TribeTagSets>
```

### TribeCombatTags

Define which unit classes can be in each tag set:

```xml
<TribeCombatTags>
    <Row TribeTagSet="DEFAULT_COMBAT_TAGS" CombatTag="UNIT_CLASS_MELEE"/>
    <Row TribeTagSet="DEFAULT_COMBAT_TAGS" CombatTag="UNIT_CLASS_RANGED"/>
    <Row TribeTagSet="DEFAULT_COMBAT_TAGS" CombatTag="UNIT_CLASS_SKIRMISH"/>
</TribeCombatTags>
```

### TribeRequiredCombatTags

Define required unit classes:

```xml
<TribeRequiredCombatTags>
    <Row TribeTagSet="DEFAULT_SCOUT_TAGS" CombatTag="UNIT_CLASS_RECON"/>
    <Row TribeTagSet="DEFAULT_DEFENDER_TAGS" CombatTag="UNIT_CLASS_COMBAT"/>
</TribeRequiredCombatTags>
```

### TribeForbiddenCombatTags

Define excluded unit classes:

```xml
<TribeForbiddenCombatTags>
    <Row TribeTagSet="DEFAULT_COMBAT_TAGS" CombatTag="UNIT_CLASS_NAVAL"/>
    <Row TribeTagSet="DEFAULT_COMBAT_TAGS" CombatTag="UNIT_CLASS_NO_INDEPENDENTS"/>
</TribeForbiddenCombatTags>
```

### Linking Tag Sets to Tribes

```xml
<TribeScoutTagSets>
    <Row TribeTypeName="TRIBE_MY_TRIBE" TribeTagSetName="DEFAULT_SCOUT_TAGS"/>
</TribeScoutTagSets>

<TribeCombatTagSets>
    <Row TribeTypeName="TRIBE_MY_TRIBE" TribeTagSetName="DEFAULT_COMBAT_TAGS"/>
</TribeCombatTagSets>

<TribeCommanderTagSets>
    <Row TribeTypeName="TRIBE_MY_TRIBE" TribeTagSetName="DEFAULT_COMMANDER_TAGS"/>
</TribeCommanderTagSets>
```

## Visual Art Configuration

### Building Cultures

Assign visual styles to independents:

```xml
<VisArt_IndependentBuildingCultures>
    <Row IndependentType="INDEPENDENT_MY_CITYSTATE" BuildingCulture="BUILDING_CULTURE_MED"/>
</VisArt_IndependentBuildingCultures>
```

### Unit Cultures

Assign unit visual styles:

```xml
<VisArt_IndependentUnitCultures>
    <Row IndependentType="INDEPENDENT_MY_CITYSTATE" UnitCulture="Med"/>
    <Row IndependentType="INDEPENDENT_MY_CITYSTATE" UnitCulture="CIVILIZATION_GREECE"/>
</VisArt_IndependentUnitCultures>
```

## Global Parameters

Control independent spawning:

```xml
<GlobalParameters>
    <Update>
        <Where Name="INDEPENDENT_NUM_PER_MAJOR"/>
        <Set Value="3"/>
    </Update>
    <Update>
        <Where Name="INDEPENDENT_EXTRAS_PER_MAJOR"/>
        <Set Value="1"/>
    </Update>
    <Update>
        <Where Name="INDEPENDENT_ENCAMPMENT_RESPAWN"/>
        <Set Value="6"/>
    </Update>
</GlobalParameters>
```

## Complete Example: Custom City-State

### Step 1: Register Types

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <Row Type="INDEPENDENT_ACADEMIA" Kind="KIND_INDEPENDENT"/>
        <Row Type="CITY_STATE_BONUS_ACADEMIA" Kind="KIND_CITY_STATE_BONUS"/>
    </Types>
</Database>
```

### Step 2: Define the City-State

```xml
<Independents>
    <Row IndependentType="INDEPENDENT_ACADEMIA"
         Name="LOC_INDEPENDENT_ACADEMIA_MINOR_NAME"
         CityStateName="LOC_INDEPENDENT_ACADEMIA_MAJOR_NAME"
         Affinity="AFFINITY_FRIENDLY"
         BiomeType="BIOME_GRASSLAND"
         CityStateType="SCIENTIFIC"/>
</Independents>
```

### Step 3: Define the Bonus

```xml
<CityStateBonuses>
    <Row CityStateBonusType="CITY_STATE_BONUS_ACADEMIA"
         Name="LOC_CITY_STATE_BONUS_ACADEMIA_NAME"
         Description="LOC_CITY_STATE_BONUS_ACADEMIA_DESCRIPTION"
         CityStateType="SCIENTIFIC"/>
</CityStateBonuses>

<CityStateBonusModifiers>
    <Row CityStateBonusType="CITY_STATE_BONUS_ACADEMIA" ModifierID="MOD_CS_ACADEMIA_SCIENCE"/>
</CityStateBonusModifiers>

<GameModifiers>
    <Row ModifierId="MOD_CS_ACADEMIA_SCIENCE"/>
</GameModifiers>
```

### Step 4: Define the Modifier (GameEffects)

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_CS_ACADEMIA_SCIENCE"
              collection="COLLECTION_ALL_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_PLAYER_IS_SUZERAIN">
                <Argument name="CityStateBonusType">CITY_STATE_BONUS_ACADEMIA</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="YieldType">YIELD_SCIENCE</Argument>
        <Argument name="Amount">3</Argument>
    </Modifier>
</GameEffects>
```

### Step 5: Visual Configuration

```xml
<VisArt_IndependentBuildingCultures>
    <Row IndependentType="INDEPENDENT_ACADEMIA" BuildingCulture="BUILDING_CULTURE_MED"/>
</VisArt_IndependentBuildingCultures>

<VisArt_IndependentUnitCultures>
    <Row IndependentType="INDEPENDENT_ACADEMIA" UnitCulture="Med"/>
</VisArt_IndependentUnitCultures>
```

### Step 6: Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_INDEPENDENT_ACADEMIA_MINOR_NAME" Language="en_US">
            <Text>Academia Scholars</Text>
        </Row>
        <Row Tag="LOC_INDEPENDENT_ACADEMIA_MAJOR_NAME" Language="en_US">
            <Text>Academia</Text>
        </Row>
        <Row Tag="LOC_CITY_STATE_BONUS_ACADEMIA_NAME" Language="en_US">
            <Text>Hall of Learning</Text>
        </Row>
        <Row Tag="LOC_CITY_STATE_BONUS_ACADEMIA_DESCRIPTION" Language="en_US">
            <Text>+3 [ICON_SCIENCE] Science in all cities.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Example Bonus Modifiers

### Building Unlock

```xml
<Modifier id="MOD_CS_UNIQUE_BUILDING"
          collection="COLLECTION_ALL_CITIES"
          effect="EFFECT_GRANT_BUILDING_IN_CITY">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLAYER_IS_SUZERAIN">
            <Argument name="CityStateBonusType">MY_BONUS</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="BuildingType">BUILDING_MY_UNIQUE</Argument>
</Modifier>
```

### Unit Production Bonus

```xml
<Modifier id="MOD_CS_UNIT_PRODUCTION"
          collection="COLLECTION_ALL_CITIES"
          effect="EFFECT_CITY_ADJUST_UNIT_PRODUCTION_PERCENT">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLAYER_IS_SUZERAIN">
            <Argument name="CityStateBonusType">MY_BONUS</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="UnitTag">UNIT_CLASS_MELEE</Argument>
    <Argument name="Percent">25</Argument>
</Modifier>
```

### Free Tech/Civic

```xml
<Modifier id="MOD_CS_FREE_TECH"
          collection="COLLECTION_OWNER"
          effect="EFFECT_PLAYER_GRANT_PROGRESSION"
          run-once="true" permanent="true">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLAYER_IS_SUZERAIN">
            <Argument name="CityStateBonusType">MY_BONUS</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="Amount">1</Argument>
    <Argument name="SystemType">SYSTEM_TECH</Argument>
    <Argument name="FreeProgressionType">FREE_PROGRESSION_NODE_ANY_UNLOCKED</Argument>
</Modifier>
```

## AI Configuration

### AI Definitions for Independents

```xml
<AiDefinitions>
    <Row LeaderTrait="TRAIT_LEADER_INDEPENDENT" AiComponent="BehaviorTreeManager"/>
    <Row LeaderTrait="TRAIT_LEADER_INDEPENDENT" AiComponent="IndependentStrategic"/>
    <Row LeaderTrait="TRAIT_LEADER_INDEPENDENT" AiComponent="IndependentTactical"/>
    <Row LeaderTrait="TRAIT_LEADER_INDEPENDENT" AiComponent="Military"/>
    <Row LeaderTrait="TRAIT_LEADER_INDEPENDENT" AiComponent="Planning"/>
    <Row LeaderTrait="TRAIT_LEADER_INDEPENDENT" AiComponent="Tribe_AI"/>
</AiDefinitions>
```

### AI Tactics

```xml
<AiListTypes>
    <Row ListType="My Independent Tactics"/>
</AiListTypes>

<AiLists>
    <Row ListType="My Independent Tactics" LeaderType="TRAIT_LEADER_INDEPENDENT" System="Tactics"/>
</AiLists>

<AiFavoredItems>
    <Row ListType="My Independent Tactics" Item="Rampage"/>
    <Row ListType="My Independent Tactics" Item="Attack High Priority Unit"/>
    <Row ListType="My Independent Tactics" Item="Explore"/>
    <Row ListType="My Independent Tactics" Item="Defend Home"/>
</AiFavoredItems>
```

## Best Practices

1. **Diverse Bonuses** - Each city-state type should have varied bonus options
2. **Biome Distribution** - Spread independents across different biomes
3. **Affinity Balance** - Mix friendly, neutral, and hostile independents
4. **Visual Consistency** - Match building and unit cultures to region
5. **Shareable Flag** - Mark bonuses as `Shareable="true"` if appropriate
6. **Suzerain Requirement** - Always include `REQUIREMENT_PLAYER_IS_SUZERAIN` in bonus modifiers
7. **Both Modifier Tables** - Add modifiers to both `CityStateBonusModifiers` and `GameModifiers`

## Related Documentation

- [Modifier System](../modifiers/Overview.md)
- [Effect Types](../modifiers/Effect-Types.md)
- [AI Behavior](../civilization-creation/AI-Behavior.md)
- [Types and Kinds](../database/Types-and-Kinds.md)
