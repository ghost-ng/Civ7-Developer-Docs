# Diplomacy System

This document covers the diplomacy system in Civilization VII, including diplomatic actions, relationships, favor/grievance mechanics, and how to create custom diplomatic content.

## Overview

Civ VII's diplomacy system is structured around several core concepts:

- **Diplomacy Actions** - Actions players can take toward other players (declare war, form alliances, etc.)
- **Action Groups** - Categories of diplomatic actions (Projects, Endeavors, Sanctions, Treaties, Espionage)
- **Diplomacy Tokens** - Resources used in diplomatic negotiations (Favor, Grievance, Global)
- **Relationships** - The standing between players (Hostile to Helpful)
- **Statements** - Dialogue and interaction frames for diplomatic meetings

## Core Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register diplomacy elements with their Kind |
| `DiplomacyActions` | Define diplomatic actions and their properties |
| `DiplomacyActionGroups` | Group actions into categories |
| `DiplomacyTokens` | Define diplomatic currency types |
| `DiplomacyRelationships` | Define relationship levels |
| `DiplomacyStatements` | Define dialogue for diplomatic interactions |
| `DiplomacyFavorEvents` | Events that grant diplomatic favor |
| `DiplomacyGrievanceEvents` | Events that cause grievances |
| `DiplomacyHistoricalEvents` | Track diplomatic history |
| `DiplomacyActionStages` | Multi-step action processing |

## Type Kinds

```xml
<Types>
    <!-- Diplomacy Actions -->
    <Row Type="DIPLOMACY_ACTION_MY_ACTION" Kind="KIND_DIPLOMACY_ACTION"/>

    <!-- Action Groups -->
    <Row Type="DIPLOMACY_ACTION_GROUP_MY_GROUP" Kind="KIND_DIPLOMACY_ACTION_GROUP"/>

    <!-- Tokens -->
    <Row Type="DIPLOMACY_TOKEN_MY_TOKEN" Kind="KIND_DIPLOMACY_TOKEN"/>

    <!-- Relationships -->
    <Row Type="DIPLOMACY_RELATIONSHIP_MY_LEVEL" Kind="KIND_DIPLOMACY_RELATIONSHIP"/>

    <!-- Statements -->
    <Row Type="DIPLOMACY_STATEMENT_MY_STATEMENT" Kind="KIND_DIPLOMACY_STATEMENT"/>

    <!-- Events -->
    <Row Type="DIPLOMACY_FAVOR_EVENT_MY_EVENT" Kind="KIND_DIPLOMACY_FAVOR_EVENT"/>
    <Row Type="DIPLOMACY_GRIEVANCE_EVENT_MY_EVENT" Kind="KIND_DIPLOMACY_GRIEVANCE_EVENT"/>
    <Row Type="DIPLOMACY_HISTORICAL_EVENT_MY_EVENT" Kind="KIND_DIPLOMACY_HISTORICAL_EVENT"/>
</Types>
```

## Diplomacy Actions

### Built-in Actions

| Action | Group | Description |
|--------|-------|-------------|
| `DIPLOMACY_ACTION_DECLARE_WAR` | - | Declare war on another player |
| `DIPLOMACY_ACTION_FORM_ALLIANCE` | TREATY | Create an alliance |
| `DIPLOMACY_ACTION_OPEN_BORDERS` | TREATY | Allow unit passage |
| `DIPLOMACY_ACTION_RESEARCH_COLLABORATION` | ENDEAVOR | Share research |
| `DIPLOMACY_ACTION_TRADE_AGREEMENT` | TREATY | Establish trade |
| `DIPLOMACY_ACTION_DENOUNCE` | SANCTION | Publicly denounce |
| `DIPLOMACY_ACTION_MAKE_PEACE` | - | End a war |

### Action Groups

| Group | Purpose |
|-------|---------|
| `DIPLOMACY_ACTION_GROUP_PROJECT` | Cooperative projects with other civilizations |
| `DIPLOMACY_ACTION_GROUP_ENDEAVOR` | Collaborative efforts (research, culture) |
| `DIPLOMACY_ACTION_GROUP_SANCTION` | Punitive diplomatic actions |
| `DIPLOMACY_ACTION_GROUP_ESPIONAGE` | Covert operations |
| `DIPLOMACY_ACTION_GROUP_TREATY` | Formal agreements |
| `DIPLOMACY_ACTION_GROUP_UNIQUE` | Civilization-specific actions |

