# modinfo Files Reference

The `.modinfo` file is the core of any Civilization VII mod. It defines what files to load, when to load them, and how your mod relates to other mods.

## Basic Structure

```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-mod-id" version="1" xmlns="ModInfo">
    <Properties>
        <Name>My Mod Name</Name>
        <Description>What this mod does</Description>
        <Authors>Your Name</Authors>
        <AffectsSavedGames>1</AffectsSavedGames>
    </Properties>
    <Dependencies>
    </Dependencies>
    <References>
    </References>
    <ActionCriteria>
    </ActionCriteria>
    <ActionGroups>
    </ActionGroups>
</Mod>
```

## Mod Element

The root element with required attributes:

| Attribute | Description |
|-----------|-------------|
| `id` | Unique identifier (lowercase, dashes, <64 chars). Use a prefix like `yourname-mod-name` |
| `version` | Version number |
| `xmlns` | Always `ModInfo` |

## Properties

| Element | Required | Description |
|---------|----------|-------------|
| `Name` | Yes | Display name in Add-Ons menu |
| `Description` | Yes | Description shown in Add-Ons |
| `Authors` | Yes | Author name(s) |
| `AffectsSavedGames` | No | `1` = can't add/remove mid-game (default), `0` = UI-only mod |
| `ShowInBrowser` | No | `1` = show in Add-Ons (default), `0` = hide |
| `EnabledByDefault` | No | `1` = enabled by default (default), `0` = disabled |

## Dependencies

Mods that must be active and load before this mod:

```xml
<Dependencies>
    <Mod id="other-mod-id" title="Other Mod Name"/>
</Dependencies>
```

All mods automatically depend on: `core`, `base-standard`, `age-antiquity`, `age-exploration`, `age-modern`

## References

Mods that should load before this mod (if active):

```xml
<References>
    <Mod id="optional-mod-id" title="Optional Mod Name"/>
</References>
```

## ActionCriteria

Define conditions for when ActionGroups should activate:

```xml
<ActionCriteria>
    <Criteria id="antiquity-age">
        <AgeInUse>AGE_ANTIQUITY</AgeInUse>
    </Criteria>
    <Criteria id="exploration-or-modern" any="true">
        <AgeInUse>AGE_EXPLORATION</AgeInUse>
        <AgeInUse>AGE_MODERN</AgeInUse>
    </Criteria>
    <Criteria id="always">
        <AlwaysMet></AlwaysMet>
    </Criteria>
</ActionCriteria>
```

By default, ALL conditions must be met. Add `any="true"` to require only one.

### Criterion Types

#### AlwaysMet / NeverMet
Always passes or always fails.

#### AgeInUse
Current age matches:
```xml
<AgeInUse>AGE_ANTIQUITY</AgeInUse>
```
Values: `AGE_ANTIQUITY`, `AGE_EXPLORATION`, `AGE_MODERN`

Use `inverse="1"` to invert:
```xml
<AgeInUse inverse="1">AGE_ANTIQUITY</AgeInUse>
```

#### AgeWasUsed
A previous age was played (not current age, not advanced starts).

#### AgeEverInUse
Current age OR a previous age.

#### ModInUse
Another mod is active:
```xml
<ModInUse>other-mod-id</ModInUse>
```

With version check:
```xml
<ModInUse>
    <Version>2.0</Version>
    <Value>other-mod-id</Value>
</ModInUse>
```

#### ConfigurationValueMatches
Game setting matches:
```xml
<ConfigurationValueMatches>
    <Group>Game</Group>
    <ConfigurationId>SpeedType</ConfigurationId>
    <Value>GAMESPEED_STANDARD</Value>
</ConfigurationValueMatches>
```

#### ConfigurationValueContains
Game setting is one of:
```xml
<ConfigurationValueContains>
    <Group>Game</Group>
    <ConfigurationId>Handicap</ConfigurationId>
    <Value>DIFFICULTY_PRINCE,DIFFICULTY_KING</Value>
</ConfigurationValueContains>
```

#### MapInUse
Specific map type:
```xml
<MapInUse>{base-standard}maps/continents-plus.js</MapInUse>
```

#### GameModeInUse
Game mode: `WorldBuilder`, `SinglePlayer`, `HotSeat`, `MultiPlayer`

#### LeaderPlayable / CivilizationPlayable
Leader or civ is available for selection.

## ActionGroups

Define what files to load and when:

```xml
<ActionGroups>
    <ActionGroup id="main-game" scope="game" criteria="always">
        <Properties>
            <LoadOrder>10</LoadOrder>
        </Properties>
        <Actions>
            <UpdateDatabase>
                <Item>data/changes.xml</Item>
                <Item>data/changes.sql</Item>
            </UpdateDatabase>
            <UpdateText>
                <Item>text/text.xml</Item>
            </UpdateText>
        </Actions>
    </ActionGroup>
</ActionGroups>
```

