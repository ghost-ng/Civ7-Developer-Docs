# Espionage System

This document covers the espionage system in Civilization VII, including spy units, covert operations, countermeasures, and how to create custom espionage content.

## Overview

Civ VII's espionage system is integrated with the diplomacy framework:

- **Espionage Actions** - Covert operations performed through the diplomacy system
- **Spy Units** - Special units that execute espionage missions
- **Operations** - Specific missions spies can undertake
- **Countermeasures** - Defenses against enemy espionage
- **Discovery** - Consequences when spies are caught

## Core Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register espionage elements with their Kind |
| `Units` | Define spy unit types |
| `DiplomacyActions` | Define espionage operations (Action Group: ESPIONAGE) |
| `EspionageOperations` | Define operation parameters and outcomes |
| `EspionageCountermeasures` | Define defensive capabilities |
| `EspionageOutcomes` | Define success/failure consequences |

## Type Kinds

```xml
<Types>
    <!-- Spy Unit -->
    <Row Type="UNIT_MY_SPY" Kind="KIND_UNIT"/>

    <!-- Espionage Operation (as Diplomacy Action) -->
    <Row Type="DIPLOMACY_ACTION_ESPIONAGE_MY_OPERATION" Kind="KIND_DIPLOMACY_ACTION"/>

    <!-- Espionage Effect -->
    <Row Type="ESPIONAGE_OUTCOME_MY_OUTCOME" Kind="KIND_ESPIONAGE_OUTCOME"/>
</Types>
```

## Spy Units

### Built-in Spy Units

| Unit | Age | Description |
|------|-----|-------------|
| `UNIT_SPY` | Exploration | Basic espionage unit |
| `UNIT_MASTER_SPY` | Modern | Advanced espionage unit |

### Defining a Spy Unit

```xml
<Units>
    <Row>
        <UnitType>UNIT_MY_SPY</UnitType>
        <Name>LOC_UNIT_MY_SPY_NAME</Name>
        <Description>LOC_UNIT_MY_SPY_DESC</Description>
        <BaseMoves>3</BaseMoves>
        <BaseCombat>0</BaseCombat>
        <Cost>200</Cost>
        <Maintenance>4</Maintenance>
        <Domain>DOMAIN_LAND</Domain>
        <FormationClass>FORMATION_CLASS_CIVILIAN</FormationClass>
        <EnabledByReligion>false</EnabledByReligion>
    </Row>
</Units>

<!-- Grant spy abilities -->
<Unit_Abilities>
    <Row UnitType="UNIT_MY_SPY" AbilityType="ABILITY_SPY"/>
    <Row UnitType="UNIT_MY_SPY" AbilityType="ABILITY_INVISIBLE"/>
    <Row UnitType="UNIT_MY_SPY" AbilityType="ABILITY_ESPIONAGE_OPERATIONS"/>
</Unit_Abilities>

<!-- Unlock with technology -->
<Unit_TechnologyPrereqs>
    <Row UnitType="UNIT_MY_SPY" TechnologyType="TECH_ESPIONAGE"/>
</Unit_TechnologyPrereqs>
```

### Spy Abilities

| Ability | Description |
|---------|-------------|
| `ABILITY_SPY` | Base spy functionality |
| `ABILITY_INVISIBLE` | Cannot be seen by enemy units |
| `ABILITY_ESPIONAGE_OPERATIONS` | Can perform espionage operations |
| `ABILITY_COUNTERSPY` | Can defend against enemy spies |
| `ABILITY_RECRUIT_PARTISANS` | Can create rebel units |

## Espionage Operations

Espionage operations are defined as diplomacy actions in the `DIPLOMACY_ACTION_GROUP_ESPIONAGE` group.

### Built-in Operations

