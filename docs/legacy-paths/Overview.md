# Legacy Path System

This document covers the Legacy Path system in Civilization VII - the primary progression and victory mechanism that replaces traditional victory types from previous games.

## Overview

Legacy Paths are Civ VII's core victory-like progression system. Each Age has four Legacy Paths (Science, Culture, Military, Economic), and players earn "path points" toward each by accomplishing specific goals. Completing milestones on these paths grants Legacy Points, which are the primary currency for winning the game.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Legacy Path** | A progression track within an Age (e.g., `LEGACY_PATH_ANTIQUITY_SCIENCE`) |
| **Legacy Path Class** | The category of a path (Science, Culture, Military, Economic) |
| **Path Points** | Points earned toward a specific Legacy Path |
| **Milestones** | Thresholds on each path that grant rewards when reached |
| **Legacy Points** | Final victory currency - most Legacy Points at game end wins |
| **Age Progression** | Timer system that advances the game through Ages |

## Core Tables

| Table | Purpose |
|-------|---------|
| `LegacyPathClasses` | Define the four path categories |
| `LegacyPaths` | Define specific paths for each Age |
| `AgeProgressions` | Age timer configuration |
| `AgeProgressionMilestones` | Milestone thresholds per path |
| `AgeProgressionMilestoneRewards` | Rewards granted at milestones |
| `AgeProgressionRewards` | Define reward modifiers |
| `AgeProgressionEvents` | Events that accelerate Age progression |
| `Strategies` | AI strategy definitions for paths |
| `StrategyConditions` | Conditions for AI to follow a strategy |

## Legacy Path Classes

Four fundamental classes are defined in `base-standard/data/gameplay.xml`:

```xml
<LegacyPathClasses>
    <Row LegacyPathClassType="LEGACY_PATH_CLASS_CULTURE" Name="LOC_LEGACY_PATH_CLASS_CULTURE_NAME" />
    <Row LegacyPathClassType="LEGACY_PATH_CLASS_ECONOMIC" Name="LOC_LEGACY_PATH_CLASS_ECONOMIC_NAME" />
    <Row LegacyPathClassType="LEGACY_PATH_CLASS_MILITARY" Name="LOC_LEGACY_PATH_CLASS_MILITARY_NAME" />
    <Row LegacyPathClassType="LEGACY_PATH_CLASS_SCIENCE" Name="LOC_LEGACY_PATH_CLASS_SCIENCE_NAME" />
</LegacyPathClasses>
```

### LegacyPathClasses Columns

| Column | Type | Description |
|--------|------|-------------|
| `LegacyPathClassType` | TEXT | Unique identifier for the class |
| `Name` | TEXT | Localization key for display name |

## Defining Legacy Paths

### Step 1: Register the Type

Legacy Paths use `KIND_LEGACY_PATH` in Antiquity but `KIND_VICTORY` in later ages:

```xml
<!-- Antiquity uses KIND_LEGACY_PATH -->
<Types>
    <Row Type="LEGACY_PATH_ANTIQUITY_SCIENCE" Kind="KIND_LEGACY_PATH"/>
</Types>

<!-- Exploration/Modern use KIND_VICTORY -->
<Types>
    <Row Type="LEGACY_PATH_EXPLORATION_SCIENCE" Kind="KIND_VICTORY"/>
</Types>
```

### Step 2: Define the Legacy Path

```xml
<LegacyPaths>
    <Row LegacyPathType="LEGACY_PATH_ANTIQUITY_SCIENCE"
         Age="AGE_ANTIQUITY"
         LegacyPathClassType="LEGACY_PATH_CLASS_SCIENCE"
         Name="LOC_LEGACY_PATH_ANTIQUITY_SCIENCE_NAME"
         Description="LOC_LEGACY_PATH_ANTIQUITY_SCIENCE_DESCRIPTION"/>
</LegacyPaths>
```

### LegacyPaths Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `LegacyPathType` | TEXT | Unique path identifier |
| `Age` | TEXT | Age this path belongs to (AGE_ANTIQUITY, AGE_EXPLORATION, AGE_MODERN) |
| `LegacyPathClassType` | TEXT | Reference to LegacyPathClasses |
| `Name` | TEXT | Localization key for path name |
| `Description` | TEXT | Localization key for description |