### Defining a Diplomacy Action

```xml
<DiplomacyActions>
    <Row>
        <DiplomacyActionType>DIPLOMACY_ACTION_MY_TREATY</DiplomacyActionType>
        <Name>LOC_DIPLOMACY_ACTION_MY_TREATY_NAME</Name>
        <Description>LOC_DIPLOMACY_ACTION_MY_TREATY_DESC</Description>
        <ActionGroup>DIPLOMACY_ACTION_GROUP_TREATY</ActionGroup>
        <BaseTokenCost>50</BaseTokenCost>
        <BaseDuration>30</BaseDuration>
        <Symmetrical>true</Symmetrical>
        <SupportFavors>true</SupportFavors>
        <TargetFavors>true</TargetFavors>
        <RequiresRelationship>DIPLOMACY_RELATIONSHIP_FRIENDLY</RequiresRelationship>
    </Row>
</DiplomacyActions>
```

### DiplomacyActions Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `DiplomacyActionType` | TEXT | Unique action identifier |
| `Name` | TEXT | Localization key for display name |
| `Description` | TEXT | Localization key for description |
| `ActionGroup` | TEXT | Parent action group |
| `BaseTokenCost` | INTEGER | Favor cost to propose |
| `BaseDuration` | INTEGER | Turns the agreement lasts |
| `Symmetrical` | BOOLEAN | Both parties get same effects |
| `SupportFavors` | BOOLEAN | Can use favor to support |
| `TargetFavors` | BOOLEAN | Target can use favor to accept/reject |
| `RequiresRelationship` | TEXT | Minimum relationship level required |
| `RequiresAtWar` | BOOLEAN | Only available during war |
| `RequiresAtPeace` | BOOLEAN | Only available during peace |

## Diplomacy Tokens

Tokens are the currency used in diplomatic negotiations.

### Built-in Tokens

| Token | Description |
|-------|-------------|
| `DIPLOMACY_TOKEN_FAVOR` | Earned through positive interactions, spent on proposals |
| `DIPLOMACY_TOKEN_GRIEVANCE` | Accumulated from negative events, affects relationship |
| `DIPLOMACY_TOKEN_GLOBAL` | World Congress influence |

### Defining a Token

```xml
<DiplomacyTokens>
    <Row>
        <DiplomacyTokenType>DIPLOMACY_TOKEN_MY_TOKEN</DiplomacyTokenType>
        <Name>LOC_DIPLOMACY_TOKEN_MY_TOKEN_NAME</Name>
        <Description>LOC_DIPLOMACY_TOKEN_MY_TOKEN_DESC</Description>
        <MaxValue>200</MaxValue>
        <DecayPerTurn>1</DecayPerTurn>
    </Row>
</DiplomacyTokens>
```

## Player Relationships

### Relationship Levels

| Relationship | Value | Description |
|--------------|-------|-------------|
| `DIPLOMACY_RELATIONSHIP_HOSTILE` | -2 | At war or severe grievances |
| `DIPLOMACY_RELATIONSHIP_UNFRIENDLY` | -1 | Negative standing |
| `DIPLOMACY_RELATIONSHIP_NEUTRAL` | 0 | Default starting state |
| `DIPLOMACY_RELATIONSHIP_FRIENDLY` | 1 | Positive standing |
| `DIPLOMACY_RELATIONSHIP_HELPFUL` | 2 | Allied or very positive |

### Relationship Thresholds

Relationships change based on accumulated favor and grievances:

```xml
<DiplomacyRelationshipThresholds>
    <Row>
        <RelationshipType>DIPLOMACY_RELATIONSHIP_FRIENDLY</RelationshipType>
        <MinFavor>50</MinFavor>
        <MaxGrievance>20</MaxGrievance>
    </Row>
</DiplomacyRelationshipThresholds>
```

## Favor Events

Events that grant diplomatic favor.

### Built-in Favor Events

| Event | Amount | Trigger |
|-------|--------|---------|
| `FAVOR_FROM_PROJECT` | 10-30 | Completing joint projects |
| `FAVOR_FROM_TRADE` | 5 | Active trade routes |
| `FAVOR_FROM_ALLIANCE` | 10 | Alliance milestones |
| `FAVOR_FROM_OPEN_BORDERS` | 3 | Per turn with open borders |
| `FAVOR_FROM_SHARED_ENEMY` | 15 | Declaring war on shared enemy |

