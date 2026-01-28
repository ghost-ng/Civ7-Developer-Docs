# Unit Promotion System

This document covers the unit promotion system in Civilization VII, including promotion classes, disciplines, commendations, and how to create custom promotions.

## Overview

Civ VII uses a hierarchical promotion system based on:
- **Promotion Classes** - Categories of units (Land, Naval, Air, Aerodrome, Carrier)
- **Disciplines** - Specialization paths within each class (e.g., Bastion, Assault, Logistics)
- **Promotions** - Individual upgrades within disciplines, forming prerequisite trees
- **Commendations** - Special rewards earned when completing a discipline

## Core Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register promotions and disciplines as kinds |
| `UnitPromotionClasses` | Define promotion class categories |
| `UnitPromotionDisciplines` | Define discipline specializations |
| `UnitPromotionClassSets` | Link classes to their available disciplines |
| `UnitPromotionDisciplineDetails` | Define promotions within disciplines with prerequisites |
| `UnitPromotions` | Define promotion details (name, description, commendation flag) |
| `UnitPromotionModifiers` | Link promotions to their gameplay effects |
| `UnitAbilities` | Define abilities granted by promotions |
| `UnitAbilityModifiers` | Link abilities to their modifiers |

## Type Kinds

The promotion system uses three type kinds:

```xml
<Types>
    <!-- Promotion Class (Land, Naval, Air) -->
    <Row Type="PROMOTIONCLASS_MY_CLASS" Kind="KIND_PROMOTION_CLASS"/>

    <!-- Discipline (specialization within a class) -->
    <Row Type="DISCIPLINE_MY_DISCIPLINE" Kind="KIND_PROMOITON_DISCIPLINE_CLASS"/>

    <!-- Individual Promotion -->
    <Row Type="PROMOTION_MY_PROMOTION" Kind="KIND_PROMOTION"/>
</Types>
```

Note: `KIND_PROMOITON_DISCIPLINE_CLASS` has a typo in the game files (missing 'I' in PROMOTION).

## Promotion Classes

Promotion classes define what category of commander units can use a set of disciplines.

### Built-in Classes

| Class | Description |
|-------|-------------|
| `PROMOTIONCLASS_LAND` | Army commanders (land units) |
| `PROMOTIONCLASS_NAVAL` | Fleet commanders (naval units) |
| `PROMOTIONCLASS_AIR` | Squadron commanders (aircraft) |
| `PROMOTIONCLASS_AERODROME` | Aerodrome-based aircraft |
| `PROMOTIONCLASS_CARRIER` | Carrier-based aircraft |

### UnitPromotionClasses Table

```xml
<UnitPromotionClasses>
    <Row PromotionClassType="PROMOTIONCLASS_MY_CLASS"/>
</UnitPromotionClasses>
```

| Column | Type | Description |
|--------|------|-------------|
| `PromotionClassType` | TEXT | Unique identifier for the class |

## Disciplines

Disciplines are specialization paths within a promotion class. Each discipline contains a tree of promotions.

### Built-in Disciplines

#### Army Disciplines
| Discipline | Focus |
|------------|-------|
| `DISCIPLINE_ARMY_BASTION` | Defensive combat |
| `DISCIPLINE_ARMY_ASSAULT` | Offensive combat |
| `DISCIPLINE_ARMY_LOGISTICS` | Supply and healing |
| `DISCIPLINE_ARMY_MANEUVER` | Movement and mobility |
| `DISCIPLINE_ARMY_LEADERSHIP` | Unit buffs and support |

#### Fleet Disciplines
| Discipline | Focus |
|------------|-------|
| `DISCIPLINE_FLEET_BOMBARDMENT` | Ranged naval attacks |
| `DISCIPLINE_FLEET_ENGAGEMENT` | Direct naval combat |
| `DISCIPLINE_FLEET_LOGISTICS` | Supply and fleet support |
| `DISCIPLINE_FLEET_MANEUVER` | Naval movement |
| `DISCIPLINE_FLEET_LEADERSHIP` | Fleet buffs |

