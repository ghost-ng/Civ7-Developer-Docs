# The Modifier System

The Modifier System is the primary way to add dynamic effects to game entities in Civilization VII. This system allows you to create effects that respond to game conditions.

## Core Concepts

### What is a Modifier?

A Modifier is a game effect that:
- Has an **Owner** (what it's attached to)
- Has **Subjects** (what it affects)
- Has an **Effect** (what it does)
- Can have **Requirements** (conditions for activation)

### Key Components

1. **ModifierType** = Collection + Effect
2. **ModifierArguments** = Specific parameters
3. **RequirementSets** = Conditional logic
4. **Requirements** = Individual conditions

## Two Ways to Define Modifiers

### 1. Database Tables (SQL/XML)

Traditional approach using multiple tables:

```xml
<Database>
    <!-- Define the modifier type -->
    <Types>
        <Row Type="MY_MODIFIER_TYPE" Kind="KIND_MODIFIER"/>
    </Types>

    <DynamicModifiers>
        <Row
            ModifierType="MY_MODIFIER_TYPE"
            CollectionType="COLLECTION_PLAYER_CITIES"
            EffectType="EFFECT_CITY_ADJUST_YIELD"
        />
    </DynamicModifiers>

    <!-- Create the modifier instance -->
    <Modifiers>
        <Row ModifierId="MY_MODIFIER" ModifierType="MY_MODIFIER_TYPE"/>
    </Modifiers>

    <!-- Add arguments -->
    <ModifierArguments>
        <Row ModifierId="MY_MODIFIER" Name="YieldType" Value="YIELD_FOOD"/>
        <Row ModifierId="MY_MODIFIER" Name="Amount" Value="2"/>
    </ModifierArguments>
</Database>
```

### 2. GameEffects XML (Recommended)

Simplified XML format that generates all tables automatically:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <Modifier id="MY_MODIFIER" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_FOOD</Argument>
        <Argument name="Amount">2</Argument>
        <String context="Description">LOC_MY_MODIFIER_DESCRIPTION</String>
    </Modifier>
</GameEffects>
```

**Important**: GameEffects XML files use `<GameEffects>` as root, not `<Database>`. You cannot mix database operations in a GameEffects file.

## Attaching Modifiers to Game Entities

Modifiers are attached via "EntityModifier" tables:

| Table | Owner Type |
|-------|------------|
| `TraitModifiers` | Civilization/Leader traits |
| `TraditionModifiers` | Traditions and policies |
| `ConstructibleModifiers` | Buildings and improvements |
| `UnitAbilityModifiers` | Unit abilities |
| `GameModifiers` | Global game effects |
| `NarrativeStory_Rewards` | Narrative event rewards |

Example:

```xml
<TraditionModifiers>
    <Row TraditionType="TRADITION_MY_POLICY" ModifierId="MY_MODIFIER"/>
</TraditionModifiers>
```

## Requirements

Requirements add conditional logic to modifiers.

### RequirementSets

```xml
<RequirementSets>
    <Row RequirementSetId="MY_REQUIREMENTS" RequirementSetType="REQUIREMENTSET_TEST_ALL"/>
</RequirementSets>
```

| Type | Behavior |
|------|----------|
| `REQUIREMENTSET_TEST_ALL` | ALL requirements must be met |
| `REQUIREMENTSET_TEST_ANY` | ANY requirement can be met |

### Assigning Requirements to Modifiers

```xml
<Modifiers>
    <Row
        ModifierId="MY_MODIFIER"
        ModifierType="MY_MODIFIER_TYPE"
        OwnerRequirementSetID="OWNER_REQS"
        SubjectRequirementSetID="SUBJECT_REQS"
    />
</Modifiers>
```

- **OwnerRequirementSetID**: Check on the owner (must pass for modifier to activate)
- **SubjectRequirementSetID**: Check on each subject (only matching subjects are affected)

### Requirements in GameEffects XML

```xml
<Modifier id="MY_MODIFIER" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
    <OwnerRequirements type="REQUIREMENTSET_TEST_ALL">
        <Requirement type="REQUIREMENT_PLAYER_HAS_COMPLETED_PROGRESSION_TREE_NODE">
            <Argument name="ProgressionTreeNodeType">NODE_TECH_AQ_MASONRY</Argument>
        </Requirement>
    </OwnerRequirements>
    <SubjectRequirements type="REQUIREMENTSET_TEST_ALL">
        <Requirement type="REQUIREMENT_CITY_HAS_BUILDING">
            <Argument name="BuildingType">BUILDING_GRANARY</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_FOOD</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

## Modifier Flow

1. Game checks if Owner meets `OwnerRequirementSetID`
2. If passed, modifier activates
3. Collection gathers all potential Subjects
4. Each Subject is checked against `SubjectRequirementSetID`
5. Effect is applied to Subjects that pass

## Common Collections

| Collection | Subjects |
|------------|----------|
| `COLLECTION_OWNER` | The owner itself |
| `COLLECTION_PLAYER_CITIES` | All player's cities |
| `COLLECTION_PLAYER_UNITS` | All player's units |
| `COLLECTION_PLAYER_COMBAT` | Player's combat units |
| `COLLECTION_PLAYER_PLOT_YIELDS` | Player's plot yields |
| `COLLECTION_OWNER_CITY` | The owning city |

## Common Effects

See [Effect Types](Effect-Types.md) for a complete list.

| Effect | Description |
|--------|-------------|
| `EFFECT_CITY_ADJUST_YIELD` | Modify city yields |
| `EFFECT_PLOT_ADJUST_YIELD` | Modify plot yields |
| `EFFECT_UNIT_ADJUST_COMBAT_STRENGTH` | Modify unit combat |
| `EFFECT_PLAYER_GRANT_YIELD` | Grant resources to player |
| `EFFECT_CITY_GRANT_UNIT` | Grant a unit to a city |

## Permanent Modifiers

For narrative events and one-time effects, use `permanent="true"`:

```xml
<Modifier id="MY_PERMANENT_MOD" collection="COLLECTION_OWNER" effect="EFFECT_PLAYER_GRANT_YIELD" permanent="true">
    <Argument name="YieldType">YIELD_GOLD</Argument>
    <Argument name="Amount">100</Argument>
</Modifier>
```

## Best Practices

1. **Use GameEffects XML** for new modifiers - it's simpler and less error-prone
2. **Keep ModifierIds unique** - prefix with your mod ID
3. **Test with new games** - saved games may have stale modifier data
4. **Check Database.log** for modifier-related errors
5. **Reference game files** for working examples

## See Also

- [Effect Types](Effect-Types.md)
- [Collection Types](Collection-Types.md)
- [Requirement Types](Requirement-Types.md)
- [GameEffects XML](GameEffects-XML.md)