| Operation | Target | Description |
|-----------|--------|-------------|
| `DIPLOMACY_ACTION_ESPIONAGE_STEAL_TECH` | City | Steal technology research |
| `DIPLOMACY_ACTION_ESPIONAGE_STEAL_CIVIC` | City | Steal civic progress |
| `DIPLOMACY_ACTION_ESPIONAGE_SABOTAGE_PRODUCTION` | City | Damage production queue |
| `DIPLOMACY_ACTION_ESPIONAGE_SIPHON_FUNDS` | City | Steal gold |
| `DIPLOMACY_ACTION_ESPIONAGE_RECRUIT_PARTISANS` | City | Spawn rebel units |
| `DIPLOMACY_ACTION_ESPIONAGE_FOMENT_UNREST` | City | Reduce loyalty |
| `DIPLOMACY_ACTION_ESPIONAGE_BREACH_DAM` | District | Flood tiles |
| `DIPLOMACY_ACTION_ESPIONAGE_NEUTRALIZE_GOVERNOR` | City | Disable governor |

### Defining an Espionage Operation

```xml
<!-- Step 1: Register as Diplomacy Action -->
<Types>
    <Row Type="DIPLOMACY_ACTION_ESPIONAGE_STEAL_SECRETS" Kind="KIND_DIPLOMACY_ACTION"/>
</Types>

<!-- Step 2: Define the Action -->
<DiplomacyActions>
    <Row>
        <DiplomacyActionType>DIPLOMACY_ACTION_ESPIONAGE_STEAL_SECRETS</DiplomacyActionType>
        <Name>LOC_ESPIONAGE_STEAL_SECRETS_NAME</Name>
        <Description>LOC_ESPIONAGE_STEAL_SECRETS_DESC</Description>
        <ActionGroup>DIPLOMACY_ACTION_GROUP_ESPIONAGE</ActionGroup>
        <BaseTokenCost>0</BaseTokenCost>
        <BaseDuration>5</BaseDuration>
        <Symmetrical>false</Symmetrical>
        <SupportFavors>false</SupportFavors>
        <TargetFavors>false</TargetFavors>
        <RequiresAtWar>false</RequiresAtWar>
    </Row>
</DiplomacyActions>
```

### Operation Parameters

```xml
<EspionageOperations>
    <Row>
        <OperationType>DIPLOMACY_ACTION_ESPIONAGE_STEAL_SECRETS</OperationType>
        <BaseDifficulty>5</BaseDifficulty>
        <BaseSuccessChance>60</BaseSuccessChance>
        <BaseTurnsToComplete>5</BaseTurnsToComplete>
        <TargetType>ESPIONAGE_TARGET_CITY</TargetType>
        <RequiresBuilding>BUILDING_PALACE</RequiresBuilding>
    </Row>
</EspionageOperations>
```

### EspionageOperations Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `OperationType` | TEXT | Links to DiplomacyActionType |
| `BaseDifficulty` | INTEGER | Base difficulty level (1-10) |
| `BaseSuccessChance` | INTEGER | Base success percentage (0-100) |
| `BaseTurnsToComplete` | INTEGER | Turns to complete operation |
| `TargetType` | TEXT | What can be targeted |
| `RequiresBuilding` | TEXT | Building required in target city |
| `RequiresDistrict` | TEXT | District required in target city |
| `RequiresAtWar` | BOOLEAN | Only available during war |

### Target Types

| Target Type | Description |
|-------------|-------------|
| `ESPIONAGE_TARGET_CITY` | Target is a city |
| `ESPIONAGE_TARGET_DISTRICT` | Target is a specific district |
| `ESPIONAGE_TARGET_UNIT` | Target is a unit |
| `ESPIONAGE_TARGET_PLAYER` | Target is a player (global effect) |
| `ESPIONAGE_TARGET_TRADE_ROUTE` | Target is a trade route |

## Espionage Outcomes

Define what happens when operations succeed or fail.

### Success Outcomes

