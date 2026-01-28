# Great People System

This document covers the Great People system in Civilization VII, including Great Person classes, individuals, their actions, Great Works, and how to create custom Great People.

## Overview

Civ VII's Great People system differs from previous entries in the series. Great People are:
- **Civilization-specific** - Each civilization has its own unique Great Person class
- **Age-specific** - Great People are tied to specific Ages
- **Action-based** - Each Great Person has a unique "retire" action with specific requirements and effects
- **Individual-based** - Named historical figures with unique abilities

## Core Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register Great Person classes and individuals as kinds |
| `GreatPersonClasses` | Define Great Person class categories (per civilization) |
| `GreatPersonIndividuals` | Define individual Great People and their actions |
| `GreatPersonIndividualActionModifiers` | Link individuals to their action effects |
| `GreatWorks` | Define Great Works that can be created |
| `GreatWorkObjectTypes` | Define types of Great Work objects |
| `GreatWorkSlotTypes` | Define slots for storing Great Works |
| `GreatWork_YieldChanges` | Define yields from Great Works |
| `Constructible_GreatWorks` | Link buildings to Great Work slots |

## Type Kinds

The Great People system uses two type kinds:

```xml
<Types>
    <!-- Great Person Class (civilization-specific) -->
    <Row Type="GREAT_PERSON_CLASS_MY_CLASS" Kind="KIND_GREAT_PERSON_CLASS"/>

    <!-- Individual Great Person -->
    <Row Type="GREAT_PERSON_INDIVIDUAL_MY_PERSON" Kind="KIND_GREAT_PERSON_INDIVIDUAL"/>
</Types>
```

## Great Person Classes

Each civilization has a unique Great Person class that represents their cultural tradition of scholarship or leadership.

### Built-in Classes by Civilization

#### Antiquity Age
| Class | Civilization | Unit Type |
|-------|--------------|-----------|
| `GREAT_PERSON_CLASS_LOGIOS` | Greece | UNIT_LOGIOS |
| `GREAT_PERSON_CLASS_SHI_DAFU` | Han | UNIT_SHI_DAFU |
| `GREAT_PERSON_CLASS_TJATY` | Egypt | UNIT_TJATY |

#### Exploration Age
| Class | Civilization | Unit Type |
|-------|--------------|-----------|
| `GREAT_PERSON_CLASS_ALIM` | Abbasid | UNIT_ALIM |
| `GREAT_PERSON_CLASS_CONQUISTADOR` | Spain | UNIT_CONQUISTADOR |

### Defining a Great Person Class

```xml
<GreatPersonClasses>
    <Row GreatPersonClassType="GREAT_PERSON_CLASS_MY_CLASS"
         Name="LOC_UNIT_MY_CLASS_NAME"
         UnitType="UNIT_MY_GREATPERSON"
         UniqueQuarterType="QUARTER_MY_QUARTER"
         AvailableInTimeline="false"
         GenerateDuplicateIndividuals="true"
         PseudoYieldType="PSEUDOYIELD_STANDING_ARMY_COMMANDER"
         IconString="unitflag_my_class"
         ActionIcon="action_greatperson"/>
</GreatPersonClasses>
```

### GreatPersonClasses Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `GreatPersonClassType` | TEXT | Unique class identifier |
| `Name` | TEXT | Localization key for class name |
| `UnitType` | TEXT | Base unit type for this class |
| `UniqueQuarterType` | TEXT | Quarter required to spawn (optional) |
| `ConstructibleType` | TEXT | Building required to spawn (alternative to quarter) |
| `PopulationRequired` | INTEGER | Minimum population to spawn (optional) |
| `AvailableInTimeline` | BOOLEAN | Show in timeline (usually false) |
| `GenerateDuplicateIndividuals` | BOOLEAN | Allow duplicates in different games |
| `PseudoYieldType` | TEXT | AI evaluation pseudo-yield |
| `IconString` | TEXT | Icon reference |
| `ActionIcon` | TEXT | Action button icon |

