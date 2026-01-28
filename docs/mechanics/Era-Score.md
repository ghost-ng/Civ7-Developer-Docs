# Era Score & Golden Ages

This document covers the Era Score and Golden Age mechanics in Civilization VII, including how scores are calculated, age progression, celebration periods, and how to create custom content.

## Overview

Civ VII approaches Era mechanics differently than previous entries:

- **Legacy Points** replace traditional Era Score as the primary progression metric
- **Age Progression** tracks progress toward transitioning between Ages
- **Golden Ages** are celebration periods tied to governments, not Era Score thresholds
- **Celebrations** are triggered when reaching certain milestones
- **Dark Ages** occur when players fail to meet minimum progression requirements

## Era System Architecture

### Three-Age Structure

| Age | Description |
|-----|-------------|
| `AGE_ANTIQUITY` | Ancient through classical periods |
| `AGE_EXPLORATION` | Medieval through renaissance periods |
| `AGE_MODERN` | Industrial through future periods |

### Core Tables

| Table | Purpose |
|-------|---------|
| `Ages` | Define the game's ages |
| `AgeProgressions` | Track legacy paths per age |
| `AgeProgressionMilestones` | Define milestone thresholds |
| `AgeProgressionEvents` | Events triggered by progression |
| `AgeProgressionRewards` | Rewards for achieving milestones |
| `GoldenAges` | Define golden age bonuses |
| `Celebrations` | Define celebration triggers and effects |

## Legacy Points (Era Score)

Legacy Points are Civ VII's equivalent to Era Score. They're earned through Legacy Paths.

### Earning Legacy Points

```xml
<AgeProgressionYieldSources>
    <Row>
        <SourceType>SOURCE_TECH_RESEARCHED</SourceType>
        <YieldType>YIELD_LEGACY</YieldType>
        <Amount>1</Amount>
        <AgeProgressionType>AGE_PROGRESSION_ANTIQUITY_SCIENCE</AgeProgressionType>
    </Row>
</AgeProgressionYieldSources>
```

### Legacy Point Sources

| Source | Legacy Path | Amount |
|--------|-------------|--------|
| `SOURCE_TECH_RESEARCHED` | Science | 1-3 per tech |
| `SOURCE_CIVIC_COMPLETED` | Culture | 1-3 per civic |
| `SOURCE_CITY_CONQUERED` | Military | 2-5 per city |
| `SOURCE_TRADE_ROUTE_COMPLETED` | Economic | 1-2 per route |
| `SOURCE_WONDER_COMPLETED` | Varies | 3-5 per wonder |
| `SOURCE_GREAT_PERSON_EARNED` | Varies | 2-4 per person |
| `SOURCE_FIRST_TO_DISCOVER` | Science | 2 bonus |
| `SOURCE_FIRST_TO_CIRCUMNAVIGATE` | Exploration | 5 bonus |

### Custom Legacy Point Source

```xml
<Types>
    <Row Type="SOURCE_MY_CUSTOM_SOURCE" Kind="KIND_AGE_PROGRESSION_SOURCE"/>
</Types>

<AgeProgressionYieldSources>
    <Row>
        <SourceType>SOURCE_MY_CUSTOM_SOURCE</SourceType>
        <YieldType>YIELD_LEGACY</YieldType>
        <Amount>3</Amount>
        <AgeProgressionType>AGE_PROGRESSION_ANTIQUITY_SCIENCE</AgeProgressionType>
    </Row>
</AgeProgressionYieldSources>
```

## Age Progression

### Progression Thresholds

Each Legacy Path has three milestones that determine Age readiness:

```xml
<AgeProgressionMilestones>
    <Row AgeProgressionMilestoneType="ANTIQUITY_SCIENCE_MILESTONE_1"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_SCIENCE"
         Threshold="15"
         AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_1"/>
    <Row AgeProgressionMilestoneType="ANTIQUITY_SCIENCE_MILESTONE_2"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_SCIENCE"
         Threshold="30"
         AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_2"/>
    <Row AgeProgressionMilestoneType="ANTIQUITY_SCIENCE_MILESTONE_3"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_SCIENCE"
         Threshold="50"
         AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_3"
         FinalMilestone="true"/>
</AgeProgressionMilestones>
```

### Progression Events

