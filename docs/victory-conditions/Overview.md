# Victory Conditions System

This document covers the victory system in Civilization VII, including victory types, legacy paths, defeat conditions, and scoring systems.

## Overview

Civ VII has a fundamentally different victory system than previous games. Instead of a single end-game victory like Science or Culture, Civ VII uses:

1. **Legacy Victory** - Accumulate legacy points across all three ages
2. **Domination Victory** - Capture all cities of all other major civilizations
3. **Legacy Paths** - Per-age victory tracks (Science, Culture, Military, Economic)

Legacy Paths replace traditional victory types - you pursue them each age to earn Legacy Points, which determine the ultimate Legacy Victory winner at game end.

## Core Tables

| Table | Purpose |
|-------|---------|
| `Kinds` | Register `KIND_VICTORY`, `KIND_DEFEAT` |
| `Types` | Register specific victory/defeat types |
| `VictoryClasses` | Categories of victories |
| `Victories` | Define victory conditions |
| `Defeats` | Define defeat conditions |
| `LegacyPathClasses` | Categories for legacy paths (Science, Culture, etc.) |
| `LegacyPaths` | Age-specific victory pursuits |
| `AgeProgressionRewards` | Rewards for legacy path milestones |
| `RequirementSets/Requirements` | Victory condition triggers |

## Victory Classes

Victory classes categorize the types of victories available:

```xml
<VictoryClasses>
    <Row VictoryClassType="VICTORY_CLASS_DOMINATION" Name="Domination"/>
    <Row VictoryClassType="VICTORY_CLASS_LEGACY" Name="Legacy"/>
    <Row VictoryClassType="VICTORY_CLASS_CULTURE" Name="Culture"/>
    <Row VictoryClassType="VICTORY_CLASS_ECONOMIC" Name="Economic"/>
    <Row VictoryClassType="VICTORY_CLASS_MILITARY" Name="Military"/>
    <Row VictoryClassType="VICTORY_CLASS_SCIENCE" Name="Science"/>
    <Row VictoryClassType="VICTORY_CLASS_LIVE_EVENT" Name="LiveEvent"/>
</VictoryClasses>
```

## Default Victories

### Domination Victory

Capture all cities belonging to other major civilizations:

```xml
<Victories>
    <Row VictoryType="VICTORY_DOMINATION"
         VictoryClassType="VICTORY_CLASS_DOMINATION"
         Name="LOC_VICTORY_DOMINATION_NAME"
         Description="LOC_VICTORY_DOMINATION_DESCRIPTION"
         RequirementSetId="REQSET_DOMINATION_VICTORY"/>
</Victories>

<!-- Requirements -->
<RequirementSets>
    <Row RequirementSetId="REQSET_DOMINATION_VICTORY"
         RequirementSetType="REQUIREMENTSET_TEST_ALL"/>
</RequirementSets>

<RequirementSetRequirements>
    <Row RequirementSetId="REQSET_DOMINATION_VICTORY"
         RequirementId="REQ_DOMINATION_VICTORY"/>
</RequirementSetRequirements>

<Requirements>
    <Row RequirementId="REQ_DOMINATION_VICTORY"
         RequirementType="REQUIREMENT_TEAM_DOMINATION_VICTORY"/>
</Requirements>
```

### Legacy Victory

Have the most legacy points at the end of the final age:

```xml
<Victories>
    <Row VictoryType="VICTORY_LEGACY"
         VictoryClassType="VICTORY_CLASS_LEGACY"
         Name="LOC_VICTORY_LEGACY_NAME"
         Description="LOC_VICTORY_LEGACY_DESCRIPTION"
         RequirementSetId="REQSET_LEGACY_VICTORY"/>
</Victories>

<RequirementSets>
    <Row RequirementSetId="REQSET_LEGACY_VICTORY"
         RequirementSetType="REQUIREMENTSET_TEST_ALL"/>
</RequirementSets>

<RequirementSetRequirements>
    <Row RequirementSetId="REQSET_LEGACY_VICTORY"
         RequirementId="REQ_LEGACY_VICTORY"/>
</RequirementSetRequirements>

<Requirements>
    <Row RequirementId="REQ_LEGACY_VICTORY"
         RequirementType="REQUIREMENT_TEAM_LEGACY_VICTORY"/>
</Requirements>
```