## Legacy Paths by Age

### Antiquity Age

| Path | Class | Goal |
|------|-------|------|
| `LEGACY_PATH_ANTIQUITY_SCIENCE` | Science | Collect Great Works of Writing |
| `LEGACY_PATH_ANTIQUITY_CULTURE` | Culture | Build Wonders |
| `LEGACY_PATH_ANTIQUITY_MILITARY` | Military | Control settlements (conquest/expansion) |
| `LEGACY_PATH_ANTIQUITY_ECONOMIC` | Economic | Collect unique resources |

### Exploration Age

| Path | Class | Goal |
|------|-------|------|
| `LEGACY_PATH_EXPLORATION_SCIENCE` | Science | Build high-yield districts (40+ yield) |
| `LEGACY_PATH_EXPLORATION_CULTURE` | Culture | Collect Relics |
| `LEGACY_PATH_EXPLORATION_MILITARY` | Military | Settle/conquer in Distant Lands |
| `LEGACY_PATH_EXPLORATION_ECONOMIC` | Economic | Accumulate treasure resources |

### Modern Age

| Path | Class | Goal |
|------|-------|------|
| `LEGACY_PATH_MODERN_SCIENCE` | Science | Complete space projects |
| `LEGACY_PATH_MODERN_CULTURE` | Culture | Collect Artifacts |
| `LEGACY_PATH_MODERN_MILITARY` | Military | Conquer with ideology bonuses |
| `LEGACY_PATH_MODERN_ECONOMIC` | Economic | Build industrial infrastructure |

## Milestones

Each Legacy Path has 3 milestones. Reaching them grants rewards and accelerates Age progression.

### Step 1: Register Milestone Type

```xml
<Types>
    <Row Type="ANTIQUITY_SCIENCE_MILESTONE_1" Kind="KIND_AGE_PROGRESSION_MILESTONE"/>
    <Row Type="ANTIQUITY_SCIENCE_MILESTONE_2" Kind="KIND_AGE_PROGRESSION_MILESTONE"/>
    <Row Type="ANTIQUITY_SCIENCE_MILESTONE_3" Kind="KIND_AGE_PROGRESSION_MILESTONE"/>
</Types>
```

### Step 2: Define Milestones

```xml
<AgeProgressionMilestones>
    <Row AgeProgressionMilestoneType="ANTIQUITY_SCIENCE_MILESTONE_1"
         LegacyPathType="LEGACY_PATH_ANTIQUITY_SCIENCE"
         RequiredPathPoints="3"
         AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_1"/>
    <Row AgeProgressionMilestoneType="ANTIQUITY_SCIENCE_MILESTONE_2"
         LegacyPathType="LEGACY_PATH_ANTIQUITY_SCIENCE"
         RequiredPathPoints="6"
         AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_2"/>
    <Row AgeProgressionMilestoneType="ANTIQUITY_SCIENCE_MILESTONE_3"
         LegacyPathType="LEGACY_PATH_ANTIQUITY_SCIENCE"
         RequiredPathPoints="10"
         AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_3"
         FinalMilestone="true"/>
</AgeProgressionMilestones>
```

### AgeProgressionMilestones Columns

| Column | Type | Description |
|--------|------|-------------|
| `AgeProgressionMilestoneType` | TEXT | Unique milestone identifier |
| `LegacyPathType` | TEXT | Parent Legacy Path |
| `RequiredPathPoints` | INTEGER | Path points needed to reach |
| `AgeProgressionEventType` | TEXT | Event triggered (affects Age timer) |
| `FinalMilestone` | BOOLEAN | Whether this is the final milestone |

### Antiquity Milestone Requirements

| Path | Milestone 1 | Milestone 2 | Milestone 3 |
|------|-------------|-------------|-------------|
| Science | 3 points | 6 points | 10 points |
| Culture | 2 points | 4 points | 7 points |
| Military | 6 points | 9 points | 12 points |
| Economic | 7 points | 14 points | 20 points |

## Milestone Rewards

Link milestones to rewards using `AgeProgressionMilestoneRewards`:

