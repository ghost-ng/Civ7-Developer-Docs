# Cross-Reference Index

This index provides quick lookup for all documented systems, tables, types, and concepts across the Civ VII Modding API documentation.

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

### Technical
| Topic | Primary Doc | Related Docs |
|-------|-------------|--------------|
| Modifier system | [Modifiers Overview](modifiers/Overview.md) | All gameplay docs |
| Effect types | [Effect Types](modifiers/Effect-Types.md) | [Modifiers](modifiers/Overview.md) |
| Collections | [Collection Types](modifiers/Collection-Types.md) | [Modifiers](modifiers/Overview.md) |
| Requirements | [Requirement Types](modifiers/Requirement-Types.md) | [Modifiers](modifiers/Overview.md) |
| JavaScript API | [JavaScript](javascript/Overview.md) | [Narrative](narrative/Overview.md) |
| Localization | [Localization](technical-reference/Localization.md) | All content docs |
| Art assets | [Art Assets](technical-reference/Art-Assets.md) | [Units](database/Units.md), [Buildings](database/Buildings.md) |
| ModInfo | [ModInfo Files](Modinfo-Files.md) | [Getting Started](Getting-Started.md) |
| Types & Kinds | [Types and Kinds](database/Types-and-Kinds.md) | All database docs |

---

## Database Tables

### Core Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Types` | Register all game entities | [Types and Kinds](database/Types-and-Kinds.md) |
| `Kinds` | Define entity categories | [Types and Kinds](database/Types-and-Kinds.md) |

### Civilization Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Civilizations` | Define civilizations | [Civilizations](database/Civilizations.md) |
| `CivilizationTraits` | Link civs to abilities | [Civilizations](database/Civilizations.md) |
| `Leaders` | Define leaders | [Leaders](database/Leaders.md) |
| `LeaderTraits` | Link leaders to abilities | [Leaders](database/Leaders.md) |
| `CivilizationLeaders` | Link leaders to civs | [Leaders](database/Leaders.md) |
| `StartBiases` | Terrain preferences | [Starting Bias](civilization-creation/Starting-Bias.md) |

### Unit Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Units` | Define units | [Units](database/Units.md) |
| `Unit_Abilities` | Link abilities | [Units](database/Units.md) |
| `UnitPromotions` | Define promotions | [Promotions](units/Promotions.md) |
| `UnitPromotionClasses` | Promotion trees | [Promotions](units/Promotions.md) |
| `GreatPersonClasses` | Great person types | [Great People](units/Great-People.md) |
| `GreatPersonIndividuals` | Specific great people | [Great People](units/Great-People.md) |

### Building Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Buildings` | Define buildings | [Buildings](database/Buildings.md) |
| `Building_YieldChanges` | Building yields | [Buildings](database/Buildings.md) |
| `Districts` | Define districts | [Buildings](database/Buildings.md) |

### Technology & Civic Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Technologies` | Define techs | [Technology Trees](technology-civics/Technology-Trees.md) |
| `TechnologyPrereqs` | Tech prerequisites | [Technology Trees](technology-civics/Technology-Trees.md) |
| `Civics` | Define civics | [Civic Trees](technology-civics/Civic-Trees.md) |
| `Traditions` | Define policies | [Civic Trees](technology-civics/Civic-Trees.md) |
| `Governments` | Define governments | [Governments](technology-civics/Governments.md) |
| `GoldenAges` | Golden age bonuses | [Era Score](mechanics/Era-Score.md) |

### Map Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Terrains` | Terrain types | [Map-Terrain](database/Map-Terrain.md) |
| `Features` | Map features | [Map-Terrain](database/Map-Terrain.md) |
| `Resources` | Resource types | [Maps & Resources](maps-resources/Overview.md) |
| `NaturalWonders` | Natural wonders | [Map-Terrain](database/Map-Terrain.md) |

### Religion Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Religions` | Define religions | [Religion](religion/Overview.md) |
| `BeliefClasses` | Belief categories | [Religion](religion/Overview.md) |
| `Beliefs` | Define beliefs | [Religion](religion/Overview.md) |
| `BeliefModifiers` | Belief effects | [Religion](religion/Overview.md) |

### Diplomacy Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `DiplomacyActions` | Diplomatic actions | [Diplomacy](diplomacy/Overview.md) |
| `DiplomacyActionGroups` | Action categories | [Diplomacy](diplomacy/Overview.md) |
| `DiplomacyRelationships` | Relationship levels | [Diplomacy](diplomacy/Overview.md) |
| `DiplomacyFavorEvents` | Favor sources | [Diplomacy](diplomacy/Overview.md) |
| `DiplomacyGrievanceEvents` | Grievance sources | [Diplomacy](diplomacy/Overview.md) |

