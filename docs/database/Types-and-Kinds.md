# Types and Kinds System

In Civilization VII, all game entities are organized using a **Type/Kind** system. This is the foundation of the database structure.

This document lists all **108 unique KIND values** found in the game files.

## Overview

- **Kind**: A category or class of game entity (e.g., `KIND_UNIT`, `KIND_CONSTRUCTIBLE`)
- **Type**: A specific instance within a Kind (e.g., `UNIT_SCOUT`, `BUILDING_GRANARY`)

Every Type must be associated with a Kind in the `Types` table.

## Quick Navigation

- [Core Gameplay Kinds](#core-gameplay-kinds) (18 kinds)
- [Progression & Advancement Kinds](#progression--advancement-kinds) (14 kinds)
- [Governance & Traditions Kinds](#governance--traditions-kinds) (6 kinds)
- [Diplomacy & Relations Kinds](#diplomacy--relations-kinds) (23 kinds)
- [Map & Terrain Kinds](#map--terrain-kinds) (12 kinds)
- [Configuration & Settings Kinds](#configuration--settings-kinds) (15 kinds)
- [Narrative & Events Kinds](#narrative--events-kinds) (10 kinds)
- [Commands & Operations Kinds](#commands--operations-kinds) (4 kinds)
- [System & Utility Kinds](#system--utility-kinds) (6 kinds)

---

## Core Gameplay Kinds

These are the fundamental entity types that make up the game.

### Units & Military

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_UNIT` | Military and civilian units | `UNIT_SCOUT`, `UNIT_SETTLER`, `UNIT_WARRIOR`, `UNIT_TRADER` |
| `KIND_ABILITY` | Unit abilities and special powers | `ABILITY_AMPHIBIOUS`, `ABILITY_IGNORE_ZOC` |
| `KIND_WMD` | Weapons of mass destruction | Nuclear weapons |
| `KIND_PROMOTION` | Unit promotions and upgrades | Combat promotions |
| `KIND_PROMOTION_CLASS` | Categories of promotions | Promotion class types |
| `KIND_PROMOITON_DISCIPLINE_CLASS` | Promotion discipline categories | Discipline trees |
| `KIND_EMBARKATION` | Unit embarkation types | Embark modes |

### Buildings & Infrastructure

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_CONSTRUCTIBLE` | Buildings, improvements, wonders | `BUILDING_PALACE`, `IMPROVEMENT_FARM`, `IMPROVEMENT_MINE` |
| `KIND_DISTRICT` | City districts | `DISTRICT_CITY_CENTER`, `DISTRICT_URBAN`, `DISTRICT_RURAL` |
| `KIND_QUARTER` | City quarters/residential areas | Quarter types |
| `KIND_IMPROVEMENT` | Land improvements | Farm, mine, quarry |

### Civilizations & Leaders

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_CIVILIZATION` | Playable civilizations | `CIVILIZATION_EGYPT`, `CIVILIZATION_ROME`, `CIVILIZATION_HAN` |
| `KIND_LEADER` | Historical leaders | `LEADER_AUGUSTUS`, `LEADER_ASHOKA`, `LEADER_AMINA` |
| `KIND_TRAIT` | Leader and civilization traits | `TRAIT_EGYPT`, `TRAIT_GREECE`, `TRAIT_AKSUM` |

### Terrain & Resources

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_TERRAIN` | Terrain types | Grassland, hills, mountains, water |
| `KIND_BIOME` | Biome classifications | Desert, tundra, tropical |
| `KIND_FEATURE` | Natural features | Forests, rivers, natural wonders |
| `KIND_FEATURE_CLASS` | Feature categories | Feature classification |
| `KIND_RESOURCE` | Harvestable resources | `RESOURCE_FURS`, `RESOURCE_IVORY` |
| `KIND_RESOURCECLASS` | Resource categories | Strategic, luxury, bonus |
| `KIND_YIELD` | Core yields | `YIELD_FOOD`, `YIELD_PRODUCTION`, `YIELD_GOLD`, `YIELD_SCIENCE`, `YIELD_CULTURE` |
| `KIND_PSEUDOYIELD` | Special yield types | Derived yields |

---

## Progression & Advancement Kinds

Systems for advancing through the game.

### Technology & Culture

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_PROJECT` | Special projects and objectives | Projects |
| `KIND_TRADITION` | Cultural traditions with modifiers | `TRADITION_*` policies |
| `KIND_TREE` | Progression trees | Tech tree, culture tree |
| `KIND_TREE_NODE` | Individual nodes in progression trees | Individual techs/civics |
| `KIND_UNLOCK` | Unlock definitions and requirements | Unlock entries |
| `KIND_LEGACY_PATH` | Legacy civilization progression paths | Legacy paths |

### Ages & Time

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_AGE` | Game ages | `ANTIQUITY`, `EXPLORATION`, `MODERN` |
| `KIND_AGE_PROGRESSION_MILESTONE` | Milestone definitions for age progression | Milestones |
| `KIND_GAMESPEED` | Game speed settings | Online, Standard, Quick |
| `KIND_GAMESPEED_SCALING` | Game speed scaling parameters | Scaling values |
| `KIND_CALENDAR` | Calendar systems | Calendars |
| `KIND_MONTH` | Calendar months | Months |
| `KIND_SEASON` | Seasonal definitions | Seasons |
| `KIND_TURNMODE` | Turn mode definitions | Turn modes |
| `KIND_TURNPHASE` | Turn phases | Phases |
| `KIND_TURNSEGMENT` | Turn segments | Segments |

---

## Governance & Traditions Kinds

Government and policy systems.

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_GOVERNMENT` | Government types | Government forms |
| `KIND_BELIEF` | Religious beliefs | Belief types |
| `KIND_BELIEF_CLASS` | Belief categories | Belief classes |
| `KIND_RELIGION` | Religions | `RELIGION_CHRISTIANITY`, `RELIGION_ISLAM` |
| `KIND_GREATWORK` | Great works | `GREATWORK_WRITING`, `GREATWORK_SCULPTURE` |
| `KIND_GREATWORKOBJECT` | Great work objects | Great work items |
| `KIND_GREATWORKSOURCE` | Great work sources | Sources |

---

## Diplomacy & Relations Kinds

Diplomatic and relationship systems.

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_DIPLOMACY_ACTION` | Diplomatic actions | Action types |
| `KIND_DIPLOMACY_ACTION_GROUP` | Diplomacy action groupings | Groups |
| `KIND_DIPLOMACY_ACTION_GROUP_SUBTYPE` | Diplomacy action subtypes | Subtypes |
| `KIND_DIPLOMACY_ACTION_STAGE` | Diplomacy stages | Stages |
| `KIND_DIPLOMACY_ACTION_TARGET` | Diplomacy targets | Targets |
| `KIND_DIPLOMACY_ACTION_RELATIONSHIP` | Diplomacy relationships | Relationships |
| `KIND_DIPLOMACY_ACTION_TAG_TYPE` | Diplomacy tag types | Tags |
| `KIND_DIPLOMACY_STATEMENT` | Diplomacy statements | Statement types |
| `KIND_DIPLOMACY_TOKEN` | Diplomacy tokens | Tokens |
| `KIND_DIPLOMACY_RESPONSE` | Diplomatic responses | Response types |
| `KIND_DIPLOMACY_FIRST_MEET_RESPONSE` | First contact responses | First meet types |
| `KIND_DIPLOMACY_MODIFIER_TYPE` | Diplomacy modifier types | Modifier types |
| `KIND_DIPLOMACY_MODIFIER_TARGET` | Diplomacy modifier targets | Targets |
| `KIND_DIPLOMACY_FAVOR_GRIEVANCE_EVENT` | Favor/grievance events | Events |
| `KIND_DIPLOMACY_FGE_TYPE` | Favor/grievance event types | FGE types |
| `KIND_DIPLOMACY_AGENDA_AMOUNT_TYPE` | Agenda amount types | Amount types |
| `KIND_DIPLOMACY_AGENDA_AWARD_TO_TYPE` | Agenda award target types | Award types |
| `KIND_DIPLOMACY_AGENDA_WEIGHTING_TYPE` | Agenda weighting types | Weighting types |
| `KIND_DIPLOMACY_BASIC_MODIFIER_VALUE_TYPE` | Basic modifier value types | Value types |
| `KIND_DEAL_ITEM` | Trade deal items | Deal items |
| `KIND_DEAL_ITEM_AGREEMENT` | Deal item agreements | Agreements |
| `KIND_GREAT_PERSON_CLASS` | Great person classes | Great Scientist, Great Artist, etc. |
| `KIND_GREAT_PERSON_INDIVIDUAL` | Individual great persons | Specific great people |

---

## Map & Terrain Kinds

Map generation and terrain systems.

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_CONTINENT` | Continental areas | Continents |
| `KIND_MAPSIZE` | Map size configurations | Small, Standard, Large, Huge |
| `KIND_PLOTEFFECT` | Plot/tile effects | Plot effects |
| `KIND_ROUTE` | Trade route types | Route types |
| `KIND_TRADE_SYSTEM_PARAMSET` | Trade system parameters | Parameter sets |
| `KIND_FERTILITY` | Tile fertility | Fertility values |
| `KIND_PLUNDER` | Plunder definitions | Plunder types |
| `KIND_INDEPENDENT` | Independent city-states | City-state types |
| `KIND_CITY_STATE_BONUS` | City-state bonuses | Bonus types |
| `KIND_BARBARIAN_TRIBE` | Barbarian tribe types | Tribe types |
| `KIND_GOODY_HUT` | Goody hut events | Discovery types |
| `KIND_GOLDEN_AGE` | Golden age types | Golden age forms |

---

## Configuration & Settings Kinds

Game configuration and settings.

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_CAPABILITY` | Game capabilities | Capability flags |
| `KIND_GAME_SYSTEM_PREREQ` | Game system prerequisites | Prerequisites |
| `KIND_REALISM_SETTING` | Realism game settings | Realism levels |
| `KIND_DIFFICULTY` | Game difficulty levels | Easy, Normal, Hard |
| `KIND_GAMEMODE` | Game modes | Mode types |
| `KIND_WAR` | War definitions | War types |
| `KIND_VICTORY` | Victory conditions | `SCIENCE`, `CULTURE`, `DOMINATION` |
| `KIND_VICTORY_STRATEGY` | Victory strategy types | Strategy types |
| `KIND_DEFEAT` | Defeat conditions | Defeat types |
| `KIND_COMBAT_UNIT_ABILITY` | Combat abilities | Combat ability types |
| `KIND_MEMENTOS` | Memento items | Memento types |
| `KIND_NOTIFICATION` | Notification types | Notification categories |
| `KIND_KEYWORD_ABILITY` | Keyword abilities for UI | Keyword definitions |
| `KIND_ADVISOR` | Advisor types | Advisor categories |
| `KIND_ADVISOR_WARNING` | Advisor warning types | Warning types |

---

## Narrative & Events Kinds

Story and event systems.

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_NARRATIVE_STORY` | Story events and narrative sequences | Story types |
| `KIND_NARRATIVE_TAG` | Story/narrative tags | Narrative tags |
| `KIND_RANDOM_EVENT` | Random events | Random event types |
| `KIND_RANDOM_EVENT_CLASS` | Random event categories | Event classes |
| `KIND_DISCOVERY_STORY` | Discovery narrative stories | Discovery stories |
| `KIND_ADVISORY_SUBJECT` | Advisory subjects | Subject types |
| `KIND_ADVISORY_CLASS` | Advisory classifications | Advisory classes |
| `KIND_GOSSIP` | Gossip content | Gossip types |
| `KIND_MODIFIER` | Game effects/modifiers | `MODIFIER_PLAYER_ADJUST_YIELD`, etc. |
| `KIND_AFFINITY` | Personality affinities | Affinity types |

---

## Commands & Operations Kinds

UI commands and unit operations.

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_CITYCOMMAND` | City commands | Build, Train, etc. |
| `KIND_UNITCOMMAND` | Unit commands | Move, Attack, Fortify |
| `KIND_UNITOPERATION` | Unit operations | Operation types |
| `KIND_COMBAT_UNIT_ABILITY` | Combat abilities | Combat ability types |

---

## System & Utility Kinds

Internal system types.

| Kind | Description | Example Types |
|------|-------------|---------------|
| `KIND_UNLOCK` | Unlock types | Unlock entries |
| `KIND_PROPERTY` | Property definitions | Property types |
| `KIND_TAG` | Tag definitions | Tag types |
| `KIND_REQUIREMENT` | Requirement definitions | Requirement types |
| `KIND_COLLECTION` | Collection definitions | Collection types |
| `KIND_EFFECT` | Effect definitions | Effect types |

---

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

---

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
| `KIND_MODIFIER` | `MODIFIER_` | `MODIFIER_PLAYER_ADJUST_YIELD` |
| `KIND_DISTRICT` | `DISTRICT_` | `DISTRICT_CITY_CENTER` |
| `KIND_RESOURCE` | `RESOURCE_` | `RESOURCE_IRON` |
| `KIND_RELIGION` | `RELIGION_` | `RELIGION_CHRISTIANITY` |

---

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

---

## Querying Types

To find all types of a specific kind:

```sql
SELECT Type FROM Types WHERE Kind = 'KIND_UNIT';
```

To find all tags for a type:

```sql
SELECT Tag FROM TypeTags WHERE Type = 'UNIT_SCOUT';
```

To find all kinds available:

```sql
SELECT DISTINCT Kind FROM Types ORDER BY Kind;
```

---

## Complete Kind List (108 Total)

For quick reference, here is the complete alphabetical list:

```
KIND_ABILITY
KIND_ADVISOR
KIND_ADVISOR_WARNING
KIND_ADVISORY_CLASS
KIND_ADVISORY_SUBJECT
KIND_AFFINITY
KIND_AGE
KIND_AGE_PROGRESSION_MILESTONE
KIND_BARBARIAN_TRIBE
KIND_BELIEF
KIND_BELIEF_CLASS
KIND_CALENDAR
KIND_CAPABILITY
KIND_CITY_STATE_BONUS
KIND_CIVILIZATION
KIND_COMBAT_UNIT_ABILITY
KIND_CONSTRUCTIBLE
KIND_CONTINENT
KIND_DEAL_ITEM
KIND_DEAL_ITEM_AGREEMENT
KIND_DEFEAT
KIND_DIFFICULTY
KIND_DIPLOMACY_ACTION
KIND_DIPLOMACY_ACTION_GROUP
KIND_DIPLOMACY_ACTION_GROUP_SUBTYPE
KIND_DIPLOMACY_ACTION_RELATIONSHIP
KIND_DIPLOMACY_ACTION_STAGE
KIND_DIPLOMACY_ACTION_TAG_TYPE
KIND_DIPLOMACY_ACTION_TARGET
KIND_DIPLOMACY_AGENDA_AMOUNT_TYPE
KIND_DIPLOMACY_AGENDA_AWARD_TO_TYPE
KIND_DIPLOMACY_AGENDA_WEIGHTING_TYPE
KIND_DIPLOMACY_BASIC_MODIFIER_VALUE_TYPE
KIND_DIPLOMACY_FAVOR_GRIEVANCE_EVENT
KIND_DIPLOMACY_FGE_TYPE
KIND_DIPLOMACY_FIRST_MEET_RESPONSE
KIND_DIPLOMACY_MODIFIER_TARGET
KIND_DIPLOMACY_MODIFIER_TYPE
KIND_DIPLOMACY_RESPONSE
KIND_DIPLOMACY_STATEMENT
KIND_DIPLOMACY_TOKEN
KIND_DISCOVERY_STORY
KIND_DISTRICT
KIND_EMBARKATION
KIND_FEATURE
KIND_FEATURE_CLASS
KIND_FERTILITY
KIND_GAME_SYSTEM_PREREQ
KIND_GAMEMODE
KIND_GAMESPEED
KIND_GAMESPEED_SCALING
KIND_GOLDEN_AGE
KIND_GOODY_HUT
KIND_GOSSIP
KIND_GOVERNMENT
KIND_GREAT_PERSON_CLASS
KIND_GREAT_PERSON_INDIVIDUAL
KIND_GREATWORK
KIND_GREATWORKOBJECT
KIND_GREATWORKSOURCE
KIND_IMPROVEMENT
KIND_INDEPENDENT
KIND_KEYWORD_ABILITY
KIND_LEADER
KIND_LEGACY_PATH
KIND_MAPSIZE
KIND_MEMENTOS
KIND_MODIFIER
KIND_MONTH
KIND_NARRATIVE_STORY
KIND_NARRATIVE_TAG
KIND_NOTIFICATION
KIND_PLOTEFFECT
KIND_PLUNDER
KIND_PROJECT
KIND_PROMOTION
KIND_PROMOTION_CLASS
KIND_PROMOITON_DISCIPLINE_CLASS
KIND_PSEUDOYIELD
KIND_QUARTER
KIND_RANDOM_EVENT
KIND_RANDOM_EVENT_CLASS
KIND_REALISM_SETTING
KIND_RELIGION
KIND_RESOURCE
KIND_RESOURCECLASS
KIND_ROUTE
KIND_SEASON
KIND_TERRAIN
KIND_TRADE_SYSTEM_PARAMSET
KIND_TRADITION
KIND_TRAIT
KIND_TREE
KIND_TREE_NODE
KIND_TURNMODE
KIND_TURNPHASE
KIND_TURNSEGMENT
KIND_UNIT
KIND_UNITCOMMAND
KIND_UNITOPERATION
KIND_UNLOCK
KIND_VICTORY
KIND_VICTORY_STRATEGY
KIND_WAR
KIND_WMD
KIND_YIELD
```

---

## See Also

- [Effect Types](../modifiers/Effect-Types.md) - Modifier effects
- [Collection Types](../modifiers/Collection-Types.md) - Subject collections
- [Requirement Types](../modifiers/Requirement-Types.md) - Conditional requirements
- [Cross-Reference Index](../Index.md) - Quick lookup for all types