### Defining a Favor Event

```xml
<DiplomacyFavorEvents>
    <Row>
        <FavorEventType>FAVOR_FROM_MY_EVENT</FavorEventType>
        <Name>LOC_FAVOR_FROM_MY_EVENT_NAME</Name>
        <Description>LOC_FAVOR_FROM_MY_EVENT_DESC</Description>
        <FavorAmount>20</FavorAmount>
        <DecayRate>2</DecayRate>
    </Row>
</DiplomacyFavorEvents>
```

## Grievance Events

Events that cause diplomatic grievances.

### Built-in Grievance Events

| Event | Amount | Trigger |
|-------|--------|---------|
| `GRIEVANCE_FROM_CAPTURE_SETTLEMENT` | 50 | Capturing a city |
| `GRIEVANCE_FROM_DECLARE_WAR` | 30 | Declaring war |
| `GRIEVANCE_FROM_BROKEN_PROMISE` | 40 | Breaking an agreement |
| `GRIEVANCE_FROM_ESPIONAGE` | 20 | Caught spying |
| `GRIEVANCE_FROM_DENOUNCE` | 15 | Being denounced |

### Defining a Grievance Event

```xml
<DiplomacyGrievanceEvents>
    <Row>
        <GrievanceEventType>GRIEVANCE_FROM_MY_EVENT</GrievanceEventType>
        <Name>LOC_GRIEVANCE_FROM_MY_EVENT_NAME</Name>
        <Description>LOC_GRIEVANCE_FROM_MY_EVENT_DESC</Description>
        <GrievanceAmount>25</GrievanceAmount>
        <DecayRate>3</DecayRate>
        <WarmongerPenalty>true</WarmongerPenalty>
    </Row>
</DiplomacyGrievanceEvents>
```

## Diplomacy Statements

Statements define the dialogue shown during diplomatic interactions.

### Statement Types

| Type | Description |
|------|-------------|
| `DIPLOMACY_STATEMENT_FIRST_MEET` | Initial meeting dialogue |
| `DIPLOMACY_STATEMENT_MAKE_DEAL` | Proposing agreements |
| `DIPLOMACY_STATEMENT_DECLARE_SURPRISE_WAR` | Surprise war declaration |
| `DIPLOMACY_STATEMENT_DECLARE_FORMAL_WAR` | Formal war declaration |
| `DIPLOMACY_STATEMENT_DEFEAT` | Player defeat dialogue |

### Defining a Statement

```xml
<DiplomacyStatements>
    <Row>
        <DiplomacyStatementType>DIPLOMACY_STATEMENT_MY_STATEMENT</DiplomacyStatementType>
        <Name>LOC_DIPLOMACY_STATEMENT_MY_STATEMENT_NAME</Name>
    </Row>
</DiplomacyStatements>
```

### Statement Frames

Frames define the structure of dialogue sequences:

```xml
<DiplomacyStatementFrames>
    <Row>
        <DiplomacyStatementType>DIPLOMACY_STATEMENT_MY_STATEMENT</DiplomacyStatementType>
        <FrameIndex>0</FrameIndex>
        <LeaderText>LOC_MY_STATEMENT_LEADER_TEXT</LeaderText>
        <SelectionType>DIPLOMACY_SELECTION_ACCEPT_REJECT</SelectionType>
    </Row>
</DiplomacyStatementFrames>
```

### Statement Selections

Player response options:

```xml
<DiplomacyStatementSelections>
    <Row>
        <SelectionType>DIPLOMACY_SELECTION_MY_RESPONSE</SelectionType>
        <Option1>LOC_RESPONSE_ACCEPT</Option1>
        <Option2>LOC_RESPONSE_REJECT</Option2>
        <Option3>LOC_RESPONSE_COUNTER</Option3>
    </Row>
</DiplomacyStatementSelections>
```

## AI Agenda Weighting

Configure how AI evaluates diplomatic decisions.

### Agenda Comparison Types