## Victories Table Schema

```sql
CREATE TABLE 'Victories' (
    'VictoryType' TEXT NOT NULL,
    'Description' TEXT,
    'EnabledByDefault' BOOLEAN NOT NULL DEFAULT 1,
    'LegacyPathClassType' TEXT NOT NULL DEFAULT "NO_LEGACY_PATH_CLASS",
    'Name' TEXT NOT NULL,
    'RequirementSetId' TEXT NOT NULL,
    'RequiresMultipleTeams' BOOLEAN NOT NULL DEFAULT 0,
    'VictoryClassType' TEXT NOT NULL,
    PRIMARY KEY("VictoryType")
);
```

### Column Descriptions

| Column | Type | Description |
|--------|------|-------------|
| `VictoryType` | TEXT | Unique victory identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `VictoryClassType` | TEXT | Victory category reference |
| `RequirementSetId` | TEXT | Trigger conditions |
| `EnabledByDefault` | BOOLEAN | Available without special setup |
| `RequiresMultipleTeams` | BOOLEAN | Only valid with 2+ teams |
| `LegacyPathClassType` | TEXT | Associated legacy path class |

## Defeat Conditions

Defeats define how players can lose the game:

```xml
<Types>
    <Row Type="DEFEAT_DEFAULT" Kind="KIND_DEFEAT"/>
    <Row Type="DEFEAT_RETIRE" Kind="KIND_DEFEAT"/>
    <Row Type="DEFEAT_TIME" Kind="KIND_DEFEAT"/>
</Types>

<Defeats>
    <Row DefeatType="DEFEAT_DEFAULT"
         Name="LOC_DEFEAT_DEFAULT_NAME"
         Blurb="LOC_DEFEAT_DEFAULT_TEXT"
         RequirementSetId="DEFAULT_DEFEAT_REQUIREMENTS"/>
    <Row DefeatType="DEFEAT_RETIRE"
         Name="LOC_DEFEAT_RETIRE_NAME"
         Blurb="LOC_DEFEAT_RETIRE_TEXT"/>
</Defeats>
```

### Default Defeat Requirements

Player loses when they have no settlers and no cities:

```xml
<RequirementSets>
    <Row RequirementSetId="DEFAULT_DEFEAT_REQUIREMENTS"
         RequirementSetType="REQUIREMENTSET_TEST_ALL"/>
</RequirementSets>

<RequirementSetRequirements>
    <Row RequirementSetId="DEFAULT_DEFEAT_REQUIREMENTS"
         RequirementId="DEFAULT_DEFEAT_REQUIREMENT"/>
</RequirementSetRequirements>

<Requirements>
    <Row RequirementId="DEFAULT_DEFEAT_REQUIREMENT"
         RequirementType="REQUIREMENT_PLAYER_DEFAULT_DEFEAT"/>
</Requirements>

<RequirementArguments>
    <Row RequirementId="DEFAULT_DEFEAT_REQUIREMENT"
         Name="UnitType"
         Value="UNIT_FOUNDER,UNIT_SETTLER"/>
</RequirementArguments>
```

## Legacy Path System

Legacy Paths are Civ VII's primary victory mechanism. Each age offers four paths:
- **Science** - Research and discovery
- **Culture** - Wonders and cultural achievements
- **Military** - Combat and conquest
- **Economic** - Trade and wealth

### Legacy Path Classes

```xml
<LegacyPathClasses>
    <Row LegacyPathClassType="LEGACY_PATH_CLASS_CULTURE" Name="LOC_LEGACY_PATH_CLASS_CULTURE_NAME"/>
    <Row LegacyPathClassType="LEGACY_PATH_CLASS_ECONOMIC" Name="LOC_LEGACY_PATH_CLASS_ECONOMIC_NAME"/>
    <Row LegacyPathClassType="LEGACY_PATH_CLASS_MILITARY" Name="LOC_LEGACY_PATH_CLASS_MILITARY_NAME"/>
    <Row LegacyPathClassType="LEGACY_PATH_CLASS_SCIENCE" Name="LOC_LEGACY_PATH_CLASS_SCIENCE_NAME"/>
</LegacyPathClasses>
```

