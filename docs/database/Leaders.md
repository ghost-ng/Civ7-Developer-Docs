# Leaders Database Schema

This document describes the database tables used to define leaders in Civilization VII.

## Overview

Unlike Civ VI, leaders in Civ VII are separate from civilizations. Leaders can be paired with multiple civilizations, and have their own abilities.

## Types Table

Register leaders in the Types table:

```xml
<Types>
    <Row Type="LEADER_HATSHEPSUT" Kind="KIND_LEADER"/>
    <Row Type="TRAIT_LEADER_HATSHEPSUT_ABILITY" Kind="KIND_TRAIT"/>
</Types>
```

## Leaders Table

The main leader definition table:

```xml
<Leaders>
    <Row
        LeaderType="LEADER_HATSHEPSUT"
        Name="LOC_LEADER_HATSHEPSUT_NAME"
        IsMajorLeader="true"
        InheritFrom="LEADER_DEFAULT"
    />
</Leaders>
```

### Leaders Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `LeaderType` | TEXT | Primary key (e.g., `LEADER_HATSHEPSUT`) |
| `Name` | TEXT | Display name localization key |
| `IsMajorLeader` | BOOLEAN | Is a playable major leader |
| `IsIndependentLeader` | BOOLEAN | Is an independent faction leader |
| `InheritFrom` | TEXT | Parent leader to inherit from |
| `OperationList` | TEXT | AI operation set |
| `DiscountRate` | INTEGER | AI discount rate |
| `AITargetCityPercentage` | INTEGER | AI city targeting preference |

### Leader Types

| Type | IsMajorLeader | IsIndependentLeader | Description |
|------|---------------|---------------------|-------------|
| Major Leader | true | false | Playable leaders |
| Minor Civ Leader | false | false | City-state leaders |
| Independent Leader | false | true | Independent faction leaders |
| Default | - | - | Base template |

## LeaderTraits Table

Links leaders to their traits:

```xml
<LeaderTraits>
    <Row LeaderType="LEADER_HATSHEPSUT" TraitType="TRAIT_LEADER_HATSHEPSUT_ABILITY"/>
    <Row LeaderType="LEADER_HATSHEPSUT" TraitType="TRAIT_LEADER_ATTRIBUTE_CULTURAL"/>
    <Row LeaderType="LEADER_HATSHEPSUT" TraitType="TRAIT_AQ_CULTURE_VICTORY"/>
    <Row LeaderType="LEADER_HATSHEPSUT" TraitType="TRAIT_EX_CULTURE_VICTORY"/>
    <Row LeaderType="LEADER_HATSHEPSUT" TraitType="TRAIT_MO_CULTURE_VICTORY"/>
</LeaderTraits>
```

### Victory Traits

Leaders have victory preference traits for each age:

| Trait | Description |
|-------|-------------|
| `TRAIT_AQ_CULTURE_VICTORY` | Antiquity culture victory preference |
| `TRAIT_AQ_ECONOMIC_VICTORY` | Antiquity economic victory preference |
| `TRAIT_AQ_MILITARY_VICTORY` | Antiquity military victory preference |
| `TRAIT_AQ_SCIENCE_VICTORY` | Antiquity science victory preference |
| `TRAIT_EX_*_VICTORY` | Exploration age preferences |
| `TRAIT_MO_*_VICTORY` | Modern age preferences |

### Leader Attribute Traits

| Trait | Description |
|-------|-------------|
| `TRAIT_LEADER_ATTRIBUTE_CULTURAL` | Cultural bonuses |
| `TRAIT_LEADER_ATTRIBUTE_ECONOMIC` | Economic bonuses |
| `TRAIT_LEADER_ATTRIBUTE_EXPANSIONIST` | Expansion bonuses |
| `TRAIT_LEADER_ATTRIBUTE_MILITARISTIC` | Military bonuses |
| `TRAIT_LEADER_ATTRIBUTE_POLITICAL` | Diplomacy bonuses |
| `TRAIT_LEADER_ATTRIBUTE_SCIENTIFIC` | Science bonuses |

## CityEvents Table

Defines events that can occur in cities:

```xml
<CityEvents>
    <Row EventType="CITY_EVENT_DEFAULT"/>
    <Row EventType="CITY_EVENT_UNIT_OR_BUILDING"/>
    <Row EventType="CITY_EVENT_DISTRICT"/>
    <Row EventType="CITY_EVENT_IMPROVEMENT"/>
    <Row EventType="CITY_EVENT_WONDER"/>
    <Row EventType="CITY_UNDER_THREAT"/>
</CityEvents>
```

## LeaderCivPriorities Table

Defines which civilizations leaders prefer:

```xml
<LeaderCivPriorities>
    <Row Leader="LEADER_HATSHEPSUT" Civilization="CIVILIZATION_EGYPT" Priority="4"/>
    <Row Leader="LEADER_HATSHEPSUT" Civilization="CIVILIZATION_AKSUM" Priority="1"/>
    <Row Leader="LEADER_AUGUSTUS" Civilization="CIVILIZATION_ROME" Priority="4"/>
    <Row Leader="LEADER_AUGUSTUS" Civilization="CIVILIZATION_EGYPT" Priority="1"/>
</LeaderCivPriorities>
```

## Base Game Leaders

### Major Leaders

| Leader | Attributes | Era Focus |
|--------|------------|-----------|
| Amina | Economic, Militaristic | All ages |
| Ashoka | Political, Expansionist | Culture |
| Augustus | Cultural, Expansionist | Military |
| Benjamin Franklin | Multiple | Economic |
| Catherine | Multiple | Economic |
| Charlemagne | Multiple | Military |
| Confucius | Political, Scientific | Culture |
| Friedrich | Multiple | Military |
| Harriet Tubman | Multiple | Culture |
| Hatshepsut | Cultural, Economic | Culture |
| Himiko | Multiple | Culture |
| Ibn Battuta | Multiple | Economic |
| Isabella | Multiple | Economic |
| Jose Rizal | Multiple | Science |
| Lafayette | Multiple | Military |
| Machiavelli | Political, Scientific | Multiple |
| Pachacuti | Expansionist, Militaristic | Military |
| Trung Trac | Multiple | Military |
| Xerxes | Militaristic | Military |

### Minor Civ Leaders

| Leader | Type |
|--------|------|
| `LEADER_MINOR_CIV_DEFAULT` | Generic |
| `LEADER_MINOR_CIV_SCIENTIFIC` | Science focus |
| `LEADER_MINOR_CIV_RELIGIOUS` | Religion focus |
| `LEADER_MINOR_CIV_TRADE` | Trade focus |
| `LEADER_MINOR_CIV_CULTURAL` | Culture focus |
| `LEADER_MINOR_CIV_MILITARISTIC` | Military focus |
| `LEADER_MINOR_CIV_INDUSTRIAL` | Production focus |

## Example: Creating a New Leader

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Register types -->
    <Types>
        <Row Type="LEADER_MY_LEADER" Kind="KIND_LEADER"/>
        <Row Type="TRAIT_LEADER_MY_LEADER_ABILITY" Kind="KIND_TRAIT"/>
    </Types>

    <!-- Define the leader -->
    <Leaders>
        <Row
            LeaderType="LEADER_MY_LEADER"
            Name="LOC_LEADER_MY_LEADER_NAME"
            IsMajorLeader="true"
            InheritFrom="LEADER_DEFAULT"
        />
    </Leaders>

    <!-- Define leader trait -->
    <Traits>
        <Row
            TraitType="TRAIT_LEADER_MY_LEADER_ABILITY"
            Name="LOC_TRAIT_LEADER_MY_LEADER_ABILITY_NAME"
            Description="LOC_TRAIT_LEADER_MY_LEADER_ABILITY_DESCRIPTION"
        />
    </Traits>

    <!-- Link traits to leader -->
    <LeaderTraits>
        <Row LeaderType="LEADER_MY_LEADER" TraitType="TRAIT_LEADER_MY_LEADER_ABILITY"/>
        <Row LeaderType="LEADER_MY_LEADER" TraitType="TRAIT_LEADER_MAJOR_CIV"/>
        <Row LeaderType="LEADER_MY_LEADER" TraitType="TRAIT_LEADER_ATTRIBUTE_CULTURAL"/>
        <Row LeaderType="LEADER_MY_LEADER" TraitType="TRAIT_LEADER_ATTRIBUTE_SCIENTIFIC"/>
        <Row LeaderType="LEADER_MY_LEADER" TraitType="TRAIT_AQ_CULTURE_VICTORY"/>
        <Row LeaderType="LEADER_MY_LEADER" TraitType="TRAIT_EX_CULTURE_VICTORY"/>
        <Row LeaderType="LEADER_MY_LEADER" TraitType="TRAIT_MO_CULTURE_VICTORY"/>
    </LeaderTraits>

    <!-- Link to modifiers (defined in GameEffects) -->
    <TraitModifiers>
        <Row TraitType="TRAIT_LEADER_MY_LEADER_ABILITY" ModifierId="MY_LEADER_ABILITY_MODIFIER"/>
    </TraitModifiers>

    <!-- Define civ priorities -->
    <LeaderCivPriorities>
        <Row Leader="LEADER_MY_LEADER" Civilization="CIVILIZATION_EGYPT" Priority="4"/>
        <Row Leader="LEADER_MY_LEADER" Civilization="CIVILIZATION_GREECE" Priority="2"/>
    </LeaderCivPriorities>
</Database>
```

## Related Tables

- `LeaderQuotes` - Leader quotes for Civilopedia
- `LeaderInfo` - Additional leader metadata
- `AgendaTraits` - AI agenda behavior
- `HistoricMoments` - Leader-specific moments