| Comparison Type | Description |
|-----------------|-------------|
| `DIPLOMACY_AGENDA_COMPARE_NUM_CITIES` | Compare city count |
| `DIPLOMACY_AGENDA_COMPARE_MILITARY_STRENGTH` | Compare military power |
| `DIPLOMACY_AGENDA_COMPARE_SCIENCE` | Compare science output |
| `DIPLOMACY_AGENDA_COMPARE_CULTURE` | Compare culture output |
| `DIPLOMACY_AGENDA_COMPARE_GOLD` | Compare gold reserves |
| `DIPLOMACY_AGENDA_COMPARE_RELIGION` | Compare religious influence |
| `DIPLOMACY_AGENDA_COMPARE_IDEOLOGIES` | Compare government types |
| `DIPLOMACY_AGENDA_COMPARE_WONDERS` | Compare wonder count |

### Defining Agenda Weights

```xml
<DiplomacyAgendaWeights>
    <Row>
        <AgendaType>AGENDA_MY_AGENDA</AgendaType>
        <ComparisonType>DIPLOMACY_AGENDA_COMPARE_MILITARY_STRENGTH</ComparisonType>
        <PositiveWeight>20</PositiveWeight>
        <NegativeWeight>-30</NegativeWeight>
    </Row>
</DiplomacyAgendaWeights>
```

## Action Stages

Multi-step diplomatic processes use stages:

```xml
<DiplomacyActionStages>
    <Row>
        <DiplomacyActionType>DIPLOMACY_ACTION_MY_ACTION</DiplomacyActionType>
        <StageIndex>0</StageIndex>
        <StageName>LOC_STAGE_PROPOSAL</StageName>
        <Duration>5</Duration>
        <RequiresVote>false</RequiresVote>
    </Row>
    <Row>
        <DiplomacyActionType>DIPLOMACY_ACTION_MY_ACTION</DiplomacyActionType>
        <StageIndex>1</StageIndex>
        <StageName>LOC_STAGE_VOTING</StageName>
        <Duration>3</Duration>
        <RequiresVote>true</RequiresVote>
    </Row>
    <Row>
        <DiplomacyActionType>DIPLOMACY_ACTION_MY_ACTION</DiplomacyActionType>
        <StageIndex>2</StageIndex>
        <StageName>LOC_STAGE_ACTIVE</StageName>
        <Duration>30</Duration>
        <RequiresVote>false</RequiresVote>
    </Row>
</DiplomacyActionStages>
```

## Diplomacy Modifiers

Link diplomatic actions to gameplay effects using the modifier system.

### Common Diplomacy Effects

| Effect | Description |
|--------|-------------|
| `EFFECT_ADJUST_PLAYER_DIPLOMACY_FAVOR` | Modify favor with a player |
| `EFFECT_ADJUST_PLAYER_DIPLOMACY_GRIEVANCE` | Modify grievance with a player |
| `EFFECT_GRANT_DIPLOMACY_VISIBILITY` | Grant diplomatic visibility |
| `EFFECT_ENABLE_DIPLOMACY_ACTION` | Enable/disable a diplomatic action |
| `EFFECT_ADJUST_DIPLOMACY_ACTION_COST` | Modify action favor cost |

### Example: Action with Modifiers

```xml
<!-- In Database file -->
<DiplomacyActionModifiers>
    <Row DiplomacyActionType="DIPLOMACY_ACTION_MY_TREATY"
         ModifierId="MOD_MY_TREATY_EFFECT"/>
</DiplomacyActionModifiers>

<!-- In GameEffects file -->
<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_MY_TREATY_EFFECT"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_ADJUST_CITY_YIELD">
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="Amount">3</Argument>
    </Modifier>
</GameEffects>
```

## Complete Example: Custom Diplomatic Action

### Step 1: Register Types

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <Row Type="DIPLOMACY_ACTION_CULTURAL_EXCHANGE" Kind="KIND_DIPLOMACY_ACTION"/>
    </Types>
</Database>
```

### Step 2: Define the Action

```xml
<DiplomacyActions>
    <Row>
        <DiplomacyActionType>DIPLOMACY_ACTION_CULTURAL_EXCHANGE</DiplomacyActionType>
        <Name>LOC_DIPLOMACY_ACTION_CULTURAL_EXCHANGE_NAME</Name>
        <Description>LOC_DIPLOMACY_ACTION_CULTURAL_EXCHANGE_DESC</Description>
        <ActionGroup>DIPLOMACY_ACTION_GROUP_ENDEAVOR</ActionGroup>
        <BaseTokenCost>30</BaseTokenCost>
        <BaseDuration>20</BaseDuration>
        <Symmetrical>true</Symmetrical>
        <SupportFavors>true</SupportFavors>
        <TargetFavors>true</TargetFavors>
        <RequiresRelationship>DIPLOMACY_RELATIONSHIP_NEUTRAL</RequiresRelationship>
        <RequiresAtPeace>true</RequiresAtPeace>
    </Row>
