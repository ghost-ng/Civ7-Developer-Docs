# AI Behavior and Civilization Preferences

This document covers how to customize AI behavior for civilizations in Civilization VII, including biases, priorities, and settlement preferences.

## Overview

Civ VII's AI system uses a modular approach with:
- **AI Components** - Functional modules handling specific AI tasks
- **AI Lists** - Bias collections linked to traits
- **AI Favored Items** - Specific preferences and values
- **Strategies** - Conditional behavior modifiers

## AI System Architecture

### AI Components

AI components are functional modules that handle different aspects of AI decision-making:

```xml
<AiComponents>
    <Row Component="Military" DefaultPriority="AI_PRIORITY_HIGH" Consumer="True"/>
    <Row Component="Economic" DefaultPriority="AI_PRIORITY_MEDIUM"/>
    <Row Component="GrandStrategic" DefaultPriority="AI_PRIORITY_STRATEGIC"/>
    <Row Component="LandDevelopment" DefaultPriority="AI_PRIORITY_HIGH" Consumer="True"/>
</AiComponents>
```

### Key Components

| Component | Description |
|-----------|-------------|
| `Military` | Combat decisions, army management |
| `Economic` | Economy, yields, treasury |
| `Diplomatic` | Relations, deals, agreements |
| `GrandStrategic` | Long-term victory planning |
| `LandDevelopment` | Improvements, district placement |
| `Government` | Policy and government choices |
| `Religion` | Religious expansion, beliefs |
| `Technology` | Tech research priorities |
| `Culture` | Civic research priorities |
| `Trade` | Trade route decisions |
| `Tactical` | Unit tactical movement |

### AI Priorities

```xml
<AiPriorities>
    <Row Priority="AI_PRIORITY_DEFAULT" Value="1"/>
    <Row Priority="AI_PRIORITY_LOW" Value="2"/>
    <Row Priority="AI_PRIORITY_MEDIUM" Value="3"/>
    <Row Priority="AI_PRIORITY_HIGH" Value="4"/>
    <Row Priority="AI_PRIORITY_PLANNING" Value="5"/>
    <Row Priority="AI_PRIORITY_STRATEGIC" Value="6"/>
</AiPriorities>
```

## Civilization AI Biases

Civilizations receive AI biases through their traits using the `AiLists` and `AiFavoredItems` tables.

### Setting Up AI Lists

First, define list types:

```xml
<AiListTypes>
    <Row ListType="MyCiv Unit Biases"/>
    <Row ListType="MyCiv Government Bias"/>
    <Row ListType="MyCiv Settle Plot Conditions"/>
    <Row ListType="MyCiv Constructibles Biases"/>
</AiListTypes>
```

Then link lists to civilization traits:

```xml
<AiLists>
    <Row ListType="MyCiv Unit Biases" LeaderType="TRAIT_MY_CIV" System="UnitBiases"/>
    <Row ListType="MyCiv Government Bias" LeaderType="TRAIT_MY_CIV" System="GovernmentBiases"/>
    <Row ListType="MyCiv Settle Plot Conditions" LeaderType="TRAIT_MY_CIV" System="SettlementPlotEvaluations"/>
    <Row ListType="MyCiv Constructibles Biases" LeaderType="TRAIT_MY_CIV" System="ConstructibleBiases"/>
</AiLists>
```

### AI List Systems

| System | Purpose |
|--------|---------|
| `UnitBiases` | Preference for specific units |
| `GovernmentBiases` | Government type preferences |
| `SettlementPlotEvaluations` | City placement preferences |
| `ConstructibleBiases` | Building/improvement preferences |
| `TagBiases` | Tag-based preferences (PRODUCTION, SCIENCE, etc.) |
| `PseudoYieldBiases` | Internal AI value adjustments |
| `AiBudgetBiases` | Budget allocation preferences |
| `ProgressionTreeNodeBiases` | Tech/civic research priorities |

## Unit Biases

Make the AI prefer specific units:

```xml
<AiFavoredItems>
    <!-- Strongly favor unique units -->
    <Row ListType="Egypt Units Biases" Item="UNIT_MEDJAY" Value="50"/>
    <Row ListType="Egypt Units Biases" Item="UNIT_MEDJAY_2" Value="50"/>
    <Row ListType="Egypt Units Biases" Item="UNIT_MEDJAY_3" Value="50"/>

    <!-- Moderate preference for support units -->
    <Row ListType="Egypt Units Biases" Item="UNIT_CHARIOT" Value="25"/>
    <Row ListType="Egypt Units Biases" Item="UNIT_HORSEMAN" Value="25"/>

    <!-- Special units -->
    <Row ListType="Egypt Units Biases" Item="UNIT_TJATY" Value="50"/>
</AiFavoredItems>
```

### Bias Values

| Value Range | Effect |
|-------------|--------|
| 0-25 | Slight preference |
| 26-50 | Moderate preference |
| 51-100 | Strong preference |
| 100+ | Very strong preference |
| Negative | Avoidance |

## Government Biases

Make the AI prefer certain governments:

```xml
<AiFavoredItems>
    <!-- Greece prefers Classical Republic -->
    <Row ListType="Greece Government Bias" Item="GOVERNMENT_CLASSICAL_REPUBLIC" Value="50"/>

    <!-- Rome moderate preference -->
    <Row ListType="Rome Government Bias" Item="GOVERNMENT_CLASSICAL_REPUBLIC" Value="25"/>

    <!-- Military civs prefer Despotism -->
    <Row ListType="Persia Government Bias" Item="GOVERNMENT_DESPOTISM" Value="50"/>
    <Row ListType="Han Government Bias" Item="GOVERNMENT_DESPOTISM" Value="50"/>
</AiFavoredItems>
```

## Building/Constructible Biases

Make the AI prioritize specific buildings:

```xml
<AiFavoredItems>
    <!-- Strongly favor unique buildings -->
    <Row ListType="Egypt Constructibles Biases" Item="BUILDING_MASTABA" Value="200"/>
    <Row ListType="Egypt Constructibles Biases" Item="BUILDING_MORTUARY_TEMPLE" Value="200"/>

    <!-- Favor unique improvements -->
    <Row ListType="Han Constructibles Biases" Item="IMPROVEMENT_HAN_GREAT_WALL" Value="200"/>

    <!-- Favor specific wonders -->
    <Row ListType="Egypt Constructibles Biases" Item="WONDER_PYRAMIDS" Value="50"/>
</AiFavoredItems>
```

## Settlement Plot Preferences

Control where the AI prefers to settle cities:

```xml
<AiFavoredItems>
    <!-- Egypt prefers navigable rivers -->
    <Row ListType="Egypt Settle Plot Conditions" Item="Specific Terrain" Favored="false"
         Value="10" StringVal="TERRAIN_NAVIGABLE_RIVER"
         TooltipString="LOC_SETTLEMENT_RECOMMENDATION_TERRAIN"/>

    <!-- Egypt prefers floodplains -->
    <Row ListType="Egypt Settle Plot Conditions" Item="Specific Feature" Favored="false"
         Value="3" StringVal="FEATURE_DESERT_FLOODPLAIN_MINOR"
         TooltipString="LOC_SETTLEMENT_RECOMMENDATION_FEATURES"/>

    <!-- Aksum prefers coastal -->
    <Row ListType="Aksum Settle Plot Conditions" Item="Coastal" Favored="false"
         Value="15" TooltipString="LOC_SETTLEMENT_RECOMMENDATION_COAST"/>

    <!-- Inca prefers mountains -->
    <Row ListType="Inca Settle Plot Conditions" Item="Specific Terrain" Favored="false"
         Value="15" StringVal="TERRAIN_MOUNTAIN"
         TooltipString="LOC_SETTLEMENT_RECOMMENDATION_TERRAIN"/>
</AiFavoredItems>
```

### Plot Evaluation Conditions