## Great Person Individuals

Each Great Person class has multiple named individuals, each with unique action abilities.

### Defining an Individual

```xml
<GreatPersonIndividuals>
    <Row GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_MY_PERSON"
         Name="LOC_GREAT_PERSON_INDIVIDUAL_MY_PERSON_NAME"
         ActionEffectTextOverride="LOC_GREAT_PERSON_INDIVIDUAL_MY_PERSON_DESCRIPTION"
         GreatPersonClassType="GREAT_PERSON_CLASS_MY_CLASS"
         AgeType="AGE_ANTIQUITY"
         Gender="M"
         ActionCharges="1"
         ActionRequiresOwnedTile="true"
         ActionRequiresCompletedDistrictType="DISTRICT_URBAN"
         ActionNameTextOverride="LOC_GREATPERSON_ACTION_NAME_RETIRE"
         UnitType="UNIT_MY_PERSON"/>
</GreatPersonIndividuals>
```

### GreatPersonIndividuals Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `GreatPersonIndividualType` | TEXT | Unique individual identifier |
| `Name` | TEXT | Localization key for name |
| `ActionEffectTextOverride` | TEXT | Localization key for action description |
| `GreatPersonClassType` | TEXT | Parent class reference |
| `AgeType` | TEXT | Age when this person appears |
| `Gender` | TEXT | M or F for display purposes |
| `ActionCharges` | INTEGER | Number of times action can be used |
| `UnitType` | TEXT | Specific unit type for this individual |
| `ActionNameTextOverride` | TEXT | Localization key for action button |

### Action Requirements

Various requirements can be set for where/when the Great Person's action can be used:

| Requirement Column | Description |
|--------------------|-------------|
| `ActionRequiresOwnedTile` | Must be on owned territory |
| `ActionRequiresForeignTile` | Must be on foreign territory |
| `ActionRequiresCity` | Must be in a city |
| `ActionRequiresCapital` | Must be in capital |
| `ActionRequiresCompletedDistrictType` | Requires specific district |
| `ActionRequiresCompletedQuarterType` | Requires specific quarter |
| `ActionRequiresCompletedConstructibleType` | Requires specific building |
| `ActionRequiresCompletedConstructibleTag` | Requires building with tag |
| `ActionRequiresIncompleteWonder` | Requires incomplete wonder |
| `ActionRequiresNoMilitaryUnit` | No military unit on tile |
| `ActionRequiresOnUnitType` | Must be on specific unit type |
| `ActionRequiresNavigableRiver` | Must be on river |
| `ActionRequiresForeignHemisphere` | Must be in foreign hemisphere |
| `ActionRequiresArmyCommanderEmptySlots` | Commander must have empty slots |
| `ActionRequiresOnBiomeType` | Must be on specific biome |
| `ActionRequiresIndependentOwnedTile` | Must be on independent territory |
| `ActionRequiresOnOrAdjacentNaturalWonder` | Near natural wonder |
| `ActionRequiresNoConstructibleTypeInCity` | City lacks building |
| `ActionRequiresLessThanXBuildings` | City has fewer than X buildings |
| `ActionRequiresConstructionTypePermission` | Has permission to build |
| `ActionRequiresSpecialistCap` | City has specialist capacity |

## Action Modifiers

Link individuals to their action effects using modifiers:

```xml
<GreatPersonIndividualActionModifiers>
    <Row GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_MY_PERSON"
         ModifierId="GREATPERSON_MY_EFFECT"
         AttachmentTargetType="GREAT_PERSON_ACTION_ATTACHMENT_TARGET_PLAYER"/>
</GreatPersonIndividualActionModifiers>
```

### Attachment Target Types

