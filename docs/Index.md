# Cross-Reference Index

This comprehensive index provides quick lookup for all documented systems, tables, types, and concepts across the Civilization VII Modding API documentation.

## Documentation Statistics

| Category | Count | Documentation |
|----------|-------|---------------|
| **Kind Types** | 108 | [Types and Kinds](database/Types-and-Kinds.md) |
| **Effect Types** | 376+ | [Effect Types](modifiers/Effect-Types.md) |
| **Collection Types** | 40 | [Collection Types](modifiers/Collection-Types.md) |
| **Requirement Types** | 280+ | [Requirement Types](modifiers/Requirement-Types.md) |
| **Database Tables** | 673 | [Tables Reference](database/Tables-Reference.md) |
| **JavaScript Events** | 26+ | [Events Reference](javascript/Events.md) |

## Quick Navigation

- [By Topic](#by-topic)
- [Database Tables](#database-tables)
- [Type Kinds](#type-kinds)
- [Effect Types](#effect-types)
- [Collection Types](#collection-types)
- [Requirement Types](#requirement-types)
- [Common Modding Tasks](#common-modding-tasks)

---

## By Topic

### Civilizations & Leaders
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Civilization definition | [Civilizations](database/Civilizations.md) | [Types](database/Types-and-Kinds.md) |
| Leader definition | [Leaders](database/Leaders.md) | [Civilizations](database/Civilizations.md) |
| Unique abilities | [Civilizations](database/Civilizations.md), [Leaders](database/Leaders.md) | [Modifiers](modifiers/Overview.md) |
| Starting bias | [Starting Bias](civilization-creation/Starting-Bias.md) | [Map-Terrain](database/Map-Terrain.md) |
| AI behavior/agendas | [AI Behavior](civilization-creation/AI-Behavior.md) | [Leaders](database/Leaders.md) |
| Civilization colors | [Art Assets](technical-reference/Art-Assets.md) | [Civilizations](database/Civilizations.md) |

### Units & Combat
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Unit creation | [Units](database/Units.md) | [Types](database/Types-and-Kinds.md) |
| Unit abilities | [Units](database/Units.md) | [Modifiers](modifiers/Overview.md) |
| Promotions | [Promotions](units/Promotions.md) | [Units](database/Units.md) |
| Great People | [Great People](units/Great-People.md) | [Units](database/Units.md) |
| Combat stats | [Units](database/Units.md) | [Promotions](units/Promotions.md) |
| Spy units | [Espionage](espionage/Overview.md) | [Units](database/Units.md) |

### Buildings & Districts
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Building creation | [Buildings](database/Buildings.md) | [Types](database/Types-and-Kinds.md) |
| Building yields | [Buildings](database/Buildings.md) | [Modifiers](modifiers/Overview.md) |
| District placement | [Buildings](database/Buildings.md) | [Map-Terrain](database/Map-Terrain.md) |
| Wonders | [Buildings](database/Buildings.md) | [Victory](victory-conditions/Overview.md) |

### Technology & Civics
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Technology tree | [Technology Trees](technology-civics/Technology-Trees.md) | [Types](database/Types-and-Kinds.md) |
| Adding technologies | [Technology Trees](technology-civics/Technology-Trees.md) | [Modifiers](modifiers/Overview.md) |
| Civic tree | [Civic Trees](technology-civics/Civic-Trees.md) | [Types](database/Types-and-Kinds.md) |
| Traditions (policies) | [Civic Trees](technology-civics/Civic-Trees.md) | [Modifiers](modifiers/Overview.md) |
| Governments | [Governments](technology-civics/Governments.md) | [Golden Ages](mechanics/Era-Score.md) |
| Tech boosts | [Technology Trees](technology-civics/Technology-Trees.md) | [Modifiers](modifiers/Overview.md) |

### Victory & Legacy
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Victory conditions | [Victory Conditions](victory-conditions/Overview.md) | [Legacy Paths](legacy-paths/Overview.md) |
| Legacy paths | [Legacy Paths](legacy-paths/Overview.md) | [Victory](victory-conditions/Overview.md) |
| Age transitions | [Legacy Paths](legacy-paths/Overview.md) | [Era Score](mechanics/Era-Score.md) |
| Legacy points | [Era Score](mechanics/Era-Score.md) | [Legacy Paths](legacy-paths/Overview.md) |
| Golden ages | [Era Score](mechanics/Era-Score.md) | [Governments](technology-civics/Governments.md) |
| Dark ages | [Era Score](mechanics/Era-Score.md) | [Legacy Paths](legacy-paths/Overview.md) |

### Maps & Resources
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Map scripts | [Maps & Resources](maps-resources/Overview.md) | [JavaScript](javascript/Overview.md) |
| Terrain types | [Map-Terrain](database/Map-Terrain.md) | [Starting Bias](civilization-creation/Starting-Bias.md) |
| Features | [Map-Terrain](database/Map-Terrain.md) | [Maps](maps-resources/Overview.md) |
| Natural wonders | [Map-Terrain](database/Map-Terrain.md) | [Maps](maps-resources/Overview.md) |
| Resources | [Maps & Resources](maps-resources/Overview.md) | [Modifiers](modifiers/Overview.md) |
| Climate/biomes | [Maps & Resources](maps-resources/Overview.md) | [Map-Terrain](database/Map-Terrain.md) |

### Religion
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Religion system | [Religion](religion/Overview.md) | [Modifiers](modifiers/Overview.md) |
| Beliefs | [Religion](religion/Overview.md) | [Effect Types](modifiers/Effect-Types.md) |
| Pantheons | [Religion](religion/Overview.md) | [Buildings](database/Buildings.md) |

### Diplomacy & Espionage
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Diplomacy actions | [Diplomacy](diplomacy/Overview.md) | [Modifiers](modifiers/Overview.md) |
| Relationships | [Diplomacy](diplomacy/Overview.md) | [AI Behavior](civilization-creation/AI-Behavior.md) |
| Favor/grievances | [Diplomacy](diplomacy/Overview.md) | [Effect Types](modifiers/Effect-Types.md) |
| Espionage operations | [Espionage](espionage/Overview.md) | [Diplomacy](diplomacy/Overview.md) |
| Spy promotions | [Espionage](espionage/Overview.md) | [Promotions](units/Promotions.md) |

### Independents
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| City-states | [Independents](independents/Overview.md) | [Diplomacy](diplomacy/Overview.md) |
| Tribes | [Independents](independents/Overview.md) | [Units](database/Units.md) |
| Barbarians | [Independents](independents/Overview.md) | [Units](database/Units.md) |

### Events & Narrative
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Narrative events | [Narrative](narrative/Overview.md) | [JavaScript](javascript/Overview.md) |
| Event triggers | [Narrative](narrative/Overview.md) | [Requirements](modifiers/Requirement-Types.md) |
| Event choices | [Narrative](narrative/Overview.md) | [Modifiers](modifiers/Overview.md) |
| JavaScript events | [Events Reference](javascript/Events.md) | [JavaScript](javascript/Overview.md) |

### Technical
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Modifier system | [Modifiers Overview](modifiers/Overview.md) | All gameplay docs |
| Effect types | [Effect Types](modifiers/Effect-Types.md) | [Modifiers](modifiers/Overview.md) |
| Collections | [Collection Types](modifiers/Collection-Types.md) | [Modifiers](modifiers/Overview.md) |
| Requirements | [Requirement Types](modifiers/Requirement-Types.md) | [Modifiers](modifiers/Overview.md) |
| JavaScript API | [JavaScript](javascript/Overview.md) | [Narrative](narrative/Overview.md) |
| JavaScript Events | [Events Reference](javascript/Events.md) | [JavaScript](javascript/Overview.md) |
| Localization | [Localization](technical-reference/Localization.md) | All content docs |
| Art assets | [Art Assets](technical-reference/Art-Assets.md) | [Units](database/Units.md), [Buildings](database/Buildings.md) |
| ModInfo | [ModInfo Files](Modinfo-Files.md) | [Getting Started](Getting-Started.md) |
| Types & Kinds | [Types and Kinds](database/Types-and-Kinds.md) | All database docs |
| Database Tables | [Tables Reference](database/Tables-Reference.md) | All database docs |

---

## Database Tables

For the complete list of 673 database tables, see [Tables Reference](database/Tables-Reference.md).

### Core Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Types` | Register all game entities | [Types and Kinds](database/Types-and-Kinds.md) |
| `Kinds` | Define entity categories | [Types and Kinds](database/Types-and-Kinds.md) |
| `Tags` | Tag definitions | [Types and Kinds](database/Types-and-Kinds.md) |
| `TypeTags` | Type-tag associations | [Types and Kinds](database/Types-and-Kinds.md) |

### Civilization Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Civilizations` | Define civilizations | [Civilizations](database/Civilizations.md) |
| `CivilizationTraits` | Link civs to abilities | [Civilizations](database/Civilizations.md) |
| `Leaders` | Define leaders | [Leaders](database/Leaders.md) |
| `LeaderTraits` | Link leaders to abilities | [Leaders](database/Leaders.md) |
| `LeaderCivPairings` | Link leaders to civs | [Leaders](database/Leaders.md) |
| `StartBiases` | Terrain preferences | [Starting Bias](civilization-creation/Starting-Bias.md) |

### Unit Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Units` | Define units | [Units](database/Units.md) |
| `Unit_Stats` | Unit statistics | [Units](database/Units.md) |
| `Unit_Abilities` | Link abilities | [Units](database/Units.md) |
| `UnitPromotions` | Define promotions | [Promotions](units/Promotions.md) |
| `UnitPromotionModifiers` | Promotion effects | [Promotions](units/Promotions.md) |
| `GreatPersonClasses` | Great person types | [Great People](units/Great-People.md) |
| `GreatPersonIndividuals` | Specific great people | [Great People](units/Great-People.md) |

### Building Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Buildings` | Define buildings | [Buildings](database/Buildings.md) |
| `Constructibles` | Generic constructibles | [Buildings](database/Buildings.md) |
| `Constructible_YieldChanges` | Building yields | [Buildings](database/Buildings.md) |
| `Districts` | Define districts | [Buildings](database/Buildings.md) |

### Modifier Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Modifiers` | Modifier definitions | [Modifiers](modifiers/Overview.md) |
| `ModifierArguments` | Modifier parameters | [Modifiers](modifiers/Overview.md) |
| `DynamicModifiers` | Effect/collection mapping | [Modifiers](modifiers/Overview.md) |
| `Requirements` | Condition definitions | [Requirements](modifiers/Requirement-Types.md) |
| `RequirementSets` | Grouped requirements | [Requirements](modifiers/Requirement-Types.md) |
| `TraitModifiers` | Trait effects | [Modifiers](modifiers/Overview.md) |

---

## Type Kinds

For the complete list of 108 KIND values, see [Types and Kinds](database/Types-and-Kinds.md).

### Core Gameplay Kinds (18)
| Kind | Usage | Documentation |
|------|-------|---------------|
| `KIND_UNIT` | Units | [Units](database/Units.md) |
| `KIND_CONSTRUCTIBLE` | Buildings/wonders | [Buildings](database/Buildings.md) |
| `KIND_DISTRICT` | Districts | [Buildings](database/Buildings.md) |
| `KIND_CIVILIZATION` | Civilizations | [Civilizations](database/Civilizations.md) |
| `KIND_LEADER` | Leaders | [Leaders](database/Leaders.md) |
| `KIND_TRAIT` | Abilities/traits | [Civilizations](database/Civilizations.md) |
| `KIND_ABILITY` | Unit abilities | [Units](database/Units.md) |
| `KIND_TERRAIN` | Terrain types | [Map-Terrain](database/Map-Terrain.md) |
| `KIND_BIOME` | Biomes | [Map-Terrain](database/Map-Terrain.md) |
| `KIND_FEATURE` | Map features | [Map-Terrain](database/Map-Terrain.md) |
| `KIND_RESOURCE` | Resources | [Maps & Resources](maps-resources/Overview.md) |
| `KIND_YIELD` | Yields | [Types](database/Types-and-Kinds.md) |

### Progression Kinds (14)
| Kind | Usage | Documentation |
|------|-------|---------------|
| `KIND_TREE` | Progression trees | [Technology Trees](technology-civics/Technology-Trees.md) |
| `KIND_TREE_NODE` | Tech/civic nodes | [Technology Trees](technology-civics/Technology-Trees.md) |
| `KIND_TRADITION` | Policies | [Civic Trees](technology-civics/Civic-Trees.md) |
| `KIND_PROJECT` | Projects | [Buildings](database/Buildings.md) |
| `KIND_AGE` | Game ages | [Legacy Paths](legacy-paths/Overview.md) |
| `KIND_LEGACY_PATH` | Legacy paths | [Legacy Paths](legacy-paths/Overview.md) |
| `KIND_UNLOCK` | Unlocks | [Types](database/Types-and-Kinds.md) |

### Governance Kinds (6)
| Kind | Usage | Documentation |
|------|-------|---------------|
| `KIND_GOVERNMENT` | Governments | [Governments](technology-civics/Governments.md) |
| `KIND_RELIGION` | Religions | [Religion](religion/Overview.md) |
| `KIND_BELIEF` | Beliefs | [Religion](religion/Overview.md) |
| `KIND_BELIEF_CLASS` | Belief categories | [Religion](religion/Overview.md) |
| `KIND_GOLDEN_AGE` | Golden ages | [Era Score](mechanics/Era-Score.md) |

### Diplomacy Kinds (23)
| Kind | Usage | Documentation |
|------|-------|---------------|
| `KIND_DIPLOMACY_ACTION` | Diplomatic actions | [Diplomacy](diplomacy/Overview.md) |
| `KIND_DIPLOMACY_ACTION_GROUP` | Action groups | [Diplomacy](diplomacy/Overview.md) |
| `KIND_DIPLOMACY_STATEMENT` | Statements | [Diplomacy](diplomacy/Overview.md) |
| `KIND_DIPLOMACY_RESPONSE` | Responses | [Diplomacy](diplomacy/Overview.md) |
| `KIND_INDEPENDENT` | Independents | [Independents](independents/Overview.md) |
| `KIND_CITY_STATE_BONUS` | City-state bonuses | [Independents](independents/Overview.md) |

---

## Effect Types

For the complete list of 376+ effect types, see [Effect Types](modifiers/Effect-Types.md).

### Yield Effects (40+)
| Effect | Description |
|--------|-------------|
| `EFFECT_CITY_ADJUST_YIELD` | City yield bonus |
| `EFFECT_PLOT_ADJUST_YIELD` | Plot yield bonus |
| `EFFECT_PLAYER_GRANT_YIELD` | One-time yield |
| `EFFECT_PLAYER_ADJUST_YIELD_PER_NUM_TRADE_ROUTES` | Per trade route |
| `EFFECT_CITY_ADJUST_WORKER_YIELD` | Per worker |

### Unit Effects (78)
| Effect | Description |
|--------|-------------|
| `EFFECT_ADJUST_UNIT_STRENGTH_MODIFIER` | Combat bonus |
| `EFFECT_ADJUST_UNIT_MOVEMENT` | Movement bonus |
| `EFFECT_UNIT_ADJUST_ABILITY` | Grant ability |
| `EFFECT_ARMY_ADJUST_EXPERIENCE_RATE` | XP modifier |
| `EFFECT_UNIT_ADJUST_HEAL_PER_TURN` | Healing rate |

### Production Effects (24)
| Effect | Description |
|--------|-------------|
| `EFFECT_CITY_ADJUST_WONDER_PRODUCTION` | Wonder bonus |
| `EFFECT_CITY_ADJUST_UNIT_PRODUCTION` | Unit production |
| `EFFECT_CITY_ADJUST_CONSTRUCTIBLE_PRODUCTION` | Building production |
| `EFFECT_CITY_ADJUST_PROJECT_PRODUCTION` | Project production |

### DAE Effects (30)
| Effect | Description |
|--------|-------------|
| `EFFECT_DAE_ESPIONAGE_*` | Espionage operations |
| `EFFECT_DAE_COOPERATIVE_*` | Cooperative actions |
| `EFFECT_DAE_LEVY_UNIT` | City-state levy |

---

## Collection Types

For the complete list of 40 collection types, see [Collection Types](modifiers/Collection-Types.md).

### Player Collections (10)
| Collection | Target |
|------------|--------|
| `COLLECTION_OWNER` | Owning player |
| `COLLECTION_PLAYER_CITIES` | All player cities |
| `COLLECTION_PLAYER_CAPITAL_CITY` | Capital only |
| `COLLECTION_PLAYER_UNITS` | All player units |
| `COLLECTION_PLAYER_PLOT_YIELDS` | All plot yields |

### City Collections (4)
| Collection | Target |
|------------|--------|
| `COLLECTION_OWNER_CITY` | Owning city |
| `COLLECTION_CITY_DISTRICTS` | City's districts |
| `COLLECTION_CITY_PLOT_YIELDS` | City's plots |

### Global Collections (6)
| Collection | Target |
|------------|--------|
| `COLLECTION_ALL_CITIES` | All game cities |
| `COLLECTION_ALL_UNITS` | All game units |
| `COLLECTION_ALL_PLAYERS` | All players |

---

## Requirement Types

For the complete list of 280+ requirement types, see [Requirement Types](modifiers/Requirement-Types.md).

### Common Requirements
| Requirement | Check |
|-------------|-------|
| `REQUIREMENT_CITY_HAS_BUILDING` | Building in city |
| `REQUIREMENT_PLAYER_HAS_TECHNOLOGY` | Tech researched |
| `REQUIREMENT_PLOT_TERRAIN_TYPE_MATCHES` | Terrain match |
| `REQUIREMENT_UNIT_TAG_MATCHES` | Unit tag check |
| `REQUIREMENT_PLAYER_IS_RELIGION_FOUNDER` | Religion founder |
| `REQUIREMENT_PLOT_IS_RIVER` | River adjacency |
| `REQUIREMENT_CITY_IS_CAPITAL` | Is capital |

---

## Common Modding Tasks

### "I want to create a..."

| Task | Primary Docs | Key Steps |
|------|--------------|-----------|
| New civilization | [Civilizations](database/Civilizations.md), [Leaders](database/Leaders.md) | 1. Register types 2. Define civ 3. Add leader 4. Create traits |
| New unit | [Units](database/Units.md), [Art Assets](technical-reference/Art-Assets.md) | 1. Register type 2. Define stats 3. Add abilities 4. Set icon |
| New building | [Buildings](database/Buildings.md) | 1. Register type 2. Define building 3. Add yields 4. Set prereqs |
| New technology | [Technology Trees](technology-civics/Technology-Trees.md) | 1. Register type 2. Define tech 3. Set prerequisites 4. Add unlocks |
| New civic | [Civic Trees](technology-civics/Civic-Trees.md) | 1. Register type 2. Define civic 3. Set prerequisites |
| New policy/tradition | [Civic Trees](technology-civics/Civic-Trees.md) | 1. Register type 2. Define tradition 3. Add modifiers |
| New government | [Governments](technology-civics/Governments.md) | 1. Register type 2. Define government 3. Add golden ages |
| New resource | [Maps & Resources](maps-resources/Overview.md) | 1. Register type 2. Define resource 3. Set placement 4. Add icon |
| New belief | [Religion](religion/Overview.md) | 1. Register type 2. Define belief 3. Link modifiers |
| Custom modifier | [Modifiers](modifiers/Overview.md) | 1. Choose effect 2. Choose collection 3. Add requirements 4. Set arguments |
| New narrative event | [Narrative](narrative/Overview.md) | 1. Define event 2. Add triggers 3. Create choices 4. Set outcomes |
| UI extension | [UI Modding](technical-reference/UI-Modding.md) | 1. Create HTML/CSS/JS 2. Register in modinfo 3. Handle events |

### "How do I..."

| Question | Answer Location |
|----------|-----------------|
| Add yields to a building? | [Buildings](database/Buildings.md) - Constructible_YieldChanges |
| Make something unlock with a tech? | [Technology Trees](technology-civics/Technology-Trees.md) - TechnologyUnlocks |
| Create a conditional bonus? | [Modifiers](modifiers/Overview.md) - SubjectRequirements |
| Add localized text? | [Localization](technical-reference/Localization.md) - LocalizedText table |
| Create icons for my content? | [Art Assets](technical-reference/Art-Assets.md) - Icon atlas system |
| Package my mod? | [ModInfo Files](Modinfo-Files.md) - Mod structure |
| Make AI prefer something? | [AI Behavior](civilization-creation/AI-Behavior.md) - AI lists |
| Add a starting bias? | [Starting Bias](civilization-creation/Starting-Bias.md) - StartBiases |
| Listen to game events? | [Events Reference](javascript/Events.md) - Event subscription |
| Create a map script? | [Creating a Map Generator](tutorials/Creating-a-Map-Generator.md) |

---

## File Types Quick Reference

| Extension | Purpose | Documentation |
|-----------|---------|---------------|
| `.xml` | Database definitions | [Types and Kinds](database/Types-and-Kinds.md) |
| `.sql` | SQL database queries | [Tables Reference](database/Tables-Reference.md) |
| `.js` | JavaScript gameplay scripts | [JavaScript](javascript/Overview.md) |
| `.html` | UI panels | [UI Modding](technical-reference/UI-Modding.md) |
| `.css` | UI styling | [UI Modding](technical-reference/UI-Modding.md) |
| `.modinfo` | Mod packaging | [ModInfo Files](Modinfo-Files.md) |
| `.dds` | Texture/icon images | [Art Assets](technical-reference/Art-Assets.md) |
| `.gr2` | 3D models | [Art Assets](technical-reference/Art-Assets.md) |

---

## See Also

- [Getting Started](Getting-Started.md) - Initial setup
- [ModInfo Files](Modinfo-Files.md) - Mod packaging
- [Types and Kinds](database/Types-and-Kinds.md) - Type system fundamentals
- [Tables Reference](database/Tables-Reference.md) - Database schema
- [Effect Types](modifiers/Effect-Types.md) - All modifier effects
- [Collection Types](modifiers/Collection-Types.md) - Subject targeting
- [Requirement Types](modifiers/Requirement-Types.md) - Conditions
- [Events Reference](javascript/Events.md) - JavaScript events
- [Modifiers Overview](modifiers/Overview.md) - Gameplay effect system