```xml
<EspionageOutcomes>
    <Row>
        <OperationType>DIPLOMACY_ACTION_ESPIONAGE_STEAL_SECRETS</OperationType>
        <OutcomeType>OUTCOME_SUCCESS</OutcomeType>
        <ModifierId>MOD_STEAL_SECRETS_SUCCESS</ModifierId>
    </Row>
</EspionageOutcomes>

<!-- Define the modifier -->
<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_STEAL_SECRETS_SUCCESS"
              collection="COLLECTION_OWNER"
              effect="EFFECT_GRANT_DIPLOMATIC_VISIBILITY">
        <Argument name="VisibilityLevel">3</Argument>
        <Argument name="Duration">20</Argument>
    </Modifier>
</GameEffects>
```

### Failure Outcomes

```xml
<EspionageOutcomes>
    <Row>
        <OperationType>DIPLOMACY_ACTION_ESPIONAGE_STEAL_SECRETS</OperationType>
        <OutcomeType>OUTCOME_FAILURE</OutcomeType>
        <ModifierId>MOD_STEAL_SECRETS_FAILURE</ModifierId>
    </Row>
    <Row>
        <OperationType>DIPLOMACY_ACTION_ESPIONAGE_STEAL_SECRETS</OperationType>
        <OutcomeType>OUTCOME_CAPTURED</OutcomeType>
        <ModifierId>MOD_STEAL_SECRETS_CAPTURED</ModifierId>
    </Row>
</EspionageOutcomes>
```

### Outcome Types

| Outcome | Description |
|---------|-------------|
| `OUTCOME_SUCCESS` | Operation completed successfully |
| `OUTCOME_FAILURE` | Operation failed, spy escapes |
| `OUTCOME_CAPTURED` | Operation failed, spy captured |
| `OUTCOME_KILLED` | Operation failed, spy killed |
| `OUTCOME_CRITICAL_SUCCESS` | Exceptional success with bonus |

## Countermeasures

### Counterspy Operations

Players can assign spies to defend their cities:

```xml
<DiplomacyActions>
    <Row>
        <DiplomacyActionType>DIPLOMACY_ACTION_COUNTERSPY</DiplomacyActionType>
        <Name>LOC_COUNTERSPY_NAME</Name>
        <Description>LOC_COUNTERSPY_DESC</Description>
        <ActionGroup>DIPLOMACY_ACTION_GROUP_ESPIONAGE</ActionGroup>
        <BaseDuration>0</BaseDuration>
        <Symmetrical>false</Symmetrical>
    </Row>
</DiplomacyActions>
```

### Countermeasure Effects

```xml
<EspionageCountermeasures>
    <Row>
        <CountermeasureType>COUNTERMEASURE_SPY_IN_CITY</CountermeasureType>
        <DifficultyModifier>3</DifficultyModifier>
        <SuccessModifier>-20</SuccessModifier>
    </Row>
    <Row>
        <CountermeasureType>COUNTERMEASURE_ENCRYPTED_COMMUNICATIONS</CountermeasureType>
        <DifficultyModifier>2</DifficultyModifier>
        <SuccessModifier>-15</SuccessModifier>
        <RequiresTechnology>TECH_COMPUTERS</RequiresTechnology>
    </Row>
</EspionageCountermeasures>
```

### Building Countermeasures

Buildings can provide espionage defense:

```xml
<Buildings>
    <Row>
        <BuildingType>BUILDING_INTELLIGENCE_AGENCY</BuildingType>
        <Name>LOC_BUILDING_INTELLIGENCE_AGENCY_NAME</Name>
        <Description>LOC_BUILDING_INTELLIGENCE_AGENCY_DESC</Description>
        <Cost>400</Cost>
        <EspionageDefense>3</EspionageDefense>
    </Row>
</Buildings>

<BuildingModifiers>
    <Row BuildingType="BUILDING_INTELLIGENCE_AGENCY"
         ModifierId="MOD_INTELLIGENCE_AGENCY_DEFENSE"/>
</BuildingModifiers>

<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_INTELLIGENCE_AGENCY_DEFENSE"
              collection="COLLECTION_CITY_PLOT"
              effect="EFFECT_ADJUST_ESPIONAGE_DIFFICULTY">
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```