| Target Type | Description |
|-------------|-------------|
| `GREAT_PERSON_ACTION_ATTACHMENT_TARGET_PLAYER` | Attaches to player |
| `GREAT_PERSON_ACTION_ATTACHMENT_TARGET_UNIT_GREATPERSON` | Attaches to great person unit |
| `GREAT_PERSON_ACTION_ATTACHMENT_TARGET_DISTRICT_IN_TILE` | Attaches to district on tile |
| `GREAT_PERSON_ACTION_ATTACHMENT_TARGET_DISTRICT_WONDER_IN_TILE` | Attaches to wonder on tile |
| `GREAT_PERSON_ACTION_ATTACHMENT_TARGET_COMMANDER_IN_TILE` | Attaches to commander on tile |
| `GREAT_PERSON_ACTION_ATTACHMENT_TARGET_ARMY_IN_TILE` | Attaches to army on tile |

## Common Action Effects

### Yield Bonuses

```xml
<!-- Grant yield burst -->
<Modifier id="GREATPERSON_CULTURE"
          collection="COLLECTION_OWNER"
          effect="EFFECT_PLAYER_GRANT_YIELD"
          run-once="true" permanent="true">
    <Argument name="YieldType">YIELD_CULTURE</Argument>
    <Argument name="Amount" type="ScaleByGameSpeed">100</Argument>
</Modifier>

<!-- Add yield to building -->
<Modifier id="GREATPERSON_ACADEMY_CULTURE"
          collection="COLLECTION_OWNER"
          effect="EFFECT_CITY_ADJUST_CONSTRUCTIBLE_YIELD"
          run-once="false" permanent="true">
    <Argument name="ConstructibleType">BUILDING_ACADEMY</Argument>
    <Argument name="YieldType">YIELD_CULTURE</Argument>
    <Argument name="Amount">4</Argument>
</Modifier>
```

### Unit Spawning

```xml
<!-- Grant units with special ability -->
<Modifier id="GREATPERSON_GRANT_HOPLITE"
          collection="COLLECTION_UNIT_NEAREST_OWNER_CITY"
          effect="EFFECT_GRANT_UNIT_OF_CLASS_AND_APPLY_ABILITY"
          run-once="true" permanent="true">
    <Argument name="UnitTag">UNIT_CLASS_HOPLITE</Argument>
    <Argument name="UnitAbilityType">ABILITY_MY_BONUS</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

### Wonder Production

```xml
<Modifier id="GREATPERSON_WONDER_PRODUCTION_LARGE"
          collection="COLLECTION_OWNER"
          effect="EFFECT_GRANT_PRODUCTION_IN_CITY"
          run-once="true" permanent="true">
    <Argument name="Amount" type="ScaleByGameSpeed">250</Argument>
    <Argument name="KeepOverflow">false</Argument>
</Modifier>
```

### Special Effects

```xml
<!-- Grant Golden Age -->
<Modifier id="GREATPERSON_GOLDEN_AGE"
          collection="COLLECTION_OWNER"
          effect="EFFECT_PLAYER_GRANT_GOLDEN_AGE"
          run-once="true" permanent="true"/>

<!-- Grant free technology -->
<Modifier id="GREATPERSON_RANDOM_TECH"
          collection="COLLECTION_OWNER"
          effect="EFFECT_PLAYER_GRANT_PROGRESSION"
          permanent="true">
    <Argument name="Amount">1</Argument>
    <Argument name="SystemType">SYSTEM_TECH</Argument>
    <Argument name="FreeProgressionType">FREE_PROGRESSION_NODE_ANY_UNLOCKED</Argument>
</Modifier>

<!-- Grant commander promotion -->
<Modifier id="GREATPERSON_COMMANDER_PROMOTION"
          collection="COLLECTION_OWNER"
          effect="EFFECT_GRANT_UNIT_PROMOTION"
          run-once="true" permanent="true">
    <Argument name="Amount">1</Argument>
</Modifier>
```

## Great Works

Great Works are collectible items created by Great People or found through exploration.

### Great Work Object Types

```xml
<GreatWorkObjectTypes>
    <Row GreatWorkObjectType="GREATWORKOBJECT_WRITING"
         Value="0"
         Name="LOC_GREATWORK_CODEX_NAME"
         PseudoYieldType="PSEUDOYIELD_GREAT_WORK"
         IconString="[ICON_GreatWork_Codex]"/>
