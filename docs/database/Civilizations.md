# Civilizations Database Schema

This document describes the database tables used to define civilizations in Civilization VII.

## Overview

Civilizations in Civ VII are age-specific. Each age has its own set of civilizations defined in its module:
- `age-antiquity/data/civilizations.xml`
- `age-exploration/data/civilizations.xml`
- `age-modern/data/civilizations.xml`

## Types Table

Register civilizations in the Types table:

```xml
<Types>
    <Row Type="CIVILIZATION_MY_CIV" Kind="KIND_CIVILIZATION"/>
    <Row Type="TRAIT_MY_CIV_ABILITY" Kind="KIND_TRAIT"/>
</Types>
```

## Civilizations Table

The main civilization definition table:

```xml
<Civilizations>
    <Row
        CivilizationType="CIVILIZATION_EGYPT"
        Name="LOC_CIVILIZATION_EGYPT_NAME"
        FullName="LOC_CIVILIZATION_EGYPT_FULLNAME"
        Description="LOC_CIVILIZATION_EGYPT_DESCRIPTION"
        Adjective="LOC_CIVILIZATION_EGYPT_ADJECTIVE"
        StartingCivilizationLevelType="CIVILIZATION_LEVEL_FULL_CIV"
        UniqueCultureProgressionTree="TREE_CIVICS_AQ_EGYPT"
        CapitalName="LOC_CITY_NAME_EGYPT1"
        RandomCityNameDepth="10"
    />
</Civilizations>
```

### Civilizations Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `CivilizationType` | TEXT | Primary key (e.g., `CIVILIZATION_EGYPT`) |
| `Name` | TEXT | Short name localization key |
| `FullName` | TEXT | Full name localization key |
| `Description` | TEXT | Description localization key |
| `Adjective` | TEXT | Adjective form (e.g., "Egyptian") |
| `StartingCivilizationLevelType` | TEXT | Starting civ level |
| `UniqueCultureProgressionTree` | TEXT | Unique civic tree for traditions |
| `CapitalName` | TEXT | Default capital city name |
| `RandomCityNameDepth` | INTEGER | Number of city names available |
| `AITargetCityPercentage` | INTEGER | AI city targeting preference |

### CivilizationLevelType Values

| Level | Description |
|-------|-------------|
| `CIVILIZATION_LEVEL_FULL_CIV` | Full playable civilization |
| `CIVILIZATION_LEVEL_CITY_STATE` | City-state |
| `CIVILIZATION_LEVEL_INDEPENDENT` | Independent peoples |
| `CIVILIZATION_LEVEL_NONE` | Background/system entities |

## Traits System

Civilizations gain abilities through traits:

### Traits Table

```xml
<Traits>
    <Row
        TraitType="TRAIT_EGYPT_ABILITY"
        Name="LOC_TRAIT_EGYPT_ABILITY_NAME"
        Description="LOC_TRAIT_EGYPT_ABILITY_DESCRIPTION"
        InternalOnly="true"
    />
</Traits>
```

| Column | Type | Description |
|--------|------|-------------|
| `TraitType` | TEXT | Primary key |
| `Name` | TEXT | Display name |
| `Description` | TEXT | Description |
| `InternalOnly` | BOOLEAN | Hidden from UI if true |

### CivilizationTraits Table

Links civilizations to their traits:

```xml
<CivilizationTraits>
    <Row CivilizationType="CIVILIZATION_EGYPT" TraitType="TRAIT_ANTIQUITY_CIV"/>
    <Row CivilizationType="CIVILIZATION_EGYPT" TraitType="TRAIT_EGYPT"/>
    <Row CivilizationType="CIVILIZATION_EGYPT" TraitType="TRAIT_EGYPT_ABILITY"/>
    <Row CivilizationType="CIVILIZATION_EGYPT" TraitType="TRAIT_ATTRIBUTE_CULTURAL"/>
    <Row CivilizationType="CIVILIZATION_EGYPT" TraitType="TRAIT_ATTRIBUTE_ECONOMIC"/>
</CivilizationTraits>
```

### Common Trait Types

| Trait | Purpose |
|-------|---------|
| `TRAIT_ANTIQUITY_CIV` | Shared by all Antiquity civs |
| `TRAIT_[CIV]` | Internal civ identifier |
| `TRAIT_[CIV]_ABILITY` | Unique civ ability |
| `TRAIT_ATTRIBUTE_*` | Attribute bonuses |

### Attribute Traits

| Trait | Description |
|-------|-------------|
| `TRAIT_ATTRIBUTE_CULTURAL` | Cultural bonuses |
| `TRAIT_ATTRIBUTE_ECONOMIC` | Economic bonuses |
| `TRAIT_ATTRIBUTE_EXPANSIONIST` | Expansion bonuses |
| `TRAIT_ATTRIBUTE_MILITARISTIC` | Military bonuses |
| `TRAIT_ATTRIBUTE_POLITICAL` | Diplomacy bonuses |
| `TRAIT_ATTRIBUTE_SCIENTIFIC` | Science bonuses |