#### Squadron Disciplines
| Discipline | Focus |
|------------|-------|
| `DISCIPLINE_SQUADRON_DOGFIGHTING` | Air-to-air combat |
| `DISCIPLINE_SQUADRON_RAIDS` | Ground attack missions |
| `DISCIPLINE_SQUADRON_LOGISTICS` | Air support operations |
| `DISCIPLINE_SQUADRON_AIRLIFT_OPERATIONS` | Transport operations |
| `DISCIPLINE_AERODROME_LOGISTICS` | Aerodrome support |
| `DISCIPLINE_CARRIER_OPERATIONS` | Carrier aircraft |
| `DISCIPLINE_CARRIER_LOGISTICS` | Carrier support |

### Commendation Disciplines

Each commander type has special commendation disciplines for rewards:

| Discipline | Commander Type |
|------------|----------------|
| `DISCIPLINE_ARMY_COMMENDATION_VALOR` | Army |
| `DISCIPLINE_ARMY_COMMENDATION_SERVICE` | Army |
| `DISCIPLINE_ARMY_COMMENDATION_ORDER` | Army |
| `DISCIPLINE_ARMY_COMMENDATION_DUTY` | Army |
| `DISCIPLINE_ARMY_COMMENDATION_MERIT` | Army |
| `DISCIPLINE_FLEET_COMMENDATION_*` | Fleet |
| `DISCIPLINE_SQUADRON_COMMENDATION_*` | Squadron |

### Defining Disciplines

```xml
<UnitPromotionDisciplines>
    <Row UnitPromotionDisciplineType="DISCIPLINE_MY_DISCIPLINE"
         Name="LOC_DISCIPLINE_MY_DISCIPLINE_NAME"
         Description="LOC_DISCIPLINE_MY_DISCIPLINE_DESCRIPTION"/>
</UnitPromotionDisciplines>
```

| Column | Type | Description |
|--------|------|-------------|
| `UnitPromotionDisciplineType` | TEXT | Unique discipline identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |

### Linking Disciplines to Classes

```xml
<UnitPromotionClassSets>
    <Row UnitPromotionClassType="PROMOTIONCLASS_LAND"
         UnitPromotionDisciplineType="DISCIPLINE_MY_DISCIPLINE"/>
</UnitPromotionClassSets>
```

## Promotion Trees

### Defining Promotions in a Discipline

The `UnitPromotionDisciplineDetails` table defines which promotions belong to a discipline and their prerequisites:

```xml
<UnitPromotionDisciplineDetails>
    <!-- Root promotion (no prerequisite) -->
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_BASTION"
         UnitPromotionType="PROMOTION_ARMY_STEADFAST"/>

    <!-- Second tier (requires first) -->
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_BASTION"
         UnitPromotionType="PROMOTION_ARMY_BULWARK"
         PrereqUnitPromotion="PROMOTION_ARMY_STEADFAST"/>
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_BASTION"
         UnitPromotionType="PROMOTION_ARMY_HOLD_THE_LINE"
         PrereqUnitPromotion="PROMOTION_ARMY_STEADFAST"/>

    <!-- Third tier with multiple paths (grants commendation) -->
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_BASTION"
         UnitPromotionType="PROMOTION_ARMY_GARRISON"
         PrereqUnitPromotion="PROMOTION_ARMY_BULWARK"
         GrantsCommendation="true"/>
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_BASTION"
         UnitPromotionType="PROMOTION_ARMY_GARRISON"
         PrereqUnitPromotion="PROMOTION_ARMY_HOLD_THE_LINE"
         GrantsCommendation="true"/>
</UnitPromotionDisciplineDetails>
```

| Column | Type | Description |
|--------|------|-------------|
| `UnitPromotionDisciplineType` | TEXT | The discipline this belongs to |
| `UnitPromotionType` | TEXT | The promotion being added |
| `PrereqUnitPromotion` | TEXT | Required promotion (optional for root) |
| `GrantsCommendation` | BOOLEAN | Whether earning this grants a commendation |

### Typical Tree Structure

Most disciplines follow this pattern:
```
        [Root Promotion]
             |
      +------+------+
      |             |
  [Tier 2A]    [Tier 2B]
      |             |
      +------+------+
             |
    [Final Promotion]
       (Commendation)
```

The final promotion typically has two prerequisite paths (can reach it via either Tier 2A or 2B) and grants a commendation.