## Diplomatic Consequences

When spies are caught, it affects diplomacy:

### Grievance on Discovery

```xml
<DiplomacyGrievanceEvents>
    <Row>
        <GrievanceEventType>GRIEVANCE_FROM_ESPIONAGE</GrievanceEventType>
        <Name>LOC_GRIEVANCE_ESPIONAGE_NAME</Name>
        <Description>LOC_GRIEVANCE_ESPIONAGE_DESC</Description>
        <GrievanceAmount>20</GrievanceAmount>
        <DecayRate>2</DecayRate>
    </Row>
    <Row>
        <GrievanceEventType>GRIEVANCE_FROM_ESPIONAGE_SABOTAGE</GrievanceEventType>
        <Name>LOC_GRIEVANCE_SABOTAGE_NAME</Name>
        <Description>LOC_GRIEVANCE_SABOTAGE_DESC</Description>
        <GrievanceAmount>35</GrievanceAmount>
        <DecayRate>2</DecayRate>
    </Row>
</DiplomacyGrievanceEvents>
```

### Casus Belli

Repeated espionage can provide justification for war:

```xml
<CasusBellis>
    <Row>
        <CasusBelliType>CASUS_BELLI_ESPIONAGE</CasusBelliType>
        <Name>LOC_CASUS_BELLI_ESPIONAGE_NAME</Name>
        <Description>LOC_CASUS_BELLI_ESPIONAGE_DESC</Description>
        <RequiredGrievance>50</RequiredGrievance>
        <WarmongerPenaltyReduction>50</WarmongerPenaltyReduction>
    </Row>
</CasusBellis>
```

## Espionage Effects

### Common Espionage Effects

| Effect | Description |
|--------|-------------|
| `EFFECT_STEAL_TECH_BOOST` | Grant tech boost from target |
| `EFFECT_STEAL_CIVIC_BOOST` | Grant civic boost from target |
| `EFFECT_STEAL_GOLD` | Transfer gold from target |
| `EFFECT_DAMAGE_PRODUCTION` | Reduce target city production |
| `EFFECT_REDUCE_LOYALTY` | Reduce target city loyalty |
| `EFFECT_GRANT_DIPLOMATIC_VISIBILITY` | Reveal target's information |
| `EFFECT_SPAWN_REBEL_UNITS` | Create rebel units in target city |
| `EFFECT_DISABLE_DISTRICT` | Temporarily disable a district |

### Example: Steal Technology Effect

```xml
<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_STEAL_TECH_SUCCESS"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_GRANT_TECH_BOOST">
        <Argument name="TechBoostType">BOOST_FROM_ESPIONAGE</Argument>
        <Argument name="Amount">50</Argument>
    </Modifier>
</GameEffects>
```

### Example: Sabotage Production Effect

```xml
<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_SABOTAGE_PRODUCTION"
              collection="COLLECTION_TARGET_CITY"
              effect="EFFECT_CITY_ADJUST_PRODUCTION_QUEUE">
        <Argument name="PercentDamage">50</Argument>
    </Modifier>
</GameEffects>
```

## Spy Promotions

Spies can earn promotions to become more effective:

```xml
<UnitPromotions>
    <Row>
        <UnitPromotionType>PROMOTION_SPY_DISGUISE</UnitPromotionType>
        <Name>LOC_PROMOTION_SPY_DISGUISE_NAME</Name>
        <Description>LOC_PROMOTION_SPY_DISGUISE_DESC</Description>
        <PromotionClass>PROMOTION_CLASS_SPY</PromotionClass>
        <Level>1</Level>
    </Row>
</UnitPromotions>

<UnitPromotionModifiers>
    <Row UnitPromotionType="PROMOTION_SPY_DISGUISE"
         ModifierId="MOD_SPY_DISGUISE_BONUS"/>
</UnitPromotionModifiers>

<GameEffects xmlns="GameEffects">
    <Modifier id="MOD_SPY_DISGUISE_BONUS"
              collection="COLLECTION_OWNER"
              effect="EFFECT_ADJUST_SPY_SUCCESS_CHANCE">
        <Argument name="Amount">15</Argument>
    </Modifier>
</GameEffects>
```

