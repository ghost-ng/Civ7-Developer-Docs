# Technology Tree System

This document covers the technology tree system in Civilization VII, including how to define technology trees, individual technologies, prerequisites, and unlocks.

## Overview

Civ VII uses a **ProgressionTree** system for both technologies and civics. Each Age has its own tech tree:
- **Antiquity Age** - `TREE_TECHS_AQ`
- **Exploration Age** - `TREE_TECHS_EX`
- **Modern Age** - `TREE_TECHS_MO`

Technologies are `KIND_TREE_NODE` types that belong to a progression tree and unlock various game elements.

## Core Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register tech nodes as `KIND_TREE_NODE` |
| `ProgressionTrees` | Define the tech tree itself |
| `ProgressionTreeNodes` | Define individual technologies |
| `ProgressionTreeNodeUnlocks` | What each tech unlocks |
| `ProgressionTreePrereqs` | Tech prerequisites |
| `TypeQuotes` | Tech quotes and audio |
| `ProgressionTree_Advisories` | Advisor recommendations |

## Defining a Technology Tree

### Step 1: Register the Tree Type

```xml
<Types>
    <Row Type="TREE_TECHS_MY_AGE" Kind="KIND_TREE"/>
</Types>
```

### Step 2: Define the Tree

```xml
<ProgressionTrees>
    <Row ProgressionTreeType="TREE_TECHS_MY_AGE"
         AgeType="AGE_ANTIQUITY"
         SystemType="SYSTEM_TECH"
         Name="LOC_TREE_TECHS_MY_AGE_NAME"/>
</ProgressionTrees>
```

### ProgressionTrees Columns

| Column | Type | Description |
|--------|------|-------------|
| `ProgressionTreeType` | TEXT | Unique tree identifier |
| `AgeType` | TEXT | Which age (`AGE_ANTIQUITY`, `AGE_EXPLORATION`, `AGE_MODERN`) |
| `SystemType` | TEXT | `SYSTEM_TECH` for technologies, `SYSTEM_CULTURE` for civics |
| `Name` | TEXT | Localization key for tree name |
| `CostProgressionModel` | TEXT | Cost scaling model (optional) |
| `CostProgressionParam1` | INTEGER | Base cost parameter (optional) |
| `PrereqFormat` | TEXT | `AND` (default) or `OR` for prerequisites |

## Defining Technologies

### Step 1: Register the Type

```xml
<Types>
    <Row Type="NODE_TECH_AQ_MY_TECH" Kind="KIND_TREE_NODE"/>
</Types>
```

### Step 2: Define the Node

```xml
<ProgressionTreeNodes>
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_MY_TECH"
         ProgressionTree="TREE_TECHS_AQ"
         Cost="125"
         Name="LOC_TECH_MY_TECH_NAME"
         IconString="tech_mytech"/>
</ProgressionTreeNodes>
```

### ProgressionTreeNodes Columns

| Column | Type | Description |
|--------|------|-------------|
| `ProgressionTreeNodeType` | TEXT | Unique technology identifier |
| `ProgressionTree` | TEXT | Parent tree reference |
| `Cost` | INTEGER | Science cost to research |
| `Name` | TEXT | Localization key for name |
| `IconString` | TEXT | Icon identifier |
| `Repeatable` | BOOLEAN | Can be researched multiple times |
| `RepeatableCostProgressionModel` | TEXT | Cost model for repeatable techs |
| `RepeatableCostProgressionParam1` | INTEGER | Cost increase per repeat |

### Standard Tech Costs (Antiquity)

| Tier | Cost | Example Techs |
|------|------|---------------|
| Starting | 1 | Agriculture |
| Early | 70-85 | Pottery, Animal Husbandry, Sailing |
| Mid-Early | 125 | Writing, Irrigation, Masonry |
| Mid | 245-255 | Currency, Bronze Working, Wheel |
| Mid-Late | 430 | Navigation, Engineering, Military Training |
| Late | 738 | Mathematics, Iron Working |
| Endgame | 1255 | Future Tech |

## Technology Unlocks

The `ProgressionTreeNodeUnlocks` table defines what each tech grants access to.

