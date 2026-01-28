# Types and Kinds System

In Civilization VII, all game entities are organized using a **Type/Kind** system. This is the foundation of the database structure.

## Overview

- **Kind**: A category or class of game entity (e.g., `KIND_UNIT`, `KIND_CONSTRUCTIBLE`)
- **Type**: A specific instance within a Kind (e.g., `UNIT_SCOUT`, `BUILDING_GRANARY`)

Every Type must be associated with a Kind in the `Types` table.

## Available Kinds

### Core Kinds (from kinds.xml)

| Kind | Description |
|------|-------------|
| `KIND_AGE` | Game ages (Antiquity, Exploration, Modern) |
| `KIND_BELIEF` | Religious beliefs |
| `KIND_BELIEF_CLASS` | Belief categories |
| `KIND_CAPABILITY` | Game capabilities |
| `KIND_CITY_STATE_BONUS` | City-state bonuses |
| `KIND_CONSTRUCTIBLE` | Buildings, improvements, wonders |
| `KIND_GAME_SYSTEM_PREREQ` | Game system prerequisites |
| `KIND_GOLDEN_AGE` | Golden age types |
| `KIND_GOODY_HUT` | Discovery/goody hut types |
| `KIND_GREATWORK` | Great work types |
| `KIND_GREATWORKOBJECT` | Great work objects |
| `KIND_GREATWORKSOURCE` | Great work sources |
| `KIND_INDEPENDENT` | Independent peoples |
| `KIND_LEGACY_PATH` | Victory legacy paths |
| `KIND_PROJECT` | Project types |
| `KIND_RELIGION` | Religion types |
| `KIND_ROUTE` | Trade route types |
| `KIND_TRADE_SYSTEM_PARAMSET` | Trade system parameters |
| `KIND_TRADITION` | Traditions and policies |
| `KIND_TREE_NODE` | Progression tree nodes |
| `KIND_TREE` | Progression trees |
| `KIND_UNLOCK` | Unlock types |

### Unit-Related Kinds (from units.xml)

| Kind | Description |
|------|-------------|
| `KIND_UNIT` | Unit types |
| `KIND_ABILITY` | Unit abilities |
| `KIND_GREAT_PERSON_CLASS` | Great person classes |
| `KIND_GREAT_PERSON_INDIVIDUAL` | Individual great people |
| `KIND_WMD` | Weapons of mass destruction |

### Other Kinds

| Kind | Description |
|------|-------------|
| `KIND_CIVILIZATION` | Civilization types |
| `KIND_LEADER` | Leader types |
| `KIND_TRAIT` | Traits for civs/leaders |
| `KIND_YIELD` | Yield types (Food, Production, etc.) |
| `KIND_PLUNDER` | Plunder types |
| `KIND_KEYWORD_ABILITY` | Keyword abilities |
| `KIND_QUARTER` | City quarter types |
| `KIND_MODIFIER` | Modifier types |
| `KIND_NARRATIVE_STORY` | Narrative event types |

## Creating New Types

To add a new game entity, you must first register it in the `Types` table:

### XML Example

```xml
<Database>
    <Types>
        <Row Type="UNIT_MY_CUSTOM_UNIT" Kind="KIND_UNIT"/>
        <Row Type="BUILDING_MY_CUSTOM_BUILDING" Kind="KIND_CONSTRUCTIBLE"/>
        <Row Type="TRAIT_MY_CUSTOM_TRAIT" Kind="KIND_TRAIT"/>
    </Types>
</Database>
```

### SQL Example

```sql
INSERT INTO Types (Type, Kind)
VALUES
    ('UNIT_MY_CUSTOM_UNIT', 'KIND_UNIT'),
    ('BUILDING_MY_CUSTOM_BUILDING', 'KIND_CONSTRUCTIBLE'),
    ('TRAIT_MY_CUSTOM_TRAIT', 'KIND_TRAIT');
```

## Type Naming Conventions

Types follow a naming convention based on their Kind:

| Kind | Prefix | Example |
|------|--------|---------|
| `KIND_UNIT` | `UNIT_` | `UNIT_WARRIOR` |
| `KIND_CONSTRUCTIBLE` | `BUILDING_`, `IMPROVEMENT_`, `WONDER_` | `BUILDING_GRANARY` |
| `KIND_CIVILIZATION` | `CIVILIZATION_` | `CIVILIZATION_ROME` |
| `KIND_LEADER` | `LEADER_` | `LEADER_AUGUSTUS` |
| `KIND_TRAIT` | `TRAIT_` | `TRAIT_ROME` |
| `KIND_ABILITY` | `ABILITY_` | `ABILITY_AMPHIBIOUS` |
| `KIND_TRADITION` | `TRADITION_` | `TRADITION_CRAFTSMANSHIP` |
| `KIND_YIELD` | `YIELD_` | `YIELD_FOOD` |

## Type Tags

Types can be categorized using the `TypeTags` table:

```xml
<TypeTags>
    <Row Type="UNIT_SCOUT" Tag="UNIT_CLASS_RECON"/>
    <Row Type="UNIT_SCOUT" Tag="UNIT_CLASS_NON_COMBAT"/>
    <Row Type="BUILDING_GRANARY" Tag="FOOD"/>
</TypeTags>
```

### Common Unit Class Tags

| Tag | Description |
|-----|-------------|
| `UNIT_CLASS_COMBAT` | Combat units |
| `UNIT_CLASS_NON_COMBAT` | Non-combat units |
| `UNIT_CLASS_RECON` | Reconnaissance units |
| `UNIT_CLASS_MELEE` | Melee combat |
| `UNIT_CLASS_RANGED` | Ranged combat |
| `UNIT_CLASS_CAVALRY` | Cavalry units |
| `UNIT_CLASS_SIEGE` | Siege units |
| `UNIT_CLASS_NAVAL` | Naval units |
| `UNIT_CLASS_AIRCRAFT` | Air units |
| `UNIT_CLASS_COMMAND` | Commander units |

### Common Constructible Tags

| Tag | Description |
|-----|-------------|
| `FOOD` | Food-related |
| `PRODUCTION` | Production-related |
| `GOLD` | Gold/economic |
| `SCIENCE` | Science-related |
| `CULTURE` | Culture-related |
| `MILITARY` | Military structures |
| `RELIGIOUS` | Religious buildings |
| `UNIQUE` | Unique to a civilization |
| `AGELESS` | Persists across ages |

## Querying Types

To find all types of a specific kind:

```sql
SELECT Type FROM Types WHERE Kind = 'KIND_UNIT';
```

To find all tags for a type:

```sql
SELECT Tag FROM TypeTags WHERE Type = 'UNIT_SCOUT';
```
