# Collection Types Reference

This document lists all **40 collection types** available for modifiers in Civilization VII. Collections determine which subjects a modifier affects.

> **Note**: Collections are used with the `collection=` attribute in GameEffects XML or the `CollectionType` column in the DynamicModifiers database table.

## Quick Navigation

- [How Collections Work](#how-collections-work)
- [Player-Level Collections](#player-level-collections) (10 collections)
- [City-Level Collections](#city-level-collections) (4 collections)
- [Unit-Level Collections](#unit-level-collections) (5 collections)
- [All-Game Collections](#all-game-collections) (6 collections)
- [Player-Query Collections](#player-query-collections) (4 collections)
- [Religion-Based Collections](#religion-based-collections) (1 collection)
- [Story/Narrative Collections](#storynarrative-collections) (7 collections)
- [Utility/Special Collections](#utilityspecial-collections) (3 collections)

---

## How Collections Work

1. Modifier activates (owner meets OwnerRequirements)
2. Collection gathers potential subjects
3. SubjectRequirements filter subjects
4. Effect applies to remaining subjects

```
Owner → Collection → [Subjects] → SubjectRequirements → [Filtered Subjects] → Effect
```

---

## Player-Level Collections

Collections that target entities owned by or related to the modifier's owner player.

### COLLECTION_OWNER

The owner/player that the modifier is attached to. Use for effects that apply to the modifier's owner directly.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_OWNER" effect="EFFECT_PLAYER_GRANT_YIELD">
    <Argument name="YieldType">YIELD_GOLD</Argument>
    <Argument name="Amount">100</Argument>
</Modifier>
```

**Common Effects**: `EFFECT_PLAYER_GRANT_YIELD`, `EFFECT_PLAYER_ADJUST_*`, Scoring effects

### COLLECTION_PLAYER_CITIES

All cities belonging to the player.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_FOOD</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

**Common Effects**: `EFFECT_CITY_ADJUST_YIELD`, `EFFECT_CITY_GRANT_UNIT`, Production effects

### COLLECTION_PLAYER_CAPITAL_CITY

The player's capital city only.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_CAPITAL_CITY" effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_PRODUCTION</Argument>
    <Argument name="Amount">5</Argument>
</Modifier>
```

**Use When**: Effects should only apply to the capital.

### COLLECTION_PLAYER_UNITS

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

**Common Effects**: `EFFECT_UNIT_*`, `EFFECT_ARMY_*`, `EFFECT_ADJUST_UNIT_*`

### COLLECTION_PLAYER_DISTRICTS

All districts belonging to the player.

**Use When**: Effects should apply to player's districts.

### COLLECTION_PLAYER_CONSTRUCTIBLES

All constructible buildings belonging to the player.

**Use When**: Effects should apply across all player buildings/wonders.

### COLLECTION_PLAYER_PLOT_YIELDS

All plot yields within player territory. Used for plot-based yield bonuses.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_PLOT_YIELDS" effect="EFFECT_PLOT_ADJUST_YIELD">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLOT_RESOURCE_VISIBLE"/>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_GOLD</Argument>
    <Argument name="Amount">1</Argument>
</Modifier>
```

**Common Effects**: `EFFECT_PLOT_ADJUST_YIELD`

### COLLECTION_PLAYER_COMBAT

Player's combat units during combat resolution.

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_COMBAT" effect="EFFECT_ADJUST_UNIT_STRENGTH_MODIFIER">
    <Argument name="Amount">5</Argument>
</Modifier>
```

**Common Effects**: `EFFECT_ADJUST_UNIT_STRENGTH_MODIFIER`, combat-related effects

### COLLECTION_PLAYER_INFECTED_CITIES

Player cities affected by plague or infection.

**Use When**: Effects should apply only to cities with negative status effects.

### COLLECTION_PLAYER_CAPITAL_CITY_DISTRICTS

Districts within the capital city only.

**Use When**: Effects targeting specifically capital city districts.

---

## City-Level Collections

Collections that target entities within specific cities.

### COLLECTION_OWNER_CITY

The city that owns the modifier (for building/improvement modifiers).

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_OWNER_CITY" effect="EFFECT_CITY_GRANT_UNIT">
    <Argument name="UnitType">UNIT_WARRIOR</Argument>
</Modifier>
```

**Use When**: Building modifier should affect only its own city.

### COLLECTION_CITY_DISTRICTS

Districts within a specific city.

**Use When**: Modifier should affect all districts in a particular city.

### COLLECTION_CITY_PLOT_YIELDS

Plot yields within a specific city's borders.

**Use When**: Plot-based bonuses should be limited to one city's territory.

### COLLECTION_CITY_TRAINED_UNITS

Units trained by a specific city.

**Use When**: Effects should apply to units based on their training origin.

---

## Unit-Level Collections

Collections that target entities relative to specific units.

### COLLECTION_UNIT_COMBAT

Combat entities for units.

**Use When**: Combat-specific modifiers for unit engagements.

### COLLECTION_UNIT_OCCUPIED_CITY

Cities occupied by a unit.

**Use When**: Effects should apply to cities where a unit is stationed.

### COLLECTION_UNIT_OCCUPIED_DISTRICT

Districts occupied by a unit.

**Use When**: Effects tied to unit positioning in districts.

### COLLECTION_UNIT_NEAREST_OWNER_CITY

The nearest city owned by the unit's owner.

**Use When**: Effects should apply to the closest friendly city.

### COLLECTION_COMMANDER_PACKED_UNITS

Units packed within a commander unit.

**Use When**: Effects affecting units assigned to a specific commander.

---

## All-Game Collections

Collections that target all entities of a type in the game.

### COLLECTION_ALL_CITIES

All cities in the game (not just player's).

**Use When**: Effects should apply globally to every city.

### COLLECTION_ALL_CAPITAL_CITIES

All capital cities in the game.

**Use When**: Effects affecting all player capitals.

### COLLECTION_ALL_UNITS

All units in the game.

**Use When**: Global unit effects (use carefully).

### COLLECTION_ALL_DISTRICTS

All districts in the game.

**Use When**: Global district effects.

### COLLECTION_ALL_PLAYERS

All players in the game.

**Use When**: Effects applying to all civilizations.

### COLLECTION_ALL_PLOT_YIELDS

All plot yields in the game.

**Use When**: Global map-wide yield modifications.

---

## Player-Query Collections

Collections that target specific player types or query conditions.

### COLLECTION_MAJOR_PLAYERS

All major civilizations (not city-states or independents).

**Use When**: Effects should only apply to full civilizations.

### COLLECTION_INDEPENDENT_PLAYERS

Independent units/entities (not part of civilizations).

**Use When**: Effects targeting barbarians or independent factions.

### COLLECTION_ANY_CITY_AT_STORY

Cities at a specific narrative story location.

**Use When**: Narrative-driven city effects.

### COLLECTION_TEAM_PLAYERS

Players on the same diplomatic team.

**Use When**: Alliance-based effects.

---

## Religion-Based Collections

### COLLECTION_CITIES_FOLLOWING_OWNER_RELIGION

Cities that follow the owner's religion.

```xml
<Modifier id="RELIGIOUS_BONUS" collection="COLLECTION_CITIES_FOLLOWING_OWNER_RELIGION" effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_FAITH</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

**Use When**: Religious spread or belief bonuses.

---

## Story/Narrative Collections

Collections used with the narrative event system.

### COLLECTION_NARRATIVE_STORY

Entities involved in narrative stories.

**Use When**: Story-driven modifier application.

### COLLECTION_OWNER_CITY_NEAREST_STORY

Owner's nearest city to story location.

**Use When**: Story effects targeting nearby owned cities.

### COLLECTION_OWNER_UNIT_NEAREST_STORY

Owner's nearest unit to story location.

**Use When**: Story effects on nearby player units.

### COLLECTION_OWNER_COMMANDER_NEAREST_STORY

Owner's nearest commander to story location.

**Use When**: Commander-specific narrative effects.

### COLLECTION_OWNER_COMMANDER_HIGHEST_LEVEL

Owner's highest-level commander.

**Use When**: Effects targeting the player's best commander.

### COLLECTION_INDEPENDENT_NEAREST_STORY

Nearest independent faction to story location.

**Use When**: Narrative effects on independents.

### COLLECTION_HOMELANDS_INDEPENDENT_NEAREST_STORY

Nearest independent in homelands to story location.

**Use When**: Homeland-specific narrative effects.

---

## Utility/Special Collections

### COLLECTION_SINGLE_PLOT_YIELDS

Single plot yield (used for targeted yield modifiers).

**Use When**: Precise single-tile modifications.

### COLLECTION_ANY_MET

Entities that have met specified diplomatic requirements.

**Use When**: Effects based on diplomatic discovery.

---

## Collection + Effect Combinations

Not all collections work with all effects. Common valid combinations:

| Collection | Common Effects |
|------------|---------------|
| `COLLECTION_OWNER` | `EFFECT_PLAYER_GRANT_YIELD`, `EFFECT_PLAYER_ADJUST_*`, Scoring effects |
| `COLLECTION_PLAYER_CITIES` | `EFFECT_CITY_ADJUST_YIELD`, `EFFECT_CITY_GRANT_UNIT`, Production effects |
| `COLLECTION_PLAYER_CAPITAL_CITY` | `EFFECT_CITY_*` effects for capital-only bonuses |
| `COLLECTION_OWNER_CITY` | `EFFECT_CITY_*` effects for building modifiers |
| `COLLECTION_PLAYER_UNITS` | `EFFECT_UNIT_*`, `EFFECT_ARMY_*`, `EFFECT_ADJUST_UNIT_*` |
| `COLLECTION_PLAYER_COMBAT` | `EFFECT_ADJUST_UNIT_STRENGTH_MODIFIER`, combat effects |
| `COLLECTION_PLAYER_PLOT_YIELDS` | `EFFECT_PLOT_ADJUST_YIELD` |
| `COLLECTION_PLAYER_DISTRICTS` | `EFFECT_DISTRICT_*` effects |
| `COLLECTION_CITIES_FOLLOWING_OWNER_RELIGION` | `EFFECT_CITY_ADJUST_YIELD`, religious effects |

---

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

---

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

### Capital-Only Production Bonus
```xml
<Modifier id="CAPITAL_PRODUCTION_BONUS" collection="COLLECTION_PLAYER_CAPITAL_CITY" effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_PRODUCTION</Argument>
    <Argument name="Amount">3</Argument>
</Modifier>
```

---

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

---

## Complete Collection Type List

For quick reference, here are all 40 collection types:

**Player-Level (10):**
1. `COLLECTION_OWNER`
2. `COLLECTION_PLAYER_CITIES`
3. `COLLECTION_PLAYER_CAPITAL_CITY`
4. `COLLECTION_PLAYER_UNITS`
5. `COLLECTION_PLAYER_DISTRICTS`
6. `COLLECTION_PLAYER_CONSTRUCTIBLES`
7. `COLLECTION_PLAYER_PLOT_YIELDS`
8. `COLLECTION_PLAYER_COMBAT`
9. `COLLECTION_PLAYER_INFECTED_CITIES`
10. `COLLECTION_PLAYER_CAPITAL_CITY_DISTRICTS`

**City-Level (4):**
11. `COLLECTION_OWNER_CITY`
12. `COLLECTION_CITY_DISTRICTS`
13. `COLLECTION_CITY_PLOT_YIELDS`
14. `COLLECTION_CITY_TRAINED_UNITS`

**Unit-Level (5):**
15. `COLLECTION_UNIT_COMBAT`
16. `COLLECTION_UNIT_OCCUPIED_CITY`
17. `COLLECTION_UNIT_OCCUPIED_DISTRICT`
18. `COLLECTION_UNIT_NEAREST_OWNER_CITY`
19. `COLLECTION_COMMANDER_PACKED_UNITS`

**All-Game (6):**
20. `COLLECTION_ALL_CITIES`
21. `COLLECTION_ALL_CAPITAL_CITIES`
22. `COLLECTION_ALL_UNITS`
23. `COLLECTION_ALL_DISTRICTS`
24. `COLLECTION_ALL_PLAYERS`
25. `COLLECTION_ALL_PLOT_YIELDS`

**Player-Query (4):**
26. `COLLECTION_MAJOR_PLAYERS`
27. `COLLECTION_INDEPENDENT_PLAYERS`
28. `COLLECTION_ANY_CITY_AT_STORY`
29. `COLLECTION_TEAM_PLAYERS`

**Religion-Based (1):**
30. `COLLECTION_CITIES_FOLLOWING_OWNER_RELIGION`

**Story/Narrative (7):**
31. `COLLECTION_NARRATIVE_STORY`
32. `COLLECTION_OWNER_CITY_NEAREST_STORY`
33. `COLLECTION_OWNER_UNIT_NEAREST_STORY`
34. `COLLECTION_OWNER_COMMANDER_NEAREST_STORY`
35. `COLLECTION_OWNER_COMMANDER_HIGHEST_LEVEL`
36. `COLLECTION_INDEPENDENT_NEAREST_STORY`
37. `COLLECTION_HOMELANDS_INDEPENDENT_NEAREST_STORY`

**Utility/Special (3):**
38. `COLLECTION_SINGLE_PLOT_YIELDS`
39. `COLLECTION_ANY_MET`
40. (Reserved for future use)

---

## See Also

- [Effect Types](Effect-Types.md) - What effects can be applied
- [Requirement Types](Requirement-Types.md) - Conditional requirements
- [Modifier System Overview](Overview.md) - How modifiers work
- [Cross-Reference Index](../Index.md) - Quick lookup for all types
