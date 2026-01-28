# Unique Abilities

This document covers creating unique abilities for civilizations and leaders in Civilization VII, with complete working examples.

## Overview

Unique abilities are implemented through the **Trait system**:

- **Civilization Abilities** - Bonuses that apply to a civilization regardless of leader
- **Leader Abilities** - Bonuses specific to a leader that can be paired with different civilizations
- **Combined Effects** - The total bonuses a player receives from both civ and leader traits

## Trait System Architecture

```
Trait (KIND_TRAIT)
    └── TraitModifiers (links to modifiers)
            └── Modifier (gameplay effect)
                    └── Collection (what it affects)
                    └── Effect (what it does)
                    └── Requirements (when it applies)
```

## Creating a Civilization Ability

### Step 1: Register the Trait Type

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <Row Type="TRAIT_CIVILIZATION_NAVAL_DOMINANCE" Kind="KIND_TRAIT"/>
    </Types>
</Database>
```

### Step 2: Define the Trait

```xml
<Traits>
    <Row TraitType="TRAIT_CIVILIZATION_NAVAL_DOMINANCE"
         Name="LOC_TRAIT_NAVAL_DOMINANCE_NAME"
         Description="LOC_TRAIT_NAVAL_DOMINANCE_DESCRIPTION"
         InternalOnly="false"/>
</Traits>
```

### Step 3: Link to Civilization

```xml
<CivilizationTraits>
    <Row CivilizationType="CIVILIZATION_MY_NAVAL_CIV"
         TraitType="TRAIT_CIVILIZATION_NAVAL_DOMINANCE"/>
</CivilizationTraits>
```

### Step 4: Add Modifiers

```xml
<TraitModifiers>
    <Row TraitType="TRAIT_CIVILIZATION_NAVAL_DOMINANCE"
         ModifierId="MOD_NAVAL_COMBAT_BONUS"/>
    <Row TraitType="TRAIT_CIVILIZATION_NAVAL_DOMINANCE"
         ModifierId="MOD_COASTAL_CITY_GOLD"/>
</TraitModifiers>
```

### Step 5: Define Modifiers (GameEffects)

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- +5 Combat Strength for naval units -->
    <Modifier id="MOD_NAVAL_COMBAT_BONUS"
              collection="COLLECTION_PLAYER_UNITS"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_UNIT_DOMAIN_MATCHES">
                <Argument name="Domain">DOMAIN_SEA</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="Amount">5</Argument>
    </Modifier>

    <!-- +3 Gold in coastal cities -->
    <Modifier id="MOD_COASTAL_CITY_GOLD"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_CITY_IS_COASTAL"/>
        </SubjectRequirements>
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="Amount">3</Argument>
    </Modifier>
</GameEffects>
```

### Step 6: Localization

```xml
<LocalizedText>
    <Row Tag="LOC_TRAIT_NAVAL_DOMINANCE_NAME" Language="en_US">
        <Text>Masters of the Sea</Text>
    </Row>
    <Row Tag="LOC_TRAIT_NAVAL_DOMINANCE_DESCRIPTION" Language="en_US">
        <Text>Naval units receive +5 [ICON_STRENGTH] Combat Strength. Coastal cities gain +3 [ICON_GOLD] Gold.</Text>
    </Row>
</LocalizedText>
```

## Creating a Leader Ability

Leader abilities work the same way but link via `LeaderTraits`:

### Complete Leader Ability Example

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Register types -->
    <Types>
        <Row Type="TRAIT_LEADER_CONQUEST_GLORY" Kind="KIND_TRAIT"/>
    </Types>

    <!-- Define trait -->
    <Traits>
        <Row TraitType="TRAIT_LEADER_CONQUEST_GLORY"
             Name="LOC_TRAIT_CONQUEST_GLORY_NAME"
             Description="LOC_TRAIT_CONQUEST_GLORY_DESCRIPTION"/>
    </Traits>

    <!-- Link to leader -->
    <LeaderTraits>
        <Row LeaderType="LEADER_MY_CONQUEROR"
             TraitType="TRAIT_LEADER_CONQUEST_GLORY"/>
    </LeaderTraits>

    <!-- Link modifiers -->
    <TraitModifiers>
        <Row TraitType="TRAIT_LEADER_CONQUEST_GLORY"
             ModifierId="MOD_CONQUEST_COMBAT_BONUS"/>
        <Row TraitType="TRAIT_LEADER_CONQUEST_GLORY"
             ModifierId="MOD_CONQUEST_GOLD_ON_CAPTURE"/>
    </TraitModifiers>

    <LocalizedText>
        <Row Tag="LOC_TRAIT_CONQUEST_GLORY_NAME" Language="en_US">
            <Text>Glory of Conquest</Text>
        </Row>
        <Row Tag="LOC_TRAIT_CONQUEST_GLORY_DESCRIPTION" Language="en_US">
            <Text>+7 [ICON_STRENGTH] Combat Strength when attacking cities. Gain 50 [ICON_GOLD] Gold when capturing a city.</Text>
        </Row>
    </LocalizedText>