</DiplomacyActions>

<DiplomacyActionModifiers>
    <Row DiplomacyActionType="DIPLOMACY_ACTION_CULTURAL_EXCHANGE"
         ModifierId="MOD_CULTURAL_EXCHANGE_CULTURE"/>
    <Row DiplomacyActionType="DIPLOMACY_ACTION_CULTURAL_EXCHANGE"
         ModifierId="MOD_CULTURAL_EXCHANGE_FAVOR"/>
</DiplomacyActionModifiers>
```

### Step 3: Define Modifiers (GameEffects)

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Culture bonus for both parties -->
    <Modifier id="MOD_CULTURAL_EXCHANGE_CULTURE"
              collection="COLLECTION_PLAYER_CAPITAL"
              effect="EFFECT_ADJUST_CITY_YIELD">
        <Argument name="YieldType">YIELD_CULTURE</Argument>
        <Argument name="Amount">5</Argument>
    </Modifier>

    <!-- Generates favor over time -->
    <Modifier id="MOD_CULTURAL_EXCHANGE_FAVOR"
              collection="COLLECTION_OWNER"
              effect="EFFECT_ADJUST_PLAYER_DIPLOMACY_FAVOR_PER_TURN">
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```

### Step 4: Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_DIPLOMACY_ACTION_CULTURAL_EXCHANGE_NAME" Language="en_US">
            <Text>Cultural Exchange Program</Text>
        </Row>
        <Row Tag="LOC_DIPLOMACY_ACTION_CULTURAL_EXCHANGE_DESC" Language="en_US">
            <Text>Establish a cultural exchange with another civilization. Both parties receive +5 [ICON_CULTURE] Culture in their Capital and +2 Diplomatic Favor per turn for 20 turns.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Requirements for Diplomatic Actions

Control when actions are available:

```xml
<DiplomacyActionRequirements>
    <Row DiplomacyActionType="DIPLOMACY_ACTION_MY_ACTION"
         RequirementSetId="REQSET_MY_ACTION_AVAILABLE"/>
</DiplomacyActionRequirements>

<RequirementSets>
    <Row RequirementSetId="REQSET_MY_ACTION_AVAILABLE"
         RequirementSetType="REQUIREMENTSET_TEST_ALL"/>
</RequirementSets>

<RequirementSetRequirements>
    <Row RequirementSetId="REQSET_MY_ACTION_AVAILABLE"
         RequirementId="REQ_HAS_TECHNOLOGY"/>
    <Row RequirementSetId="REQSET_MY_ACTION_AVAILABLE"
         RequirementId="REQ_NOT_AT_WAR"/>
</RequirementSetRequirements>

<Requirements>
    <Row RequirementId="REQ_HAS_TECHNOLOGY"
         RequirementType="REQUIREMENT_PLAYER_HAS_TECHNOLOGY">
        <Argument name="TechnologyType">TECH_WRITING</Argument>
    </Row>
    <Row RequirementId="REQ_NOT_AT_WAR"
         RequirementType="REQUIREMENT_PLAYER_AT_PEACE"/>
</Requirements>
```

## Best Practices

1. **Balance Costs** - Set appropriate favor costs based on the action's power
2. **Duration Matters** - Longer agreements should provide greater benefits
3. **Relationship Gates** - Use relationship requirements to create diplomatic progression
4. **Grievance Decay** - Allow grievances to decay over time for realistic diplomacy
5. **AI Weights** - Configure proper agenda weights so AI uses custom actions appropriately
6. **Symmetry Choice** - Decide if both parties benefit equally or asymmetrically
7. **Clear Descriptions** - Explain exactly what the action does in localization
8. **Use Action Groups** - Assign actions to appropriate groups for UI organization

## Related Documentation

- [Modifier System](../modifiers/Overview.md)
- [Effect Types](../modifiers/Effect-Types.md)
- [AI Behavior](../civilization-creation/AI-Behavior.md)
- [Types and Kinds](../database/Types-and-Kinds.md)