### ActionGroup Attributes

| Attribute | Description |
|-----------|-------------|
| `id` | Unique identifier within the mod |
| `scope` | `game` (gameplay) or `shell` (frontend/menus) |
| `criteria` | ID of a Criteria from ActionCriteria |

### Properties

| Element | Description |
|---------|-------------|
| `LoadOrder` | Higher numbers load later (default: 0) |

### Action Types

#### UpdateDatabase
Load XML or SQL into the gameplay or shell database:
```xml
<UpdateDatabase>
    <Item>data/my-changes.xml</Item>
    <Item>data/my-changes.sql</Item>
</UpdateDatabase>
```

#### UpdateText
Load localization text:
```xml
<UpdateText>
    <Item>text/en_US.xml</Item>
</UpdateText>
```

#### UpdateIcons
Load icon definitions:
```xml
<UpdateIcons>
    <Item>icons/icons.xml</Item>
</UpdateIcons>
```

#### UpdateColors
Load color definitions:
```xml
<UpdateColors>
    <Item>colors/colors.xml</Item>
</UpdateColors>
```

#### ImportFiles
Import files (textures, scripts to replace):
```xml
<ImportFiles>
    <Item>textures/my-icon.png</Item>
    <Item>ui/panels/my-panel.js</Item>
</ImportFiles>
```

#### UIScripts
Load new UI JavaScript:
```xml
<UIScripts>
    <Item>ui/my-script.js</Item>
</UIScripts>
```

#### UIShortcuts
Add panels to debug menu (requires `EnableDebugPanels 1`):
```xml
<UIShortcuts>
    <Item>ui/debug/my-panel.html</Item>
</UIShortcuts>
```

#### MapGenScripts
Scripts for map generation:
```xml
<MapGenScripts>
    <Item>scripts/mapgen.js</Item>
</MapGenScripts>
```

#### ScenarioScripts
Gameplay scripts:
```xml
<ScenarioScripts>
    <Item>scripts/gameplay.js</Item>
</ScenarioScripts>
```

#### UpdateVisualRemaps
Remap visuals to different assets:
```xml
<UpdateVisualRemaps>
    <Item>data/visual-remaps.xml</Item>
</UpdateVisualRemaps>
```

## Complete Example

```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="myname-example-mod" version="1" xmlns="ModInfo">
    <Properties>
        <Name>Example Mod</Name>
        <Description>Adds new content to all ages</Description>
        <Authors>My Name</Authors>
        <AffectsSavedGames>1</AffectsSavedGames>
    </Properties>
    <Dependencies>
    </Dependencies>
    <ActionCriteria>
        <Criteria id="always">
            <AlwaysMet></AlwaysMet>
        </Criteria>
        <Criteria id="antiquity">
            <AgeInUse>AGE_ANTIQUITY</AgeInUse>
        </Criteria>
        <Criteria id="exploration">
            <AgeInUse>AGE_EXPLORATION</AgeInUse>
        </Criteria>
        <Criteria id="modern">
            <AgeInUse>AGE_MODERN</AgeInUse>
        </Criteria>
    </ActionCriteria>
    <ActionGroups>
        <!-- Always load text -->
        <ActionGroup id="text-always" scope="game" criteria="always">
            <Actions>
                <UpdateText>
                    <Item>text/text.xml</Item>
                </UpdateText>
            </Actions>
        </ActionGroup>
        <!-- Antiquity-specific content -->
        <ActionGroup id="antiquity-game" scope="game" criteria="antiquity">
            <Actions>
                <UpdateDatabase>
                    <Item>data/antiquity/changes.xml</Item>
                    <Item>data/antiquity/gameeffects.xml</Item>
                </UpdateDatabase>
            </Actions>
        </ActionGroup>
        <!-- Exploration-specific content -->
        <ActionGroup id="exploration-game" scope="game" criteria="exploration">
            <Actions>
                <UpdateDatabase>
                    <Item>data/exploration/changes.xml</Item>
                </UpdateDatabase>
            </Actions>
        </ActionGroup>
        <!-- Modern-specific content -->
        <ActionGroup id="modern-game" scope="game" criteria="modern">
            <Actions>
                <UpdateDatabase>
                    <Item>data/modern/changes.xml</Item>
                </UpdateDatabase>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

## Tips

1. **Use unique IDs** - Prefix with your name or initials
2. **Test with new games** - `AffectsSavedGames=1` mods can't be added mid-game
3. **Check Database.log** - Shows loading errors
4. **Use LoadOrder** - When order matters between your ActionGroups
5. **Reference game modules** - Look at `Base/modules/*/` for examples

## See Also

- [Getting Started](Getting-Started.md)
- [Database Modding](Database-Modding.md)
- [Adding Policies Tutorial](tutorials/Adding-Policies.md)
