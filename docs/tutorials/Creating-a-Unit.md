# Tutorial: Creating a New Unit

This tutorial walks through creating a complete custom unit for Civilization VII, including database entries, combat stats, abilities, icons, and localization.

## What You'll Learn

- Registering unit types in the database
- Defining unit stats and movement
- Adding combat abilities
- Making units trainable from buildings/techs
- Creating unit icons
- Adding localized text

## Example: The Elite Guard

We'll create an "Elite Guard" unit - a powerful melee infantry that replaces the standard Warrior and has a bonus against other melee units.

## File Structure

```
my-elite-guard/
├── data/
│   ├── units.xml
│   └── game-effects.xml
├── icons/
│   └── icons.xml
├── text/
│   └── text.xml
└── my-elite-guard.modinfo
```

## Step 1: Create the .modinfo File

**my-elite-guard.modinfo**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-elite-guard" version="1" xmlns="ModInfo">
    <Properties>
        <Name>Elite Guard Unit</Name>
        <Description>Adds the Elite Guard, a powerful melee infantry unit.</Description>
        <Authors>Your Name</Authors>
        <AffectsSavedGames>1</AffectsSavedGames>
    </Properties>
    <Dependencies>
    </Dependencies>
    <ActionCriteria>
        <Criteria id="antiquity-age">
            <AgeInUse>AGE_ANTIQUITY</AgeInUse>
        </Criteria>
    </ActionCriteria>
    <ActionGroups>
        <ActionGroup id="antiquity-game" scope="game" criteria="antiquity-age">
            <Actions>
                <UpdateDatabase>
                    <Item>data/units.xml</Item>
                    <Item>data/game-effects.xml</Item>
                </UpdateDatabase>
                <UpdateText>
                    <Item>text/text.xml</Item>
                </UpdateText>
                <UpdateIcons>
                    <Item>icons/icons.xml</Item>
                </UpdateIcons>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

## Step 2: Define the Unit Database Entries

**data/units.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register the unit type -->
    <Types>
        <Row Type="UNIT_ELITE_GUARD" Kind="KIND_UNIT"/>
        <Row Type="ABILITY_ELITE_GUARD_BONUS" Kind="KIND_ABILITY"/>
    </Types>

    <!-- 2. Define the unit -->
    <Units>
        <Row
            UnitType="UNIT_ELITE_GUARD"
            Name="LOC_UNIT_ELITE_GUARD_NAME"
            Description="LOC_UNIT_ELITE_GUARD_DESCRIPTION"
            BaseSightRange="2"
            BaseMoves="2"
            UnitMovementClass="UNIT_MOVEMENT_CLASS_FOOT"
            Domain="DOMAIN_LAND"
            CoreClass="CORE_CLASS_MILITARY"
            FormationClass="FORMATION_CLASS_LAND_COMBAT"
            ZoneOfControl="true"
            CanTrain="true"
            CanPurchase="true"
            CanEarnExperience="true"
            CanBeDamaged="true"
            ManualDelete="false"
        />
    </Units>

    <!-- 3. Set combat stats -->
    <Unit_Stats>
        <Row UnitType="UNIT_ELITE_GUARD" Combat="30"/>
    </Unit_Stats>

    <!-- 4. Assign unit class tags -->
    <TypeTags>
        <Row Type="UNIT_ELITE_GUARD" Tag="UNIT_CLASS_COMBAT"/>
        <Row Type="UNIT_ELITE_GUARD" Tag="UNIT_CLASS_MELEE"/>
        <Row Type="UNIT_ELITE_GUARD" Tag="UNIT_CLASS_INFANTRY"/>
        <Row Type="UNIT_ELITE_GUARD" Tag="UNIT_CLASS_HEAVY"/>
    </TypeTags>

    <!-- 5. Define the ability -->
    <UnitAbilities>
        <Row
            UnitAbilityType="ABILITY_ELITE_GUARD_BONUS"
            Name="LOC_ABILITY_ELITE_GUARD_BONUS_NAME"
            Description="LOC_ABILITY_ELITE_GUARD_BONUS_DESCRIPTION"
            Inactive="false"
        />
    </UnitAbilities>

    <!-- 6. Link ability to unit -->
    <Unit_Abilities>
        <Row UnitType="UNIT_ELITE_GUARD" UnitAbilityType="ABILITY_ELITE_GUARD_BONUS"/>
    </Unit_Abilities>

    <!-- 7. Unlock from technology -->
    <ProgressionTreeNodeUnlocks>
        <Row
            ProgressionTreeNodeType="NODE_TECH_AQ_BRONZE_WORKING"
            TargetKind="KIND_UNIT"
            TargetType="UNIT_ELITE_GUARD"
            UnlockDepth="1"
        />
    </ProgressionTreeNodeUnlocks>

    <!-- 8. Set costs (production and purchase) -->
    <Unit_Costs>
        <Row UnitType="UNIT_ELITE_GUARD" YieldType="YIELD_PRODUCTION" Cost="65"/>
        <Row UnitType="UNIT_ELITE_GUARD" YieldType="YIELD_GOLD" Cost="260"/>
    </Unit_Costs>

    <!-- 9. Link ability to modifier -->
    <UnitAbilityModifiers>
        <Row UnitAbilityType="ABILITY_ELITE_GUARD_BONUS" ModifierId="MOD_ELITE_GUARD_VS_MELEE"/>
    </UnitAbilityModifiers>
