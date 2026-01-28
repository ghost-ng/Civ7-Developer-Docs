# Civic Tree System

This document covers the civic tree system in Civilization VII, including civic definitions, traditions (policies), and unlocks.

## Overview

Civ VII uses the same **ProgressionTree** system for civics as it does for technologies. The key difference is that civics use `SYSTEM_CULTURE` instead of `SYSTEM_TECH` and primarily unlock **Traditions** (policies) rather than technologies/buildings.

Each Age has its own civic tree:
- **Antiquity Age** - `TREE_CIVICS_AQ_MAIN`
- **Exploration Age** - `TREE_CIVICS_EX_MAIN`
- **Modern Age** - `TREE_CIVICS_MO_MAIN`

## Core Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register civic nodes as `KIND_TREE_NODE` |
| `ProgressionTrees` | Define the civic tree |
| `ProgressionTreeNodes` | Define individual civics |
| `ProgressionTreeNodeUnlocks` | What each civic unlocks |
| `ProgressionTreePrereqs` | Civic prerequisites |
| `TypeQuotes` | Civic quotes and audio |
| `Traditions` | Define tradition (policy) details |
| `TraditionModifiers` | Link traditions to their effects |

## Civics vs Technologies

| Aspect | Technologies | Civics |
|--------|--------------|--------|
| System Type | `SYSTEM_TECH` | `SYSTEM_CULTURE` |
| Research Resource | Science | Culture |
| Primary Unlocks | Buildings, Units | Traditions, Wonders |
| Node Prefix | `NODE_TECH_` | `NODE_CIVIC_` |
| Tree Prefix | `TREE_TECHS_` | `TREE_CIVICS_` |

## Defining a Civic Tree

### Step 1: Register the Tree Type

```xml
<Types>
    <Row Type="TREE_CIVICS_MY_AGE_MAIN" Kind="KIND_TREE"/>
</Types>
```

### Step 2: Define the Tree

```xml
<ProgressionTrees>
    <Row ProgressionTreeType="TREE_CIVICS_MY_AGE_MAIN"
         AgeType="AGE_ANTIQUITY"
         SystemType="SYSTEM_CULTURE"
         Name="LOC_TREE_CIVICS_MY_AGE_NAME"
         CostProgressionModel="NO_COST_PROGRESSION"
         CostProgressionParam1="500"/>
</ProgressionTrees>
```

### ProgressionTrees Columns for Civics

| Column | Type | Description |
|--------|------|-------------|
| `ProgressionTreeType` | TEXT | Unique tree identifier |
| `AgeType` | TEXT | Which age the tree belongs to |
| `SystemType` | TEXT | `SYSTEM_CULTURE` for civics |
| `Name` | TEXT | Localization key |
| `CostProgressionModel` | TEXT | How costs scale (often `NO_COST_PROGRESSION`) |
| `CostProgressionParam1` | INTEGER | Base cost parameter |

## Defining Civics

### Step 1: Register the Type

```xml
<Types>
    <Row Type="NODE_CIVIC_AQ_MAIN_MY_CIVIC" Kind="KIND_TREE_NODE"/>
</Types>
```

### Step 2: Define the Node

```xml
<ProgressionTreeNodes>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_MY_CIVIC"
         ProgressionTree="TREE_CIVICS_AQ_MAIN"
         Cost="245"
         Name="LOC_CIVIC_MY_CIVIC_NAME"
         IconString="cult_mycivic"/>
</ProgressionTreeNodes>
```

### Standard Civic Costs (Antiquity)

| Tier | Cost | Example Civics |
|------|------|----------------|
| Starting | 90 | Chiefdom |
| Early | 125 | Mysticism, Discipline |
| Mid-Early | 245 | Public Life, Code of Laws, Tactics |
| Mid | 450 | Entertainment, Citizenship, Organized Military |
| Mid-Late | 600 | Literacy, Skilled Trades |
| Late | 750 | Philosophy, Commerce |
| Endgame | 1000 | Future Civic |

## Traditions (Policies)

Traditions are the primary unlock from civics. They provide ongoing bonuses that can be slotted into your government.