## TraitModifiers Table

Links traits to modifiers for abilities:

```xml
<TraitModifiers>
    <Row TraitType="TRAIT_EGYPT_ABILITY" ModifierId="TRAIT_MOD_GIFTS_OF_OSIRIS_NAVIGABLE_RIVER"/>
    <Row TraitType="TRAIT_EGYPT_ABILITY" ModifierId="MOD_CIV_WONDER_PRODUCTION_EGYPT"/>
</TraitModifiers>
```

## CivilizationFavoredWonders Table

Defines wonders each civ prefers:

```xml
<CivilizationFavoredWonders>
    <Row
        CivilizationType="CIVILIZATION_EGYPT"
        FavoredWonderType="WONDER_PYRAMIDS"
        FavoredWonderName="LOC_WONDER_PYRAMIDS_NAME"
    />
</CivilizationFavoredWonders>
```

## LeaderCivPriorities Table

Defines which leaders prefer which civs (AI behavior):

```xml
<LeaderCivPriorities>
    <Row Leader="LEADER_HATSHEPSUT" Civilization="CIVILIZATION_EGYPT" Priority="4"/>
    <Row Leader="LEADER_HATSHEPSUT" Civilization="CIVILIZATION_AKSUM" Priority="1"/>
</LeaderCivPriorities>
```

Higher priority = stronger preference.

## Antiquity Civilizations

| Civilization | Attributes | Unique Ability |
|--------------|------------|----------------|
| Aksum | Cultural, Economic | Trade and resource bonuses |
| Egypt | Cultural, Economic | Navigable river bonuses |
| Greece | Cultural, Political | Democracy and city-state bonuses |
| Han | Political, Scientific | Settlement bonuses |
| Khmer | Expansionist, Scientific | River bonuses |
| Maurya | Militaristic, Scientific | Pantheon and happiness |
| Maya | Political, Scientific | Palace bonuses |
| Mississippian | Economic, Expansionist | Town bonuses |
| Persia | Economic, Militaristic | Council bonuses |
| Rome | Cultural, Militaristic | Settlement bonuses |

## Example: Creating a New Civilization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Register types -->
    <Types>
        <Row Type="CIVILIZATION_MY_CIV" Kind="KIND_CIVILIZATION"/>
        <Row Type="TRAIT_MY_CIV_ABILITY" Kind="KIND_TRAIT"/>
    </Types>

    <!-- Define the civilization -->
    <Civilizations>
        <Row
            CivilizationType="CIVILIZATION_MY_CIV"
            Name="LOC_CIVILIZATION_MY_CIV_NAME"
            FullName="LOC_CIVILIZATION_MY_CIV_FULLNAME"
            Description="LOC_CIVILIZATION_MY_CIV_DESCRIPTION"
            Adjective="LOC_CIVILIZATION_MY_CIV_ADJECTIVE"
            StartingCivilizationLevelType="CIVILIZATION_LEVEL_FULL_CIV"
            UniqueCultureProgressionTree="TREE_CIVICS_AQ_MY_CIV"
            CapitalName="LOC_CITY_NAME_MY_CIV1"
            RandomCityNameDepth="10"
        />
    </Civilizations>

    <!-- Define traits -->
    <Traits>
        <Row
            TraitType="TRAIT_MY_CIV_ABILITY"
            Name="LOC_TRAIT_MY_CIV_ABILITY_NAME"
            Description="LOC_TRAIT_MY_CIV_ABILITY_DESCRIPTION"
            InternalOnly="true"
        />
    </Traits>

    <!-- Link traits to civilization -->
    <CivilizationTraits>
        <Row CivilizationType="CIVILIZATION_MY_CIV" TraitType="TRAIT_ANTIQUITY_CIV"/>
        <Row CivilizationType="CIVILIZATION_MY_CIV" TraitType="TRAIT_MY_CIV_ABILITY"/>
        <Row CivilizationType="CIVILIZATION_MY_CIV" TraitType="TRAIT_ATTRIBUTE_CULTURAL"/>
        <Row CivilizationType="CIVILIZATION_MY_CIV" TraitType="TRAIT_ATTRIBUTE_SCIENTIFIC"/>
    </CivilizationTraits>

    <!-- Link traits to modifiers (defined in GameEffects) -->
    <TraitModifiers>
        <Row TraitType="TRAIT_MY_CIV_ABILITY" ModifierId="MY_CIV_ABILITY_MODIFIER"/>
    </TraitModifiers>
</Database>
```

## Related Tables

- `CivilizationCityNames` - Available city names
- `StartingTraits` - Game start conditions
- `CivilizationLeaders` - Leader assignments
- Age transition tables
