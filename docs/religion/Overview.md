# Religion System

This document covers the religion system in Civilization VII, including religions, belief classes, beliefs, and how to create custom religious content.

## Overview

Civ VII's religion system is structured differently from previous entries:

- **Religions** are named identities that players can adopt and spread
- **Belief Classes** define categories of bonuses (Pantheon, Enhancer, Founder, Reliquary)
- **Beliefs** provide specific bonuses within each class
- **Age-Specific** - Pantheons are available in Antiquity; full religions with additional belief classes become available in Exploration

## Core Tables

| Table | Purpose |
|-------|---------|
| `Kinds` | Register belief classes as `KIND_BELIEF_CLASS` |
| `Types` | Register religions as `KIND_RELIGION`, beliefs as `KIND_BELIEF` |
| `Religions` | Define religions with names and visual properties |
| `BeliefClasses` | Define categories of beliefs |
| `Beliefs` | Define individual belief bonuses |
| `BeliefModifiers` | Link beliefs to their gameplay effects |
| `Belief_Priorities` | AI evaluation priorities for beliefs |

## Type Kinds

```xml
<Types>
    <!-- Religion -->
    <Row Type="RELIGION_MY_RELIGION" Kind="KIND_RELIGION"/>

    <!-- Belief Class -->
    <Row Type="BELIEF_CLASS_MY_CLASS" Kind="KIND_BELIEF_CLASS"/>

    <!-- Belief -->
    <Row Type="BELIEF_MY_BELIEF" Kind="KIND_BELIEF"/>
</Types>
```

## Religions

### Built-in Religions

| Religion | Icon |
|----------|------|
| `RELIGION_BUDDHISM` | Bu |
| `RELIGION_CATHOLICISM` | Ca |
| `RELIGION_CONFUCIANISM` | Co |
| `RELIGION_HINDUISM` | Hi |
| `RELIGION_ISLAM` | Is |
| `RELIGION_JUDAISM` | Ju |
| `RELIGION_ORTHODOXY` | Or |
| `RELIGION_PROTESTANTISM` | Pr |
| `RELIGION_SHINTO` | Sh |
| `RELIGION_SIKHISM` | Si |
| `RELIGION_TAOISM` | Ta |
| `RELIGION_ZOROASTRIANISM` | Zo |

Additionally, there are 12 custom religion slots (`RELIGION_CUSTOM_1` through `RELIGION_CUSTOM_12`) for player-created religions.

### Defining a Religion

```xml
<Religions>
    <Row>
        <ReligionType>RELIGION_MY_RELIGION</ReligionType>
        <Name>LOC_RELIGION_MY_RELIGION_NAME</Name>
        <IconString>My</IconString>
        <RequiresCustomName>false</RequiresCustomName>
        <Color>COLOR_RELIGION_MY_RELIGION</Color>
    </Row>
</Religions>
```

### Religions Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `ReligionType` | TEXT | Unique religion identifier |
| `Name` | TEXT | Localization key for name |
| `IconString` | TEXT | Short string (2 chars) for icon display |
| `RequiresCustomName` | BOOLEAN | True if player must name the religion |
| `Color` | TEXT | Color definition reference |

## Belief Classes

Belief classes define categories of beliefs that religions can adopt.

### Built-in Belief Classes

| Class | Age | Max Per Religion | Description |
|-------|-----|------------------|-------------|
| `BELIEF_CLASS_PANTHEON` | Antiquity | 1 | Early religious bonuses, enhances Altars |
| `BELIEF_CLASS_RELIQUARY` | Exploration | 1 | Relic acquisition bonuses |
| `BELIEF_CLASS_FOUNDER` | Exploration | 3 | Yields based on cities following religion |
| `BELIEF_CLASS_ENHANCER` | Exploration | 1 | Religion spread and conversion bonuses |

### Defining a Belief Class

```xml
<BeliefClasses>
    <Row BeliefClassType="BELIEF_CLASS_MY_CLASS"
         Name="LOC_BELIEF_CLASS_MY_CLASS_NAME"
         MaxInReligion="1"
         AdoptionOrder="4"/>
</BeliefClasses>
```