## Defining Promotions

### UnitPromotions Table

```xml
<UnitPromotions>
    <Row UnitPromotionType="PROMOTION_MY_PROMOTION"
         Name="LOC_PROMOTION_MY_PROMOTION_NAME"
         Description="LOC_PROMOTION_MY_PROMOTION_DESCRIPTION"/>

    <!-- Commendation promotion -->
    <Row UnitPromotionType="PROMOTION_COMMENDATION_MY_COMMENDATION"
         Name="LOC_COMMENDATION_MY_NAME"
         Description="LOC_COMMENDATION_MY_DESCRIPTION"
         Commendation="true"/>
</UnitPromotions>
```

| Column | Type | Description |
|--------|------|-------------|
| `UnitPromotionType` | TEXT | Unique promotion identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `Commendation` | BOOLEAN | Whether this is a commendation (special reward) |

## Promotion Effects

### Linking Promotions to Modifiers

```xml
<UnitPromotionModifiers>
    <Row UnitPromotionType="PROMOTION_MY_PROMOTION" ModifierId="MOD_MY_PROMOTION_EFFECT"/>
    <!-- A promotion can have multiple modifiers -->
    <Row UnitPromotionType="PROMOTION_MY_PROMOTION" ModifierId="MOD_MY_PROMOTION_EFFECT_2"/>
</UnitPromotionModifiers>
```

### Defining Modifiers (GameEffects)

```xml
<GameEffects xmlns="GameEffects">
    <!-- Simple strength modifier -->
    <Modifier id="MOD_MY_PROMOTION_STRENGTH"
              collection="COLLECTION_UNIT_COMBAT"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH_MODIFIER">
        <Argument name="Amount">5</Argument>
    </Modifier>

    <!-- Grant ability -->
    <Modifier id="MOD_MY_PROMOTION_ABILITY"
              collection="COLLECTION_OWNER"
              effect="EFFECT_UNIT_ADJUST_ABILITY">
        <Argument name="AbilityType">ABILITY_MY_ABILITY</Argument>
        <Argument name="Amount">1</Argument>
    </Modifier>

    <!-- Movement bonus -->
    <Modifier id="MOD_MY_PROMOTION_MOVEMENT"
              collection="COLLECTION_OWNER"
              effect="EFFECT_ADJUST_UNIT_MOVEMENT">
        <Argument name="Amount">1</Argument>
    </Modifier>
</GameEffects>
```

## Unit Abilities

Many promotions grant abilities that have their own conditional effects.

### Defining Abilities

```xml
<UnitAbilities>
    <Row UnitAbilityType="ABILITY_MY_ABILITY"
         Name="LOC_ABILITY_MY_ABILITY_NAME"
         Description="LOC_ABILITY_MY_ABILITY_DESCRIPTION"
         Inactive="true"
         Permanent="false"/>
</UnitAbilities>
```

| Column | Type | Description |
|--------|------|-------------|
| `UnitAbilityType` | TEXT | Unique ability identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `Inactive` | BOOLEAN | Start as inactive (activated by promotion) |
| `Permanent` | BOOLEAN | Whether ability persists permanently |
| `ShareWithChildren` | BOOLEAN | For air units, share ability with child aircraft |

### Ability Modifiers

```xml
<UnitAbilityModifiers>
    <Row UnitAbilityType="ABILITY_MY_ABILITY" ModifierId="MOD_ABILITY_MY_EFFECT"/>
</UnitAbilityModifiers>
```

## Commendations

Commendations are special promotions earned when a unit completes a discipline (reaches the final promotion marked with `GrantsCommendation="true"`).

### Built-in Commendations

#### Army Commendations
| Commendation | Effect |
|--------------|--------|
| `PROMOTION_COMMENDATION_ARMY_VALOR` | Combat strength bonus |
| `PROMOTION_COMMENDATION_ARMY_DUTY` | Attack damage bonus |
| `PROMOTION_COMMENDATION_ARMY_SERVICE` | Healing bonus |
| `PROMOTION_COMMENDATION_ARMY_MERIT` | Experience gain bonus |
| `PROMOTION_COMMENDATION_ARMY_ORDER` | Support bonus |