</Database>
```

### Key Tables Explained

| Table | Purpose |
|-------|---------|
| `Types` | Registers the unit and ability in the type system |
| `Units` | Core unit properties (movement, domain, classes) |
| `Unit_Stats` | Combat strength values |
| `TypeTags` | Classification tags for game logic |
| `UnitAbilities` | Defines named abilities |
| `Unit_Abilities` | Links abilities to units |
| `ProgressionTreeNodeUnlocks` | Tech/civic unlock requirements |
| `Unit_Costs` | Production and purchase costs (uses `YieldType` and `Cost` columns) |

> **Note:** The `Unit_Costs` table is a single table for all unit costs. Use `YIELD_PRODUCTION` for build cost and `YIELD_GOLD` for purchase cost. Maintenance is set via the `Maintenance` column on the `Units` row itself.

## Step 3: Create the Combat Modifier

**data/game-effects.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- +5 Combat Strength vs Melee units -->
    <Modifier id="MOD_ELITE_GUARD_VS_MELEE"
              collection="COLLECTION_UNIT_COMBAT"
              effect="EFFECT_ADJUST_UNIT_COMBAT_STRENGTH">
        <SubjectRequirements type="REQUIREMENTSET_TEST_ALL">
            <Requirement type="REQUIREMENT_OPPONENT_UNIT_TAG_MATCHES">
                <Argument name="Tag">UNIT_CLASS_MELEE</Argument>
            </Requirement>
        </SubjectRequirements>
        <Argument name="Amount">5</Argument>
        <String context="Preview">LOC_ABILITY_ELITE_GUARD_BONUS_PREVIEW</String>
    </Modifier>
</GameEffects>
```

### Modifier Breakdown

- **collection**: `COLLECTION_UNIT_COMBAT` - Applies during combat
- **effect**: `EFFECT_ADJUST_UNIT_COMBAT_STRENGTH` - Modifies combat strength
- **SubjectRequirements**: Only triggers when fighting units tagged `UNIT_CLASS_MELEE`
- **Amount**: +5 combat strength bonus

## Step 4: Add Localization Text

**text/text.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <EnglishText>
        <!-- Unit name and description -->
        <Row Tag="LOC_UNIT_ELITE_GUARD_NAME">
            <Text>Elite Guard</Text>
        </Row>
        <Row Tag="LOC_UNIT_ELITE_GUARD_DESCRIPTION">
            <Text>Powerful heavy infantry unit with a bonus against other melee units. Unlocked with Bronze Working.</Text>
        </Row>

        <!-- Ability name and description -->
        <Row Tag="LOC_ABILITY_ELITE_GUARD_BONUS_NAME">
            <Text>Elite Training</Text>
        </Row>
        <Row Tag="LOC_ABILITY_ELITE_GUARD_BONUS_DESCRIPTION">
            <Text>+5[icon:STAT_COMBAT] Combat Strength when fighting melee units.</Text>
        </Row>
        <Row Tag="LOC_ABILITY_ELITE_GUARD_BONUS_PREVIEW">
            <Text>Elite Training vs Melee: +5</Text>
        </Row>
    </EnglishText>
