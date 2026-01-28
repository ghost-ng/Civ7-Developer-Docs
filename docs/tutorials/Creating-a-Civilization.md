# Tutorial: Creating a New Civilization

This tutorial walks through creating a complete playable civilization for Civilization VII, including unique abilities, a leader, starting bias, city names, and all required database entries.

## What You'll Learn

- Registering civilization and leader types
- Creating unique civilization abilities with modifiers
- Defining leader traits and agendas
- Setting up starting bias for terrain preferences
- Adding city names and localization
- Linking everything together

## Example: The Kingdom of Carthage

We'll create Carthage as a new Antiquity Age civilization focused on trade and naval power, with Hannibal as the leader.

## File Structure

```
my-carthage-civ/
├── data/
│   ├── civilizations.xml
│   ├── leaders.xml
│   ├── game-effects.xml
│   └── city-names.xml
├── text/
│   └── text.xml
└── my-carthage-civ.modinfo
```

## Step 1: Create the .modinfo File

**my-carthage-civ.modinfo**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-carthage-civ" version="1" xmlns="ModInfo">
    <Properties>
        <Name>Carthage Civilization</Name>
        <Description>Adds Carthage as a playable civilization led by Hannibal.</Description>
        <Authors>Your Name</Authors>
        <AffectsSavedGames>1</AffectsSavedGames>
    </Properties>
    <Dependencies>
    </Dependencies>
    <ActionCriteria>
        <Criteria id="antiquity-age">
            <AgeInUse>AGE_ANTIQUITY</AgeInUse>
        </Criteria>
        <Criteria id="always">
            <AlwaysMet></AlwaysMet>
        </Criteria>
    </ActionCriteria>
    <ActionGroups>
        <!-- Shell scope for leader/civ selection screens -->
        <ActionGroup id="shell-content" scope="shell" criteria="always">
            <Actions>
                <UpdateDatabase>
                    <Item>data/civilizations.xml</Item>
                    <Item>data/leaders.xml</Item>
                </UpdateDatabase>
                <UpdateText>
                    <Item>text/text.xml</Item>
                </UpdateText>
            </Actions>
        </ActionGroup>

        <!-- Game scope for gameplay data -->
        <ActionGroup id="antiquity-game" scope="game" criteria="antiquity-age">
            <Actions>
                <UpdateDatabase>
                    <Item>data/civilizations.xml</Item>
                    <Item>data/leaders.xml</Item>
                    <Item>data/game-effects.xml</Item>
                    <Item>data/city-names.xml</Item>
                </UpdateDatabase>
                <UpdateText>
                    <Item>text/text.xml</Item>
                </UpdateText>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

### Important: Two Database Scopes

- **Shell scope**: Needed for the civilization/leader to appear in game setup menus
- **Game scope**: Needed for actual gameplay mechanics

## Step 2: Define the Civilization