### BeliefClasses Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `BeliefClassType` | TEXT | Unique belief class identifier |
| `Name` | TEXT | Localization key for name |
| `MaxInReligion` | INTEGER | Maximum beliefs of this class per religion |
| `AdoptionOrder` | INTEGER | Order in which classes are adopted |

## Beliefs

### Defining a Belief

```xml
<Beliefs>
    <Row BeliefType="BELIEF_MY_BELIEF"
         Name="LOC_BELIEF_MY_BELIEF_NAME"
         Description="LOC_BELIEF_MY_BELIEF_DESCRIPTION"
         BeliefClassType="BELIEF_CLASS_FOUNDER"
         Shareable="false"
         AISelectionBin="1"/>
</Beliefs>
```

### Beliefs Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `BeliefType` | TEXT | Unique belief identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `BeliefClassType` | TEXT | Parent belief class |
| `Shareable` | BOOLEAN | Can multiple religions share this belief |
| `AISelectionBin` | INTEGER | AI evaluation priority (1-3, lower = higher priority) |

### Linking Beliefs to Modifiers

```xml
<BeliefModifiers>
    <Row BeliefType="BELIEF_MY_BELIEF" ModifierId="MOD_MY_BELIEF_EFFECT"/>
    <!-- A belief can have multiple modifiers -->
    <Row BeliefType="BELIEF_MY_BELIEF" ModifierId="MOD_MY_BELIEF_EFFECT_2"/>
</BeliefModifiers>
```

## Pantheon Beliefs (Antiquity)

Pantheon beliefs are available in the Antiquity Age and primarily enhance the Altar building.

### Altar Adjacency Bonuses

Pantheons can activate adjacency bonuses for Altars:

```xml
<Constructible_Adjacencies>
    <Row ConstructibleType="BUILDING_ALTAR"
         YieldChangeId="AltarMountainHappiness"
         RequiresActivation="true"/>
</Constructible_Adjacencies>

<Adjacency_YieldChanges>
    <Row ID="AltarMountainHappiness"
         Age="AGE_ANTIQUITY"
         YieldType="YIELD_HAPPINESS"
         YieldChange="1"
         TilesRequired="1"
         ProjectMaxYield="true"
         AdjacentTerrain="TERRAIN_MOUNTAIN"/>
</Adjacency_YieldChanges>
```

### Altar Warehouse Bonuses

Pantheons can also grant warehouse yields based on city resources:

```xml
<Constructible_WarehouseYields>
    <Row ConstructibleType="BUILDING_ALTAR"
         YieldChangeId="PantheonAltarPastureFood"
         RequiresActivation="true"/>
</Constructible_WarehouseYields>

<Warehouse_YieldChanges>
    <Row ID="PantheonAltarPastureFood"
         Age="AGE_ANTIQUITY"
         YieldType="YIELD_FOOD"
         YieldChange="1"
         ConstructibleInCity="IMPROVEMENT_PASTURE"/>
</Warehouse_YieldChanges>
```

## Religion Beliefs (Exploration)

Full religions in the Exploration Age can adopt beliefs from three additional classes:

### Reliquary Beliefs

Provide bonus Relics when converting cities, capitals, wonders, etc.:

```xml
<Modifier id="BONUS_9_RELIC_CITY_STATE"
          collection="COLLECTION_MAJOR_PLAYERS"
          effect="EFFECT_ATTACH_MODIFIERS">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLAYER_IS_RELIGION_FOUNDER"/>
    </SubjectRequirements>
    <Argument name="ModifierId">ATTACH_RELIC_CITY_STATE</Argument>
</Modifier>

<Modifier id="ATTACH_RELIC_CITY_STATE"
          collection="COLLECTION_OWNER"
          effect="EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_CITY_STATE">
    <Argument name="Amount">2</Argument>
</Modifier>
```

### Founder Beliefs

Grant yields based on cities following your religion:

```xml
<Modifier id="ATTACH_CITIES_FOLLOWING_SCIENCE"
          collection="COLLECTION_OWNER"
          effect="EFFECT_ADD_RELIGIOUS_BELIEF_YIELD">
    <Argument name="Amount">4</Argument>
    <Argument name="YieldType">YIELD_SCIENCE</Argument>
    <Argument name="BeliefYieldType">BELIEF_YIELD_PER_FOREIGN_CITY</Argument>
</Modifier>
```

#### Belief Yield Types

| Type | Description |
|------|-------------|
| `BELIEF_YIELD_PER_FOREIGN_CITY` | Per foreign city following religion |
| `BELIEF_YIELD_PER_DOMESTIC_CITY` | Per owned city following religion |
| `BELIEF_YIELD_PER_NATURAL_WONDER` | Per natural wonder owned |
| `BELIEF_YIELD_PER_WONDER` | Per wonder owned |
| `BELIEF_YIELD_PER_BIOME` | Per city in specific biome |
| `BELIEF_YIELD_PER_TERRAIN_TYPE` | Per city on specific terrain |

### Enhancer Beliefs

Improve religion spread and provide conversion bonuses:

```xml
<!-- Auto-spread religion in foreign hemisphere -->
<Modifier id="BONUS_1_AUTO_SPREAD_FOREIGN_HEMISPHERE"
          collection="COLLECTION_OWNER"
          effect="EFFECT_ENABLE_RELIGION_AUTO_SPREAD_FOREIGN_HEMISPHERE">
    <Argument name="Enable">true</Argument>
</Modifier>

<!-- Additional spread charges for missionaries -->
<Modifier id="ATTACH_ADDITIONAL_CHARGE"
          collection="COLLECTION_PLAYER_UNITS"
          effect="EFFECT_ADJUST_UNIT_SPREAD_CHARGES">
    <Argument name="Amount">1</Argument>
</Modifier>
```

## Religion-Specific Effects

### Auto-Spread Effects

| Effect | Description |
|--------|-------------|
| `EFFECT_ENABLE_RELIGION_AUTO_SPREAD_FOREIGN_HEMISPHERE` | Spread to foreign hemisphere |
| `EFFECT_ENABLE_RELIGION_AUTO_SPREAD_CONQUERED` | Spread to conquered cities |
| `EFFECT_ENABLE_RELIGION_AUTO_SPREAD_TRADE_ROUTE` | Spread via trade routes |

### Relic Conversion Effects

| Effect | Description |
|--------|-------------|
| `EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_CITY_STATE` | Relics when converting city-states |
| `EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_CAPITAL` | Relics when converting capitals |
| `EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_WONDER` | Relics when converting cities with wonders |
| `EFFECT_ADJUST_PLAYER_RELIC_NEW_WORLD` | Relics when discovering new world |
| `EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_TREASURE_FLEETS` | Relics from treasure fleets |
| `EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_HOLY_CITY` | Relics from holy city |

## Requirements

### Religion-Specific Requirements

| Requirement | Description |
|-------------|-------------|
| `REQUIREMENT_PLAYER_IS_RELIGION_FOUNDER` | Player founded a religion |
| `REQUIREMENT_CITY_FOLLOWS_RELIGION` | City follows player's religion |
| `REQUIREMENT_NEAR_RELIGIOUS_CITY` | Unit is near religious city |
| `REQUIREMENT_CITY_IS_HOLY_CITY` | City is the holy city |

## Complete Example: Custom Belief

### Step 1: Register Types

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <Row Type="BELIEF_PILGRIMAGE_BONUS" Kind="KIND_BELIEF"/>
    </Types>
</Database>
```

### Step 2: Define the Belief

```xml
<Beliefs>
    <Row BeliefType="BELIEF_PILGRIMAGE_BONUS"
         Name="LOC_BELIEF_PILGRIMAGE_BONUS_NAME"
         Description="LOC_BELIEF_PILGRIMAGE_BONUS_DESCRIPTION"
         BeliefClassType="BELIEF_CLASS_FOUNDER"
         AISelectionBin="1"/>
</Beliefs>

<BeliefModifiers>
    <Row BeliefType="BELIEF_PILGRIMAGE_BONUS" ModifierId="MOD_PILGRIMAGE_GOLD"/>