### Defining Traditions

```xml
<Types>
    <Row Type="TRADITION_MY_POLICY" Kind="KIND_TRADITION"/>
</Types>

<Traditions>
    <Row TraditionType="TRADITION_MY_POLICY"
         Name="LOC_TRADITION_MY_POLICY_NAME"
         Description="LOC_TRADITION_MY_POLICY_DESCRIPTION"/>
</Traditions>
```

### Traditions Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `TraditionType` | TEXT | Unique tradition identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `IsCrisis` | BOOLEAN | Is this a crisis policy? |

### Linking Traditions to Effects

Use `TraditionModifiers` to define what a tradition does:

```xml
<TraditionModifiers>
    <Row TraditionType="TRADITION_SCHOLARS" ModifierId="SCHOLARS_MOD_WORKER_SCIENCE"/>
</TraditionModifiers>
```

A tradition can have multiple modifiers:

```xml
<TraditionModifiers>
    <Row TraditionType="TRADITION_DRILLS" ModifierId="DRILLS_MOD_RANGED_PRODUCTION"/>
    <Row TraditionType="TRADITION_DRILLS" ModifierId="DRILLS_MOD_INFANTRY_PRODUCTION"/>
</TraditionModifiers>
```

### Example Tradition with Modifier

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Register type -->
    <Types>
        <Row Type="TRADITION_MY_POLICY" Kind="KIND_TRADITION"/>
    </Types>

    <!-- Define tradition -->
    <Traditions>
        <Row TraditionType="TRADITION_MY_POLICY"
             Name="LOC_TRADITION_MY_POLICY_NAME"
             Description="LOC_TRADITION_MY_POLICY_DESCRIPTION"/>
    </Traditions>

    <!-- Link to modifiers -->
    <TraditionModifiers>
        <Row TraditionType="TRADITION_MY_POLICY" ModifierId="MY_POLICY_MOD_BONUS"/>
    </TraditionModifiers>
</Database>
```

And in a separate GameEffects file:

```xml
<GameEffects xmlns="GameEffects">
    <Modifier id="MY_POLICY_MOD_BONUS"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_SCIENCE</Argument>
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```

## Civic Unlocks

Civics unlock various game elements through `ProgressionTreeNodeUnlocks`:

### Unlocking Traditions

```xml
<ProgressionTreeNodeUnlocks>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_CHIEFDOM"
         TargetKind="KIND_TRADITION"
         TargetType="TRADITION_CHARISMATIC_LEADER"
         UnlockDepth="1"/>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_CHIEFDOM"
         TargetKind="KIND_TRADITION"
         TargetType="TRADITION_TOOL_MAKING"
         UnlockDepth="1"/>
</ProgressionTreeNodeUnlocks>
```

### Unlocking Buildings/Wonders

```xml
<ProgressionTreeNodeUnlocks>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_MYSTICISM"
         TargetKind="KIND_CONSTRUCTIBLE"
         TargetType="BUILDING_ALTAR"
         UnlockDepth="1"/>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_MYSTICISM"
         TargetKind="KIND_CONSTRUCTIBLE"
         TargetType="WONDER_GREAT_STELE"
         UnlockDepth="2"/>
</ProgressionTreeNodeUnlocks>
```

### Unlocking Units

```xml
<ProgressionTreeNodeUnlocks>
    <!-- Standard unit -->
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DISCIPLINE"
         TargetKind="KIND_UNIT"
         TargetType="UNIT_ARMY_COMMANDER"
         UnlockDepth="1"/>

    <!-- Civ-specific unique unit -->
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DISCIPLINE"
         TargetKind="KIND_UNIT"
         TargetType="UNIT_HAZARAPATIS"
         UnlockDepth="1"
         RequiredTraitType="TRAIT_PERSIA"/>

    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DISCIPLINE"
         TargetKind="KIND_UNIT"
         TargetType="UNIT_LEGATUS"
         UnlockDepth="1"
         RequiredTraitType="TRAIT_ROME"/>
