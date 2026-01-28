# Collection Types Reference

Collections determine which subjects a modifier affects. The modifier system gathers subjects through collections and then applies effects to matching subjects.

## How Collections Work

1. Modifier activates (owner meets requirements)
2. Collection gathers potential subjects
3. Subject requirements filter subjects
4. Effect applies to remaining subjects

## Available Collection Types

### Player Collections

#### COLLECTION_OWNER

The owner of the modifier itself. Use for effects that apply to the modifier's owner directly.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_OWNER" effect="EFFECT_PLAYER_GRANT_YIELD">
    <Argument name="YieldType">YIELD_GOLD</Argument>
    <Argument name="Amount">100</Argument>
</Modifier>
```

#### COLLECTION_PLAYER_CITIES

All cities belonging to the player.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_FOOD</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

#### COLLECTION_OWNER_CITY

The city that owns the modifier (for building/improvement modifiers).

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_OWNER_CITY" effect="EFFECT_CITY_GRANT_UNIT">
    <Argument name="UnitType">UNIT_WARRIOR</Argument>
</Modifier>
```

### Unit Collections

#### COLLECTION_PLAYER_UNITS

All units belonging to the player.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_UNITS" effect="EFFECT_UNIT_ADJUST_ABILITY">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_UNIT_TAG_MATCHES">
            <Argument name="Tag">UNIT_CLASS_INFANTRY</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="AbilityType">ABILITY_FIRST_STRIKE</Argument>
</Modifier>
```

#### COLLECTION_PLAYER_COMBAT

Player's combat units during combat resolution.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_COMBAT" effect="EFFECT_ADJUST_UNIT_STRENGTH_MODIFIER">
    <Argument name="Amount">5</Argument>
</Modifier>
```

### Plot Collections

#### COLLECTION_PLAYER_PLOT_YIELDS

Plots that provide yields to the player. Used for plot-based yield bonuses.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_PLOT_YIELDS" effect="EFFECT_PLOT_ADJUST_YIELD">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLOT_RESOURCE_VISIBLE"/>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_GOLD</Argument>
    <Argument name="Amount">1</Argument>
</Modifier>
```

## Collection + Effect Combinations

Not all collections work with all effects. Common combinations:

| Collection | Common Effects |
|------------|---------------|
| `COLLECTION_OWNER` | `EFFECT_PLAYER_GRANT_YIELD`, `EFFECT_PLAYER_ADJUST_*`, Scoring effects |
| `COLLECTION_PLAYER_CITIES` | `EFFECT_CITY_ADJUST_YIELD`, `EFFECT_CITY_GRANT_UNIT`, Production effects |
| `COLLECTION_OWNER_CITY` | `EFFECT_CITY_*` effects for building modifiers |
| `COLLECTION_PLAYER_UNITS` | `EFFECT_UNIT_*`, `EFFECT_ARMY_*` effects |
| `COLLECTION_PLAYER_COMBAT` | `EFFECT_ADJUST_UNIT_STRENGTH_MODIFIER` |
| `COLLECTION_PLAYER_PLOT_YIELDS` | `EFFECT_PLOT_ADJUST_YIELD` |

## Using Collections with Requirements

Collections gather all potential subjects, then SubjectRequirements filter them:

```xml
<Modifier id="COASTAL_CITY_BONUS" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLOT_ADJACENT_TO_COAST"/>
        <Requirement type="REQUIREMENT_CITY_IS_CITY"/>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_GOLD</Argument>
    <Argument name="Amount">3</Argument>
</Modifier>
```

This applies +3 gold only to coastal cities (filtering out inland cities and towns).

## Examples from Game Files

### Trade Route Culture Bonus (Aksum)
```xml
<Modifier id="PORT_OF_NATIONS_MOD_TRADE_ROUTE_CULTURE" collection="COLLECTION_OWNER" effect="EFFECT_PLAYER_ADJUST_YIELD_PER_NUM_TRADE_ROUTES">
    <Argument name="YieldType">YIELD_CULTURE</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

### Commander XP Bonus (Greece)
```xml
<Modifier id="STRATEGOI_MOD_COMMANDER_XP" collection="COLLECTION_PLAYER_UNITS" effect="EFFECT_ARMY_ADJUST_EXPERIENCE_RATE">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_UNIT_TAG_MATCHES">
            <Argument name="Tag">UNIT_CLASS_COMMAND</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="Percent">25</Argument>
</Modifier>
```

### Navigable River Food Bonus (Egypt)
```xml
<Modifier id="AKHET_MOD_NAVIGABLE_RIVER_FOOD" collection="COLLECTION_PLAYER_PLOT_YIELDS" effect="EFFECT_PLOT_ADJUST_YIELD">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLOT_IS_RIVER">
            <Argument name="Navigable">true</Argument>
        </Requirement>
        <Requirement type="REQUIREMENT_PLOT_DISTRICT_CLASS">
            <Argument name="DistrictClass">RURAL, URBAN, CITYCENTER</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_FOOD</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

## Database Definition

In traditional database format, collections are defined in `DynamicModifiers`:

```xml
<DynamicModifiers>
    <Row
        ModifierType="MY_MODIFIER_TYPE"
        CollectionType="COLLECTION_PLAYER_CITIES"
        EffectType="EFFECT_CITY_ADJUST_YIELD"
    />
</DynamicModifiers>
```

## See Also

- [Effect Types](Effect-Types.md)
- [Requirement Types](Requirement-Types.md)
- [Modifier System Overview](Overview.md)