Events fire when milestones are reached:

| Event | Trigger |
|-------|---------|
| `AGE_PROGRESSION_PLAYER_MILESTONE_1` | First milestone reached |
| `AGE_PROGRESSION_PLAYER_MILESTONE_2` | Second milestone reached |
| `AGE_PROGRESSION_PLAYER_MILESTONE_3` | Final milestone reached |
| `AGE_PROGRESSION_AGE_ENDING` | Age is about to end |
| `AGE_PROGRESSION_AGE_ENDED` | Age has ended |

### Defining Age Events

```xml
<AgeProgressionEvents>
    <Row AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_1"
         Name="LOC_MILESTONE_1_NAME"
         Description="LOC_MILESTONE_1_DESC"
         LegacyPointBonus="5"/>
    <Row AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_2"
         Name="LOC_MILESTONE_2_NAME"
         Description="LOC_MILESTONE_2_DESC"
         LegacyPointBonus="10"/>
    <Row AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_3"
         Name="LOC_MILESTONE_3_NAME"
         Description="LOC_MILESTONE_3_DESC"
         LegacyPointBonus="20"/>
</AgeProgressionEvents>
```

## Golden Ages

Golden Ages in Civ VII are tied to governments and celebrations, not Era Score.

### Golden Age Structure

```xml
<Types>
    <Row Type="GOLDEN_AGE_MY_GOLDEN_AGE" Kind="KIND_GOLDEN_AGE"/>
</Types>

<GoldenAges>
    <Row GoldenAgeType="GOLDEN_AGE_MY_GOLDEN_AGE"
         Name="LOC_GOLDEN_AGE_MY_NAME"
         Description="LOC_GOLDEN_AGE_MY_DESC"
         Duration="20"/>
</GoldenAges>
```

### GoldenAges Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `GoldenAgeType` | TEXT | Unique golden age identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `Duration` | INTEGER | Turns the golden age lasts |

### Linking to Governments

Each government has associated golden ages:

```xml
<Government_ValidGoldenAges>
    <Row GovernmentType="GOVERNMENT_DESPOTISM" GoldenAgeType="GOLDEN_AGE_DESPOTISM_1"/>
    <Row GovernmentType="GOVERNMENT_DESPOTISM" GoldenAgeType="GOLDEN_AGE_DESPOTISM_2"/>
</Government_ValidGoldenAges>
```

### Golden Age Modifiers

```xml
<GoldenAgeModifiers>
    <Row GoldenAgeType="GOLDEN_AGE_MY_GOLDEN_AGE" ModifierID="MOD_GOLDEN_AGE_EFFECT"/>
</GoldenAgeModifiers>

<!-- GameEffects file -->
<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_GOLDEN_AGE_EFFECT"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_PRODUCTION</Argument>
        <Argument name="Percent">20</Argument>
    </Modifier>
</GameEffects>
```

## Celebrations

Celebrations are triggered events that can lead to golden ages.

### Celebration Types

| Type | Trigger |
|------|---------|
| `CELEBRATION_WONDER_COMPLETED` | Completing a wonder |
| `CELEBRATION_AGE_MILESTONE` | Reaching a legacy milestone |
| `CELEBRATION_GREAT_PERSON` | Recruiting a great person |
| `CELEBRATION_VICTORY_PROGRESS` | Major victory progress |
| `CELEBRATION_CITY_FOUNDED` | Founding a new city |

### Defining Celebrations

```xml
<Types>
    <Row Type="CELEBRATION_MY_CELEBRATION" Kind="KIND_CELEBRATION"/>
</Types>

<Celebrations>
    <Row CelebrationType="CELEBRATION_MY_CELEBRATION"
         Name="LOC_CELEBRATION_MY_NAME"
         Description="LOC_CELEBRATION_MY_DESC"
         Duration="10"
         TriggersGoldenAge="true"/>
</Celebrations>
```

### Celebrations Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `CelebrationType` | TEXT | Unique celebration identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `Duration` | INTEGER | Turns the celebration lasts |
| `TriggersGoldenAge` | BOOLEAN | Whether this triggers a golden age |
| `StacksWithGoldenAge` | BOOLEAN | Can run alongside golden age |

### Celebration Modifiers