</ProgressionTreeNodeUnlocks>
```

### Unlocking Modifiers

```xml
<ProgressionTreeNodeUnlocks>
    <!-- Pantheon unlock -->
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_MYSTICISM"
         TargetKind="KIND_MODIFIER"
         TargetType="MOD_PANTHEON_UNLOCK"
         UnlockDepth="1"/>

    <!-- Settlement cap increase -->
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_ENTERTAINMENT"
         TargetKind="KIND_MODIFIER"
         TargetType="MOD_AQ_SETTLEMENT_CAP_INCREASE"
         UnlockDepth="1"/>

    <!-- Tradition slot increase -->
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_PHILOSOPHY"
         TargetKind="KIND_MODIFIER"
         TargetType="MOD_AQ_TRADITION_SLOT"
         UnlockDepth="1"/>
</ProgressionTreeNodeUnlocks>
```

### Unlocking Projects

```xml
<ProgressionTreeNodeUnlocks>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_CITIZENSHIP"
         TargetKind="KIND_PROJECT"
         TargetType="PROJECT_CITY_CULTURE_PROJECT"
         UnlockDepth="1"/>
</ProgressionTreeNodeUnlocks>
```

### Unlocking Diplomatic Actions

```xml
<ProgressionTreeNodeUnlocks>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_CODE_OF_LAWS"
         TargetKind="KIND_DIPLOMATIC_ACTION"
         TargetType="DIPLOMACY_ACTION_IMPROVE_TRADE_RELATIONS"
         UnlockDepth="1"/>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_CODE_OF_LAWS"
         TargetKind="KIND_DIPLOMATIC_ACTION"
         TargetType="DIPLOMACY_ACTION_ESPIONAGE_STEAL_CIVIC"
         UnlockDepth="2"/>
</ProgressionTreeNodeUnlocks>
```

## Crisis Traditions

Crisis traditions are negative policies applied during crisis events:

```xml
<Types>
    <Row Type="TRADITION_BANDITRY" Kind="KIND_TRADITION"/>
</Types>

<Traditions>
    <Row TraditionType="TRADITION_BANDITRY"
         Name="LOC_TRADITION_BANDITRY_NAME"
         Description="LOC_TRADITION_BANDITRY_DESCRIPTION"
         IsCrisis="true"/>
</Traditions>

<TraditionModifiers>
    <Row TraditionType="TRADITION_BANDITRY" ModifierId="BANDITRY_MOD_GOLD"/>
</TraditionModifiers>
```

Crisis types include:
- **Invasion Crisis** - Military threats
- **Plague Crisis** - Disease events
- **Loyalty Crisis** - Government instability

## Civic Prerequisites

```xml
<ProgressionTreePrereqs>
    <!-- Linear progression -->
    <Row Node="NODE_CIVIC_AQ_MAIN_MYSTICISM" PrereqNode="NODE_CIVIC_AQ_MAIN_CHIEFDOM"/>
    <Row Node="NODE_CIVIC_AQ_MAIN_DISCIPLINE" PrereqNode="NODE_CIVIC_AQ_MAIN_CHIEFDOM"/>

    <!-- Branching with multiple prereqs -->
    <Row Node="NODE_CIVIC_AQ_MAIN_CODE_OF_LAWS" PrereqNode="NODE_CIVIC_AQ_MAIN_MYSTICISM"/>
    <Row Node="NODE_CIVIC_AQ_MAIN_CODE_OF_LAWS" PrereqNode="NODE_CIVIC_AQ_MAIN_DISCIPLINE"/>

    <!-- End-game civic with many prereqs -->
    <Row Node="NODE_CIVIC_AQ_MAIN_FUTURE_CIVIC" PrereqNode="NODE_CIVIC_AQ_MAIN_ENTERTAINMENT"/>
    <Row Node="NODE_CIVIC_AQ_MAIN_FUTURE_CIVIC" PrereqNode="NODE_CIVIC_AQ_MAIN_PHILOSOPHY"/>
    <Row Node="NODE_CIVIC_AQ_MAIN_FUTURE_CIVIC" PrereqNode="NODE_CIVIC_AQ_MAIN_COMMERCE"/>
    <Row Node="NODE_CIVIC_AQ_MAIN_FUTURE_CIVIC" PrereqNode="NODE_CIVIC_AQ_MAIN_ORG_MILITARY"/>