</GreatWorkObjectTypes>
```

### Great Work Slot Types

```xml
<GreatWorkSlotTypes>
    <Row GreatWorkSlotType="GREATWORKSLOT_WRITING"/>
</GreatWorkSlotTypes>

<GreatWork_ValidSubTypes>
    <Row GreatWorkSlotType="GREATWORKSLOT_WRITING"
         GreatWorkObjectType="GREATWORKOBJECT_WRITING"/>
</GreatWork_ValidSubTypes>
```

### Defining Great Works

```xml
<Types>
    <Row Type="GREATWORK_MY_CODEX" Kind="KIND_GREATWORK"/>
</Types>

<GreatWorks>
    <!-- Generic Great Work -->
    <Row GreatWorkType="GREATWORK_MY_CODEX"
         GreatWorkObjectType="GREATWORKOBJECT_WRITING"
         Name="LOC_GREATWORK_MY_CODEX_NAME"
         Description="LOC_GREATWORK_MY_CODEX_DESCRIPTION"
         Image="fs://game/gw_books.png"/>

    <!-- Great Person-linked Great Work -->
    <Row GreatWorkType="GREATWORK_SAPPHO_APHRODITE"
         GreatWorkObjectType="GREATWORKOBJECT_WRITING"
         GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_LOGIOS_SAPPHO"
         Name="LOC_GREATWORK_SAPPHO_APHRODITE_NAME"
         Description="LOC_GREATWORK_SAPPHO_APHRODITE_DESCRIPTION"
         Generic="false"
         Image="fs://game/gw_rotulus.png"/>
</GreatWorks>
```

### Great Work Yields

```xml
<GreatWork_YieldChanges>
    <Row GreatWorkType="GREATWORK_MY_CODEX"
         YieldType="YIELD_SCIENCE"
         YieldChange="2"/>
</GreatWork_YieldChanges>
```

### Buildings with Great Work Slots

```xml
<Constructible_GreatWorks>
    <Row ConstructibleType="BUILDING_LIBRARY"
         GreatWorkSlotType="GREATWORKSLOT_WRITING"
         NumSlots="2"/>
    <Row ConstructibleType="BUILDING_ACADEMY"
         GreatWorkSlotType="GREATWORKSLOT_WRITING"
         NumSlots="3"/>
</Constructible_GreatWorks>
```

## Complete Example: Custom Great Person Class

### Step 1: Register Types

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <!-- Great Person Class -->
        <Row Type="GREAT_PERSON_CLASS_SAGE" Kind="KIND_GREAT_PERSON_CLASS"/>

        <!-- Individuals -->
        <Row Type="GREAT_PERSON_INDIVIDUAL_SAGE_CONFUCIUS" Kind="KIND_GREAT_PERSON_INDIVIDUAL"/>
        <Row Type="GREAT_PERSON_INDIVIDUAL_SAGE_SUNZI" Kind="KIND_GREAT_PERSON_INDIVIDUAL"/>
        <Row Type="GREAT_PERSON_INDIVIDUAL_SAGE_ZHUANGZI" Kind="KIND_GREAT_PERSON_INDIVIDUAL"/>

        <!-- Unit ability -->
        <Row Type="ABILITY_SAGE_ENLIGHTENMENT" Kind="KIND_ABILITY"/>

        <!-- Great Work -->
        <Row Type="GREATWORK_ART_OF_WAR" Kind="KIND_GREATWORK"/>
    </Types>
</Database>
```

### Step 2: Define the Class

```xml
<GreatPersonClasses>
    <Row GreatPersonClassType="GREAT_PERSON_CLASS_SAGE"
         Name="LOC_UNIT_SAGE_NAME"
         UnitType="UNIT_SAGE"
         UniqueQuarterType="QUARTER_CONFUCIAN_ACADEMY"
         AvailableInTimeline="false"
         GenerateDuplicateIndividuals="true"
         PseudoYieldType="PSEUDOYIELD_STANDING_ARMY_COMMANDER"
         IconString="unitflag_sage"
         ActionIcon="action_greatperson"/>
</GreatPersonClasses>
```