```xml
<ProgressionTreeNodeUnlocks>
    <!-- Unlock a building -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_AGRICULTURE"
         TargetKind="KIND_CONSTRUCTIBLE"
         TargetType="BUILDING_GRANARY"
         UnlockDepth="1"/>

    <!-- Unlock a unit -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_BRONZE_WORKING"
         TargetKind="KIND_UNIT"
         TargetType="UNIT_SPEARMAN"
         UnlockDepth="1"/>

    <!-- Unlock a modifier (passive bonus) -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_AGRICULTURE"
         TargetKind="KIND_MODIFIER"
         TargetType="MOD_TECH_AQ_FLAT_FOOD"
         UnlockDepth="1"/>

    <!-- Unlock a wonder -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_MASONRY"
         TargetKind="KIND_CONSTRUCTIBLE"
         TargetType="WONDER_PYRAMIDS"
         UnlockDepth="1"/>

    <!-- Unlock a project -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_WRITING"
         TargetKind="KIND_PROJECT"
         TargetType="PROJECT_CITY_SCIENCE_PROJECT"
         UnlockDepth="1"/>

    <!-- Unlock a diplomatic action -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_WRITING"
         TargetKind="KIND_DIPLOMATIC_ACTION"
         TargetType="DIPLOMACY_ACTION_ESPIONAGE_STEAL_TECH"
         UnlockDepth="2"/>
</ProgressionTreeNodeUnlocks>
```

### Unlock Columns

| Column | Type | Description |
|--------|------|-------------|
| `ProgressionTreeNodeType` | TEXT | Tech that grants the unlock |
| `TargetKind` | TEXT | Kind of unlocked item |
| `TargetType` | TEXT | Specific item type |
| `UnlockDepth` | INTEGER | 1=primary unlock, 2=mastery bonus |
| `Hidden` | BOOLEAN | Don't show in tech tree UI |
| `RequiredTraitType` | TEXT | Only for civs with this trait |

### Target Kinds

| TargetKind | Description |
|------------|-------------|
| `KIND_CONSTRUCTIBLE` | Buildings, wonders, improvements |
| `KIND_UNIT` | Military and civilian units |
| `KIND_MODIFIER` | Passive bonuses and abilities |
| `KIND_PROJECT` | City projects |
| `KIND_DIPLOMATIC_ACTION` | Espionage and diplomacy actions |

### Civilization-Specific Unlocks

Use `RequiredTraitType` to unlock unique units/buildings only for specific civilizations:

```xml
<ProgressionTreeNodeUnlocks>
    <!-- Standard unit for everyone -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_ANIMAL_HUSBANDRY"
         TargetKind="KIND_UNIT"
         TargetType="UNIT_SLINGER"
         UnlockDepth="1"/>

    <!-- Unique unit only for Maya -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_ANIMAL_HUSBANDRY"
         TargetKind="KIND_UNIT"
         TargetType="UNIT_HULCHE"
         UnlockDepth="1"
         RequiredTraitType="TRAIT_MAYA"/>

    <!-- Unique unit only for Han -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_ANIMAL_HUSBANDRY"
         TargetKind="KIND_UNIT"
         TargetType="UNIT_NU"
         UnlockDepth="1"
         RequiredTraitType="TRAIT_HAN"/>

    <!-- Unique unit only for Mississippian -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_ANIMAL_HUSBANDRY"
         TargetKind="KIND_UNIT"
         TargetType="UNIT_BURNING_ARROW"
         UnlockDepth="1"
         RequiredTraitType="TRAIT_MISSISSIPPIAN"/>
</ProgressionTreeNodeUnlocks>
```

### Unlock Depth

- **UnlockDepth="1"** - Primary unlock, shown prominently in tech tree
- **UnlockDepth="2"** - Mastery bonus, granted as secondary benefit

## Technology Prerequisites

The `ProgressionTreePrereqs` table defines the tech tree structure.