```xml
<AgeProgressionMilestoneRewards>
    <!-- Immediate legacy point reward -->
    <Row AgeProgressionMilestoneType="ANTIQUITY_SCIENCE_MILESTONE_1"
         AgeProgressionRewardType="AGE_REWARD_LEGACY_POINT_SCIENCE"
         IsImmediate="true"/>
    <!-- Attribute unlock reward -->
    <Row AgeProgressionMilestoneType="ANTIQUITY_SCIENCE_MILESTONE_1"
         AgeProgressionRewardType="AGE_REWARD_MILESTONE1_SCIENCE_1"/>
</AgeProgressionMilestoneRewards>
```

### AgeProgressionMilestoneRewards Columns

| Column | Type | Description |
|--------|------|-------------|
| `AgeProgressionMilestoneType` | TEXT | Milestone this reward is for |
| `AgeProgressionRewardType` | TEXT | Reward to grant |
| `IsImmediate` | BOOLEAN | Whether reward applies immediately |

## Age Progression Rewards

Define what rewards do using GameEffects:

```xml
<AgeProgressionRewards>
    <Row AgeProgressionRewardType="AGE_REWARD_LEGACY_POINT_SCIENCE"
         ModifierId="MOD_AGE_REWARD_LEGACY_POINT_SCIENCE"
         Name="LOC_LEGACY_POINT_SCIENCE_REWARD_NAME"
         Description="LOC_LEGACY_POINT_SCIENCE_REWARD_DESCRIPTION"
         DescriptionFinalAge="LOC_LEGACY_POINT_SCIENCE_REWARD_FINAL_AGE_DESCRIPTION"
         icon="AGE_REWARD_LEGACY_POINT_SCIENCE"/>
</AgeProgressionRewards>
```

### AgeProgressionRewards Columns

| Column | Type | Description |
|--------|------|-------------|
| `AgeProgressionRewardType` | TEXT | Unique reward identifier |
| `ModifierId` | TEXT | GameEffect modifier to apply |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `DescriptionFinalAge` | TEXT | Alternate description in final Age |
| `icon` | TEXT | Icon reference |

### Reward GameEffects

```xml
<GameEffects xmlns="GameEffects">
    <!-- Grant 1 Legacy Point of Science type -->
    <Modifier id="MOD_AGE_REWARD_LEGACY_POINT_SCIENCE"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_GRANT_LEGACY_POINTS"
              permanent="true">
        <Argument name="Amount">1</Argument>
        <Argument name="Type">CARD_CATEGORY_SCIENTIFIC</Argument>
    </Modifier>

    <!-- Unlock selection for Age transition -->
    <Modifier id="MOD_AGE_REWARD_GOLDEN_AGE_SCIENCE_1"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_GRANT_UNLOCK"
              permanent="true">
        <Argument name="Unlock">UNLOCK_WON_SCIENTIFIC_VICTORY_1</Argument>
    </Modifier>
</GameEffects>
```

## Path Point Scoring

Define how players earn path points using scoring modifiers:

```xml
<PlayerModifiers>
    <Row ModifierId="VICTORY_ANTIQUITY_SCIENCE_GREAT_WORK_SCORING"/>
</PlayerModifiers>

<Modifiers>
    <Row ModifierId="VICTORY_ANTIQUITY_SCIENCE_GREAT_WORK_SCORING"
         ModifierType="MODIFIER_PLAYER_ADJUST_GREAT_WORK_SCORING"
         SubjectRequirementSetId="REQSET_PLAYER_IS_MAJOR"/>
</Modifiers>

<ModifierArguments>
    <Row ModifierId="VICTORY_ANTIQUITY_SCIENCE_GREAT_WORK_SCORING"
         Name="VP" Value="1"/>
    <Row ModifierId="VICTORY_ANTIQUITY_SCIENCE_GREAT_WORK_SCORING"
         Name="LegacyPathType" Value="LEGACY_PATH_ANTIQUITY_SCIENCE"/>
    <Row ModifierId="VICTORY_ANTIQUITY_SCIENCE_GREAT_WORK_SCORING"
         Name="GreatWorkObjectType" Value="GREATWORKOBJECT_WRITING"/>
</ModifierArguments>
```

### Scoring Modifier Types