| Condition | Description |
|-----------|-------------|
| `Coastal` | Adjacent to coast |
| `Fresh Water` | Adjacent to fresh water |
| `Specific Terrain` | Specific terrain type (StringVal) |
| `Specific Feature` | Specific feature type (StringVal) |
| `Specific Biome` | Specific biome (StringVal) |
| `Resource Class` | Resource class nearby |
| `Lake` | Near lake |
| `New Resources` | Unique resources in area |

## Tag Biases

Control AI focus on yield/tag types:

```xml
<AiFavoredItems>
    <!-- Han focuses on science -->
    <Row ListType="Han Tag Biases" Item="SCIENCE" Value="50"/>
    <Row ListType="Han Tag Biases" Item="HAPPINESS" Value="25"/>

    <!-- Production focus for all Antiquity civs -->
    <Row ListType="Antiquity Tag Biases" Item="PRODUCTION" Value="100"/>
</AiFavoredItems>
```

### Available Tags

- `SCIENCE` - Science yields
- `CULTURE` - Culture yields
- `PRODUCTION` - Production yields
- `GOLD` - Gold yields
- `FOOD` - Food yields
- `HAPPINESS` - Happiness focus
- `FAITH` - Faith yields

## PseudoYield Biases

Adjust internal AI scoring values:

```xml
<AiFavoredItems>
    <!-- Rome strongly values expansion -->
    <Row ListType="Rome PseudoYield Biases" Item="PSEUDOYIELD_NEW_CITY" Value="50"/>

    <!-- Rome slightly devalues keeping cities busy -->
    <Row ListType="Rome PseudoYield Biases" Item="PSEUDOYIELD_BUILD_QUEUE_IN_CITY" Value="-10"/>

    <!-- Mississippian values resource imports -->
    <Row ListType="Mississippian PseudoYield Biases" Item="PSEUDOYIELD_RESOURCE_IMPORT" Value="50"/>
</AiFavoredItems>
```

### Common PseudoYields

| PseudoYield | Description |
|-------------|-------------|
| `PSEUDOYIELD_NEW_CITY` | Value of founding cities |
| `PSEUDOYIELD_BUILD_QUEUE_IN_CITY` | Value of keeping production busy |
| `PSEUDOYIELD_RESOURCE_IMPORT` | Value of importing resources |

## Budget Biases

Adjust AI resource allocation:

```xml
<AiFavoredItems>
    <!-- Persia allocates more to military -->
    <Row ListType="Persia Budget Biases" Item="AI_BUDGET_STANDING_ARMY" Value="50"/>
</AiFavoredItems>
```

### AI Budgets

| Budget | Purpose |
|--------|---------|
| `AI_BUDGET_CITY_DEVELOPMENT` | Building construction |
| `AI_BUDGET_EXPANSION` | Settler production |
| `AI_BUDGET_GARRISON` | Defensive units |
| `AI_BUDGET_EXPLORATION` | Scouts, exploration |
| `AI_BUDGET_RELIGION` | Religious units |
| `AI_BUDGET_STANDING_ARMY` | Military forces |
| `AI_BUDGET_SCRIPTING` | Script-driven actions |

## Research Biases

Prioritize specific techs/civics:

```xml
<AiFavoredItems>
    <!-- Maurya prioritizes Mysticism civic -->
    <Row ListType="Maurya ProgressionTreeNodes Biases"
         Item="NODE_CIVIC_AQ_MAIN_MYSTICISM" Value="500"/>
</AiFavoredItems>
```

## City Strategies

Define conditional behavior:

```xml
<Strategies>
    <Row StrategyType="CITY_STRATEGY_WONDERS" CityStrategy="true"
         MinNumConditionsNeeded="1" MaxNumConditionsNeeded="1"/>
</Strategies>

<StrategyConditions>
    <Row StrategyType="CITY_STRATEGY_WONDERS"
         ConditionFunction="HasProductionGreaterThan" ThresholdValue="20"/>
</StrategyConditions>

<Strategy_Priorities>
    <Row StrategyType="CITY_STRATEGY_WONDERS" ListType="CityWonderBias"/>
</Strategy_Priorities>

<AiListTypes>
    <Row ListType="CityWonderBias"/>
</AiListTypes>

<AiLists>
    <Row ListType="CityWonderBias" System="ConstructibleClassBiases"/>
</AiLists>

<AiFavoredItems>
    <Row ListType="CityWonderBias" Item="WONDER" Value="200"/>
</AiFavoredItems>
```