</Database>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Combat bonus vs cities -->
    <Modifier id="MOD_CONQUEST_COMBAT_BONUS"
              collection="COLLECTION_PLAYER_UNITS"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_UNIT_ATTACKING"/>
            <Requirement type="REQUIREMENT_OPPONENT_IS_CITY"/>
        </SubjectRequirements>
        <Argument name="Amount">7</Argument>
    </Modifier>

    <!-- Gold on city capture -->
    <Modifier id="MOD_CONQUEST_GOLD_ON_CAPTURE"
              collection="COLLECTION_OWNER"
              effect="EFFECT_GRANT_YIELD_ON_CITY_CAPTURE">
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="Amount">50</Argument>
    </Modifier>
</GameEffects>
```

## Common Ability Types

### Yield Bonuses

```xml
<!-- Flat yield bonus -->
<Modifier id="MOD_PRODUCTION_BONUS"
          collection="COLLECTION_PLAYER_CITIES"
          effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_PRODUCTION</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>

<!-- Percentage yield bonus -->
<Modifier id="MOD_SCIENCE_PERCENT_BONUS"
          collection="COLLECTION_PLAYER_CITIES"
          effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_SCIENCE</Argument>
    <Argument name="Percent">10</Argument>
</Modifier>

<!-- Conditional yield (e.g., near mountains) -->
<Modifier id="MOD_MOUNTAIN_FAITH"
          collection="COLLECTION_PLAYER_CITIES"
          effect="EFFECT_CITY_ADJUST_YIELD">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_CITY_ADJACENT_TO_TERRAIN">
            <Argument name="TerrainType">TERRAIN_MOUNTAIN</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_FAITH</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

### Combat Bonuses

```xml
<!-- Unit type bonus -->
<Modifier id="MOD_CAVALRY_BONUS"
          collection="COLLECTION_PLAYER_UNITS"
          effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_UNIT_TAG_MATCHES">
            <Argument name="Tag">CLASS_CAVALRY</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="Amount">5</Argument>
</Modifier>

<!-- Terrain combat bonus -->
<Modifier id="MOD_FOREST_COMBAT"
          collection="COLLECTION_PLAYER_UNITS"
          effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLOT_FEATURE_TYPE_MATCHES">
            <Argument name="FeatureType">FEATURE_FOREST</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="Amount">3</Argument>
</Modifier>

<!-- Defensive bonus -->
<Modifier id="MOD_DEFENSIVE_BONUS"
          collection="COLLECTION_PLAYER_UNITS"
          effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_UNIT_DEFENDING"/>
    </SubjectRequirements>
    <Argument name="Amount">5</Argument>
</Modifier>
```

### Production Bonuses

```xml
<!-- Wonder production bonus -->
<Modifier id="MOD_WONDER_PRODUCTION"
          collection="COLLECTION_PLAYER_CITIES"
          effect="EFFECT_CITY_ADJUST_WONDER_PRODUCTION">
    <Argument name="Percent">15</Argument>
</Modifier>

<!-- Unit production bonus -->
<Modifier id="MOD_UNIT_PRODUCTION"
          collection="COLLECTION_PLAYER_CITIES"
          effect="EFFECT_CITY_ADJUST_UNIT_PRODUCTION">
    <Argument name="Percent">20</Argument>
</Modifier>

<!-- Specific unit type production -->
<Modifier id="MOD_CAVALRY_PRODUCTION"
          collection="COLLECTION_PLAYER_CITIES"
          effect="EFFECT_CITY_ADJUST_UNIT_PRODUCTION">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_UNIT_TAG_MATCHES">
            <Argument name="Tag">CLASS_CAVALRY</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="Percent">50</Argument>
</Modifier>
```

### Movement and Range

```xml
<!-- Extra movement -->
<Modifier id="MOD_EXTRA_MOVEMENT"
          collection="COLLECTION_PLAYER_UNITS"
          effect="EFFECT_ADJUST_UNIT_MOVEMENT">
    <Argument name="Amount">1</Argument>
</Modifier>

<!-- Extra range for ranged units -->
<Modifier id="MOD_EXTRA_RANGE"
          collection="COLLECTION_PLAYER_UNITS"
          effect="EFFECT_ADJUST_UNIT_RANGE">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_UNIT_IS_RANGED"/>
    </SubjectRequirements>
    <Argument name="Amount">1</Argument>
</Modifier>
```

### Trade and Diplomacy