| ModifierType | What it scores |
|--------------|----------------|
| `MODIFIER_PLAYER_ADJUST_GREAT_WORK_SCORING` | Great Works by type |
| `MODIFIER_PLAYER_ADJUST_WONDER_SCORING` | Wonders built |
| `MODIFIER_PLAYER_ADJUST_SETTLEMENT_COUNT_SCORING` | Settlements (conquest/non-conquest) |
| `MODIFIER_PLAYER_ADJUST_RESOURCE_SCORING` | Resources collected |
| `MODIFIER_PLAYER_ADJUST_HIGH_YIELD_DISTRICT_SCORING` | High-yield districts |
| `MODIFIER_PLAYER_ADJUST_SETTLEMENT_SCORING` | Modern settlement scoring |

### Scoring Arguments

| Argument | Description |
|----------|-------------|
| `VP` | Path points awarded per item |
| `LegacyPathType` | Which path gets the points |
| `Conquest` | Only count conquered settlements |
| `NonConquest` | Only count peacefully acquired settlements |
| `DistantLands` | Only count settlements in distant lands |
| `FollowsReligion` | Bonus if settlement follows your religion |
| `HaveIdeology` | Bonus if you have an ideology |
| `OpposingIdeology` | Bonus for conquering opposing ideology |
| `GreatWorkObjectType` | Filter by Great Work type |
| `Reversible` | Whether losing the item loses points |

## Age Progression System

Ages advance based on accumulated progression points:

```xml
<AgeProgressions>
    <Row AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_AGE_TIMER"
         AgeType="AGE_ANTIQUITY"
         EndsAge="true"
         MaxPoints_Abbreviated="160"
         MaxPoints_Standard="200"
         MaxPoints_Long="240"/>
</AgeProgressions>
```

### Progression Events

Events add points to the Age timer:

```xml
<AgeProgressionEvents>
    <Row AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_1"
         Points="5"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_AGE_TIMER"/>
    <Row AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_2"
         Points="5"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_AGE_TIMER"/>
    <Row AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_3"
         Points="10"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_AGE_TIMER"/>
    <Row AgeProgressionEventType="AGE_PROGRESSION_FUTURE_CIVIC"
         Points="10"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_AGE_TIMER"/>
    <Row AgeProgressionEventType="AGE_PROGRESSION_FUTURE_TECH"
         Points="10"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_AGE_TIMER"/>
    <Row AgeProgressionEventType="AGE_PROGRESSION_PLAYER_ELIMINATED"
         Points="25"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_AGE_TIMER"/>
</AgeProgressionEvents>
```

### Per-Turn Progression

```xml
<AgeProgressionTurns>
    <Row AgeProgressionTurnType="AGE_PROGRESSION_PER_TURN_BASE"
         Points="1"
         GameSpeedScaling="false"
         AgeProgressionType="AGE_PROGRESSION_ANTIQUITY_AGE_TIMER"/>
</AgeProgressionTurns>
```

## Dark Age Rewards

When failing to achieve milestones, Dark Age rewards provide consolation bonuses:

```xml
<AgeProgressionDarkAgeRewardInfos>
    <Row LegacyPathType="LEGACY_PATH_ANTIQUITY_SCIENCE"
         Name="LOC_LEGACY_PATH_ANTIQUITY_SCIENCE_DARK_AGE_REWARD_TEXT"
         Description="LOC_LEGACY_PATH_ANTIQUITY_SCIENCE_DARK_AGE_DESCRIPTION"
         Icon="CARD_AT_EXP_DARK_AGE_SCIENCE"/>
</AgeProgressionDarkAgeRewardInfos>
```

## AI Legacy Path Strategies

Define how AI pursues Legacy Paths:

```xml
<Strategies>
    <Row StrategyType="LEGACY_PATH_STRATEGY_SCIENCE"
         LegacyPathType="LEGACY_PATH_ANTIQUITY_SCIENCE"
         MinNumConditionsNeeded="2"
         MaxNumConditionsNeeded="5"/>
</Strategies>
```

### Strategy Conditions

Conditions the AI evaluates to determine if it should pursue a strategy:

```xml
<StrategyConditions>
    <Row StrategyType="LEGACY_PATH_STRATEGY_SCIENCE"
         ConditionFunction="GovernmentMatches"
         ThresholdValue="1"
         StringValue="GOVERNMENT_DESPOTISM"/>
    <Row StrategyType="LEGACY_PATH_STRATEGY_SCIENCE"
         ConditionFunction="NumCities"
         ThresholdValue="3"/>
    <Row StrategyType="LEGACY_PATH_STRATEGY_SCIENCE"
         ConditionFunction="TopSciencePercent"
         ThresholdValue="33"/>
    <Row StrategyType="LEGACY_PATH_STRATEGY_SCIENCE"
         ConditionFunction="NumGreatWorks"
         ThresholdValue="2"/>
    <Row StrategyType="LEGACY_PATH_STRATEGY_SCIENCE"
         ConditionFunction="HasLeaderTrait"
         ThresholdValue="1"
         StringValue="TRAIT_LEADER_ATTRIBUTE_SCIENTIFIC"/>
</StrategyConditions>
```

### Available Condition Functions

| Function | Description |
|----------|-------------|
| `GovernmentMatches` | Player has specific government |
| `NumCities` | Player has at least X cities |
| `NumConqueredCities` | Player has conquered at least X cities |
| `NumCommanders` | Player has at least X commanders |
| `NumGreatWorks` | Player has at least X great works |
| `NumWonders` | Player has at least X wonders |
| `TopMilitaryPercent` | Player is in top X% military |
| `TopSciencePercent` | Player is in top X% science |
| `TopYieldPercent` | Player is in top X% for specific yield |
| `VictoryPoints` | Player has X path points |
| `HasLeaderTrait` | Leader has specific trait |
| `LimitedWars` | Player has limited wars |
| `NumMajorsMet` | Player has met X major civs |

## Victory Conditions

The ultimate victory is determined by Legacy Points:

```xml
<Victories>
    <Row VictoryType="VICTORY_LEGACY"
         VictoryClassType="VICTORY_CLASS_LEGACY"
         Name="LOC_VICTORY_LEGACY_NAME"
         Description="LOC_VICTORY_LEGACY_DESCRIPTION"
         RequirementSetId="REQSET_LEGACY_VICTORY"/>
</Victories>

<Requirements>
    <Row RequirementId="REQ_LEGACY_VICTORY"
         RequirementType="REQUIREMENT_TEAM_LEGACY_VICTORY"/>
</Requirements>
```

The `REQUIREMENT_TEAM_LEGACY_VICTORY` checks which team has the most Legacy Points at the end of the final Age.

## Complete Example: Custom Legacy Path