```xml
<ProgressionTreePrereqs>
    <!-- Single prereq -->
    <Row Node="NODE_TECH_AQ_POTTERY" PrereqNode="NODE_TECH_AQ_AGRICULTURE"/>

    <!-- Multiple prereqs (AND by default) -->
    <Row Node="NODE_TECH_AQ_IRRIGATION" PrereqNode="NODE_TECH_AQ_POTTERY"/>
    <Row Node="NODE_TECH_AQ_IRRIGATION" PrereqNode="NODE_TECH_AQ_ANIMAL_HUSBANDRY"/>

    <!-- End-game tech with multiple prereqs -->
    <Row Node="NODE_TECH_AQ_FUTURE_TECH" PrereqNode="NODE_TECH_AQ_NAVIGATION"/>
    <Row Node="NODE_TECH_AQ_FUTURE_TECH" PrereqNode="NODE_TECH_AQ_IRON_WORKING"/>
    <Row Node="NODE_TECH_AQ_FUTURE_TECH" PrereqNode="NODE_TECH_AQ_MATHEMATICS"/>
</ProgressionTreePrereqs>
```

### Prerequisite Columns

| Column | Type | Description |
|--------|------|-------------|
| `Node` | TEXT | Tech requiring the prerequisite |
| `PrereqNode` | TEXT | Required tech to unlock |

When multiple prerequisites exist for the same node, they are combined with AND by default. Set `PrereqFormat="OR"` on the tree for OR logic.

## Technology Quotes

Add flavor quotes that display when researching technologies:

```xml
<TypeQuotes>
    <Row Type="NODE_TECH_AQ_MY_TECH"
         Quote="LOC_TECH_MY_TECH_QUOTE"
         QuoteAuthor="LOC_TECH_MY_TECH_QUOTE_AUTHOR"
         QuoteAudio="VO_Quote_MyTech"/>
</TypeQuotes>
```

### TypeQuotes Columns

| Column | Type | Description |
|--------|------|-------------|
| `Type` | TEXT | Technology type |
| `Quote` | TEXT | Localization key for quote text |
| `QuoteAuthor` | TEXT | Localization key for attribution |
| `QuoteAudio` | TEXT | Voice-over audio reference |

## Advisor Recommendations

Mark which advisors recommend this technology:

```xml
<ProgressionTree_Advisories>
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_BRONZE_WORKING"
         AdvisoryClassType="ADVISORY_CLASS_MILITARY"/>

    <Row ProgressionTreeNodeType="NODE_TECH_AQ_WRITING"
         AdvisoryClassType="ADVISORY_CLASS_SCIENCE"/>

    <!-- Tech can have multiple advisories -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_WHEEL"
         AdvisoryClassType="ADVISORY_CLASS_HAPPINESS"/>
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_WHEEL"
         AdvisoryClassType="ADVISORY_CLASS_MILITARY"/>
</ProgressionTree_Advisories>
```

### Advisory Classes

| AdvisoryClassType | Focus |
|-------------------|-------|
| `ADVISORY_CLASS_FOOD` | Food and growth |
| `ADVISORY_CLASS_SCIENCE` | Science and research |
| `ADVISORY_CLASS_CULTURE` | Culture and tourism |
| `ADVISORY_CLASS_ECONOMIC` | Gold and trade |
| `ADVISORY_CLASS_MILITARY` | Combat and defense |
| `ADVISORY_CLASS_HAPPINESS` | Happiness and amenities |

## Technology Modifiers

Technologies often unlock modifiers that provide passive bonuses. Define these in GameEffects XML:

```xml
<GameEffects xmlns="GameEffects">
    <!-- Settlement cap increase -->
    <Modifier id="MOD_AQ_SETTLEMENT_CAP_INCREASE"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_ADJUST_SETTLEMENT_CAP"
              permanent="true">
        <Argument name="Amount">1</Argument>
        <String context="Description">LOC_MOD_AQ_SETTLEMENT_CAP_INCREASE_DESCRIPTION</String>
    </Modifier>

    <!-- Embarkation ability -->
    <Modifier id="MOD_SAILING_EMBARKATION"
              collection="COLLECTION_PLAYER_UNITS"
              effect="EFFECT_UNIT_ADJUST_EMBARKATION_TYPE"
              permanent="true">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_UNIT_DOMAIN_MATCHES">
                <Argument name="UnitDomain">DOMAIN_LAND</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="EmbarkationType">UNIT_EMBARKATION_SHALLOW_WATER</Argument>
        <String context="Name">LOC_MOD_SAILING_EMBARKATION_NAME</String>
        <String context="Description">LOC_MOD_SAILING_EMBARKATION_DESCRIPTION</String>
    </Modifier>

    <!-- Unit class strength bonus -->
    <Modifier id="MOD_AQ_TECH_INFANTRY_STRENGTH"
              collection="COLLECTION_PLAYER_UNITS"
              effect="EFFECT_UNIT_ADJUST_ABILITY">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_UNIT_TAG_MATCHES">
                <Argument name="Tag">UNIT_CLASS_INFANTRY</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="AbilityType">ABILITY_AQ_TECH_INFANTRY</Argument>
        <String context="Description">LOC_MOD_AQ_TECH_INFANTRY_STRENGTH_DESCRIPTION</String>
    </Modifier>

    <!-- Yield bonus for building tag -->
    <Modifier id="MOD_AQ_TECH_CULTURE_BUILDING"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD">
        <Argument name="Tag">CULTURE</Argument>
        <Argument name="YieldType">YIELD_CULTURE</Argument>
        <Argument name="Amount">1</Argument>
        <String context="Description">LOC_MOD_AQ_TECH_CULTURE_BUILDING_DESCRIPTION</String>
    </Modifier>
</GameEffects>
```

## Repeatable Technologies

End-game "Future Tech" can be researched multiple times:

```xml
<ProgressionTreeNodes>
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_FUTURE_TECH"
         ProgressionTree="TREE_TECHS_AQ"
         Cost="1255"
         Name="LOC_TECH_FUTURE_TECH_NAME"
         IconString="tech_futuretech"
         Repeatable="true"
         RepeatableCostProgressionModel="COST_PROGRESSION_PREVIOUS_COPIES"
         RepeatableCostProgressionParam1="1255"/>
</ProgressionTreeNodes>

<ProgressionTreeNodeUnlocks>
    <!-- Each completion grants these bonuses -->
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_FUTURE_TECH"
         TargetKind="KIND_MODIFIER"
         TargetType="MOD_AQ_FUTURE_TECH_ATTRIBUTE"
         UnlockDepth="1"/>
    <Row ProgressionTreeNodeType="NODE_TECH_AQ_FUTURE_TECH"
         TargetKind="KIND_MODIFIER"
         TargetType="MOD_AQ_FUTURE_TECH_AGE_PROGRESS"
         UnlockDepth="1"/>
</ProgressionTreeNodeUnlocks>
```

## Complete Example: Adding a New Technology

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register the type -->
    <Types>
        <Row Type="NODE_TECH_AQ_ADVANCED_WARFARE" Kind="KIND_TREE_NODE"/>
    </Types>

    <!-- 2. Add quote -->
    <TypeQuotes>
        <Row Type="NODE_TECH_AQ_ADVANCED_WARFARE"
             Quote="LOC_TECH_ADVANCED_WARFARE_QUOTE"
             QuoteAuthor="LOC_TECH_ADVANCED_WARFARE_QUOTE_AUTHOR"/>
    </TypeQuotes>

    <!-- 3. Define the technology -->
    <ProgressionTreeNodes>
        <Row ProgressionTreeNodeType="NODE_TECH_AQ_ADVANCED_WARFARE"
             ProgressionTree="TREE_TECHS_AQ"
             Cost="500"
             Name="LOC_TECH_ADVANCED_WARFARE_NAME"
             IconString="tech_advancedwarfare"/>
    </ProgressionTreeNodes>

    <!-- 4. Define prerequisites -->
    <ProgressionTreePrereqs>
        <Row Node="NODE_TECH_AQ_ADVANCED_WARFARE"
             PrereqNode="NODE_TECH_AQ_MILITARY_TRAINING"/>
        <Row Node="NODE_TECH_AQ_ADVANCED_WARFARE"
             PrereqNode="NODE_TECH_AQ_IRON_WORKING"/>
    </ProgressionTreePrereqs>

    <!-- 5. Define unlocks -->
    <ProgressionTreeNodeUnlocks>
        <!-- New building -->
        <Row ProgressionTreeNodeType="NODE_TECH_AQ_ADVANCED_WARFARE"
             TargetKind="KIND_CONSTRUCTIBLE"
             TargetType="BUILDING_WAR_ACADEMY"
             UnlockDepth="1"/>

        <!-- New unit -->
        <Row ProgressionTreeNodeType="NODE_TECH_AQ_ADVANCED_WARFARE"
             TargetKind="KIND_UNIT"
             TargetType="UNIT_ELITE_INFANTRY"
             UnlockDepth="1"/>

        <!-- Mastery bonus -->
        <Row ProgressionTreeNodeType="NODE_TECH_AQ_ADVANCED_WARFARE"
             TargetKind="KIND_MODIFIER"
             TargetType="MOD_TECH_ADVANCED_WARFARE_BONUS"
             UnlockDepth="2"/>
    </ProgressionTreeNodeUnlocks>

    <!-- 6. Advisor recommendations -->
    <ProgressionTree_Advisories>
        <Row ProgressionTreeNodeType="NODE_TECH_AQ_ADVANCED_WARFARE"
             AdvisoryClassType="ADVISORY_CLASS_MILITARY"/>
    </ProgressionTree_Advisories>