```xml
<!-- Extra trade routes -->
<Modifier id="MOD_EXTRA_TRADE_ROUTES"
          collection="COLLECTION_OWNER"
          effect="EFFECT_PLAYER_ADJUST_TRADE_ROUTE_CAPACITY">
    <Argument name="Amount">2</Argument>
</Modifier>

<!-- Trade route yield bonus -->
<Modifier id="MOD_TRADE_GOLD_BONUS"
          collection="COLLECTION_PLAYER_TRADE_ROUTES"
          effect="EFFECT_TRADE_ROUTE_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_GOLD</Argument>
    <Argument name="Amount">3</Argument>
</Modifier>

<!-- Diplomatic favor bonus -->
<Modifier id="MOD_FAVOR_BONUS"
          collection="COLLECTION_OWNER"
          effect="EFFECT_ADJUST_PLAYER_DIPLOMACY_FAVOR_PER_TURN">
    <Argument name="Amount">2</Argument>
</Modifier>
```

### Great People

```xml
<!-- Bonus Great Person points -->
<Modifier id="MOD_GREAT_SCIENTIST_POINTS"
          collection="COLLECTION_OWNER"
          effect="EFFECT_ADJUST_GREAT_PERSON_POINTS">
    <Argument name="GreatPersonClassType">GREAT_PERSON_CLASS_SCIENTIST</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>

<!-- Bonus from Great Works -->
<Modifier id="MOD_GREAT_WORK_CULTURE"
          collection="COLLECTION_PLAYER_GREAT_WORKS"
          effect="EFFECT_ADJUST_GREAT_WORK_YIELD">
    <Argument name="YieldType">YIELD_CULTURE</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

### Religion

```xml
<!-- Religious spread bonus -->
<Modifier id="MOD_RELIGION_SPREAD"
          collection="COLLECTION_PLAYER_UNITS"
          effect="EFFECT_ADJUST_UNIT_SPREAD_CHARGES">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_UNIT_IS_RELIGIOUS"/>
    </SubjectRequirements>
    <Argument name="Amount">1</Argument>
</Modifier>

<!-- Faith purchase discount -->
<Modifier id="MOD_FAITH_PURCHASE_DISCOUNT"
          collection="COLLECTION_OWNER"
          effect="EFFECT_ADJUST_FAITH_PURCHASE_COST">
    <Argument name="Percent">-15</Argument>
</Modifier>
```

## Complete Example: Full Civilization

### Database File (MyCiv_Database.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- Types -->
    <Types>
        <Row Type="CIVILIZATION_PHOENICIA" Kind="KIND_CIVILIZATION"/>
        <Row Type="TRAIT_CIV_PHOENICIA" Kind="KIND_TRAIT"/>
        <Row Type="LEADER_DIDO" Kind="KIND_LEADER"/>
        <Row Type="TRAIT_LEADER_DIDO" Kind="KIND_TRAIT"/>
    </Types>

    <!-- Civilization -->
    <Civilizations>
        <Row CivilizationType="CIVILIZATION_PHOENICIA"
             Name="LOC_CIV_PHOENICIA_NAME"
             Description="LOC_CIV_PHOENICIA_DESC"
             Adjective="LOC_CIV_PHOENICIA_ADJECTIVE"
             StartingCivilizationLevelType="CIVILIZATION_LEVEL_FULL_CIV"/>
    </Civilizations>

    <!-- Civilization Trait -->
    <Traits>
        <Row TraitType="TRAIT_CIV_PHOENICIA"
             Name="LOC_TRAIT_CIV_PHOENICIA_NAME"
             Description="LOC_TRAIT_CIV_PHOENICIA_DESC"/>
    </Traits>

    <CivilizationTraits>
        <Row CivilizationType="CIVILIZATION_PHOENICIA"
             TraitType="TRAIT_CIV_PHOENICIA"/>
    </CivilizationTraits>

    <!-- Leader -->
    <Leaders>
        <Row LeaderType="LEADER_DIDO"
             Name="LOC_LEADER_DIDO_NAME"
             InheritFrom="LEADER_DEFAULT"/>
    </Leaders>

    <CivilizationLeaders>
        <Row CivilizationType="CIVILIZATION_PHOENICIA"
             LeaderType="LEADER_DIDO"
             CapitalName="LOC_CITY_NAME_CARTHAGE"/>
    </CivilizationLeaders>

    <!-- Leader Trait -->
    <Traits>
        <Row TraitType="TRAIT_LEADER_DIDO"
             Name="LOC_TRAIT_LEADER_DIDO_NAME"
             Description="LOC_TRAIT_LEADER_DIDO_DESC"/>
    </Traits>

    <LeaderTraits>
        <Row LeaderType="LEADER_DIDO"
             TraitType="TRAIT_LEADER_DIDO"/>
    </LeaderTraits>

    <!-- Trait Modifiers -->
    <TraitModifiers>
        <!-- Civ ability: Trade bonuses -->
        <Row TraitType="TRAIT_CIV_PHOENICIA" ModifierId="MOD_PHOENICIA_TRADE_GOLD"/>
        <Row TraitType="TRAIT_CIV_PHOENICIA" ModifierId="MOD_PHOENICIA_COASTAL_PRODUCTION"/>

        <!-- Leader ability: Naval power -->
        <Row TraitType="TRAIT_LEADER_DIDO" ModifierId="MOD_DIDO_NAVAL_COMBAT"/>
        <Row TraitType="TRAIT_LEADER_DIDO" ModifierId="MOD_DIDO_SETTLER_EMBARK"/>
    </TraitModifiers>
</Database>
```