</ProgressionTreePrereqs>
```

## Civic Quotes

```xml
<TypeQuotes>
    <Row Type="NODE_CIVIC_AQ_MAIN_CHIEFDOM"
         Quote="LOC_CIVIC_CHIEFDOM_QUOTE"
         QuoteAuthor="LOC_CIVIC_CHIEFDOM_QUOTE_AUTHOR"
         QuoteAudio="VO_Quote_Chiefdom"/>
</TypeQuotes>
```

## Advisor Recommendations

```xml
<ProgressionTree_Advisories>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_MYSTICISM"
         AdvisoryClassType="ADVISORY_CLASS_CULTURE"/>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DISCIPLINE"
         AdvisoryClassType="ADVISORY_CLASS_MILITARY"/>

    <!-- Multiple advisories for one civic -->
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_CODE_OF_LAWS"
         AdvisoryClassType="ADVISORY_CLASS_CULTURE"/>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_CODE_OF_LAWS"
         AdvisoryClassType="ADVISORY_CLASS_DIPLOMACY"/>
    <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_CODE_OF_LAWS"
         AdvisoryClassType="ADVISORY_CLASS_TRADE"/>
</ProgressionTree_Advisories>
```

### Advisory Classes for Civics

| AdvisoryClassType | Focus |
|-------------------|-------|
| `ADVISORY_CLASS_CULTURE` | Culture and traditions |
| `ADVISORY_CLASS_MILITARY` | Military strength |
| `ADVISORY_CLASS_ECONOMIC` | Economy and trade |
| `ADVISORY_CLASS_DEFENSE` | City defense |
| `ADVISORY_CLASS_DIPLOMACY` | Foreign relations |
| `ADVISORY_CLASS_TRADE` | Trade routes |
| `ADVISORY_CLASS_HAPPINESS` | Citizen happiness |
| `ADVISORY_CLASS_SCIENCE` | Research and knowledge |

## Complete Example: Adding a New Civic

### Database XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register types -->
    <Types>
        <Row Type="NODE_CIVIC_AQ_MAIN_DIPLOMACY" Kind="KIND_TREE_NODE"/>
        <Row Type="TRADITION_EMBASSIES" Kind="KIND_TRADITION"/>
    </Types>

    <!-- 2. Add quote -->
    <TypeQuotes>
        <Row Type="NODE_CIVIC_AQ_MAIN_DIPLOMACY"
             Quote="LOC_CIVIC_DIPLOMACY_QUOTE"
             QuoteAuthor="LOC_CIVIC_DIPLOMACY_QUOTE_AUTHOR"/>
    </TypeQuotes>

    <!-- 3. Define the civic -->
    <ProgressionTreeNodes>
        <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DIPLOMACY"
             ProgressionTree="TREE_CIVICS_AQ_MAIN"
             Cost="300"
             Name="LOC_CIVIC_DIPLOMACY_NAME"
             IconString="cult_diplomacy"/>
    </ProgressionTreeNodes>

    <!-- 4. Define prerequisites -->
    <ProgressionTreePrereqs>
        <Row Node="NODE_CIVIC_AQ_MAIN_DIPLOMACY"
             PrereqNode="NODE_CIVIC_AQ_MAIN_CODE_OF_LAWS"/>
    </ProgressionTreePrereqs>

    <!-- 5. Define tradition -->
    <Traditions>
        <Row TraditionType="TRADITION_EMBASSIES"
             Name="LOC_TRADITION_EMBASSIES_NAME"
             Description="LOC_TRADITION_EMBASSIES_DESCRIPTION"/>
    </Traditions>

    <!-- 6. Link tradition to modifiers -->
    <TraditionModifiers>
        <Row TraditionType="TRADITION_EMBASSIES" ModifierId="EMBASSIES_MOD_DIPLOMACY"/>
    </TraditionModifiers>

    <!-- 7. Define civic unlocks -->
    <ProgressionTreeNodeUnlocks>
        <!-- Primary tradition -->
        <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DIPLOMACY"
             TargetKind="KIND_TRADITION"
             TargetType="TRADITION_EMBASSIES"
             UnlockDepth="1"/>

        <!-- Wonder -->
        <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DIPLOMACY"
             TargetKind="KIND_CONSTRUCTIBLE"
             TargetType="WONDER_FORUM"
             UnlockDepth="1"/>

        <!-- Mastery bonus tradition -->
        <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DIPLOMACY"
             TargetKind="KIND_TRADITION"
             TargetType="TRADITION_ALLIANCES"
             UnlockDepth="2"/>
    </ProgressionTreeNodeUnlocks>

    <!-- 8. Advisor recommendations -->
    <ProgressionTree_Advisories>
        <Row ProgressionTreeNodeType="NODE_CIVIC_AQ_MAIN_DIPLOMACY"
             AdvisoryClassType="ADVISORY_CLASS_DIPLOMACY"/>
    </ProgressionTree_Advisories>
</Database>
```