```xml
<CelebrationModifiers>
    <Row CelebrationType="CELEBRATION_MY_CELEBRATION"
         ModifierId="MOD_CELEBRATION_HAPPINESS"/>
</CelebrationModifiers>

<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_CELEBRATION_HAPPINESS"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_HAPPINESS</Argument>
        <Argument name="Amount">3</Argument>
    </Modifier>
</GameEffects>
```

## Dark Ages

Dark Ages occur when players fail to meet minimum progression requirements.

### Dark Age Triggers

```xml
<DarkAgeConditions>
    <Row ConditionType="CONDITION_BELOW_MILESTONE_1"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_SCIENCE"
         ThresholdPercent="25"
         TriggersOnAgeEnd="true"/>
</DarkAgeConditions>
```

### Dark Age Effects

```xml
<DarkAgeModifiers>
    <Row DarkAgeType="DARK_AGE_DEFAULT" ModifierId="MOD_DARK_AGE_PENALTY"/>
</DarkAgeModifiers>

<GameEffects xmlns="GameEffects">
    <!-- Penalty during dark age -->
    <Modifier id="MOD_DARK_AGE_PENALTY"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_CULTURE</Argument>
        <Argument name="Percent">-15</Argument>
    </Modifier>
</GameEffects>
```

### Dark Age Consolation Rewards

Even in dark ages, players get some rewards:

```xml
<AgeProgressionRewards>
    <Row AgeProgressionRewardType="AGE_REWARD_DARK_AGE_SCIENCE_1"
         Name="LOC_DARK_AGE_SCIENCE_REWARD_NAME"
         Description="LOC_DARK_AGE_SCIENCE_REWARD_DESC"
         RewardCategory="REWARD_CATEGORY_DARK_AGE"/>
</AgeProgressionRewards>
```

## Era Score Modifiers

### Effects for Era Score

| Effect | Description |
|--------|-------------|
| `EFFECT_ADJUST_LEGACY_POINTS` | Grant or remove legacy points |
| `EFFECT_ADJUST_AGE_PROGRESSION` | Modify path progress |
| `EFFECT_TRIGGER_GOLDEN_AGE` | Force a golden age |
| `EFFECT_EXTEND_GOLDEN_AGE` | Add turns to active golden age |
| `EFFECT_TRIGGER_CELEBRATION` | Force a celebration |

### Example: Leader Ability for Era Score

```xml
<GameEffects xmlns="GameEffects">
    <!-- Leader earns bonus legacy points from wonders -->
    <Modifier id="MOD_LEADER_WONDER_LEGACY"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_ATTACH_MODIFIERS">
        <Argument name="ModifierId">ATTACH_WONDER_LEGACY_BONUS</Argument>
    </Modifier>

    <Modifier id="ATTACH_WONDER_LEGACY_BONUS"
              collection="COLLECTION_CITY_WONDERS"
              effect="EFFECT_ADJUST_LEGACY_POINTS">
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```

## Complete Example: Custom Era Mechanics

### Custom Celebration with Golden Age

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <Row Type="CELEBRATION_CULTURAL_RENAISSANCE" Kind="KIND_CELEBRATION"/>
        <Row Type="GOLDEN_AGE_CULTURAL_RENAISSANCE" Kind="KIND_GOLDEN_AGE"/>
    </Types>

    <!-- Define the celebration -->
    <Celebrations>
        <Row CelebrationType="CELEBRATION_CULTURAL_RENAISSANCE"
             Name="LOC_CELEBRATION_CULTURAL_RENAISSANCE_NAME"
             Description="LOC_CELEBRATION_CULTURAL_RENAISSANCE_DESC"
             Duration="15"
             TriggersGoldenAge="true"/>
    </Celebrations>

    <!-- Define the golden age -->
    <GoldenAges>
        <Row GoldenAgeType="GOLDEN_AGE_CULTURAL_RENAISSANCE"
             Name="LOC_GOLDEN_AGE_CULTURAL_RENAISSANCE_NAME"
             Description="LOC_GOLDEN_AGE_CULTURAL_RENAISSANCE_DESC"
             Duration="25"/>
    </GoldenAges>

    <!-- Link celebration to golden age -->
    <Celebration_GoldenAges>
        <Row CelebrationType="CELEBRATION_CULTURAL_RENAISSANCE"
             GoldenAgeType="GOLDEN_AGE_CULTURAL_RENAISSANCE"/>
    </Celebration_GoldenAges>

    <!-- Celebration modifiers -->
    <CelebrationModifiers>
        <Row CelebrationType="CELEBRATION_CULTURAL_RENAISSANCE"
             ModifierId="MOD_RENAISSANCE_GREAT_WORKS"/>
    </CelebrationModifiers>

    <!-- Golden age modifiers -->
    <GoldenAgeModifiers>
        <Row GoldenAgeType="GOLDEN_AGE_CULTURAL_RENAISSANCE"
             ModifierID="MOD_RENAISSANCE_CULTURE_BONUS"/>
    </GoldenAgeModifiers>