### Step 3: Define Individuals

```xml
<GreatPersonIndividuals>
    <!-- Confucius: Grants culture and happiness -->
    <Row GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_SAGE_CONFUCIUS"
         Name="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_CONFUCIUS_NAME"
         ActionEffectTextOverride="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_CONFUCIUS_DESCRIPTION"
         GreatPersonClassType="GREAT_PERSON_CLASS_SAGE"
         AgeType="AGE_ANTIQUITY"
         Gender="M"
         ActionCharges="1"
         ActionRequiresOwnedTile="true"
         ActionRequiresCompletedDistrictType="DISTRICT_URBAN"
         ActionNameTextOverride="LOC_GREATPERSON_ACTION_NAME_RETIRE"
         UnitType="UNIT_SAGE_CONFUCIUS"/>

    <!-- Sun Zi: Grants military bonus -->
    <Row GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_SAGE_SUNZI"
         Name="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_SUNZI_NAME"
         ActionEffectTextOverride="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_SUNZI_DESCRIPTION"
         GreatPersonClassType="GREAT_PERSON_CLASS_SAGE"
         AgeType="AGE_ANTIQUITY"
         Gender="M"
         ActionCharges="1"
         ActionRequiresOnUnitType="UNIT_ARMY_COMMANDER"
         ActionRequiresOwnedTile="false"
         ActionNameTextOverride="LOC_GREATPERSON_ACTION_NAME_RETIRE"
         UnitType="UNIT_SAGE_SUNZI"/>

    <!-- Zhuangzi: Creates Great Work -->
    <Row GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_SAGE_ZHUANGZI"
         Name="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_ZHUANGZI_NAME"
         GreatPersonClassType="GREAT_PERSON_CLASS_SAGE"
         AgeType="AGE_ANTIQUITY"
         Gender="M"
         ActionCharges="0"
         ActionNameTextOverride="LOC_GREATPERSON_ACTION_NAME_RETIRE"
         UnitType="UNIT_SAGE_ZHUANGZI"/>
</GreatPersonIndividuals>
```

### Step 4: Define Action Modifiers

```xml
<GreatPersonIndividualActionModifiers>
    <!-- Confucius: Culture and happiness to city -->
    <Row GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_SAGE_CONFUCIUS"
         ModifierId="GREATPERSON_SAGE_CONFUCIUS_CULTURE"
         AttachmentTargetType="GREAT_PERSON_ACTION_ATTACHMENT_TARGET_DISTRICT_IN_TILE"/>
    <Row GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_SAGE_CONFUCIUS"
         ModifierId="GREATPERSON_SAGE_CONFUCIUS_HAPPINESS"
         AttachmentTargetType="GREAT_PERSON_ACTION_ATTACHMENT_TARGET_DISTRICT_IN_TILE"/>

    <!-- Sun Zi: Grants promotion to commander -->
    <Row GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_SAGE_SUNZI"
         ModifierId="GREATPERSON_SAGE_SUNZI_PROMOTION"
         AttachmentTargetType="GREAT_PERSON_ACTION_ATTACHMENT_TARGET_COMMANDER_IN_TILE"/>
</GreatPersonIndividualActionModifiers>
```