</BeliefModifiers>
```

### Step 3: Define Modifiers (GameEffects)

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Only applies to religion founder -->
    <Modifier id="MOD_PILGRIMAGE_GOLD"
              collection="COLLECTION_MAJOR_PLAYERS"
              effect="EFFECT_ATTACH_MODIFIERS">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_PLAYER_IS_RELIGION_FOUNDER"/>
        </SubjectRequirements>
        <Argument name="ModifierId">ATTACH_PILGRIMAGE_GOLD</Argument>
    </Modifier>

    <!-- Grant gold per foreign city following -->
    <Modifier id="ATTACH_PILGRIMAGE_GOLD"
              collection="COLLECTION_OWNER"
              effect="EFFECT_ADD_RELIGIOUS_BELIEF_YIELD">
        <Argument name="Amount">6</Argument>
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="BeliefYieldType">BELIEF_YIELD_PER_FOREIGN_CITY</Argument>
    </Modifier>
</GameEffects>
```

### Step 4: Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_BELIEF_PILGRIMAGE_BONUS_NAME" Language="en_US">
            <Text>Pilgrimage Routes</Text>
        </Row>
        <Row Tag="LOC_BELIEF_PILGRIMAGE_BONUS_DESCRIPTION" Language="en_US">
            <Text>+6 [ICON_GOLD] Gold for each foreign city following your religion.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Complete Example: Custom Religion

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <Row Type="RELIGION_ANIMISM" Kind="KIND_RELIGION"/>
    </Types>

    <Religions>
        <Row>
            <ReligionType>RELIGION_ANIMISM</ReligionType>
            <Name>LOC_RELIGION_ANIMISM_NAME</Name>
            <IconString>An</IconString>
            <RequiresCustomName>false</RequiresCustomName>
            <Color>COLOR_RELIGION_ANIMISM</Color>
        </Row>
    </Religions>

    <Colors>
        <Row ColorType="COLOR_RELIGION_ANIMISM">
            <Red>0.3</Red>
            <Green>0.6</Green>
            <Blue>0.2</Blue>
            <Alpha>1</Alpha>
        </Row>
    </Colors>

    <LocalizedText>
        <Row Tag="LOC_RELIGION_ANIMISM_NAME" Language="en_US">
            <Text>Animism</Text>
        </Row>
    </LocalizedText>
</Database>
```

## AI Belief Preferences

Configure AI evaluation of beliefs:

```xml
<Belief_Priorities>
    <Row BeliefType="BELIEF_MY_BELIEF" ListType="MyBeliefPseudoYieldBiases"/>
</Belief_Priorities>

<AiListTypes>
    <Row ListType="MyBeliefPseudoYieldBiases"/>
</AiListTypes>

<AiLists>
    <Row ListType="MyBeliefPseudoYieldBiases" System="PseudoYieldBiases"/>
</AiLists>

<AiFavoredItems>
    <Row ListType="MyBeliefPseudoYieldBiases"
         Item="PSEUDOYIELD_CONVERT_SETTLEMENT_WITH_TEMPLE"
         Value="30000"/>
</AiFavoredItems>
```

## Best Practices

1. **Age-Appropriate** - Only add Pantheon beliefs for Antiquity; other beliefs are Exploration+
2. **Founder Check** - Always include `REQUIREMENT_PLAYER_IS_RELIGION_FOUNDER` in belief modifiers
3. **Balance Beliefs** - Ensure custom beliefs are comparable to existing ones
4. **AI Considerations** - Set appropriate `AISelectionBin` values and priorities
5. **Clear Descriptions** - Explain exactly what the belief provides
6. **Use Attachment Pattern** - Attach modifiers through `EFFECT_ATTACH_MODIFIERS` for proper scoping
7. **Shareable Flag** - Mark beliefs as `Shareable="true"` if multiple religions should be able to select them

## Related Documentation

- [Modifier System](../modifiers/Overview.md)
- [Effect Types](../modifiers/Effect-Types.md)
- [Buildings](../database/Buildings.md) - For Altar and Temple definitions
- [Types and Kinds](../database/Types-and-Kinds.md)
