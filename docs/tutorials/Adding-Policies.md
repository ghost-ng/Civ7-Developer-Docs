# Tutorial: Adding New Policies (Traditions)

This tutorial walks through creating a mod that adds new policies (called "Traditions" in Civ VII) to the game. We'll add two new traditions that provide bonuses to your Palace.

## What You'll Learn

- Creating a .modinfo file with ActionCriteria
- Using SQL for database modifications
- Using GameEffects XML for modifiers
- Adding localized text

## File Structure

```
my-new-policies/
├── data/
│   ├── traditions.sql
│   └── game-effects.xml
├── text/
│   └── text.xml
└── my-new-policies.modinfo
```

## Step 1: Create the .modinfo File

The .modinfo file tells the game about your mod and what files to load.

**my-new-policies.modinfo**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-new-policies" version="1" xmlns="ModInfo">
    <Properties>
        <Name>My New Policies</Name>
        <Description>Adds new policies to the Antiquity Age</Description>
        <Authors>Your Name</Authors>
        <AffectsSavedGames>1</AffectsSavedGames>
    </Properties>
    <Dependencies>
    </Dependencies>
    <ActionCriteria>
        <Criteria id="antiquity-age-current">
            <AgeInUse>AGE_ANTIQUITY</AgeInUse>
        </Criteria>
    </ActionCriteria>
    <ActionGroups>
        <ActionGroup id="antiquity-game" scope="game" criteria="antiquity-age-current">
            <Actions>
                <UpdateDatabase>
                    <Item>data/traditions.sql</Item>
                    <Item>data/game-effects.xml</Item>
                </UpdateDatabase>
                <UpdateText>
                    <Item>text/text.xml</Item>
                </UpdateText>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

### Key Concepts

- **ActionCriteria**: Conditions for when actions run. `AgeInUse` makes the mod only load during the specified age.
- **ActionGroup scope="game"**: Loads into the gameplay database (not frontend).
- **UpdateDatabase**: Processes SQL and XML files to modify the database.
- **UpdateText**: Loads localization text.

## Step 2: Create the Database Entries (SQL)

We need to register the new traditions and link them to the civic tree.

**data/traditions.sql**
```sql
-- Register the new tradition types
INSERT INTO Types
    (Type, Kind)
VALUES
    ('TRADITION_MY_FIRST_POLICY', 'KIND_TRADITION'),
    ('TRADITION_MY_SECOND_POLICY', 'KIND_TRADITION');

-- Define the traditions
INSERT INTO Traditions
    (TraditionType, Name, Description)
VALUES
    ('TRADITION_MY_FIRST_POLICY', 'LOC_TRADITION_MY_FIRST_POLICY_NAME', 'LOC_TRADITION_MY_FIRST_POLICY_DESCRIPTION'),
    ('TRADITION_MY_SECOND_POLICY', 'LOC_TRADITION_MY_SECOND_POLICY_NAME', 'LOC_TRADITION_MY_SECOND_POLICY_DESCRIPTION');

-- Unlock the traditions from the Chiefdom civic
INSERT INTO ProgressionTreeNodeUnlocks
    (ProgressionTreeNodeType, TargetKind, TargetType, UnlockDepth)
VALUES
    ('NODE_CIVIC_AQ_MAIN_CHIEFDOM', 'KIND_TRADITION', 'TRADITION_MY_FIRST_POLICY', 1),
    ('NODE_CIVIC_AQ_MAIN_CHIEFDOM', 'KIND_TRADITION', 'TRADITION_MY_SECOND_POLICY', 1);

-- Link traditions to their modifiers
INSERT INTO TraditionModifiers
    (TraditionType, ModifierId)
VALUES
    ('TRADITION_MY_FIRST_POLICY', 'MY_FIRST_POLICY_MOD_PALACE_FOOD'),
    ('TRADITION_MY_FIRST_POLICY', 'MY_FIRST_POLICY_MOD_PALACE_GOLD'),
    ('TRADITION_MY_SECOND_POLICY', 'MY_SECOND_POLICY_MOD_CITY_SCIENCE');
```

### Key Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register new game objects |
| `Traditions` | Define tradition metadata |
| `ProgressionTreeNodeUnlocks` | Link to civic tree |
| `TraditionModifiers` | Link to effects |

### Civic Tree Nodes

Common Antiquity civic nodes for unlocks:
- `NODE_CIVIC_AQ_MAIN_CHIEFDOM` - Early game
- `NODE_CIVIC_AQ_MAIN_FOREIGN_TRADE` - Trade
- `NODE_CIVIC_AQ_MAIN_ENGINEERING` - Building
- `NODE_CIVIC_AQ_MAIN_PHILOSOPHY` - Culture
- `NODE_CIVIC_AQ_MAIN_DRAMA_AND_POETRY` - Late game