### GameEffects XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <Modifier id="EMBASSIES_MOD_DIPLOMACY"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_ADJUST_INFLUENCE_PER_TURN">
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```

### Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_CIVIC_DIPLOMACY_NAME" Language="en_US">
            <Text>Diplomacy</Text>
        </Row>
        <Row Tag="LOC_CIVIC_DIPLOMACY_QUOTE" Language="en_US">
            <Text>In the realm of international affairs, words speak louder than swords.</Text>
        </Row>
        <Row Tag="LOC_CIVIC_DIPLOMACY_QUOTE_AUTHOR" Language="en_US">
            <Text>Ancient Proverb</Text>
        </Row>
        <Row Tag="LOC_TRADITION_EMBASSIES_NAME" Language="en_US">
            <Text>Embassies</Text>
        </Row>
        <Row Tag="LOC_TRADITION_EMBASSIES_DESCRIPTION" Language="en_US">
            <Text>+2 [ICON_INFLUENCE] Influence per turn.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Antiquity Civic Tree Reference

| Civic | Cost | Prerequisites | Primary Unlocks |
|-------|------|---------------|-----------------|
| Chiefdom | 90 | - | Charismatic Leader, Tool Making |
| Mysticism | 125 | Chiefdom | Priesthood, Altar, Pantheon |
| Discipline | 125 | Chiefdom | Survey, Army Commander |
| Public Life | 245 | Mysticism | City Guard |
| Code of Laws | 245 | Mysticism, Discipline | Oratory, Merchant |
| Tactics | 245 | Discipline | Drills |
| Entertainment | 450 | Public Life | Rites and Rituals, +1 Settlement Cap |
| Citizenship | 450 | Code of Laws | Castes, Drama and Poetry |
| Organized Military | 450 | Tactics | Conscription, +1 Settlement Cap |
| Literacy | 600 | Citizenship | Literature, Codex |
| Skilled Trades | 600 | Citizenship | Coinage |
| Philosophy | 750 | Literacy | Scholars, +1 Tradition Slot |
| Commerce | 750 | Skilled Trades | Commodities, Medicine |
| Future Civic | 1000 | All late civics | Repeatable bonuses |

## Best Practices

1. **Balance Costs** - Follow tier system for consistent progression
2. **Meaningful Traditions** - Each tradition should provide a distinct playstyle choice
3. **Logical Prerequisites** - Build sensible paths through the tree
4. **Unlock Variety** - Mix traditions, buildings, and modifiers for interest
5. **Mastery Bonuses** - Use `UnlockDepth="2"` for secondary rewards
6. **Crisis Policies** - Use `IsCrisis="true"` for negative effects
7. **Test UI** - Verify civic tree displays correctly in-game

## Related Documentation

- [Technology Trees](Technology-Trees.md)
- [Types and Kinds](../database/Types-and-Kinds.md)
- [Modifier System](../modifiers/Overview.md)
- [Adding Policies Tutorial](../tutorials/Adding-Policies.md)