</Database>
```

### Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_TECH_ADVANCED_WARFARE_NAME" Language="en_US">
            <Text>Advanced Warfare</Text>
        </Row>
        <Row Tag="LOC_TECH_ADVANCED_WARFARE_QUOTE" Language="en_US">
            <Text>In war, the way is to avoid what is strong and strike at what is weak.</Text>
        </Row>
        <Row Tag="LOC_TECH_ADVANCED_WARFARE_QUOTE_AUTHOR" Language="en_US">
            <Text>Sun Tzu</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Antiquity Tech Tree Reference

| Technology | Cost | Prerequisites | Primary Unlocks |
|------------|------|---------------|-----------------|
| Agriculture | 1 | - | Granary, Fishing Quay |
| Pottery | 70 | Agriculture | Brickyard |
| Animal Husbandry | 70 | Agriculture | Saw Pit, Slinger |
| Sailing | 85 | Agriculture | Harbor, Galley, Embarkation |
| Writing | 125 | Pottery | Library |
| Irrigation | 125 | Pottery, Animal Husbandry | Garden, +1 Settlement Cap |
| Masonry | 125 | Animal Husbandry | Walls, Monument, Pyramids |
| Currency | 245 | Sailing, Writing | Bath, Market, Colossus |
| Bronze Working | 255 | Writing, Irrigation | Barracks, Spearman, Archer |
| Wheel | 245 | Irrigation, Masonry | Villa, Ballista, Chariot |
| Navigation | 430 | Currency | Lighthouse, Quadrireme |
| Engineering | 430 | Currency, Bronze Working | Amphitheater, Bridge |
| Military Training | 430 | Bronze Working, Wheel | Arena, Blacksmith, Flanking |
| Mathematics | 738 | Engineering | Academy, Great Library |
| Iron Working | 738 | Military Training | Horseman, Phalanx |
| Future Tech | 1255 | Navigation, Mathematics, Iron Working | Repeatable bonuses |

## Best Practices

1. **Cost Balance** - Follow the tier system; don't make techs too cheap or expensive
2. **Prerequisites** - Ensure logical tech paths; avoid dead ends
3. **Unique Unlocks** - Use `RequiredTraitType` for civ-specific content
4. **Mastery Bonuses** - Use `UnlockDepth="2"` for secondary rewards
5. **Advisors** - Assign appropriate advisory classes for AI guidance
6. **Localization** - Always provide localized names and quotes
7. **Testing** - Verify tech tree displays correctly in-game

## Related Documentation

- [Types and Kinds](../database/Types-and-Kinds.md)
- [Units](../database/Units.md)
- [Buildings](../database/Buildings.md)
- [Modifier System](../modifiers/Overview.md)
- [Civic Trees](Civic-Trees.md)