### GameEffects File (MyCiv_Effects.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Phoenicia Civ Ability -->
    <Modifier id="MOD_PHOENICIA_TRADE_GOLD"
              collection="COLLECTION_PLAYER_TRADE_ROUTES"
              effect="EFFECT_TRADE_ROUTE_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="Amount">4</Argument>
    </Modifier>

    <Modifier id="MOD_PHOENICIA_COASTAL_PRODUCTION"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_CITY_IS_COASTAL"/>
        </SubjectRequirements>
        <Argument name="YieldType">YIELD_PRODUCTION</Argument>
        <Argument name="Amount">2</Argument>
    </Modifier>

    <!-- Dido Leader Ability -->
    <Modifier id="MOD_DIDO_NAVAL_COMBAT"
              collection="COLLECTION_PLAYER_UNITS"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_UNIT_DOMAIN_MATCHES">
                <Argument name="Domain">DOMAIN_SEA</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="Amount">7</Argument>
    </Modifier>

    <Modifier id="MOD_DIDO_SETTLER_EMBARK"
              collection="COLLECTION_PLAYER_UNITS"
              effect="EFFECT_ADJUST_UNIT_MOVEMENT">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_UNIT_TYPE_MATCHES">
                <Argument name="UnitType">UNIT_SETTLER</Argument>
            </Requirement>
            <Requirement type="REQUIREMENT_UNIT_IS_EMBARKED"/>
        </SubjectRequirements>
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```

### Localization (MyCiv_Text.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <!-- Civilization -->
        <Row Tag="LOC_CIV_PHOENICIA_NAME" Language="en_US">
            <Text>Phoenicia</Text>
        </Row>
        <Row Tag="LOC_CIV_PHOENICIA_DESC" Language="en_US">
            <Text>The ancient maritime traders of the Mediterranean.</Text>
        </Row>
        <Row Tag="LOC_CIV_PHOENICIA_ADJECTIVE" Language="en_US">
            <Text>Phoenician</Text>
        </Row>

        <!-- Civ Ability -->
        <Row Tag="LOC_TRAIT_CIV_PHOENICIA_NAME" Language="en_US">
            <Text>Mediterranean Traders</Text>
        </Row>
        <Row Tag="LOC_TRAIT_CIV_PHOENICIA_DESC" Language="en_US">
            <Text>Trade Routes yield +4 [ICON_GOLD] Gold. Coastal cities gain +2 [ICON_PRODUCTION] Production.</Text>
        </Row>

        <!-- Leader -->
        <Row Tag="LOC_LEADER_DIDO_NAME" Language="en_US">
            <Text>Dido</Text>
        </Row>

        <!-- Leader Ability -->
        <Row Tag="LOC_TRAIT_LEADER_DIDO_NAME" Language="en_US">
            <Text>Founder of Carthage</Text>
        </Row>
        <Row Tag="LOC_TRAIT_LEADER_DIDO_DESC" Language="en_US">
            <Text>Naval units gain +7 [ICON_STRENGTH] Combat Strength. Settlers gain +2 [ICON_MOVEMENT] Movement when embarked.</Text>
        </Row>

        <!-- City -->
        <Row Tag="LOC_CITY_NAME_CARTHAGE" Language="en_US">
            <Text>Carthage</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Best Practices

1. **Clear Descriptions** - Explain exactly what the ability does with numbers
2. **Use Icons** - Include yield icons in description text
3. **Balance Effects** - Compare to similar existing abilities
4. **Thematic Consistency** - Match abilities to historical/cultural themes
5. **Test Requirements** - Verify conditional modifiers trigger correctly
6. **Combine Effects** - Multiple small modifiers often better than one large one
7. **Consider AI** - AI should be able to leverage the ability effectively

## Related Documentation

- [Civilizations](../database/Civilizations.md)
- [Leaders](../database/Leaders.md)
- [Modifier System](../modifiers/Overview.md)
- [Effect Types](../modifiers/Effect-Types.md)
- [Requirement Types](../modifiers/Requirement-Types.md)
- [Localization](../technical-reference/Localization.md)