#### Fleet Commendations
| Commendation | Effect |
|--------------|--------|
| `PROMOTION_COMMENDATION_FLEET_VALOR` | Combat strength bonus |
| `PROMOTION_COMMENDATION_FLEET_DUTY` | Attack damage bonus |
| `PROMOTION_COMMENDATION_FLEET_SERVICE` | Healing bonus |
| `PROMOTION_COMMENDATION_FLEET_MERIT` | Experience gain bonus |
| `PROMOTION_COMMENDATION_FLEET_ORDER` | Support bonus |

#### Squadron Commendations
| Commendation | Effect |
|--------------|--------|
| `PROMOTION_COMMENDATION_SQUADRON_ACHIEVEMENT` | Combat bonus |
| `PROMOTION_COMMENDATION_SQUADRON_SERVICE` | Support bonus |
| `PROMOTION_COMMENDATION_SQUADRON_ORDER` | Coordination bonus |

## Complete Example: Custom Discipline

### Step 1: Register Types

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <!-- Discipline type -->
        <Row Type="DISCIPLINE_ARMY_SIEGE" Kind="KIND_PROMOITON_DISCIPLINE_CLASS"/>

        <!-- Promotion types -->
        <Row Type="PROMOTION_ARMY_SIEGE_BOMBARDIER" Kind="KIND_PROMOTION"/>
        <Row Type="PROMOTION_ARMY_SIEGE_SAPPERS" Kind="KIND_PROMOTION"/>
        <Row Type="PROMOTION_ARMY_SIEGE_BREACH" Kind="KIND_PROMOTION"/>
        <Row Type="PROMOTION_ARMY_SIEGE_DEVASTATION" Kind="KIND_PROMOTION"/>

        <!-- Ability type -->
        <Row Type="ABILITY_ARMY_SIEGE_BREACH" Kind="KIND_ABILITY"/>
    </Types>
</Database>
```

### Step 2: Define Discipline

```xml
<UnitPromotionDisciplines>
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_SIEGE"
         Name="LOC_DISCIPLINE_ARMY_SIEGE_NAME"
         Description="LOC_DISCIPLINE_ARMY_SIEGE_DESCRIPTION"/>
</UnitPromotionDisciplines>

<!-- Link to Army class -->
<UnitPromotionClassSets>
    <Row UnitPromotionClassType="PROMOTIONCLASS_LAND"
         UnitPromotionDisciplineType="DISCIPLINE_ARMY_SIEGE"/>
</UnitPromotionClassSets>
```

### Step 3: Define Promotion Tree

```xml
<UnitPromotionDisciplineDetails>
    <!-- Root: Bombardier -->
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_SIEGE"
         UnitPromotionType="PROMOTION_ARMY_SIEGE_BOMBARDIER"/>

    <!-- Tier 2: Sappers (requires Bombardier) -->
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_SIEGE"
         UnitPromotionType="PROMOTION_ARMY_SIEGE_SAPPERS"
         PrereqUnitPromotion="PROMOTION_ARMY_SIEGE_BOMBARDIER"/>

    <!-- Tier 2: Breach (requires Bombardier) -->
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_SIEGE"
         UnitPromotionType="PROMOTION_ARMY_SIEGE_BREACH"
         PrereqUnitPromotion="PROMOTION_ARMY_SIEGE_BOMBARDIER"/>

    <!-- Final: Devastation (via either path, grants commendation) -->
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_SIEGE"
         UnitPromotionType="PROMOTION_ARMY_SIEGE_DEVASTATION"
         PrereqUnitPromotion="PROMOTION_ARMY_SIEGE_SAPPERS"
         GrantsCommendation="true"/>
    <Row UnitPromotionDisciplineType="DISCIPLINE_ARMY_SIEGE"
         UnitPromotionType="PROMOTION_ARMY_SIEGE_DEVASTATION"
         PrereqUnitPromotion="PROMOTION_ARMY_SIEGE_BREACH"
         GrantsCommendation="true"/>