### Built-in Spy Promotions

| Promotion | Effect |
|-----------|--------|
| `PROMOTION_SPY_DISGUISE` | +15% success chance |
| `PROMOTION_SPY_CATBURGLAR` | Faster siphon funds operations |
| `PROMOTION_SPY_TECHNOLOGIST` | Bonus to tech stealing |
| `PROMOTION_SPY_QUARTERMASTER` | Reduced operation time |
| `PROMOTION_SPY_SEDUCTRESS` | Bonus to recruiting assets |
| `PROMOTION_SPY_ACE_DRIVER` | Improved escape chances |

## Complete Example: Custom Espionage Operation

### Step 1: Register Types

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <Row Type="DIPLOMACY_ACTION_ESPIONAGE_DISRUPT_ROCKETRY"
             Kind="KIND_DIPLOMACY_ACTION"/>
    </Types>
</Database>
```

### Step 2: Define the Operation

```xml
<DiplomacyActions>
    <Row>
        <DiplomacyActionType>DIPLOMACY_ACTION_ESPIONAGE_DISRUPT_ROCKETRY</DiplomacyActionType>
        <Name>LOC_ESPIONAGE_DISRUPT_ROCKETRY_NAME</Name>
        <Description>LOC_ESPIONAGE_DISRUPT_ROCKETRY_DESC</Description>
        <ActionGroup>DIPLOMACY_ACTION_GROUP_ESPIONAGE</ActionGroup>
        <BaseTokenCost>0</BaseTokenCost>
        <BaseDuration>6</BaseDuration>
        <Symmetrical>false</Symmetrical>
        <SupportFavors>false</SupportFavors>
        <TargetFavors>false</TargetFavors>
    </Row>
</DiplomacyActions>

<EspionageOperations>
    <Row>
        <OperationType>DIPLOMACY_ACTION_ESPIONAGE_DISRUPT_ROCKETRY</OperationType>
        <BaseDifficulty>7</BaseDifficulty>
        <BaseSuccessChance>45</BaseSuccessChance>
        <BaseTurnsToComplete>6</BaseTurnsToComplete>
        <TargetType>ESPIONAGE_TARGET_DISTRICT</TargetType>
        <RequiresDistrict>DISTRICT_SPACEPORT</RequiresDistrict>
    </Row>
</EspionageOperations>

<EspionageOutcomes>
    <Row OperationType="DIPLOMACY_ACTION_ESPIONAGE_DISRUPT_ROCKETRY"
         OutcomeType="OUTCOME_SUCCESS"
         ModifierId="MOD_DISRUPT_ROCKETRY_SUCCESS"/>
    <Row OperationType="DIPLOMACY_ACTION_ESPIONAGE_DISRUPT_ROCKETRY"
         OutcomeType="OUTCOME_FAILURE"
         ModifierId="MOD_DISRUPT_ROCKETRY_FAILURE"/>
</EspionageOutcomes>
```

### Step 3: Define Modifiers

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Success: Delay space race project by 10 turns -->
    <Modifier id="MOD_DISRUPT_ROCKETRY_SUCCESS"
              collection="COLLECTION_TARGET_CITY"
              effect="EFFECT_CITY_ADJUST_PROJECT_PROGRESS">
        <Argument name="ProjectType">PROJECT_LAUNCH_MARS_COLONY</Argument>
        <Argument name="Amount">-500</Argument>
    </Modifier>

    <!-- Failure: Generate grievance -->
    <Modifier id="MOD_DISRUPT_ROCKETRY_FAILURE"
              collection="COLLECTION_TARGET_PLAYER"
              effect="EFFECT_GRANT_GRIEVANCE_EVENT">
        <Argument name="GrievanceType">GRIEVANCE_FROM_ESPIONAGE_SABOTAGE</Argument>
    </Modifier>
</GameEffects>
```