**data/civilizations.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register types -->
    <Types>
        <Row Type="CIVILIZATION_CARTHAGE" Kind="KIND_CIVILIZATION"/>
        <Row Type="TRAIT_CARTHAGE" Kind="KIND_TRAIT"/>
        <Row Type="TRAIT_CARTHAGE_ABILITY" Kind="KIND_TRAIT"/>
    </Types>

    <!-- 2. Define the civilization -->
    <Civilizations>
        <Row
            CivilizationType="CIVILIZATION_CARTHAGE"
            Name="LOC_CIVILIZATION_CARTHAGE_NAME"
            FullName="LOC_CIVILIZATION_CARTHAGE_FULLNAME"
            Description="LOC_CIVILIZATION_CARTHAGE_DESCRIPTION"
            Adjective="LOC_CIVILIZATION_CARTHAGE_ADJECTIVE"
            StartingCivilizationLevelType="CIVILIZATION_LEVEL_FULL_CIV"
            CapitalName="LOC_CITY_NAME_CARTHAGE_CAPITAL"
            RandomCityNameDepth="15"
        />
    </Civilizations>

    <!-- 3. Define traits -->
    <Traits>
        <!-- Internal identifier trait -->
        <Row
            TraitType="TRAIT_CARTHAGE"
            Name="LOC_TRAIT_CARTHAGE_NAME"
            Description="LOC_TRAIT_CARTHAGE_DESCRIPTION"
            InternalOnly="true"
        />
        <!-- Unique ability trait (visible to player) -->
        <Row
            TraitType="TRAIT_CARTHAGE_ABILITY"
            Name="LOC_TRAIT_CARTHAGE_ABILITY_NAME"
            Description="LOC_TRAIT_CARTHAGE_ABILITY_DESCRIPTION"
            InternalOnly="false"
        />
    </Traits>

    <!-- 4. Link traits to civilization -->
    <CivilizationTraits>
        <!-- Age trait - required for Antiquity civs -->
        <Row CivilizationType="CIVILIZATION_CARTHAGE" TraitType="TRAIT_ANTIQUITY_CIV"/>
        <!-- Internal identifier -->
        <Row CivilizationType="CIVILIZATION_CARTHAGE" TraitType="TRAIT_CARTHAGE"/>
        <!-- Unique ability -->
        <Row CivilizationType="CIVILIZATION_CARTHAGE" TraitType="TRAIT_CARTHAGE_ABILITY"/>
        <!-- Attribute bonuses (pick 2) -->
        <Row CivilizationType="CIVILIZATION_CARTHAGE" TraitType="TRAIT_ATTRIBUTE_ECONOMIC"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" TraitType="TRAIT_ATTRIBUTE_MILITARISTIC"/>
    </CivilizationTraits>

    <!-- 5. Link ability trait to modifiers -->
    <TraitModifiers>
        <Row TraitType="TRAIT_CARTHAGE_ABILITY" ModifierId="MOD_CARTHAGE_TRADE_GOLD"/>
        <Row TraitType="TRAIT_CARTHAGE_ABILITY" ModifierId="MOD_CARTHAGE_NAVAL_PRODUCTION"/>
    </TraitModifiers>

    <!-- 6. Starting bias (terrain preferences) -->
    <StartBiases>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" TerrainType="TERRAIN_COAST" Tier="1"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" FeatureType="FEATURE_NAVIGABLE_RIVER" Tier="2"/>
    </StartBiases>

    <!-- 7. Favored wonders -->
    <CivilizationFavoredWonders>
        <Row CivilizationType="CIVILIZATION_CARTHAGE"
             FavoredWonderType="WONDER_COLOSSUS"
             FavoredWonderName="LOC_WONDER_COLOSSUS_NAME"/>
    </CivilizationFavoredWonders>