</UnitPromotionDisciplineDetails>
```

### Step 4: Define Promotions

```xml
<UnitPromotions>
    <Row UnitPromotionType="PROMOTION_ARMY_SIEGE_BOMBARDIER"
         Name="LOC_PROMOTION_ARMY_SIEGE_BOMBARDIER_NAME"
         Description="LOC_PROMOTION_ARMY_SIEGE_BOMBARDIER_DESCRIPTION"/>
    <Row UnitPromotionType="PROMOTION_ARMY_SIEGE_SAPPERS"
         Name="LOC_PROMOTION_ARMY_SIEGE_SAPPERS_NAME"
         Description="LOC_PROMOTION_ARMY_SIEGE_SAPPERS_DESCRIPTION"/>
    <Row UnitPromotionType="PROMOTION_ARMY_SIEGE_BREACH"
         Name="LOC_PROMOTION_ARMY_SIEGE_BREACH_NAME"
         Description="LOC_PROMOTION_ARMY_SIEGE_BREACH_DESCRIPTION"/>
    <Row UnitPromotionType="PROMOTION_ARMY_SIEGE_DEVASTATION"
         Name="LOC_PROMOTION_ARMY_SIEGE_DEVASTATION_NAME"
         Description="LOC_PROMOTION_ARMY_SIEGE_DEVASTATION_DESCRIPTION"/>
</UnitPromotions>
```

### Step 5: Define Modifiers and Abilities

```xml
<UnitPromotionModifiers>
    <Row UnitPromotionType="PROMOTION_ARMY_SIEGE_BOMBARDIER" ModifierId="MOD_SIEGE_BOMBARDIER"/>
    <Row UnitPromotionType="PROMOTION_ARMY_SIEGE_SAPPERS" ModifierId="MOD_SIEGE_SAPPERS"/>
    <Row UnitPromotionType="PROMOTION_ARMY_SIEGE_BREACH" ModifierId="MOD_SIEGE_BREACH"/>
    <Row UnitPromotionType="PROMOTION_ARMY_SIEGE_DEVASTATION" ModifierId="MOD_SIEGE_DEVASTATION"/>
</UnitPromotionModifiers>

<UnitAbilities>
    <Row UnitAbilityType="ABILITY_ARMY_SIEGE_BREACH"
         Name="LOC_ABILITY_ARMY_SIEGE_BREACH_NAME"
         Description="LOC_ABILITY_ARMY_SIEGE_BREACH_DESCRIPTION"
         Inactive="true" Permanent="false"/>
</UnitAbilities>

<UnitAbilityModifiers>
    <Row UnitAbilityType="ABILITY_ARMY_SIEGE_BREACH" ModifierId="MOD_ABILITY_SIEGE_BREACH"/>
</UnitAbilityModifiers>
```

### Step 6: GameEffects

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Bombardier: +5 vs districts -->
    <Modifier id="MOD_SIEGE_BOMBARDIER"
              collection="COLLECTION_UNIT_COMBAT"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH_MODIFIER">
        <Argument name="Amount">5</Argument>
        <SubjectRequirement id="REQ_DISTRICT_DEFENDER">
            <Argument name="DefenderType">DISTRICT</Argument>
        </SubjectRequirement>
    </Modifier>

    <!-- Sappers: Bonus vs walls -->
    <Modifier id="MOD_SIEGE_SAPPERS"
              collection="COLLECTION_UNIT_COMBAT"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH_MODIFIER">
        <Argument name="Amount">10</Argument>
        <SubjectRequirement id="REQ_WALL_DEFENDER">
            <Argument name="DefenderHasWalls">true</Argument>
        </SubjectRequirement>
    </Modifier>

    <!-- Breach: Grant ability -->
    <Modifier id="MOD_SIEGE_BREACH"
              collection="COLLECTION_OWNER"
              effect="EFFECT_UNIT_ADJUST_ABILITY">
        <Argument name="AbilityType">ABILITY_ARMY_SIEGE_BREACH</Argument>
        <Argument name="Amount">1</Argument>
    </Modifier>

    <!-- Breach Ability: Ignore fortifications -->
    <Modifier id="MOD_ABILITY_SIEGE_BREACH"
              collection="COLLECTION_OWNER"
              effect="EFFECT_UNIT_IGNORE_WALLS"/>

    <!-- Devastation: Massive siege bonus -->
    <Modifier id="MOD_SIEGE_DEVASTATION"
              collection="COLLECTION_UNIT_COMBAT"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH_MODIFIER">
        <Argument name="Amount">15</Argument>
        <SubjectRequirement id="REQ_DISTRICT_DEFENDER">
            <Argument name="DefenderType">DISTRICT</Argument>
        </SubjectRequirement>
    </Modifier>
</GameEffects>
```