## Step 3: Create the Modifiers (GameEffects XML)

Define what each tradition actually does using GameEffects XML.

**data/game-effects.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- First Policy: +2 Food, +3 Gold on Palace -->
    <Modifier id="MY_FIRST_POLICY_MOD_PALACE_FOOD"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD">
        <Argument name="YieldType">YIELD_FOOD</Argument>
        <Argument name="Amount">2</Argument>
        <Argument name="ConstructibleType">BUILDING_PALACE</Argument>
    </Modifier>

    <Modifier id="MY_FIRST_POLICY_MOD_PALACE_GOLD"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_ADJUST_CONSTRUCTIBLE_YIELD">
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="Amount">3</Argument>
        <Argument name="ConstructibleType">BUILDING_PALACE</Argument>
        <String context="Description">LOC_TRADITION_MY_FIRST_POLICY_DESCRIPTION</String>
    </Modifier>

    <!-- Second Policy: +2 Science in all Cities -->
    <Modifier id="MY_SECOND_POLICY_MOD_CITY_SCIENCE"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <SubjectRequirements>
            <Requirement type="REQUIREMENT_CITY_HAS_BUILD_QUEUE"/>
        </SubjectRequirements>
        <Argument name="YieldType">YIELD_SCIENCE</Argument>
        <Argument name="Amount">2</Argument>
        <String context="Description">LOC_TRADITION_MY_SECOND_POLICY_DESCRIPTION</String>
    </Modifier>
</GameEffects>
```

### Important Notes

1. **Root element is `<GameEffects>`**, not `<Database>`
2. **Modifier ids must match** what you used in `TraditionModifiers`
3. **Include `<String context="Description">`** on one modifier for tooltip display
4. Use `SubjectRequirements` to filter which subjects get the effect

## Step 4: Add Localization Text

Create the display text for your traditions.

**text/text.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_TRADITION_MY_FIRST_POLICY_NAME" Language="en_US">
            <Text>My First Policy</Text>
        </Row>
        <Row Tag="LOC_TRADITION_MY_FIRST_POLICY_DESCRIPTION" Language="en_US">
            <Text>+2[icon:YIELD_FOOD] Food, +3[icon:YIELD_GOLD] Gold on the Palace.</Text>
        </Row>
        <Row Tag="LOC_TRADITION_MY_SECOND_POLICY_NAME" Language="en_US">
            <Text>My Second Policy</Text>
        </Row>
        <Row Tag="LOC_TRADITION_MY_SECOND_POLICY_DESCRIPTION" Language="en_US">
            <Text>+2[icon:YIELD_SCIENCE] Science in all Cities.</Text>
        </Row>
    </LocalizedText>
</Database>
```

### Icon Tags

Use these in descriptions:
- `[icon:YIELD_FOOD]` - Food
- `[icon:YIELD_PRODUCTION]` - Production
- `[icon:YIELD_GOLD]` - Gold
- `[icon:YIELD_SCIENCE]` - Science
- `[icon:YIELD_CULTURE]` - Culture
- `[icon:YIELD_HAPPINESS]` - Happiness
- `[icon:YIELD_DIPLOMACY]` - Diplomacy

## Step 5: Install and Test

1. Copy your mod folder to:
   - **Windows**: `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Mods\`
   - **macOS**: `~/Library/Application Support/Civilization VII/Mods/`

2. Launch the game and enable your mod in the Mods menu

3. Start a new game in the Antiquity Age

4. Research the Chiefdom civic to unlock your new policies

## Troubleshooting

- **Check `Database.log`** in your Logs folder for errors
- **Verify XML syntax** - missing quotes or brackets break parsing
- **Match modifier IDs exactly** between SQL and GameEffects
- **Use unique IDs** - prefix with your mod name to avoid conflicts

## Advanced: Adding to Other Ages

Change the ActionCriteria to target different ages:

```xml
<ActionCriteria>
    <Criteria id="exploration-age-current">
        <AgeInUse>AGE_EXPLORATION</AgeInUse>
    </Criteria>
</ActionCriteria>
```

And use appropriate civic nodes:
- `NODE_CIVIC_EX_MAIN_*` for Exploration Age
- `NODE_CIVIC_MO_MAIN_*` for Modern Age

## Next Steps

- [Modifier System Overview](../modifiers/Overview.md)
- [Effect Types Reference](../modifiers/Effect-Types.md)
- [modinfo Files](../Modinfo-Files.md)