</Database>
```

### Civilization Traits Explained

| Trait Type | Purpose |
|------------|---------|
| `TRAIT_ANTIQUITY_CIV` | Shared by all Antiquity civs, provides base mechanics |
| `TRAIT_[CIV]` | Internal identifier for the civ |
| `TRAIT_[CIV]_ABILITY` | The unique ability shown in UI |
| `TRAIT_ATTRIBUTE_*` | Attribute bonuses (pick 2 for balance) |

### Available Attributes

| Attribute | Focus |
|-----------|-------|
| `TRAIT_ATTRIBUTE_CULTURAL` | Culture, Great Works |
| `TRAIT_ATTRIBUTE_ECONOMIC` | Gold, Trade |
| `TRAIT_ATTRIBUTE_EXPANSIONIST` | Settlers, Growth |
| `TRAIT_ATTRIBUTE_MILITARISTIC` | Units, Combat |
| `TRAIT_ATTRIBUTE_POLITICAL` | Diplomacy, Influence |
| `TRAIT_ATTRIBUTE_SCIENTIFIC` | Science, Research |

### Starting Bias Tiers

| Tier | Strength |
|------|----------|
| 1 | Strong preference |
| 2 | Moderate preference |
| 3 | Slight preference |
| 4 | Weak preference |
| 5 | Very weak preference |

## Step 3: Define the Leader

**data/leaders.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register types -->
    <Types>
        <Row Type="LEADER_HANNIBAL" Kind="KIND_LEADER"/>
        <Row Type="TRAIT_HANNIBAL_ABILITY" Kind="KIND_TRAIT"/>
        <Row Type="AGENDA_HANNIBAL" Kind="KIND_AGENDA"/>
    </Types>

    <!-- 2. Define the leader -->
    <Leaders>
        <Row
            LeaderType="LEADER_HANNIBAL"
            Name="LOC_LEADER_HANNIBAL_NAME"
            InheritFrom="LEADER_DEFAULT"
        />
    </Leaders>

    <!-- 3. Define leader trait -->
    <Traits>
        <Row
            TraitType="TRAIT_HANNIBAL_ABILITY"
            Name="LOC_TRAIT_HANNIBAL_ABILITY_NAME"
            Description="LOC_TRAIT_HANNIBAL_ABILITY_DESCRIPTION"
            InternalOnly="false"
        />
    </Traits>

    <!-- 4. Link trait to leader -->
    <LeaderTraits>
        <Row LeaderType="LEADER_HANNIBAL" TraitType="TRAIT_HANNIBAL_ABILITY"/>
    </LeaderTraits>

    <!-- 5. Link trait to modifiers -->
    <TraitModifiers>
        <Row TraitType="TRAIT_HANNIBAL_ABILITY" ModifierId="MOD_HANNIBAL_COMBAT_MOUNTAINS"/>
        <Row TraitType="TRAIT_HANNIBAL_ABILITY" ModifierId="MOD_HANNIBAL_UNIT_EXPERIENCE"/>
    </TraitModifiers>

    <!-- 6. Link leader to civilization -->
    <CivilizationLeaders>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" LeaderType="LEADER_HANNIBAL"/>
    </CivilizationLeaders>

    <!-- 7. Set civ priorities for AI -->
    <LeaderCivPriorities>
        <Row Leader="LEADER_HANNIBAL" Civilization="CIVILIZATION_CARTHAGE" Priority="4"/>
        <Row Leader="LEADER_HANNIBAL" Civilization="CIVILIZATION_PERSIA" Priority="2"/>
        <Row Leader="LEADER_HANNIBAL" Civilization="CIVILIZATION_ROME" Priority="1"/>
    </LeaderCivPriorities>

    <!-- 8. Define AI agenda -->
    <Agendas>
        <Row
            AgendaType="AGENDA_HANNIBAL"
            Name="LOC_AGENDA_HANNIBAL_NAME"
            Description="LOC_AGENDA_HANNIBAL_DESCRIPTION"
        />
    </Agendas>

    <!-- 9. Link agenda to leader -->
    <LeaderAgendas>
        <Row LeaderType="LEADER_HANNIBAL" AgendaType="AGENDA_HANNIBAL"/>
    </LeaderAgendas>
</Database>
```

## Step 4: Create the Ability Modifiers

**data/game-effects.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- ========================================
         CIVILIZATION ABILITY: Masters of Trade
         ======================================== -->

    <!-- +2 Gold from Trade Routes -->
    <Modifier id="MOD_CARTHAGE_TRADE_GOLD"
              collection="COLLECTION_PLAYER_TRADE_ROUTES"
              effect="EFFECT_TRADE_ROUTE_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="Amount">2</Argument>
        <String context="Description">LOC_TRAIT_CARTHAGE_ABILITY_DESCRIPTION</String>
    </Modifier>

    <!-- +25% Production towards Naval units -->
    <Modifier id="MOD_CARTHAGE_NAVAL_PRODUCTION"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_UNIT_TAG_ERA_PRODUCTION">
        <Argument name="UnitTag">UNIT_CLASS_NAVAL</Argument>
        <Argument name="Amount">25</Argument>
    </Modifier>

    <!-- ========================================
         LEADER ABILITY: Crossing the Alps
         ======================================== -->

    <!-- +5 Combat Strength for units in hills/mountains -->
    <Modifier id="MOD_HANNIBAL_COMBAT_MOUNTAINS"
              collection="COLLECTION_PLAYER_UNITS"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH">
        <SubjectRequirements type="REQUIREMENTSET_TEST_ANY">
            <Requirement type="REQUIREMENT_PLOT_TERRAIN_TYPE_MATCHES">
                <Argument name="TerrainType">TERRAIN_HILL</Argument>
            </Requirement>
            <Requirement type="REQUIREMENT_PLOT_FEATURE_TYPE_MATCHES">
                <Argument name="FeatureType">FEATURE_MOUNTAIN_PASSABLE</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="Amount">5</Argument>
        <String context="Description">LOC_TRAIT_HANNIBAL_ABILITY_DESCRIPTION</String>
    </Modifier>

    <!-- +25% Experience gain for military units -->
    <Modifier id="MOD_HANNIBAL_UNIT_EXPERIENCE"
              collection="COLLECTION_PLAYER_UNITS"
              effect="EFFECT_ADJUST_UNIT_EXPERIENCE_MODIFIER">
        <SubjectRequirements type="REQUIREMENTSET_TEST_ALL">
            <Requirement type="REQUIREMENT_UNIT_TAG_MATCHES">
                <Argument name="Tag">UNIT_CLASS_COMBAT</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="Amount">25</Argument>
    </Modifier>