### Database XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register Types -->
    <Types>
        <Row Type="LEGACY_PATH_MY_DIPLOMATIC" Kind="KIND_VICTORY"/>
        <Row Type="MY_DIPLOMATIC_MILESTONE_1" Kind="KIND_AGE_PROGRESSION_MILESTONE"/>
        <Row Type="MY_DIPLOMATIC_MILESTONE_2" Kind="KIND_AGE_PROGRESSION_MILESTONE"/>
        <Row Type="MY_DIPLOMATIC_MILESTONE_3" Kind="KIND_AGE_PROGRESSION_MILESTONE"/>
    </Types>

    <!-- 2. Define Legacy Path Class (or use existing) -->
    <!-- Using existing LEGACY_PATH_CLASS_CULTURE for diplomatic -->

    <!-- 3. Define the Legacy Path -->
    <LegacyPaths>
        <Row LegacyPathType="LEGACY_PATH_MY_DIPLOMATIC"
             Age="AGE_EXPLORATION"
             LegacyPathClassType="LEGACY_PATH_CLASS_CULTURE"
             Name="LOC_LEGACY_PATH_MY_DIPLOMATIC_NAME"
             Description="LOC_LEGACY_PATH_MY_DIPLOMATIC_DESCRIPTION"/>
    </LegacyPaths>

    <!-- 4. Define Milestones -->
    <AgeProgressionMilestones>
        <Row AgeProgressionMilestoneType="MY_DIPLOMATIC_MILESTONE_1"
             LegacyPathType="LEGACY_PATH_MY_DIPLOMATIC"
             RequiredPathPoints="5"
             AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_1"/>
        <Row AgeProgressionMilestoneType="MY_DIPLOMATIC_MILESTONE_2"
             LegacyPathType="LEGACY_PATH_MY_DIPLOMATIC"
             RequiredPathPoints="10"
             AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_2"/>
        <Row AgeProgressionMilestoneType="MY_DIPLOMATIC_MILESTONE_3"
             LegacyPathType="LEGACY_PATH_MY_DIPLOMATIC"
             RequiredPathPoints="15"
             AgeProgressionEventType="AGE_PROGRESSION_PLAYER_MILESTONE_3"
             FinalMilestone="true"/>
    </AgeProgressionMilestones>

    <!-- 5. Define Milestone Rewards -->
    <AgeProgressionMilestoneRewards>
        <Row AgeProgressionMilestoneType="MY_DIPLOMATIC_MILESTONE_1"
             AgeProgressionRewardType="AGE_REWARD_LEGACY_POINT_CULTURE"
             IsImmediate="true"/>
        <Row AgeProgressionMilestoneType="MY_DIPLOMATIC_MILESTONE_2"
             AgeProgressionRewardType="AGE_REWARD_LEGACY_POINT_CULTURE"
             IsImmediate="true"/>
        <Row AgeProgressionMilestoneType="MY_DIPLOMATIC_MILESTONE_3"
             AgeProgressionRewardType="AGE_REWARD_LEGACY_POINT_CULTURE"
             IsImmediate="true"/>
    </AgeProgressionMilestoneRewards>

    <!-- 6. Define Scoring -->
    <PlayerModifiers>
        <Row ModifierId="MY_DIPLOMATIC_ALLIANCE_SCORING"/>
    </PlayerModifiers>

    <Modifiers>
        <Row ModifierId="MY_DIPLOMATIC_ALLIANCE_SCORING"
             ModifierType="MODIFIER_PLAYER_ADJUST_ALLIANCE_SCORING"
             SubjectRequirementSetId="REQSET_PLAYER_IS_MAJOR"/>
    </Modifiers>

    <ModifierArguments>
        <Row ModifierId="MY_DIPLOMATIC_ALLIANCE_SCORING"
             Name="VP" Value="3"/>
        <Row ModifierId="MY_DIPLOMATIC_ALLIANCE_SCORING"
             Name="LegacyPathType" Value="LEGACY_PATH_MY_DIPLOMATIC"/>
    </ModifierArguments>

    <!-- 7. Dark Age Info -->
    <AgeProgressionDarkAgeRewardInfos>
        <Row LegacyPathType="LEGACY_PATH_MY_DIPLOMATIC"
             Name="LOC_LEGACY_PATH_MY_DIPLOMATIC_DARK_AGE_REWARD_TEXT"
             Description="LOC_LEGACY_PATH_MY_DIPLOMATIC_DARK_AGE_DESCRIPTION"
             Icon="CARD_AT_MOD_DARK_AGE_CULTURE"/>
    </AgeProgressionDarkAgeRewardInfos>
</Database>
```

### Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_LEGACY_PATH_MY_DIPLOMATIC_NAME" Language="en_US">
            <Text>Diplomatic Legacy</Text>
        </Row>
        <Row Tag="LOC_LEGACY_PATH_MY_DIPLOMATIC_DESCRIPTION" Language="en_US">
            <Text>Form alliances and build diplomatic relationships to earn Legacy Points.</Text>
        </Row>
        <Row Tag="LOC_LEGACY_PATH_MY_DIPLOMATIC_DARK_AGE_REWARD_TEXT" Language="en_US">
            <Text>Isolated Start</Text>
        </Row>
        <Row Tag="LOC_LEGACY_PATH_MY_DIPLOMATIC_DARK_AGE_DESCRIPTION" Language="en_US">
            <Text>Begin the next Age with reduced diplomatic visibility.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Best Practices

1. **Balance Path Points** - Ensure milestones are achievable within the Age duration
2. **Distinct Scoring** - Each path should have unique ways to earn points
3. **AI Strategies** - Define reasonable conditions for AI pursuit
4. **Reward Variety** - Mix immediate rewards with persistent bonuses
5. **Age Consistency** - Keep path themes appropriate to the historical era
6. **Legacy Point Balance** - Ensure paths award comparable Legacy Points total
7. **Dark Age Fallbacks** - Provide meaningful consolation for missed milestones

## Related Documentation

- [Victory Conditions](../victory-conditions/Overview.md)
- [Governments](../technology-civics/Governments.md)
- [Modifier System](../modifiers/Overview.md)
- [AI Behavior](../civilization-creation/AI-Behavior.md)