</Database>
```

> **Note:** Use language-specific table names (`EnglishText`, `FrenchText`, etc.) instead of a `Language` attribute.

### Icon Tags for Stats

| Tag | Icon |
|-----|------|
| `[icon:STAT_COMBAT]` | Combat Strength |
| `[icon:STAT_RANGED]` | Ranged Strength |
| `[icon:STAT_MOVEMENT]` | Movement |
| `[icon:STAT_SIGHT]` | Sight Range |

## Step 5: Add Unit Icon (Optional)

**icons/icons.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- If you have a custom icon texture -->
    <IconTextureAtlases>
        <Row Name="ICON_ATLAS_ELITE_GUARD"
             IconSize="50"
             Filename="icons/elite_guard_50.dds"/>
        <Row Name="ICON_ATLAS_ELITE_GUARD"
             IconSize="38"
             Filename="icons/elite_guard_38.dds"/>
    </IconTextureAtlases>

    <IconDefinitions>
        <Row Name="ICON_UNIT_ELITE_GUARD"
             Atlas="ICON_ATLAS_ELITE_GUARD"
             Index="0"/>
    </IconDefinitions>
</Database>
```

Without a custom icon, the game will use a default placeholder.

## Step 6: Install and Test

1. Copy your mod folder to the Mods directory:
   - **Windows**: `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Mods\`
   - **macOS**: `~/Library/Application Support/Civilization VII/Mods/`

2. Launch the game and enable the mod in Additional Content

3. Start a new Antiquity Age game

4. Research Bronze Working

5. Build your Elite Guard from a city with a Barracks

## Advanced: Making a Unique Unit Replacement

To make the Elite Guard replace the Warrior for a specific civilization:

```xml
<!-- In your database XML -->
<CivilizationUniqueUnits>
    <Row CivilizationType="CIVILIZATION_ROME"
         UnitType="UNIT_ELITE_GUARD"
         ReplacesUnitType="UNIT_WARRIOR"/>
</CivilizationUniqueUnits>

<!-- Hide from other civs -->
<Unit_CivilizationRequirements>
    <Row UnitType="UNIT_ELITE_GUARD"
         CivilizationType="CIVILIZATION_ROME"/>
</Unit_CivilizationRequirements>
```

## Advanced: Adding a Ranged Unit

For ranged units, add `RangedCombat` and `Range` to stats:

```xml
<Units>
    <Row
        UnitType="UNIT_MY_ARCHER"
        Name="LOC_UNIT_MY_ARCHER_NAME"
        BaseMoves="2"
        Domain="DOMAIN_LAND"
        CoreClass="CORE_CLASS_MILITARY"
        FormationClass="FORMATION_CLASS_LAND_COMBAT"
        ZoneOfControl="false"
        ...
    />
</Units>

<Unit_Stats>
    <Row UnitType="UNIT_MY_ARCHER" Combat="17" RangedCombat="25" Range="2"/>
</Unit_Stats>

<TypeTags>
    <Row Type="UNIT_MY_ARCHER" Tag="UNIT_CLASS_RANGED"/>
</TypeTags>
```

## Advanced: Adding Resource Requirements

To require a strategic resource:

```xml
<Unit_ResourceCosts>
    <Row UnitType="UNIT_ELITE_GUARD" ResourceType="RESOURCE_IRON" Amount="1"/>
</Unit_ResourceCosts>
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Unit doesn't appear | Check tech unlock and ActionCriteria age |
| Can't train unit | Verify `CanTrain="true"` and building prerequisites |
| No combat stats | Check `Unit_Stats` table entries |
| Ability not working | Verify ModifierId matches between tables |
| No icon showing | Check icon atlas paths and definitions |

## Common Antiquity Tech Nodes

| Node | Technology |
|------|------------|
| `NODE_TECH_AQ_MASONRY` | Masonry |
| `NODE_TECH_AQ_BRONZE_WORKING` | Bronze Working |
| `NODE_TECH_AQ_IRON_WORKING` | Iron Working |
| `NODE_TECH_AQ_HORSEBACK_RIDING` | Horseback Riding |
| `NODE_TECH_AQ_SHIPBUILDING` | Shipbuilding |
| `NODE_TECH_AQ_ENGINEERING` | Engineering |

## Next Steps

- [Units Database Schema](../database/Units.md)
- [Promotions System](../units/Promotions.md)
- [Effect Types Reference](../modifiers/Effect-Types.md)
- [Creating a Civilization](Creating-a-Civilization.md)