</GameEffects>
```

### Common Civ/Leader Effects

| Effect | Description |
|--------|-------------|
| `EFFECT_CITY_ADJUST_YIELD` | Flat yield bonus to cities |
| `EFFECT_PLOT_ADJUST_YIELD` | Bonus to specific plots |
| `EFFECT_TRADE_ROUTE_ADJUST_YIELD` | Trade route bonuses |
| `EFFECT_ADJUST_UNIT_COMBAT_STRENGTH` | Combat bonuses |
| `EFFECT_CITY_ADJUST_UNIT_TAG_ERA_PRODUCTION` | Production bonus for unit types |
| `EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD` | Building/wonder bonuses |

## Step 5: Add City Names

**data/city-names.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <CivilizationCityNames>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_1" SortIndex="1"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_2" SortIndex="2"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_3" SortIndex="3"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_4" SortIndex="4"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_5" SortIndex="5"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_6" SortIndex="6"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_7" SortIndex="7"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_8" SortIndex="8"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_9" SortIndex="9"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_10" SortIndex="10"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_11" SortIndex="11"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_12" SortIndex="12"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_13" SortIndex="13"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_14" SortIndex="14"/>
        <Row CivilizationType="CIVILIZATION_CARTHAGE" CityName="LOC_CITY_NAME_CARTHAGE_15" SortIndex="15"/>
    </CivilizationCityNames>
</Database>
```

## Step 6: Add Localization

**text/text.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <!-- Civilization -->
        <Row Tag="LOC_CIVILIZATION_CARTHAGE_NAME" Language="en_US">
            <Text>Carthage</Text>
        </Row>
        <Row Tag="LOC_CIVILIZATION_CARTHAGE_FULLNAME" Language="en_US">
            <Text>Carthaginian Empire</Text>
        </Row>
        <Row Tag="LOC_CIVILIZATION_CARTHAGE_DESCRIPTION" Language="en_US">
            <Text>A great maritime trading empire of the ancient Mediterranean.</Text>
        </Row>
        <Row Tag="LOC_CIVILIZATION_CARTHAGE_ADJECTIVE" Language="en_US">
            <Text>Carthaginian</Text>
        </Row>

        <!-- Civilization Traits -->
        <Row Tag="LOC_TRAIT_CARTHAGE_NAME" Language="en_US">
            <Text>Carthage</Text>
        </Row>
        <Row Tag="LOC_TRAIT_CARTHAGE_DESCRIPTION" Language="en_US">
            <Text>Carthaginian civilization</Text>
        </Row>
        <Row Tag="LOC_TRAIT_CARTHAGE_ABILITY_NAME" Language="en_US">
            <Text>Masters of Trade</Text>
        </Row>
        <Row Tag="LOC_TRAIT_CARTHAGE_ABILITY_DESCRIPTION" Language="en_US">
            <Text>+2[icon:YIELD_GOLD] Gold from Trade Routes. +25% [icon:YIELD_PRODUCTION] Production towards Naval units.</Text>
        </Row>

        <!-- Leader -->
        <Row Tag="LOC_LEADER_HANNIBAL_NAME" Language="en_US">
            <Text>Hannibal</Text>
        </Row>
        <Row Tag="LOC_TRAIT_HANNIBAL_ABILITY_NAME" Language="en_US">
            <Text>Crossing the Alps</Text>
        </Row>
        <Row Tag="LOC_TRAIT_HANNIBAL_ABILITY_DESCRIPTION" Language="en_US">
            <Text>+5[icon:STAT_COMBAT] Combat Strength for units fighting in Hills or Mountains. Military units earn experience 25% faster.</Text>
        </Row>

        <!-- Agenda -->
        <Row Tag="LOC_AGENDA_HANNIBAL_NAME" Language="en_US">
            <Text>Eternal Rival</Text>
        </Row>
        <Row Tag="LOC_AGENDA_HANNIBAL_DESCRIPTION" Language="en_US">
            <Text>Respects civilizations with strong militaries. Dislikes those who rely solely on diplomacy.</Text>
        </Row>

        <!-- Capital -->
        <Row Tag="LOC_CITY_NAME_CARTHAGE_CAPITAL" Language="en_US">
            <Text>Carthage</Text>
        </Row>

        <!-- City Names -->
        <Row Tag="LOC_CITY_NAME_CARTHAGE_1" Language="en_US"><Text>Carthage</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_2" Language="en_US"><Text>Utica</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_3" Language="en_US"><Text>Hippo Regius</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_4" Language="en_US"><Text>Gades</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_5" Language="en_US"><Text>Saguntum</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_6" Language="en_US"><Text>Carthago Nova</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_7" Language="en_US"><Text>Panormus</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_8" Language="en_US"><Text>Lilybaeum</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_9" Language="en_US"><Text>Hadrumetum</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_10" Language="en_US"><Text>Leptis Magna</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_11" Language="en_US"><Text>Thapsus</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_12" Language="en_US"><Text>Malaca</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_13" Language="en_US"><Text>Tingis</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_14" Language="en_US"><Text>Cirta</Text></Row>
        <Row Tag="LOC_CITY_NAME_CARTHAGE_15" Language="en_US"><Text>Zama</Text></Row>
    </LocalizedText>