### Defining Legacy Paths

Each age has its own set of legacy paths:

```xml
<!-- Exploration Age Legacy Paths -->
<Types>
    <Row Type="LEGACY_PATH_EXPLORATION_CULTURE" Kind="KIND_VICTORY"/>
    <Row Type="LEGACY_PATH_EXPLORATION_ECONOMIC" Kind="KIND_VICTORY"/>
    <Row Type="LEGACY_PATH_EXPLORATION_MILITARY" Kind="KIND_VICTORY"/>
    <Row Type="LEGACY_PATH_EXPLORATION_SCIENCE" Kind="KIND_VICTORY"/>
</Types>

<LegacyPaths>
    <Row LegacyPathType="LEGACY_PATH_EXPLORATION_CULTURE"
         Age="AGE_EXPLORATION"
         LegacyPathClassType="LEGACY_PATH_CLASS_CULTURE"
         Name="LOC_LEGACY_PATH_EXPLORATION_CULTURE_NAME"
         Description="LOC_LEGACY_PATH_EXPLORATION_CULTURE_DESCRIPTION"/>
    <Row LegacyPathType="LEGACY_PATH_EXPLORATION_ECONOMIC"
         Age="AGE_EXPLORATION"
         LegacyPathClassType="LEGACY_PATH_CLASS_ECONOMIC"
         Name="LOC_LEGACY_PATH_EXPLORATION_ECONOMIC_NAME"
         Description="LOC_LEGACY_PATH_EXPLORATION_ECONOMIC_DESCRIPTION"/>
    <Row LegacyPathType="LEGACY_PATH_EXPLORATION_MILITARY"
         Age="AGE_EXPLORATION"
         LegacyPathClassType="LEGACY_PATH_CLASS_MILITARY"
         Name="LOC_LEGACY_PATH_EXPLORATION_MILITARY_NAME"
         Description="LOC_LEGACY_PATH_EXPLORATION_MILITARY_DESCRIPTION"/>
    <Row LegacyPathType="LEGACY_PATH_EXPLORATION_SCIENCE"
         Age="AGE_EXPLORATION"
         LegacyPathClassType="LEGACY_PATH_CLASS_SCIENCE"
         Name="LOC_LEGACY_PATH_EXPLORATION_SCIENCE_NAME"
         Description="LOC_LEGACY_PATH_EXPLORATION_SCIENCE_DESCRIPTION"/>
</LegacyPaths>
```

### Legacy Paths Table Schema

```sql
CREATE TABLE 'LegacyPaths' (
    'LegacyPathType' TEXT NOT NULL,
    'Age' TEXT NOT NULL,
    'Description' TEXT,
    'EnabledByDefault' BOOLEAN NOT NULL DEFAULT 1,
    'LegacyPathClassType' TEXT NOT NULL,
    'Name' TEXT NOT NULL,
    PRIMARY KEY("LegacyPathType")
);
```

## Age Progression Rewards

Players earn rewards at milestones along their legacy path:

```xml
<AgeProgressionRewards>
    <!-- Milestone 1: Attribute point -->
    <Row AgeProgressionRewardType="AGE_REWARD_MILESTONE1_SCIENCE_1"
         ModifierId="MOD_AGE_REWARD_MILESTONE1_SCIENCE_1"
         Name="LOC_LEGACY_PATH_ATTRIBUTE_SCIENCE_REWARD_TEXT"
         Description="LOC_LEGACY_PATH_ATTRIBUTE_SCIENCE_DESCRIPTION"
         icon="ATTRIBUTE_SCIENTIFIC"/>

    <!-- Milestone 2: Special bonus -->
    <Row AgeProgressionRewardType="AGE_REWARD_MILESTONE2_SCIENCE_1"
         ModifierId="MOD_AGE_REWARD_MILESTONE2_SCIENCE_1"
         Name="LOC_LEGACY_PATH_ANTIQUITY_SCIENCE_MILESTONE_2_REWARD_TEXT"
         Description="LOC_LEGACY_PATH_ANTIQUITY_SCIENCE_MILESTONE_2_DESCRIPTION"
         icon="CARD_AT_EXP_VICTORY_SCIENTIFIC_SECOND"/>

    <!-- Golden Age bonus -->
    <Row AgeProgressionRewardType="AGE_REWARD_GOLDEN_AGE_SCIENCE_1"
         ModifierId="MOD_AGE_REWARD_GOLDEN_AGE_SCIENCE_1"
         Name="LOC_LEGACY_PATH_ANTIQUITY_SCIENCE_GOLDEN_AGE_REWARD_TEXT"
         Description="LOC_LEGACY_PATH_ANTIQUITY_SCIENCE_GOLDEN_AGE_DESCRIPTION"
         icon="CARD_AT_EXP_VICTORY_SCIENCE_GOLDEN_AGE"/>

    <!-- Legacy Points (all ages) -->
    <Row AgeProgressionRewardType="AGE_REWARD_LEGACY_POINT_SCIENCE"
         ModifierId="MOD_AGE_REWARD_LEGACY_POINT_SCIENCE"
         Name="LOC_LEGACY_POINT_SCIENCE_REWARD_NAME"
         Description="LOC_LEGACY_POINT_SCIENCE_REWARD_DESCRIPTION"
         DescriptionFinalAge="LOC_LEGACY_POINT_SCIENCE_REWARD_FINAL_AGE_DESCRIPTION"
         icon="AGE_REWARD_LEGACY_POINT_SCIENCE"/>
</AgeProgressionRewards>
```

### Reward Milestones

| Milestone | Reward Type | Description |
|-----------|-------------|-------------|
| 1st | Attribute | +1 to associated attribute |
| 2nd | Special | Path-specific bonus |
| 3rd | Golden Age | Triggers celebration period |
| Completion | Legacy Point | +1 Legacy Point for final scoring |

## Victory Types (Extended)

The `VictoryTypes` table provides additional configuration for victories:

```sql
CREATE TABLE 'VictoryTypes' (
    'VictoryType' TEXT NOT NULL,
    'CountdownDuration' INTEGER NOT NULL DEFAULT -1,
    'Description' TEXT NOT NULL,
    'DominationAmount' INTEGER NOT NULL DEFAULT -1,
    'DominationPercent' INTEGER NOT NULL DEFAULT -1,
    'FinalAge' TEXT NOT NULL,
    'Name' TEXT NOT NULL,
    'PrereqRequirementSetId' TEXT,
    PRIMARY KEY("VictoryType")
);
```

### Column Descriptions

| Column | Description |
|--------|-------------|
| `CountdownDuration` | Turns before victory is finalized |
| `DominationAmount` | Fixed number of cities to capture |
| `DominationPercent` | Percentage of cities to capture |
| `FinalAge` | Age where victory is possible |
| `PrereqRequirementSetId` | Prerequisites to attempt this victory |

## Victory Scoring

Track progress toward victory conditions:

```sql
CREATE TABLE 'VictoryScorings' (
    'ScoringId' TEXT NOT NULL,
    'Data' TEXT,
    'Name' TEXT NOT NULL,
    'Points' INTEGER NOT NULL DEFAULT 0,
    'ScoringType' TEXT NOT NULL,
    'StaticCarryover' BOOLEAN NOT NULL DEFAULT 0,
    'VictoryType' TEXT NOT NULL,
    PRIMARY KEY("ScoringId")
);
```

### Column Descriptions

| Column | Description |
|--------|-------------|
| `ScoringId` | Unique identifier for this scoring entry |
| `ScoringType` | Type of scoring mechanism |
| `Points` | Points awarded for this achievement |
| `StaticCarryover` | Whether points carry between ages |
| `VictoryType` | Associated victory condition |

## Victory Cinematics

Define victory movies and presentations:

```xml
<VictoryCinematics>
    <Row VictoryType="VICTORY_DOMINATION"
         Name="LOC_VICTORY_CINEMATIC_DOMINATION"
         VictoryCinematicType="CINEMATIC_DOMINATION"/>
</VictoryCinematics>
```

## Creating a Custom Victory