## Complete Example: New Civilization AI

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Define list types -->
    <AiListTypes>
        <Row ListType="MyCiv Unit Biases"/>
        <Row ListType="MyCiv Government Bias"/>
        <Row ListType="MyCiv Settle Plot Conditions"/>
        <Row ListType="MyCiv Constructibles Biases"/>
        <Row ListType="MyCiv Tag Biases"/>
    </AiListTypes>

    <!-- Link to civilization trait -->
    <AiLists>
        <Row ListType="MyCiv Unit Biases" LeaderType="TRAIT_MY_CIV" System="UnitBiases"/>
        <Row ListType="MyCiv Government Bias" LeaderType="TRAIT_MY_CIV" System="GovernmentBiases"/>
        <Row ListType="MyCiv Settle Plot Conditions" LeaderType="TRAIT_MY_CIV" System="SettlementPlotEvaluations"/>
        <Row ListType="MyCiv Constructibles Biases" LeaderType="TRAIT_MY_CIV" System="ConstructibleBiases"/>
        <Row ListType="MyCiv Tag Biases" LeaderType="TRAIT_MY_CIV" System="TagBiases"/>
    </AiLists>

    <!-- Define specific preferences -->
    <AiFavoredItems>
        <!-- Favor unique unit -->
        <Row ListType="MyCiv Unit Biases" Item="UNIT_MY_UNIQUE" Value="50"/>
        <Row ListType="MyCiv Unit Biases" Item="UNIT_MY_UNIQUE_2" Value="50"/>

        <!-- Favor cavalry -->
        <Row ListType="MyCiv Unit Biases" Item="UNIT_HORSEMAN" Value="25"/>

        <!-- Prefer Republic -->
        <Row ListType="MyCiv Government Bias" Item="GOVERNMENT_CLASSICAL_REPUBLIC" Value="50"/>

        <!-- Favor unique improvement -->
        <Row ListType="MyCiv Constructibles Biases" Item="IMPROVEMENT_MY_UNIQUE" Value="200"/>

        <!-- Favor specific wonder -->
        <Row ListType="MyCiv Constructibles Biases" Item="WONDER_MY_FAVORITE" Value="50"/>

        <!-- Prefer coastal settling -->
        <Row ListType="MyCiv Settle Plot Conditions" Item="Coastal" Favored="false"
             Value="15" TooltipString="LOC_SETTLEMENT_RECOMMENDATION_COAST"/>

        <!-- Focus on culture -->
        <Row ListType="MyCiv Tag Biases" Item="CULTURE" Value="50"/>
    </AiFavoredItems>
</Database>
```

## AI Definition by Leader Trait

The AI system links components to leader traits:

```xml
<AiDefinitions>
    <Row LeaderTrait="TRAIT_LEADER_MAJOR_CIV" AiComponent="Military"/>
    <Row LeaderTrait="TRAIT_LEADER_MAJOR_CIV" AiComponent="Economic"/>
    <Row LeaderTrait="TRAIT_LEADER_MAJOR_CIV" AiComponent="Diplomatic"/>
    <!-- ... more components ... -->
</AiDefinitions>
```

This ensures major civilizations use the full AI system while city-states use a reduced set.

## Best Practices

1. **Match AI to abilities** - AI biases should complement civ abilities
2. **Use moderate values** - Extreme biases can cause erratic behavior
3. **Test extensively** - Watch AI games to verify behavior
4. **Layer biases** - Use both civ and age-based biases
5. **Consider balance** - Don't make AI exploitable

## Related Documentation

- [Civilizations](../database/Civilizations.md)
- [Leaders](../database/Leaders.md)
- [Starting Bias](Starting-Bias.md)
- [Modifier System](../modifiers/Overview.md)