### Victory & Legacy Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Victories` | Victory conditions | [Victory Conditions](victory-conditions/Overview.md) |
| `AgeProgressions` | Legacy paths | [Legacy Paths](legacy-paths/Overview.md) |
| `AgeProgressionMilestones` | Milestone thresholds | [Legacy Paths](legacy-paths/Overview.md) |
| `AgeProgressionRewards` | Milestone rewards | [Legacy Paths](legacy-paths/Overview.md) |

### Modifier Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `Modifiers` | XML modifier definitions | [Modifiers](modifiers/Overview.md) |
| `ModifierArguments` | Modifier parameters | [Modifiers](modifiers/Overview.md) |
| `Requirements` | Condition definitions | [Requirements](modifiers/Requirement-Types.md) |
| `RequirementSets` | Grouped requirements | [Requirements](modifiers/Requirement-Types.md) |

### Art & Localization Tables
| Table | Purpose | Documentation |
|-------|---------|---------------|
| `LocalizedText` | Translated strings | [Localization](technical-reference/Localization.md) |
| `IconTextureAtlases` | Icon atlases | [Art Assets](technical-reference/Art-Assets.md) |
| `IconDefinitions` | Icon mappings | [Art Assets](technical-reference/Art-Assets.md) |

---

## Type Kinds

| Kind | Usage | Documentation |
|------|-------|---------------|
| `KIND_CIVILIZATION` | Civilizations | [Civilizations](database/Civilizations.md) |
| `KIND_LEADER` | Leaders | [Leaders](database/Leaders.md) |
| `KIND_TRAIT` | Abilities/traits | [Civilizations](database/Civilizations.md) |
| `KIND_UNIT` | Units | [Units](database/Units.md) |
| `KIND_BUILDING` | Buildings | [Buildings](database/Buildings.md) |
| `KIND_DISTRICT` | Districts | [Buildings](database/Buildings.md) |
| `KIND_TECHNOLOGY` | Technologies | [Technology Trees](technology-civics/Technology-Trees.md) |
| `KIND_CIVIC` | Civics | [Civic Trees](technology-civics/Civic-Trees.md) |
| `KIND_TRADITION` | Policies | [Civic Trees](technology-civics/Civic-Trees.md) |
| `KIND_GOVERNMENT` | Governments | [Governments](technology-civics/Governments.md) |
| `KIND_GOLDEN_AGE` | Golden ages | [Era Score](mechanics/Era-Score.md) |
| `KIND_RESOURCE` | Resources | [Maps & Resources](maps-resources/Overview.md) |
| `KIND_TERRAIN` | Terrains | [Map-Terrain](database/Map-Terrain.md) |
| `KIND_FEATURE` | Features | [Map-Terrain](database/Map-Terrain.md) |
| `KIND_RELIGION` | Religions | [Religion](religion/Overview.md) |
| `KIND_BELIEF` | Beliefs | [Religion](religion/Overview.md) |
| `KIND_BELIEF_CLASS` | Belief categories | [Religion](religion/Overview.md) |
| `KIND_PROMOTION` | Unit promotions | [Promotions](units/Promotions.md) |
| `KIND_GREAT_PERSON_CLASS` | Great person types | [Great People](units/Great-People.md) |
| `KIND_GREAT_PERSON_INDIVIDUAL` | Specific great people | [Great People](units/Great-People.md) |
| `KIND_DIPLOMACY_ACTION` | Diplomatic actions | [Diplomacy](diplomacy/Overview.md) |
| `KIND_DIPLOMACY_ACTION_GROUP` | Action groups | [Diplomacy](diplomacy/Overview.md) |
| `KIND_VICTORY` | Victory conditions | [Victory Conditions](victory-conditions/Overview.md) |
| `KIND_AGE_PROGRESSION` | Legacy paths | [Legacy Paths](legacy-paths/Overview.md) |
| `KIND_INDEPENDENT` | Independents | [Independents](independents/Overview.md) |

---

## Effect Types

### Yield Effects
| Effect | Description | Documentation |
|--------|-------------|---------------|
| `EFFECT_CITY_ADJUST_YIELD` | City yield bonus | [Effect Types](modifiers/Effect-Types.md) |
| `EFFECT_PLOT_ADJUST_YIELD` | Plot yield bonus | [Effect Types](modifiers/Effect-Types.md) |
| `EFFECT_PLAYER_GRANT_YIELD` | One-time yield | [Effect Types](modifiers/Effect-Types.md) |
| `EFFECT_TRADE_ROUTE_ADJUST_YIELD` | Trade route yields | [Effect Types](modifiers/Effect-Types.md) |

### Unit Effects
| Effect | Description | Documentation |
|--------|-------------|---------------|
| `EFFECT_ADJUST_UNIT_COMBAT_STRENGTH` | Combat bonus | [Effect Types](modifiers/Effect-Types.md) |
| `EFFECT_ADJUST_UNIT_MOVEMENT` | Movement bonus | [Effect Types](modifiers/Effect-Types.md) |
| `EFFECT_GRANT_ABILITY` | Grant ability | [Effect Types](modifiers/Effect-Types.md) |
| `EFFECT_ADJUST_UNIT_EXPERIENCE` | XP modifier | [Effect Types](modifiers/Effect-Types.md) |