### Step 4: Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_ESPIONAGE_DISRUPT_ROCKETRY_NAME" Language="en_US">
            <Text>Disrupt Rocketry</Text>
        </Row>
        <Row Tag="LOC_ESPIONAGE_DISRUPT_ROCKETRY_DESC" Language="en_US">
            <Text>Sabotage the target's space program, delaying their space race projects. Requires a Spaceport in the target city.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Requirements for Operations

Control when operations are available:

```xml
<DiplomacyActionRequirements>
    <Row DiplomacyActionType="DIPLOMACY_ACTION_ESPIONAGE_DISRUPT_ROCKETRY"
         RequirementSetId="REQSET_DISRUPT_ROCKETRY_AVAILABLE"/>
</DiplomacyActionRequirements>

<RequirementSets>
    <Row RequirementSetId="REQSET_DISRUPT_ROCKETRY_AVAILABLE"
         RequirementSetType="REQUIREMENTSET_TEST_ALL"/>
</RequirementSets>

<RequirementSetRequirements>
    <Row RequirementSetId="REQSET_DISRUPT_ROCKETRY_AVAILABLE"
         RequirementId="REQ_HAS_SATELLITE_TECH"/>
    <Row RequirementSetId="REQSET_DISRUPT_ROCKETRY_AVAILABLE"
         RequirementId="REQ_TARGET_HAS_SPACEPORT"/>
</RequirementSetRequirements>

<Requirements>
    <Row RequirementId="REQ_HAS_SATELLITE_TECH"
         RequirementType="REQUIREMENT_PLAYER_HAS_TECHNOLOGY">
        <Argument name="TechnologyType">TECH_SATELLITES</Argument>
    </Row>
    <Row RequirementId="REQ_TARGET_HAS_SPACEPORT"
         RequirementType="REQUIREMENT_TARGET_CITY_HAS_DISTRICT">
        <Argument name="DistrictType">DISTRICT_SPACEPORT</Argument>
    </Row>
</Requirements>
```

## AI Espionage Behavior

Configure how AI uses espionage:

```xml
<AiOperationPriorities>
    <Row>
        <OperationType>DIPLOMACY_ACTION_ESPIONAGE_STEAL_TECH</OperationType>
        <PriorityType>PRIORITY_SCIENCE_FOCUSED</PriorityType>
        <PriorityValue>80</PriorityValue>
    </Row>
    <Row>
        <OperationType>DIPLOMACY_ACTION_ESPIONAGE_SIPHON_FUNDS</OperationType>
        <PriorityType>PRIORITY_GOLD_FOCUSED</PriorityType>
        <PriorityValue>70</PriorityValue>
    </Row>
</AiOperationPriorities>
```

## Best Practices

1. **Balance Difficulty** - Set appropriate difficulty and success chances
2. **Meaningful Consequences** - Ensure outcomes have real gameplay impact
3. **Diplomatic Integration** - Tie operations to grievance/favor system
4. **Counter Play** - Provide ways for players to defend against operations
5. **Clear Feedback** - Use localization to explain results clearly
6. **Tech Gates** - Lock powerful operations behind appropriate technologies
7. **Target Requirements** - Require relevant buildings/districts for thematic operations
8. **Spy Progression** - Create meaningful promotion paths for spies

## Related Documentation

- [Diplomacy System](../diplomacy/Overview.md)
- [Modifier System](../modifiers/Overview.md)
- [Effect Types](../modifiers/Effect-Types.md)
- [Units](../database/Units.md)
- [Types and Kinds](../database/Types-and-Kinds.md)