### Step 5: GameEffects

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Confucius Culture -->
    <Modifier id="GREATPERSON_SAGE_CONFUCIUS_CULTURE"
              collection="COLLECTION_OWNER"
              effect="EFFECT_CITY_ADJUST_YIELD"
              run-once="false" permanent="true">
        <Argument name="YieldType">YIELD_CULTURE</Argument>
        <Argument name="Amount">3</Argument>
        <String context="Summary">LOC_GREATPERSON_SAGE_CONFUCIUS_CULTURE</String>
    </Modifier>

    <!-- Confucius Happiness -->
    <Modifier id="GREATPERSON_SAGE_CONFUCIUS_HAPPINESS"
              collection="COLLECTION_OWNER"
              effect="EFFECT_CITY_ADJUST_YIELD"
              run-once="false" permanent="true">
        <Argument name="YieldType">YIELD_HAPPINESS</Argument>
        <Argument name="Amount">2</Argument>
    </Modifier>

    <!-- Sun Zi Promotion -->
    <Modifier id="GREATPERSON_SAGE_SUNZI_PROMOTION"
              collection="COLLECTION_OWNER"
              effect="EFFECT_GRANT_UNIT_PROMOTION"
              run-once="true" permanent="true">
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```

### Step 6: Great Work for Zhuangzi

```xml
<GreatWorks>
    <Row GreatWorkType="GREATWORK_ZHUANGZI_BUTTERFLY"
         GreatWorkObjectType="GREATWORKOBJECT_WRITING"
         GreatPersonIndividualType="GREAT_PERSON_INDIVIDUAL_SAGE_ZHUANGZI"
         Name="LOC_GREATWORK_ZHUANGZI_BUTTERFLY_NAME"
         Description="LOC_GREATWORK_ZHUANGZI_BUTTERFLY_DESCRIPTION"
         Generic="false"
         Image="fs://game/gw_books.png"/>
</GreatWorks>

<GreatWork_YieldChanges>
    <Row GreatWorkType="GREATWORK_ZHUANGZI_BUTTERFLY"
         YieldType="YIELD_HAPPINESS"
         YieldChange="3"/>
</GreatWork_YieldChanges>
```

### Step 7: Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_UNIT_SAGE_NAME" Language="en_US">
            <Text>Sage</Text>
        </Row>
        <Row Tag="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_CONFUCIUS_NAME" Language="en_US">
            <Text>Confucius</Text>
        </Row>
        <Row Tag="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_CONFUCIUS_DESCRIPTION" Language="en_US">
            <Text>+3 [ICON_CULTURE] Culture and +2 [ICON_HAPPINESS] Happiness to this city.</Text>
        </Row>
        <Row Tag="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_SUNZI_NAME" Language="en_US">
            <Text>Sun Zi</Text>
        </Row>
        <Row Tag="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_SUNZI_DESCRIPTION" Language="en_US">
            <Text>Grants 2 promotions to this Army Commander.</Text>
        </Row>
        <Row Tag="LOC_GREAT_PERSON_INDIVIDUAL_SAGE_ZHUANGZI_NAME" Language="en_US">
            <Text>Zhuangzi</Text>
        </Row>
        <Row Tag="LOC_GREATWORK_ZHUANGZI_BUTTERFLY_NAME" Language="en_US">
            <Text>The Butterfly Dream</Text>
        </Row>
        <Row Tag="LOC_GREATWORK_ZHUANGZI_BUTTERFLY_DESCRIPTION" Language="en_US">
            <Text>A philosophical parable about the nature of reality.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Best Practices

1. **Civilization-Appropriate** - Design Great People that fit the civilization's culture and history
2. **Varied Actions** - Provide diverse action types (yields, units, buildings, special effects)
3. **Meaningful Requirements** - Action requirements should make thematic sense
4. **Balanced Effects** - Scale effects appropriately with game speed
5. **10 Individuals per Class** - Follow the pattern of having about 10 named individuals
6. **Great Works for Writers** - Consider having some individuals create Great Works instead of immediate effects
7. **Gender Diversity** - Include both male and female historical figures
8. **Clear Descriptions** - Explain exactly what the Great Person's action provides

## Related Documentation

- [Units](../database/Units.md)
- [Modifier System](../modifiers/Overview.md)
- [Effect Types](../modifiers/Effect-Types.md)
- [Types and Kinds](../database/Types-and-Kinds.md)
- [Governments](../technology-civics/Governments.md) - For Golden Age integration