### Step 7: Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_DISCIPLINE_ARMY_SIEGE_NAME" Language="en_US">
            <Text>Siege Warfare</Text>
        </Row>
        <Row Tag="LOC_DISCIPLINE_ARMY_SIEGE_DESCRIPTION" Language="en_US">
            <Text>Specialize in breaking fortifications and conquering cities.</Text>
        </Row>
        <Row Tag="LOC_PROMOTION_ARMY_SIEGE_BOMBARDIER_NAME" Language="en_US">
            <Text>Bombardier</Text>
        </Row>
        <Row Tag="LOC_PROMOTION_ARMY_SIEGE_BOMBARDIER_DESCRIPTION" Language="en_US">
            <Text>+5 Combat Strength against districts.</Text>
        </Row>
        <!-- ... additional localization ... -->
    </LocalizedText>
</Database>
```

## Common Promotion Effects

### Combat Strength Modifiers

```xml
<!-- General strength bonus -->
<Modifier id="MOD_STRENGTH_BONUS"
          collection="COLLECTION_UNIT_COMBAT"
          effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH_MODIFIER">
    <Argument name="Amount">5</Argument>
</Modifier>

<!-- Defensive strength -->
<Modifier id="MOD_DEFENSE_BONUS"
          collection="COLLECTION_UNIT_COMBAT"
          effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH_MODIFIER">
    <Argument name="Amount">7</Argument>
    <SubjectRequirement id="REQ_IS_DEFENDER"/>
</Modifier>
```

### Movement Modifiers

```xml
<!-- Extra movement -->
<Modifier id="MOD_EXTRA_MOVEMENT"
          collection="COLLECTION_OWNER"
          effect="EFFECT_ADJUST_UNIT_MOVEMENT">
    <Argument name="Amount">1</Argument>
</Modifier>

<!-- Ignore terrain cost -->
<Modifier id="MOD_IGNORE_TERRAIN"
          collection="COLLECTION_OWNER"
          effect="EFFECT_UNIT_ADJUST_IGNORE_TERRAIN_COST">
    <Argument name="Amount">1</Argument>
</Modifier>
```

### Healing and Support

```xml
<!-- Heal after combat -->
<Modifier id="MOD_HEAL_AFTER_KILL"
          collection="COLLECTION_OWNER"
          effect="EFFECT_UNIT_ADJUST_HEAL_PER_KILL">
    <Argument name="Amount">25</Argument>
</Modifier>

<!-- Faster healing -->
<Modifier id="MOD_BONUS_HEALING"
          collection="COLLECTION_OWNER"
          effect="EFFECT_UNIT_ADJUST_HEALING_BONUS">
    <Argument name="Amount">10</Argument>
</Modifier>
```

### Unit Capacity

```xml
<!-- Increase attached unit capacity -->
<Modifier id="MOD_UNIT_CAPACITY"
          collection="COLLECTION_OWNER"
          effect="EFFECT_UNIT_ADJUST_UNIT_CAPACITY">
    <Argument name="Amount">1</Argument>
</Modifier>
```

## Best Practices

1. **Follow the Tree Pattern** - Use root -> tier 2 branch -> final convergence structure
2. **Balance Choices** - Make tier 2 options meaningfully different
3. **Commendations at End** - Only mark the final promotion as `GrantsCommendation="true"`
4. **Thematic Consistency** - Keep all promotions in a discipline related to its theme
5. **Clear Descriptions** - Explain exactly what bonuses the promotion provides
6. **Test Prerequisites** - Ensure all prerequisite chains are valid
7. **Unique Identifiers** - Use clear naming conventions (e.g., `PROMOTION_ARMY_SIEGE_*`)

## Related Documentation

- [Units](../database/Units.md)
- [Modifier System](../modifiers/Overview.md)
- [Effect Types](../modifiers/Effect-Types.md)
- [Types and Kinds](../database/Types-and-Kinds.md)
