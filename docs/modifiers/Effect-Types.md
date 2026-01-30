# Effect Types Reference

This document lists all **376 effect types** available for modifiers in Civilization VII. Effects are organized by category for easy reference.

> **Note**: Effects are used with the `effect=` attribute in GameEffects XML or the `EffectType` column in the DynamicModifiers database table.

## Quick Navigation

- [Yield Adjustment Effects](#yield-adjustment-effects) (40+ effects)
- [Construction & Production Effects](#construction--production-effects) (24 effects)
- [City Management Effects](#city-management-effects) (28 effects)
- [Unit & Combat Effects](#unit--combat-effects) (78 effects)
- [Army & Command Effects](#army--command-effects) (7 effects)
- [Player Effects](#player-effects) (15 effects)
- [Progression & Technology Effects](#progression--technology-effects) (10 effects)
- [Golden Age & Celebration Effects](#golden-age--celebration-effects) (5 effects)
- [Religion & Belief Effects](#religion--belief-effects) (7 effects)
- [Diplomacy & Relations Effects](#diplomacy--relations-effects) (19 effects)
- [Discovery & Narrative Effects](#discovery--narrative-effects) (8 effects)
- [Victory & Scoring Effects](#victory--scoring-effects) (6 effects)
- [Legacy & Attributes Effects](#legacy--attributes-effects) (6 effects)
- [Reveal & Visibility Effects](#reveal--visibility-effects) (4 effects)
- [Government & Policy Effects](#government--policy-effects) (3 effects)
- [Constructible Unlock & Replace Effects](#constructible-unlock--replace-effects) (3 effects)
- [Espionage & DAE Effects](#espionage--dae-effects) (30 effects)
- [City State & Independent Effects](#city-state--independent-effects) (4 effects)
- [Economic & Resource Effects](#economic--resource-effects) (4 effects)
- [Special & Miscellaneous Effects](#special--miscellaneous-effects) (26 effects)

---

## Yield Adjustment Effects

These effects modify yields (Food, Production, Gold, Science, Culture, Faith, etc.) at various levels.

### City-Level Yield Effects

#### EFFECT_CITY_ADJUST_YIELD
Adjusts a city's base yield output.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | `YIELD_FOOD`, `YIELD_PRODUCTION`, `YIELD_GOLD`, `YIELD_SCIENCE`, `YIELD_CULTURE` |
| `Amount` | INTEGER | Flat bonus |
| `Percent` | INTEGER | Percentage bonus |
| `Tooltip` | TEXT | Tooltip localization key |

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
    <Argument name="YieldType">YIELD_FOOD</Argument>
    <Argument name="Amount">2</Argument>
</Modifier>
```

#### EFFECT_CITY_ADJUST_YIELD_CONVERSION
Converts yields from one type to another in cities.

#### EFFECT_CITY_ADJUST_YIELD_WITH_DECAY
Provides yield with decay over time.

#### EFFECT_CITY_ADJUST_YIELD_PER_ACTIVE_TRADITION
Adjusts city yield per active tradition.

#### EFFECT_CITY_ADJUST_YIELD_PER_POPULATION
Adjusts city yield based on population count.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Per population point |

#### EFFECT_CITY_ADJUST_YIELD_PER_RESOURCE
Adjusts city yield per resource type.

#### EFFECT_CITY_ADJUST_YIELD_PER_RESOURCE_CLASS
Adjusts city yield per resource class.

#### EFFECT_CITY_ADJUST_YIELD_PER_AVAILABLE_RESOURCE_TYPE
Adjusts yield per available resource type.

#### EFFECT_CITY_ADJUST_YIELD_PER_SURPLUS_HAPPINESS
Adjusts city yield per surplus happiness.

#### EFFECT_CITY_ADJUST_YIELD_PER_SUZERAIN
Adjusts city yield per suzerain relationship.

#### EFFECT_CITY_ADJUST_YIELD_PER_SUZERAINED_CITY_STATE_TYPE
Adjusts yield per suzerained city-state type.

#### EFFECT_CITY_ADJUST_YIELD_PER_CONNECTED_CITY
Adjusts city yield per connected city.

#### EFFECT_CITY_ADJUST_YIELD_PER_GREAT_WORK
Adjusts city yield per great work.

#### EFFECT_CITY_ADJUST_YIELD_PER_NUM_CITIES
Adjusts city yield per number of cities owned.

#### EFFECT_CITY_ADJUST_YIELD_PER_ATTRIBUTE
Adjusts city yield per attribute level.

#### EFFECT_CITY_ADJUST_YIELD_PER_COMMANDER_LEVEL
Adjusts city yield per commander level.

#### EFFECT_CITY_ADJUST_YIELDS_PER_SETTLEMENT_OVER_CAP
Adjusts yields per settlement over the cap.

#### EFFECT_CITY_GRANT_YIELD
Grants a one-time yield to a city.

#### EFFECT_CITY_GRANT_YIELD_DISCOVERY
Grants yield to city from discovery.

#### EFFECT_CITY_GRANT_NATURAL_YIELDS
Grants natural yields to a city.

### Plot/Tile-Level Yield Effects

#### EFFECT_PLOT_ADJUST_YIELD
Adjusts yield from a specific plot/tile.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Flat bonus |

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_PLOT_YIELDS" effect="EFFECT_PLOT_ADJUST_YIELD">
    <SubjectRequirements>
        <Requirement type="REQUIREMENT_PLOT_BIOME_TYPE_MATCHES">
            <Argument name="BiomeType">BIOME_DESERT</Argument>
        </Requirement>
    </SubjectRequirements>
    <Argument name="YieldType">YIELD_PRODUCTION</Argument>
    <Argument name="Amount">1</Argument>
</Modifier>
```

#### EFFECT_PLOT_ADJUST_YIELD_PER_PLAYER_NATURAL_WONDER
Adjusts plot yield per natural wonder owned.

### Player-Level Yield Effects

#### EFFECT_PLAYER_GRANT_YIELD
Grants a one-time yield to the player.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Amount to grant |

#### EFFECT_PLAYER_GRANT_YIELD_DISCOVERY
Grants yield from discovery.

#### EFFECT_PLAYER_GRANT_YIELD_NARRATIVE
Grants yield from narrative events.

#### EFFECT_PLAYER_GRANT_YIELD_ASSIGNED_WORKERS
Grants yield to assigned workers.

#### EFFECT_PLAYER_GRANT_YIELD_PER_XP_EARNED
Grants yield per XP earned.

#### EFFECT_PLAYER_ADJUST_YIELD
Adjusts base player yield.

#### EFFECT_PLAYER_ADJUST_YIELD_CONVERSION
Converts player yields between types.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_ACTIVE_TRADITION
Adjusts player yield per active tradition.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_NUM_CITIES
Adjusts player yield per city count.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_NUM_CIVS_TRADING_WITH
Adjusts yield per civilization trading with.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_NUM_TRADE_ROUTES
Adjusts yield per trade route count.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Per trade route |
| `Tooltip` | TEXT | Tooltip key |

#### EFFECT_PLAYER_ADJUST_YIELD_PER_NUM_TRADE_ROUTES_AND_SUZERAINED_TYPE
Combined trade routes and suzerained type adjustment.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_PROPERTY_VALUE
Adjusts yield per property value.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_RESOURCE
Adjusts player yield per resource type.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_RESOURCE_TYPE
Adjusts yield by specific resource type.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_SUZERAIN
Adjusts yield per suzerain relationship.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_UNIQUE_RESOURCE
Adjusts yield per unique resource.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_UNIT_LEVEL
Adjusts yield per unit level.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_CONQUERED_CITY
Adjusts yield per conquered city.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_RAZED_SETTLEMENT
Adjusts yield per razed settlement.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_CONQUERED_SETTLEMENT
Adjusts yield per conquered settlement.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_UNIQUE_CIV_CONQUERED_CITY
Adjusts yield per unique civilization's city conquered.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_ATTRIBUTE_AND_ALLIANCES
Adjusts yield per attribute and alliance count.

#### EFFECT_PLAYER_ADJUST_YIELD_FOR_RESOURCE
Adjusts player yield for specific resources.

#### EFFECT_PLAYER_ADJUST_YIELD_FOR_BECOMING_SUZERAIN
Grants yield upon becoming suzerain.

#### EFFECT_PLAYER_ADJUST_YIELD_FOR_COMPLETING_NARRATIVE_EVENTS
Adjusts yield for completing narrative events.

#### EFFECT_PLAYER_ADJUST_YIELD_FROM_DISTATERS
Adjusts yield from disasters.

### Modifier Yield Conversions

#### EFFECT_MODIFY_PLAYER_TRADE_YIELD_CONVERSION
Modifies trade yield conversion rates.

#### EFFECT_MODIFY_PLAYER_YIELD_FOR_X_TURNS
Provides temporary yield modification for X turns.

#### EFFECT_MODIFY_CITY_YIELD_FOR_X_TURNS
Provides temporary city yield modification.

### Diplomacy Yield Effects

#### EFFECT_DIPLOMACY_ADJUST_YIELD_PER_PLAYER_RELATIONSHIP
Adjusts yield per relationship value.

#### EFFECT_DIPLOMACY_ADJUST_YIELD_PER_PLAYER_INVOLVED_ACTION
Adjusts yield per diplomatic action taken.

#### EFFECT_DIPLOMACY_ADJUST_CITY_YIELD_PER_PLAYER_RELATIONSHIP
Adjusts city yield per player relationship.

#### EFFECT_ADJUST_YIELD_FOR_ALLIES
Adjusts yield for allied players.

---

## Construction & Production Effects

### Wonder Production

#### EFFECT_CITY_ADJUST_WONDER_PRODUCTION
Adjusts wonder production speed.

| Argument | Type | Description |
|----------|------|-------------|
| `Percent` | INTEGER | Percentage bonus |

#### EFFECT_CITY_ADJUST_FAVORED_WONDER_PRODUCTION
Adjusts production for favored wonders.

### General Constructible Production

#### EFFECT_CITY_ADJUST_CONSTRUCTIBLE_PRODUCTION
Adjusts general constructible production speed.

| Argument | Type | Description |
|----------|------|-------------|
| `ConstructibleType` | TEXT | Specific constructible (optional) |
| `Percent` | INTEGER | Percentage bonus |

#### EFFECT_CITY_ADJUST_CONSTRUCTIBLE_PRODUCTION_PER_RESOURCE
Adjusts production per resource.

#### EFFECT_CITY_ADJUST_CONSTRUCTIBLE_PRODUCTION_PER_SLOTTED_RESOURCE
Adjusts production per slotted resource.

#### EFFECT_CITY_ADJUST_CONSTRUCTIBLE_PRODUCTION_PER_SUZERAIN_OF
Adjusts production per suzerain relationship.

#### EFFECT_CITY_ADJUST_BUILDING_PRODUCTION_PRODUCTION_PER_LEGACY_POINT
Adjusts building production per legacy point.

### Unit Production

#### EFFECT_CITY_ADJUST_UNIT_PRODUCTION
Adjusts unit production speed.

| Argument | Type | Description |
|----------|------|-------------|
| `UnitType` | TEXT | Specific unit (optional) |
| `Percent` | INTEGER | Percentage bonus |

#### EFFECT_CITY_ADJUST_UNIT_PRODUCTION_MOD_PER_SETTLEMENT
Adjusts unit production per settlement.

#### EFFECT_CITY_ADJUST_UNIT_PRODUCTION_PER_RESOURCE
Adjusts unit production per resource.

#### EFFECT_CITY_ADJUST_UNIT_PRODUCTION_PER_SLOTTED_RESOURCE
Adjusts unit production per slotted resource.

#### EFFECT_CITY_ADJUST_UNIT_DOMAIN_PRODUCTION
Adjusts production by unit domain (land, sea, air).

### Project Production

#### EFFECT_CITY_ADJUST_PROJECT_PRODUCTION
Adjusts project production speed.

#### EFFECT_CITY_ADJUST_PROJECT_PRODUCTION_PER_LEGACY_POINT
Adjusts project production per legacy point.

### Constructible Yields

#### EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD
Adjusts yields from player constructibles.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Bonus amount |
| `Tag` | TEXT | Constructible tag filter |

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_OWNER" effect="EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD">
    <Argument name="YieldType">YIELD_SCIENCE</Argument>
    <Argument name="Amount">1</Argument>
    <Argument name="Tag">SCIENCE</Argument>
</Modifier>
```

#### EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD_BY_ATTRIBUTE
Adjusts constructible yield by attribute.

### Constructible Maintenance

#### EFFECT_CITY_ADJUST_BUILDING_MAINTENANCE_EFFICIENCY
Adjusts building maintenance efficiency.

### Purchase Efficiency

#### EFFECT_ADJUST_PLAYER_OR_CITY_BUILDING_PURCHASE_EFFICIENCY
Adjusts building purchase efficiency.

#### EFFECT_ADJUST_PLAYER_OR_CITY_UNIT_PURCHASE_EFFICIENCY
Adjusts unit purchase efficiency.

#### EFFECT_PLAYER_ADJUST_PURCHASE_EFFICIENCY_PER_RESOURCE
Adjusts purchase efficiency per resource.

### Overbuild & Advancement

#### EFFECT_CITY_ADJUST_OVERBUILD_PRODUCTION_MOD
Adjusts overbuild production modifier.

#### EFFECT_CONSTRUCTIBLE_ADJUST_GREAT_WORK_SLOTS
Adjusts great work slots on constructibles.

---

## City Management Effects

### Growth & Population

#### EFFECT_CITY_ADJUST_GROWTH
Adjusts city growth rate.

#### EFFECT_CITY_ADJUST_GROWTH_PER_WORKER
Adjusts growth per worker.

#### EFFECT_CITY_ADJUST_POPULATION
Directly adjusts population count.

#### TRIGGER_CITY_ADJUST_POPULATION_ON_GOLDEN_AGE
Adjusts population when golden age starts.

#### EFFECT_CITY_ADD_FOOD_AFTER_GROWTH_EVENT
Adds food bonus after growth event.

### Worker Management

#### EFFECT_CITY_ADJUST_WORKER_CAP
Adjusts maximum workers in a city.

#### EFFECT_CITY_ADJUST_WORKER_MAINTENANCE_EFFICIENCY
Adjusts worker maintenance costs.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type(s) |
| `Percent` | INTEGER | Efficiency bonus |

#### EFFECT_CITY_ADJUST_WORKER_YIELD
Adjusts yield per worker.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Per worker |
| `Tooltip` | TEXT | Tooltip key |

#### EFFECT_PLOT_ADJUST_WORKER_YIELD
Adjusts plot worker yield.

#### EFFECT_PLOT_ADJUST_WORKER_MAINTENANCE
Adjusts plot worker maintenance.

### Resources

#### EFFECT_CITY_ADD_RESOURCE_TO_PLOT
Adds a resource to a plot.

#### EFFECT_CITY_ADJUST_RESOURCE_CAP
Adjusts resource storage capacity.

#### EFFECT_PLOT_PLACE_RESOURCE
Places a resource on a plot.

### Adjacency

#### EFFECT_CITY_ACTIVATE_CONSTRUCTIBLE_ADJACENCY
Activates adjacency bonus for constructibles.

#### EFFECT_CITY_ACTIVATE_CONSTRUCTIBLE_WAREHOUSE_YIELD
Activates warehouse yield bonus.

#### EFFECT_CITY_ADJUST_ADJACENCY_FLAT_AMOUNT
Adjusts flat adjacency bonus amount.

### Trade Routes

#### EFFECT_CITY_ADJUST_TRADE_ROUTE_RANGE
Adjusts trade route range.

| Argument | Type | Description |
|----------|------|-------------|
| `Distance` | INTEGER | Range modifier |

#### EFFECT_CITY_ADJUST_TRADE_ROUTE_RANGE_PER_SUZERAIN_OF
Adjusts trade range per suzerain.

#### EFFECT_CITY_ADJUST_TRADE_YIELD
Adjusts trade route yield.

#### EFFECT_CITY_ADJUST_TRADE_ROUTE_COMBAT_MODIFIER
Adjusts combat near trade routes.

### District & Infrastructure

#### EFFECT_CITY_ADJUST_TOTAL_DISTRICT_HEALTH
Adjusts total district health.

#### EFFECT_DISTRICT_ADJUST_TOTAL_HEALTH
Adjusts specific district health.

#### EFFECT_DISTRICT_ADJUST_FORTIFIED_COMBAT_STRENGTH
Adjusts fortified combat strength.

#### EFFECT_DISTRICT_ADJUST_HEAL_PER_TURN
Adjusts district healing per turn.

### Governance

#### EFFECT_CITY_ADJUST_TOWN_UPGRADE_DISCOUNT
Reduces town upgrade cost.

#### EFFECT_PROMOTE_TOWN_TO_CITY
Promotes a town to city status.

#### EFFECT_CITY_TRANSFER_OWNER
Transfers city ownership.

#### EFFECT_CITY_DAMAGE_CONSTRUCTIBLES
Damages constructibles in city.

### Great Works & Culture

#### EFFECT_CITY_ADJUST_GREAT_WORK_SLOTS
Adjusts great work slots.

#### EFFECT_CITY_GRANT_GREAT_WORK
Grants a great work to city.

### Unhappiness

#### EFFECT_CITY_ADJUST_UNHAPPINESS_EFFECT
Adjusts unhappiness effects.

#### EFFECT_CITY_ADJUST_IGNORE_UNHAPPINESS_EFFECT
Ignores unhappiness effects.

#### EFFECT_CITY_ADJUST_COMMANDER_UNHAPPINESS_REDUCTION
Adjusts commander unhappiness reduction.

### Events & Random

#### EFFECT_CITY_ADJUST_AVOID_RANDOM_EVENT
Adjusts chance to avoid random events.

### Treasure Fleets

#### EFFECT_ADJUST_CITY_AUTO_TREASURE_FLEET
Adjusts auto treasure fleet behavior.

#### EFFECT_ADJUST_CITY_TREASURE_FLEET_MOD
Adjusts treasure fleet modifier.

### Unlock Projects

#### EFFECT_CITY_UNLOCK_PROJECT
Unlocks a specific project.

---

## Unit & Combat Effects

### Combat Strength

#### EFFECT_ADJUST_UNIT_STRENGTH_MODIFIER
Adjusts unit strength modifier.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Strength modifier |

#### EFFECT_ADJUST_UNIT_BASE_COMBAT_STRENGTH
Adjusts base combat strength.

#### EFFECT_ADJUST_UNIT_COMBAT_STRENGTH_MULTIPLE_ADJACENT_ENEMIES_MODIFIER
Adjusts strength when facing multiple adjacent enemies.

#### EFFECT_ADJUST_UNIT_RELATIONSHIP_COMBAT_STRENGTH
Adjusts combat strength based on relationship.

#### EFFECT_ADJUST_UNIT_NEIGHBOR_COMBAT_MODIFIER
Adjusts combat modifier from adjacent neighbors.

#### EFFECT_ADJUST_UNIT_SUZERAIN_OF_COMBAT_MODIFIER
Adjusts combat modifier from suzerain status.

#### EFFECT_ADJUST_UNIT_TRADE_ROUTE_COMBAT_MODIFIER
Adjusts combat modifier near trade routes.

#### EFFECT_ADJUST_UNIT_URBAN_POPULATION_COMBAT_MODIFIER
Adjusts combat modifier based on urban population.

#### EFFECT_UNIT_ADJUST_COMBAT_STRENGTH_PER_RESOURCE
Adjusts combat strength per resource type.

### Combat Mechanics

#### EFFECT_ADJUST_UNIT_NUM_ATTACKS
Adjusts number of attacks per turn.

#### EFFECT_ADJUST_UNIT_FOCUSED_ATTACK_DAMAGE
Adjusts focused attack damage.

#### EFFECT_ADJUST_UNIT_ATTACK_RANGE
Adjusts attack range.

#### EFFECT_ADJUST_UNIT_ATTACK_CREATES_PLOT_EFFECT
Creates plot effect on attack.

#### EFFECT_ADJUST_UNIT_COMBAT_CAPTURE
Enables capture instead of kill.

#### EFFECT_ADJUST_UNIT_POST_COMBAT_YIELD
Grants yield after combat.

| Argument | Type | Description |
|----------|------|-------------|
| `YieldType` | TEXT | Yield type |
| `Amount` | INTEGER | Amount granted |

#### EFFECT_ADJUST_UNIT_POST_COMBAT_HEAL
Heals unit after combat.

#### EFFECT_UNIT_ADJUST_DAMAGE
Adjusts damage taken by unit.

#### EFFECT_ADJUST_UNIT_WATER_DAMAGE_PROTECTION
Protects from water damage.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Protection level |

#### EFFECT_ADJUST_UNIT_SPLASH_DAMAGE
Adjusts splash damage modifier.

### Healing

#### EFFECT_ADJUST_HEAL_CHARGES
Adjusts healing charges.

#### EFFECT_UNIT_ADJUST_HEAL_PER_TURN
Adjusts healing per turn.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Healing per turn |

#### EFFECT_UNIT_ADJUST_HEAL_PER_RESOURCE
Adjusts healing per resource.

### Pillaging

#### EFFECT_ADJUST_UNIT_ADVANCED_PILLAGING
Enables advanced pillaging abilities.

#### EFFECT_ADJUST_UNIT_BUILDING_PILLAGE_YIELD_MODIFIER
Adjusts building pillage yield.

#### EFFECT_ADJUST_UNIT_IMPROVEMENT_PILLAGE_YIELD_MODIFIER
Adjusts improvement pillage yield.

#### EFFECT_ADJUST_PLAYER_UNITS_PILLAGE_BUILDING_MODIFIER
Adjusts player building pillage modifier.

#### EFFECT_ADJUST_PLAYER_UNITS_PILLAGE_IMPROVEMENT_MODIFIER
Adjusts player improvement pillage modifier.

#### EFFECT_ADJUST_UNIT_PERCENT_PILLAGE_BUILDING_MODIFIER
Adjusts percent building pillage modifier.

#### EFFECT_ADJUST_UNIT_PERCENT_PILLAGE_IMPROVEMENT_MODIFIER
Adjusts percent improvement pillage modifier.

#### EFFECT_UNIT_SET_CAN_PLUNDER_TRADE_ROUTES_BETWEEN_OTHER_CIVS
Enables plundering trade routes between other civs.

### Movement & Navigation

#### EFFECT_ADJUST_UNIT_MOVEMENT
Adjusts unit movement points.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Movement modifier |

#### EFFECT_UNIT_ADJUST_MOVEMENT
Adjusts movement (alternative).

#### EFFECT_ADJUST_UNIT_EMBARKED_MOVEMENT
Adjusts embarked movement.

#### EFFECT_ADJUST_UNIT_EMBARKED_STRENGTH_MODIFIER
Adjusts embarked strength.

#### EFFECT_ADJUST_UNIT_ENTER_FOREIGN_LANDS
Allows entering foreign territory.

#### EFFECT_ADJUST_UNIT_IGNORE_ZOC
Ignores zone of control.

#### EFFECT_ADJUST_UNIT_VALID_TERRAIN
Adds valid terrain for unit.

| Argument | Type | Description |
|----------|------|-------------|
| `TerrainType` | TEXT | Terrain type |

#### EFFECT_UNIT_ADJUST_IGNORE_MOVEMENT_OBSTACLE
Ignores movement obstacles.

| Argument | Type | Description |
|----------|------|-------------|
| `Obstacle` | TEXT | Obstacle type |

#### EFFECT_RESTORE_UNIT_MOVEMENT
Restores unit movement points.

### Vision & Stealth

#### EFFECT_ADJUST_UNIT_SIGHT
Adjusts unit sight range.

#### EFFECT_ADJUST_UNIT_SEE_HIDDEN
Allows seeing hidden units.

#### EFFECT_ADJUST_UNIT_SEE_THROUGH_TERRAIN
Allows seeing through terrain.

#### EFFECT_ADJUST_UNIT_SEE_THROUGH_FEATURES
Allows seeing through features.

#### EFFECT_ADJUST_UNIT_HIDDEN_VISIBILITY
Adjusts hidden visibility level.

### Abilities

#### EFFECT_UNIT_ADJUST_ABILITY
Grants or adjusts unit ability.

| Argument | Type | Description |
|----------|------|-------------|
| `AbilityType` | TEXT | Ability to grant |

```xml
<Modifier id="EXAMPLE" collection="COLLECTION_PLAYER_UNITS" effect="EFFECT_UNIT_ADJUST_ABILITY">
    <Argument name="AbilityType">ABILITY_AMPHIBIOUS</Argument>
</Modifier>
```

#### EFFECT_ADJUST_UNIT_CIV_UNIQUE_TRADITION_COMBAT_MODIFIER
Adjusts combat modifier from unique tradition.

#### EFFECT_GRANT_UNIT_ABILITY_CHARGE
Grants ability charge.

#### EFFECT_GRANT_UNIT_RESPAWN_CHARGE
Grants respawn charge.

#### EFFECT_GRANT_UNIT_PROMOTION
Grants a promotion.

### Experience & Leveling

#### EFFECT_GRANT_UNIT_EXPERIENCE
Grants XP to unit.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | XP amount |

#### EFFECT_ARMY_ADJUST_EXPERIENCE_RATE
Adjusts XP generation rate for armies.

| Argument | Type | Description |
|----------|------|-------------|
| `Percent` | INTEGER | XP rate modifier |

### Unit Creation & Grants

#### EFFECT_CITY_GRANT_UNIT
Grants a unit to a city.

| Argument | Type | Description |
|----------|------|-------------|
| `UnitType` | TEXT | Unit to grant |

#### EFFECT_PLAYER_GRANT_UNIT_AT_PLOT
Grants unit at a specific plot.

| Argument | Type | Description |
|----------|------|-------------|
| `UnitType` | TEXT | Unit type |
| `Amount` | INTEGER | Number of units |
| `AllowUniqueOverride` | BOOLEAN | Allow unique override |

#### EFFECT_GRANT_UNIT_ON_SUCCESSFUL_UNDETECTED_ESPIONAGE_ACTION
Grants unit on successful espionage.

#### EFFECT_GRANT_UNIT_OF_CLASS_AND_APPLY_ABILITY
Grants unit class with ability.

#### EFFECT_PLAYER_GRANT_NAVAL_UNIT_WITH_OCEAN_ACCESS
Grants naval unit with ocean access.

### Yields from Units

#### EFFECT_GRANT_UNIT_YIELD_PROMOTION
Grants yield from promotion.

#### EFFECT_GRANT_UNIT_YIELD_ADJACENT_NATURAL_WONDERS
Grants yield near natural wonders.

#### EFFECT_GRANT_UNIT_YIELD_PER_RESOURCES_IN_CITY
Grants yield per resources in city.

#### EFFECT_GRANT_UNIT_YIELD_RIVER_LENGTH
Grants yield per river length.

#### EFFECT_UNIT_ADJUST_PLAYER_YIELD
Unit grants player yield.

### Flanking & Tactics

#### EFFECT_UNIT_ADJUST_FLANK_DEFENSE_MODIFIER
Adjusts flank defense.

#### EFFECT_UNIT_ADJUST_FLANKING_ATTACK_MODIFIER
Adjusts flanking attack modifier.

### Fortification

#### EFFECT_UNIT_ADJUST_FORTIFICATION_CONSTRUCTION_TIME
Adjusts fortification build time.

### Unit Properties

#### EFFECT_UNIT_PROPERTY
Sets unit property.

#### EFFECT_ADJUST_UNIT_INITIATION_YIELD
Adjusts initiation yield.

#### EFFECT_ADJUST_UNIT_EMBARKATION_TYPE
Changes embarkation type.

#### EFFECT_ADJUST_UNIT_VALID_UNIT_BUILD
Adjusts valid unit build.

#### EFFECT_UNIT_SET_TRADE_ROUTE_PROTECTED
Sets trade route as protected.

#### EFFECT_UNIT_SET_PRIVATEER
Sets unit as privateer.

### Special Unit Effects

#### EFFECT_UNITS_EXTRA_RANDOM_EVENT_DAMAGE
Adds extra damage from random events.

#### EFFECT_UNITS_IMMUNE_TO_RANDOM_EVENTS
Makes units immune to random events.

#### EFFECT_PREVIOUS_ENGAGEMENT_KILL_BONUS
Grants kill bonus from previous engagement.

---

## Army & Command Effects

#### EFFECT_ARMY_ADJUST_COMMAND_RADIUS
Adjusts command radius.

#### EFFECT_ARMY_ADJUST_MOVEMENT_RATE
Adjusts army movement rate.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Movement rate modifier |

#### EFFECT_ARMY_ADJUST_UNIT_CAPACITY
Adjusts unit capacity.

#### EFFECT_ARMY_ADJUST_UNIT_REINFORCE_SPEED
Adjusts reinforcement speed.

#### EFFECT_ARMY_ADJUST_RESPAWN_SPEED
Adjusts respawn speed.

#### EFFECT_ARMY_GRANT_UNIT
Grants unit to army.

#### EFFECT_ARMY_ADJUST_COMMANDER_ATTACK_DAMAGE
Adjusts commander attack damage.

#### EFFECT_UNIT_ADJUST_COMMAND_AWARD
Adjusts command award.

#### EFFECT_ARMY_ADJUST_AIR_INTERCEPTION_RANGE
Adjusts air interception range.

#### EFFECT_ARMY_ADJUST_NAME
Adjusts army name.

---

## Player Effects

#### EFFECT_PLAYER_ADJUST_SETTLEMENT_CAP
Adjusts settlement capacity.

| Argument | Type | Description |
|----------|------|-------------|
| `Amount` | INTEGER | Settlement cap modifier |

#### EFFECT_PLAYER_ADJUST_UNIT_MAINTENANCE_EFFICIENCY
Adjusts unit maintenance efficiency.

#### EFFECT_PLAYER_ADJUST_UNIT_EXTRA_COPY
Adjusts extra copy of units.

#### EFFECT_PLAYER_ADJUST_WMD_COUNT
Adjusts WMD count.

#### EFFECT_PLAYER_ADJUST_TRADE_CAPACITY
Adjusts trade capacity.

#### EFFECT_ADJUST_PLAYER_TRADE_DURING_WAR
Allows trade during war.

#### EFFECT_ADJUST_PLAYER_TRADE_UNITS_SAFE_DEEP_WATER
Enables safe deep water trade.

### Relic Conversion Effects

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_CAPITAL
Relic conversion in capital.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_CITY_STATE
Relic conversion from city-state.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_CITY_WITH_NATURAL_WONDER
Relic conversion with natural wonder.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_CITY_WITH_X_SPECIALISTS
Relic conversion with specialists.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_HOLY_CITY
Relic conversion in holy city.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_OWNED_CITY_FIRST_TIME
Relic conversion on first owned city.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_RELIGIOUS_BUILDING
Relic conversion from religious building.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_RURAL_POP
Relic conversion from rural population.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_TREASURE_FLEETS
Relic conversion from treasure fleets.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_URBAN_POP
Relic conversion from urban population.

#### EFFECT_ADJUST_PLAYER_RELIC_CONVERTING_WONDER
Relic conversion from wonder.

#### EFFECT_ADJUST_PLAYER_RELIC_NEW_WORLD
New world relic bonus.

---

## Progression & Technology Effects

#### EFFECT_PLAYER_ADJUST_PROGRESSION_TREE_EFFICIENCY
Adjusts research/civic speed.

| Argument | Type | Description |
|----------|------|-------------|
| `ProgressionTreeType` | TEXT | Tree type |
| `Percent` | INTEGER | Speed modifier |

#### EFFECT_PLAYER_ADJUST_PROGRESSION_TREE_MASTERY_EFFICIENCY
Adjusts mastery efficiency.

#### EFFECT_PLAYER_GRANT_PROGRESSION
Grants progression in a tree.

| Argument | Type | Description |
|----------|------|-------------|
| `ProgressionTreeType` | TEXT | Tree type |
| `Amount` | INTEGER | Progress amount |

#### EFFECT_PLAYER_ADJUST_AGE_PROGRESS
Adjusts age progression rate.

#### EFFECT_PLAYER_ADJUST_ARTIFACT_EXTRACTION_SPEED
Adjusts artifact extraction speed.

### Progression Triggers

#### TRIGGER_PLAYER_GRANT_YIELD_ON_PROGRESSION_TREE_NODE_DEPTH_UNLOCKED
Grants yield on node unlock.

#### TRIGGER_PLAYER_ADJUST_YIELD_PER_ATTRIBUTE_TREE_UNLOCKED
Adjusts yield per attribute tree unlocked.

#### TRIGGER_CITY_GRANT_YIELD_ON_PROGRESSION_TREE_NODE_DEPTH_UNLOCKED
City grants yield on node unlock.

#### TRIGGER_PLAYER_GRANT_UNIT_ON_PROGRESSION_TREE_NODE_DEPTH_UNLOCKED
Grants unit on node unlock.

---

## Golden Age & Celebration Effects

#### EFFECT_PLAYER_GRANT_GOLDEN_AGE
Grants golden age.

#### EFFECT_PLAYER_ADJUST_GOLDEN_AGE_DURATION
Adjusts golden age duration.

#### EFFECT_PLAYER_ADD_GOLDEN_AGE_CHOICE
Adds golden age choice.

#### TRIGGER_PLAYER_GRANT_UNIT_ON_GOLDEN_AGE
Grants unit on golden age start.

#### TRIGGER_PLAYER_ADJUST_POPULATION_ON_CELEBRATION_STARTED
Adjusts population on celebration.

---

## Religion & Belief Effects

#### EFFECT_ADD_PANTHEON
Adds a pantheon belief.

#### EFFECT_ADD_BELIEF
Adds a belief.

#### EFFECT_ADD_RELIGIOUS_BELIEF_YIELD
Adds yield from religious belief.

#### EFFECT_ENABLE_RELIGION_AUTO_SPREAD_CONQUERED
Enables auto spread in conquered cities.

#### EFFECT_ENABLE_RELIGION_AUTO_SPREAD_FOREIGN_HEMISPHERE
Enables spread in foreign hemisphere.

#### EFFECT_ENABLE_RELIGION_AUTO_SPREAD_TRADE_ROUTE
Enables spread via trade routes.

#### EFFECT_PLAYER_UNLOCK_PANTHEON
Unlocks a pantheon.

#### EFFECT_ADD_EVENT_FREQUENCY_OVERRIDE
Overrides event frequency.

---

## Diplomacy & Relations Effects

### Diplomatic Actions

#### EFFECT_DIPLOMACY_ADJUST_DIPLOMATIC_ACTION_TYPE_EFFICIENCY
Adjusts diplomatic action efficiency.

| Argument | Type | Description |
|----------|------|-------------|
| `ActionType` | TEXT | Specific action |
| `ActionGroupType` | TEXT | Action group |
| `Percent` | INTEGER | Efficiency modifier |

#### EFFECT_DIPLOMACY_ADJUST_DIPLOMATIC_ACTION_TYPE_EFFICIENCY_PER_GREAT_WORK
Adjusts efficiency per great work.

#### EFFECT_DIPLOMACY_ADJUST_DIPLOMATIC_ACTION_TYPE_EFFICIENCY_PER_SUZERAINED_CITY_STATE_TYPE
Adjusts efficiency per city-state type.

#### EFFECT_DIPLOMACY_ADJUST_DIPLOMATIC_ACTION_TOKEN_BONUS
Adjusts token bonus.

#### EFFECT_DIPLOMACY_ADJUST_DIPLOMATIC_RESPONSE_EFFICIENCY
Adjusts response efficiency.

#### EFFECT_DIPLOMACY_GRANT_DIPLOMATIC_ACTION
Grants a diplomatic action.

### Diplomatic Relationships

#### EFFECT_DIPLOMACY_ADJUST_RELATIONSHIP_GAIN_FROM_EVENT
Adjusts relationship gain from events.

#### EFFECT_DIPLOMACY_AGENDA_ON_WAR_START
Triggers agenda on war start.

#### EFFECT_DIPLOMACY_AGENDA_TIMED_UPDATE
Triggers timed agenda update.

### Envoys & Suzerain

#### EFFECT_GRANT_DIPLOMATIC_ENVOYS
Grants envoys.

#### EFFECT_PLAYER_DIPLOMACY_FAVOR_ACCRUE_MOD
Modifies favor accrual.

#### EFFECT_PLAYER_DIPLOMACY_IGNORE_WAR_REQUIREMENTS
Ignores war requirements.

### Diplomacy Modifiers

#### EFFECT_BASIC_DIPLOMACY_MODS
Applies basic diplomacy modifiers.

#### EFFECT_PLAYER_DIPLOMACY_ACTION_BASIC_DIPLOMACY_MODS
Action-based diplomacy modifiers.

#### EFFECT_PLAYER_DIPLOMACY_ACTION_GROUP_MOD
Action group modifiers.

#### EFFECT_PLAYER_DIPLOMACY_ACTION_PROGRESS_ENVOY_MOD
Envoy progress modifiers.

#### EFFECT_PLAYER_DIPLOMACY_ALLOW_LEVY_FROM_ALL_CITY_STATES
Allows levy from all city-states.

#### EFFECT_PLAYER_DIPLOMACY_CHANGE_TRADE_ROUTE_CAPACITY
Changes trade capacity.

#### EFFECT_PLAYER_DIPLOMACY_ADJUST_HAS_BOOSTED_SUPPORT_EFFECTS
Adjusts support effects.

---

## Discovery & Narrative Effects

#### TRIGGER_PLAYER_GRANT_YIELD_ON_FIND_WONDER
Grants yield on wonder discovery.

#### EFFECT_PLOT_DESTROY_DISCOVERY
Destroys discovery.

#### EFFECT_PLAYER_ADD_YIELD_DEBT_NARRATIVE
Adds yield debt from narrative.

#### EFFECT_PLAYER_GRANT_NARRATIVE_TAG
Grants narrative tag.

#### EFFECT_PLAYER_GRANT_BONUS_STORIES
Grants bonus stories.

#### EFFECT_METAPROGRESSION_SEND_EVENT
Sends meta-progression event.

#### EFFECT_METAPROGRESSION_COMPLETE_CHALLENGE
Completes meta-progression challenge.

---

## Victory & Scoring Effects

#### EFFECT_PLAYER_GRANT_VICTORY_POINT
Grants victory points.

#### EFFECT_PLAYER_ADJUST_LEGACY_PATH_HIGH_YIELD_DISTRICT_SCORING
Adjusts high-yield district scoring.

#### EFFECT_PLAYER_ADJUST_LEGACY_PATH_GREAT_WORK_SCORING
Adjusts great work scoring.

#### EFFECT_PLAYER_ADJUST_LEGACY_PATH_RESOURCE_SCORING
Adjusts resource scoring.

#### EFFECT_PLAYER_ADJUST_LEGACY_PATH_SETTLEMENT_SCORING
Adjusts settlement scoring.

#### EFFECT_PLAYER_ADJUST_LEGACY_PATH_WONDER_SCORING
Adjusts wonder scoring.

---

## Legacy & Attributes Effects

#### EFFECT_PLAYER_ATTRIBUTE
Sets player attribute.

#### EFFECT_CITY_ADJUST_CONSTRUCTIBLE_YIELD_PER_ATTRIBUTE
Adjusts constructible yield per attribute.

#### EFFECT_PLAYER_ADJUST_YIELD_PER_COMPLETED_MASTERY
Adjusts yield per completed mastery.

#### EFFECT_PLAYER_GRANT_LEGACY_POINTS
Grants legacy points.

#### EFFECT_PLAYER_UNLOCK_TRADITION
Unlocks tradition.

| Argument | Type | Description |
|----------|------|-------------|
| `TraditionType` | TEXT | Tradition to unlock |

#### EFFECT_PLAYER_GRANT_TRADITION_SLOTS
Grants tradition slots.

---

## Reveal & Visibility Effects

#### EFFECT_GRANT_REVEAL_TYPE
Reveals specific type.

#### EFFECT_REVEAL_PLOTS
Reveals plots.

#### EFFECT_REVEAL_UNIT_PLOT_FOR_PLAYER
Reveals unit plot for player.

#### EFFECT_PLAYER_REVEAL_CULTURE_TREE
Reveals culture tree.

---

## Government & Policy Effects

#### EFFECT_PLAYER_SET_GOVERNMENT
Sets government type.

#### EFFECT_PLAYER_BLOCK_GOVERNMENT_SELECTION
Blocks government selection.

#### EFFECT_PLAYER_DISABLE_BUILD_UNIT
Disables unit building.

---

## Constructible Unlock & Replace Effects

#### EFFECT_PLAYER_GRANT_CONSTRUCTIBLE_UNLOCK
Unlocks a constructible.

| Argument | Type | Description |
|----------|------|-------------|
| `ConstructibleType` | TEXT | Constructible to unlock |

#### EFFECT_PLAYER_GRANT_UNLOCK
Grants a general unlock.

#### EFFECT_PLAYER_REPLACE_CONSTRUCTIBLE
Replaces a constructible.

#### EFFECT_ADJUST_TOWN_CAN_PURCHASE_TAGGED_CONSTRUCTIBLES
Allows towns to purchase tagged constructibles.

---

## Espionage & DAE Effects

DAE (Diplomatic Action & Espionage) effects control espionage operations and diplomatic actions.

### DAE Frameworks

#### EFFECT_DAE_COMPLETE_APPLY_BEHAVIOR
Applies behavior on completion.

#### EFFECT_DAE_DESCRIBE_GRANT_COOPERATIVE_YIELD
Describes cooperative yield grant.

#### EFFECT_DAE_DESCRIBE_IMPROVE_TRADE_RELATIONS
Describes trade relations improvement.

#### EFFECT_DAE_DESCRIBE_GRANT_FAVORS_GRIEVANCES
Describes favors/grievances grant.

#### EFFECT_DAE_DESCRIBE_INDEPENDENT_AND_TARGET
Describes independent action and target.

### Espionage Operations

#### EFFECT_DAE_ESPIONAGE_ADJUST_CITY_CONSTRUCTIBLE_PRODUCTION
Adjusts city constructible production via espionage.

#### EFFECT_DAE_ESPIONAGE_ADJUST_CITY_PROJECT_PRODUCTION
Adjusts city project production.

#### EFFECT_DAE_ESPIONAGE_ATTACH_MODIFIER
Attaches modifier via espionage.

#### EFFECT_DAE_ESPIONAGE_BLOCK_CITY_GREAT_WORKS
Blocks city great works.

#### EFFECT_DAE_ESPIONAGE_CONVERT_SETTLEMENT
Converts settlement.

#### EFFECT_DAE_ESPIONAGE_DESTROY_TARGET_INFRASTRUCTURE
Destroys target infrastructure.

#### EFFECT_DAE_ESPIONAGE_DESTROY_TARGET_INFRASTRUCTURE_WITH_RESOURCE_TYPE
Destroys infrastructure with resource.

#### EFFECT_DAE_ESPIONAGE_DISABLE_SETTLEMENT_WORKERS
Disables settlement workers.

#### EFFECT_DAE_ESPIONAGE_GRANT_TARGET_COMMANDER_VISION
Grants vision of target commander.

#### EFFECT_DAE_ESPIONAGE_SPAWN_TREASURE_FLEET
Spawns treasure fleet.

#### EFFECT_DAE_ESPIONAGE_STEAL_TREE_NODE
Steals tree node.

### Cooperative Actions

#### EFFECT_DAE_COOPERATIVE_ATTACH_MODIFIER
Attaches cooperative modifier.

#### EFFECT_DAE_START_GRANT_COOPERATIVE_YIELD
Starts cooperative yield grant.

#### EFFECT_DAE_COMPLETE_GRANT_COOPERATIVE_YIELD
Completes cooperative yield grant.

#### EFFECT_DAE_GRANT_COOPERATIVE_YIELD_SUPPORT_BASE_PER_TURN
Grants base yield per turn.

### Diplomatic Support

#### EFFECT_DAE_APPLY_NON_AGGRESSION_PACT
Applies non-aggression pact.

#### EFFECT_DAE_COMPLETE_IMPROVE_TRADE_RELATIONS
Completes trade relations improvement.

#### EFFECT_DAE_COMPLETE_GRANT_FAVORS_GRIEVANCES
Completes favors/grievances grant.

#### EFFECT_DAE_COMPLETE_FREE_CAPTURED_COMMANDER
Frees captured commander.

#### EFFECT_DAE_FREE_CAPTURED_COMMANDER_CHANGE_INITIATOR_GOLD
Changes gold on commander freedom.

#### EFFECT_DAE_GRANT_CAPITAL_DISTRICT_VISION
Grants capital district vision.

### Other DAE Effects

#### EFFECT_DAE_ADJUST_SETTLEMENT_YIELD
Adjusts settlement yield.

#### EFFECT_DAE_CHANGE_INDEPENDENT_AT_LOCATION_INVESTMENT
Changes independent investment.

#### EFFECT_DAE_TARGET_ATTACH_MODIFIER
Attaches modifier to target.

#### EFFECT_DAE_SEND_GOLD
Sends gold.

#### EFFECT_DAE_TRADE_MAP
Trades map information.

#### EFFECT_DAE_LEVY_UNIT
Levies unit from city-state.

#### EFFECT_DAE_INCORPORATE_CITY_STATE
Incorporates city-state.

---

## City State & Independent Effects

#### EFFECT_ELEVATE_INDEPENDENT
Elevates independent faction.

#### EFFECT_CHANGE_INDEPENDENT_AT_LOCATION_INVESTMENT
Changes independent investment at location.

#### EFFECT_GRANT_CITY_FROM_CITY_STATE_PLOT
Grants city from city-state plot.

#### EFFECT_GRANT_SUZERAIN_UNIT_PLOT
Grants unit at suzerain plot.

---

## Economic & Resource Effects

#### EFFECT_PLAYER_ADJUST_YIELD_FOR_RESOURCE
Adjusts yield for specific resources.

#### EFFECT_ADJUST_PLAYER_YIELD_FOR_RESOURCE
Alternative resource yield adjustment.

#### EFFECT_ADJUST_PLAYER_YIELD_PER_SLOTTED_RESOURCE
Adjusts yield per slotted resource.

---

## Special & Miscellaneous Effects

### Utility

#### EFFECT_DO_NOTHING
No operation (placeholder).

#### EFFECT_ATTACH_MODIFIERS
Attaches other modifiers.

#### EFFECT_GRANT_PRODUCTION_IN_CITY
Grants production to city.

#### EFFECT_TELEPORT_UNIT
Teleports unit.

#### EFFECT_GRANT_WALLS
Grants walls to city.

| Argument | Type | Description |
|----------|------|-------------|
| `WallLevel` | INTEGER | Wall level |

#### EFFECT_PLAYER_OPEN_ARCHAEOLOGY
Opens archaeology.

### Property System

#### EFFECT_PLAYER_PROPERTY
Sets player property.

### Yield Debt & Treasury

#### EFFECT_PLAYER_DEDUCT_TREASURY
Deducts from treasury.

#### EFFECT_PLAYER_DEDUCT_TREASURY_DISCOVERY
Deducts for discovery.

#### EFFECT_PLAYER_DEDUCT_TREASURY_NARRATIVE
Deducts for narrative.

### Military Operations

#### EFFECT_PLAYER_FORCE_MILITARY_OPERATION
Forces military operation.

#### EFFECT_PLAYER_DIPLOMACY_INCITE_RAID
Incites raid.

### Advanced Start

#### EFFECT_ADVANCED_START_ADD_ARMY
Adds army at advanced start.

#### EFFECT_ADVANCED_START_MEET_PLAYERS
Meets players at advanced start.

### Age Transitions

#### EFFECT_AGE_TRANSITION_PLAYER_RESPAWN_COAST
Respawns player on coast during age transition.

### War Effects

#### EFFECT_ADJUST_WAR_SUPPORT_BONUS
Adjusts war support bonus.

#### EFFECT_ADJUST_WAR_WEARINESS_YIELD_PENALTY
Adjusts war weariness yield penalty.

#### EFFECT_PLAYER_WAR_DIPLOMATIC_RELATIONSHIP_CHANGE
Changes war diplomatic relationship.

### Exhibit & Great Works

#### EFFECT_GRANT_EXHIBIT_GREAT_WORK
Grants exhibit great work.

### Achievement Bonuses

#### EFFECT_GRANT_RETURNED_CITIES_BONUS
Grants bonus for returned cities.

### Trigger Effects

#### TRIGGER_CITY_GRANT_YIELD_ON_ADD_PLOT
Grants yield when plot added.

#### TRIGGER_PLAYER_GRANT_YIELD_ON_MASTERY_COMPLETED
Grants yield on mastery completion.

#### TRIGGER_PLAYER_GRANT_YIELD_ON_UNIT_CREATED
Grants yield on unit creation.

#### TRIGGER_CITY_GRANT_YIELD_ON_CONSTRUCTIBLE_CREATED
Grants yield on constructible creation.

---

## Finding More Effects

To discover all available effects in your game version:

1. Search game files for `EffectType="EFFECT_` or `effect="EFFECT_`
2. Check `gameplay-copy.sqlite` in the debug folder
3. Look at `*-gameeffects.xml` files in `modules/base-standard/data/`
4. Use SQL: `SELECT DISTINCT EffectType FROM DynamicModifiers;`

## Common Argument Types

| Argument | Description | Examples |
|----------|-------------|----------|
| `YieldType` | Yield type reference | `YIELD_FOOD`, `YIELD_PRODUCTION`, `YIELD_GOLD`, `YIELD_SCIENCE`, `YIELD_CULTURE` |
| `Amount` | Integer modifier value | `1`, `2`, `5`, `-1` |
| `Percent` | Percentage modifier | `10`, `25`, `50`, `100` |
| `UnitType` | Unit type reference | `UNIT_SCOUT`, `UNIT_WARRIOR` |
| `AbilityType` | Ability reference | `ABILITY_AMPHIBIOUS`, `ABILITY_IGNORE_ZOC` |
| `ConstructibleType` | Building/wonder reference | `BUILDING_LIBRARY`, `WONDER_PYRAMIDS` |
| `Tag` | Content tag filter | `SCIENCE`, `MILITARY`, `RELIGIOUS` |
| `Tooltip` | Localization key | `LOC_MODIFIER_TOOLTIP_*` |

## See Also

- [Collection Types](Collection-Types.md) - What entities modifiers target
- [Requirement Types](Requirement-Types.md) - Conditional requirements
- [Modifier System Overview](Overview.md) - How modifiers work
- [Cross-Reference Index](../Index.md) - Quick lookup for all types