</Database>
```

## Step 7: Install and Test

1. Copy your mod folder to:
   - **Windows**: `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Mods\`

2. Launch the game and enable the mod

3. In game setup, you should see Hannibal as an available leader and Carthage as a civilization

4. Start a new game and verify:
   - Starting position near coast
   - Trade route gold bonus working
   - Combat bonuses on hills

## Advanced: Adding a Unique Unit

Add to your civilizations.xml:

```xml
<Types>
    <Row Type="UNIT_SACRED_BAND" Kind="KIND_UNIT"/>
</Types>

<Units>
    <Row UnitType="UNIT_SACRED_BAND" ... />
</Units>

<!-- Make it unique to Carthage -->
<CivilizationUniqueUnits>
    <Row CivilizationType="CIVILIZATION_CARTHAGE"
         UnitType="UNIT_SACRED_BAND"
         ReplacesUnitType="UNIT_WARRIOR"/>
</CivilizationUniqueUnits>
```

## Advanced: Adding a Unique Building

```xml
<Types>
    <Row Type="BUILDING_COTHON" Kind="KIND_BUILDING"/>
</Types>

<Buildings>
    <Row BuildingType="BUILDING_COTHON" ... />
</Buildings>

<CivilizationUniqueBuildings>
    <Row CivilizationType="CIVILIZATION_CARTHAGE"
         BuildingType="BUILDING_COTHON"
         ReplacesBuildingType="BUILDING_HARBOR"/>
</CivilizationUniqueBuildings>
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Civ not in selection | Check shell scope ActionGroup loads data |
| Ability not working | Verify ModifierId links in TraitModifiers |
| Wrong starting location | Check StartBiases tier values |
| Leader not linked | Check CivilizationLeaders table |
| Text not showing | Verify LOC_ tags match between XML files |

## Required Tables Checklist

- [ ] Types (civilization, traits, leader, agenda)
- [ ] Civilizations
- [ ] Traits
- [ ] CivilizationTraits
- [ ] TraitModifiers
- [ ] StartBiases
- [ ] Leaders
- [ ] LeaderTraits
- [ ] CivilizationLeaders
- [ ] LeaderCivPriorities
- [ ] Agendas
- [ ] LeaderAgendas
- [ ] CivilizationCityNames
- [ ] LocalizedText

## Next Steps

- [Civilizations Database](../database/Civilizations.md)
- [Leaders Database](../database/Leaders.md)
- [Unique Abilities Guide](../civilization-creation/Unique-Abilities.md)
- [Starting Bias Guide](../civilization-creation/Starting-Bias.md)
- [AI Behavior Guide](../civilization-creation/AI-Behavior.md)