### Production Effects
| Effect | Description | Documentation |
|--------|-------------|---------------|
| `EFFECT_CITY_ADJUST_WONDER_PRODUCTION` | Wonder bonus | [Effect Types](modifiers/Effect-Types.md) |
| `EFFECT_CITY_ADJUST_UNIT_PRODUCTION` | Unit production | [Effect Types](modifiers/Effect-Types.md) |
| `EFFECT_CITY_ADJUST_BUILDING_PRODUCTION` | Building production | [Effect Types](modifiers/Effect-Types.md) |

### Diplomacy Effects
| Effect | Description | Documentation |
|--------|-------------|---------------|
| `EFFECT_ADJUST_PLAYER_DIPLOMACY_FAVOR` | Modify favor | [Diplomacy](diplomacy/Overview.md) |
| `EFFECT_GRANT_DIPLOMATIC_VISIBILITY` | Grant visibility | [Espionage](espionage/Overview.md) |

See [Effect Types](modifiers/Effect-Types.md) for complete list.

---

## Collection Types

| Collection | Target | Documentation |
|------------|--------|---------------|
| `COLLECTION_PLAYER_CITIES` | All player cities | [Collection Types](modifiers/Collection-Types.md) |
| `COLLECTION_PLAYER_UNITS` | All player units | [Collection Types](modifiers/Collection-Types.md) |
| `COLLECTION_CITY_BUILDINGS` | Buildings in city | [Collection Types](modifiers/Collection-Types.md) |
| `COLLECTION_OWNER` | Owning player | [Collection Types](modifiers/Collection-Types.md) |
| `COLLECTION_PLAYER_PLOT_YIELDS` | All plot yields | [Collection Types](modifiers/Collection-Types.md) |
| `COLLECTION_ALL_PLAYERS` | All players | [Collection Types](modifiers/Collection-Types.md) |

See [Collection Types](modifiers/Collection-Types.md) for complete list.

---

## Requirement Types

| Requirement | Check | Documentation |
|-------------|-------|---------------|
| `REQUIREMENT_CITY_HAS_BUILDING` | Building in city | [Requirement Types](modifiers/Requirement-Types.md) |
| `REQUIREMENT_PLAYER_HAS_TECHNOLOGY` | Tech researched | [Requirement Types](modifiers/Requirement-Types.md) |
| `REQUIREMENT_PLOT_TERRAIN_TYPE_MATCHES` | Terrain match | [Requirement Types](modifiers/Requirement-Types.md) |
| `REQUIREMENT_UNIT_TAG_MATCHES` | Unit tag check | [Requirement Types](modifiers/Requirement-Types.md) |
| `REQUIREMENT_PLAYER_IS_RELIGION_FOUNDER` | Religion founder | [Religion](religion/Overview.md) |

See [Requirement Types](modifiers/Requirement-Types.md) for complete list.

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

### "How do I..."

| Question | Answer Location |
|----------|-----------------|
| Add yields to a building? | [Buildings](database/Buildings.md) - Building_YieldChanges |
| Make something unlock with a tech? | [Technology Trees](technology-civics/Technology-Trees.md) - TechnologyUnlocks |
| Create a conditional bonus? | [Modifiers](modifiers/Overview.md) - SubjectRequirements |
| Add localized text? | [Localization](technical-reference/Localization.md) - LocalizedText table |
| Create icons for my content? | [Art Assets](technical-reference/Art-Assets.md) - Icon atlas system |
| Package my mod? | [ModInfo Files](Modinfo-Files.md) - Mod structure |
| Make AI prefer something? | [AI Behavior](civilization-creation/AI-Behavior.md) - AI lists |
| Add a starting bias? | [Starting Bias](civilization-creation/Starting-Bias.md) - StartBiases |

---

## File Types Quick Reference

| Extension | Purpose | Documentation |
|-----------|---------|---------------|
| `.xml` | Database definitions | [Types and Kinds](database/Types-and-Kinds.md) |
| `.sql` | SQL database queries | [Types and Kinds](database/Types-and-Kinds.md) |
| `.js` | JavaScript gameplay scripts | [JavaScript](javascript/Overview.md) |
| `.modinfo` | Mod packaging | [ModInfo Files](Modinfo-Files.md) |
| `.dds` | Texture/icon images | [Art Assets](technical-reference/Art-Assets.md) |
| `.gr2` | 3D models | [Art Assets](technical-reference/Art-Assets.md) |

---

## See Also

- [Getting Started](Getting-Started.md) - Initial setup
- [ModInfo Files](Modinfo-Files.md) - Mod packaging
- [Types and Kinds](database/Types-and-Kinds.md) - Type system fundamentals
- [Modifiers Overview](modifiers/Overview.md) - Gameplay effect system