</Database>
```

### GameEffects for Custom Era Mechanics

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Celebration: +2 Great Work slots in museums -->
    <Modifier id="MOD_RENAISSANCE_GREAT_WORKS"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_ADJUST_GREAT_WORK_SLOTS">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_CITY_HAS_BUILDING">
                <Argument name="BuildingType">BUILDING_MUSEUM</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="Amount">2</Argument>
    </Modifier>

    <!-- Golden Age: +50% culture in all cities -->
    <Modifier id="MOD_RENAISSANCE_CULTURE_BONUS"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_CULTURE</Argument>
        <Argument name="Percent">50</Argument>
    </Modifier>
</GameEffects>
```

### Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_CELEBRATION_CULTURAL_RENAISSANCE_NAME" Language="en_US">
            <Text>Cultural Renaissance</Text>
        </Row>
        <Row Tag="LOC_CELEBRATION_CULTURAL_RENAISSANCE_DESC" Language="en_US">
            <Text>A period of artistic flourishing. Museums gain +2 Great Work slots.</Text>
        </Row>
        <Row Tag="LOC_GOLDEN_AGE_CULTURAL_RENAISSANCE_NAME" Language="en_US">
            <Text>Golden Age of Art</Text>
        </Row>
        <Row Tag="LOC_GOLDEN_AGE_CULTURAL_RENAISSANCE_DESC" Language="en_US">
            <Text>+50% [ICON_CULTURE] Culture in all cities for 25 turns.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Requirements for Era Mechanics

### Era-Gated Content

```xml
<RequirementSets>
    <Row RequirementSetId="REQSET_IN_GOLDEN_AGE"
         RequirementSetType="REQUIREMENTSET_TEST_ALL"/>
</RequirementSets>

<RequirementSetRequirements>
    <Row RequirementSetId="REQSET_IN_GOLDEN_AGE"
         RequirementId="REQ_PLAYER_IN_GOLDEN_AGE"/>
</RequirementSetRequirements>

<Requirements>
    <Row RequirementId="REQ_PLAYER_IN_GOLDEN_AGE"
         RequirementType="REQUIREMENT_PLAYER_IN_GOLDEN_AGE"/>
</Requirements>
```

### Age-Specific Requirements

| Requirement | Description |
|-------------|-------------|
| `REQUIREMENT_PLAYER_IN_GOLDEN_AGE` | Player has active golden age |
| `REQUIREMENT_PLAYER_IN_DARK_AGE` | Player is in dark age |
| `REQUIREMENT_PLAYER_IN_CELEBRATION` | Player has active celebration |
| `REQUIREMENT_PLAYER_AGE_MATCHES` | Player is in specific age |
| `REQUIREMENT_MILESTONE_REACHED` | Specific milestone achieved |

## Best Practices

1. **Balance Thresholds** - Set achievable but challenging milestone thresholds
2. **Meaningful Rewards** - Make milestone rewards worth pursuing
3. **Thematic Golden Ages** - Match golden age effects to government themes
4. **Celebration Variety** - Create diverse celebration triggers
5. **Dark Age Recovery** - Provide paths out of dark ages
6. **Clear Communication** - Use localization to explain progress clearly
7. **AI Awareness** - Ensure AI understands era mechanics

## Related Documentation

- [Legacy Paths](../legacy-paths/Overview.md)
- [Governments](../technology-civics/Governments.md)
- [Modifier System](../modifiers/Overview.md)
- [Victory Conditions](../victory-conditions/Overview.md)
- [Effect Types](../modifiers/Effect-Types.md)