### Complete Example

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register the type -->
    <Types>
        <Row Type="VICTORY_WONDER_BUILDER" Kind="KIND_VICTORY"/>
    </Types>

    <!-- 2. Create victory class (optional, can use existing) -->
    <VictoryClasses>
        <Row VictoryClassType="VICTORY_CLASS_WONDER" Name="Wonder"/>
    </VictoryClasses>

    <!-- 3. Define the victory -->
    <Victories>
        <Row VictoryType="VICTORY_WONDER_BUILDER"
             VictoryClassType="VICTORY_CLASS_WONDER"
             Name="LOC_VICTORY_WONDER_BUILDER_NAME"
             Description="LOC_VICTORY_WONDER_BUILDER_DESCRIPTION"
             RequirementSetId="REQSET_WONDER_BUILDER_VICTORY"
             EnabledByDefault="false"/>
    </Victories>

    <!-- 4. Define victory requirements -->
    <RequirementSets>
        <Row RequirementSetId="REQSET_WONDER_BUILDER_VICTORY"
             RequirementSetType="REQUIREMENTSET_TEST_ALL"/>
    </RequirementSets>

    <RequirementSetRequirements>
        <Row RequirementSetId="REQSET_WONDER_BUILDER_VICTORY"
             RequirementId="REQ_WONDER_BUILDER_COUNT"/>
    </RequirementSetRequirements>

    <Requirements>
        <Row RequirementId="REQ_WONDER_BUILDER_COUNT"
             RequirementType="REQUIREMENT_PLAYER_HAS_WONDER_COUNT"/>
    </Requirements>

    <RequirementArguments>
        <Row RequirementId="REQ_WONDER_BUILDER_COUNT"
             Name="Count"
             Value="10"/>
    </RequirementArguments>
</Database>
```

### Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_VICTORY_WONDER_BUILDER_NAME" Language="en_US">
            <Text>Wonder Victory</Text>
        </Row>
        <Row Tag="LOC_VICTORY_WONDER_BUILDER_DESCRIPTION" Language="en_US">
            <Text>Build 10 Wonders to achieve a Wonder Victory!</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Existing Legacy Paths by Age

### Antiquity Age
- `LEGACY_PATH_ANTIQUITY_CULTURE`
- `LEGACY_PATH_ANTIQUITY_ECONOMIC`
- `LEGACY_PATH_ANTIQUITY_MILITARY`
- `LEGACY_PATH_ANTIQUITY_SCIENCE`

### Exploration Age
- `LEGACY_PATH_EXPLORATION_CULTURE`
- `LEGACY_PATH_EXPLORATION_ECONOMIC`
- `LEGACY_PATH_EXPLORATION_MILITARY`
- `LEGACY_PATH_EXPLORATION_SCIENCE`

### Modern Age
- `LEGACY_PATH_MODERN_CULTURE`
- `LEGACY_PATH_MODERN_ECONOMIC`
- `LEGACY_PATH_MODERN_MILITARY`
- `LEGACY_PATH_MODERN_SCIENCE`

## Victory-Related Requirements

| Requirement Type | Description |
|-----------------|-------------|
| `REQUIREMENT_TEAM_DOMINATION_VICTORY` | Team controls all enemy cities |
| `REQUIREMENT_TEAM_LEGACY_VICTORY` | Team has most legacy points at game end |
| `REQUIREMENT_PLAYER_DEFAULT_DEFEAT` | Player has no cities or settlers |
| `REQUIREMENT_GAME_TURN_MAX_REACHED` | Maximum turns reached |
| `REQUIREMENT_PLAYER_HAS_WONDER_COUNT` | Player has built X wonders |

## Best Practices

1. **Use Existing Classes** - Prefer existing victory classes unless you need a truly unique category
2. **Requirement Sets** - Design clear, testable victory conditions
3. **Balance** - Ensure custom victories are achievable but challenging
4. **Localization** - Provide clear, descriptive text
5. **Age Awareness** - Consider which ages your victory should be available in
6. **Legacy Integration** - Consider how custom victories interact with the legacy point system

## Related Documentation

- [Legacy Paths](Legacy-Paths.md)
- [Modifier System](../modifiers/Overview.md)
- [Requirement Types](../modifiers/Requirement-Types.md)
- [Ages and Progression](Ages.md)
