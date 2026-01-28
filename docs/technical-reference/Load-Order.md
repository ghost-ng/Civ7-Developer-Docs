# Load Order & Mod Compatibility

This document covers mod load order, compatibility considerations, and best practices for ensuring mods work together in Civilization VII.

## Overview

Understanding load order is critical for:

- **Mod Compatibility** - Ensuring mods don't conflict
- **Overriding Content** - Replacing base game data
- **Dependencies** - Loading mods in correct sequence
- **Debugging** - Understanding why conflicts occur

## Load Order Phases

Civ VII loads content in distinct phases:

### 1. Base Game
```
Base Game Data
└── Core gameplay tables
└── Default civilizations, units, buildings
└── Base modifiers and effects
```

### 2. DLC Content
```
Official DLC
└── New civilizations
└── New mechanics
└── Updated base content
```

### 3. Mods (Alphabetical by default)
```
User Mods
└── Mod A
└── Mod B
└── Mod C
```

## Controlling Load Order

### Using Dependencies

Specify dependencies in modinfo to control load order:

```xml
<Mod id="my-mod" version="1.0">
    <Dependencies>
        <!-- Require specific mod -->
        <Mod id="required-mod" title="Required Mod"/>

        <!-- Require mod version -->
        <Mod id="another-mod" version="2.0"/>

        <!-- Require DLC -->
        <Mod id="dlc-expansion" title="Expansion DLC"/>
    </Dependencies>
</Mod>
```

### Mod Load After

Force your mod to load after another:

```xml
<Mod id="my-mod" version="1.0">
    <LoadAfter>
        <Mod id="other-mod"/>
    </LoadAfter>
</Mod>
```

### Mod Load Before

Force your mod to load before another:

```xml
<Mod id="my-mod" version="1.0">
    <LoadBefore>
        <Mod id="other-mod"/>
    </LoadBefore>
</Mod>
```

## Database Operations

### Update vs Replace

#### UpdateDatabase (Additive)
Adds new rows without affecting existing data:

```xml
<Components>
    <UpdateDatabase id="add-content">
        <Items>
            <File>Data/NewContent.xml</File>
        </Items>
    </UpdateDatabase>
</Components>
```

#### ReplaceDatabase (Destructive)
Replaces entire tables or specific rows:

```xml
<Components>
    <ReplaceDatabase id="replace-content">
        <Items>
            <File>Data/Replacements.xml</File>
        </Items>
    </ReplaceDatabase>
</Components>
```

### Row Operations

#### Adding New Rows
```xml
<Units>
    <Row UnitType="UNIT_MY_NEW_UNIT" ... />
</Units>
```

#### Updating Existing Rows
Use the primary key to target specific rows:

```xml
<Units>
    <Update>
        <Where UnitType="UNIT_WARRIOR"/>
        <Set BaseCombat="25"/>
    </Update>
</Units>
```

#### Deleting Rows
```xml
<Units>
    <Delete UnitType="UNIT_OBSOLETE"/>
</Units>
```

## Component Loading Priority

Components load in this order within a mod:

1. **UpdateDatabase** - Additive database changes
2. **ReplaceDatabase** - Replacement database changes
3. **UpdateSchema** - Schema modifications
4. **GameEffects** - Modifier definitions
5. **UserInterface** - UI components
6. **LocalizedText** - Text strings
7. **Scripts** - JavaScript files

### Specifying Order

Use `LoadOrder` attribute:

```xml
<Components>
    <UpdateDatabase id="base-types" LoadOrder="1">
        <Items><File>Data/Types.xml</File></Items>
    </UpdateDatabase>

    <UpdateDatabase id="content" LoadOrder="2">
        <Items><File>Data/Content.xml</File></Items>
    </UpdateDatabase>

    <UpdateDatabase id="modifiers" LoadOrder="3">
        <Items><File>Data/Modifiers.xml</File></Items>
    </UpdateDatabase>
</Components>
```

## Common Compatibility Issues

### Problem: Missing Type Reference

**Symptom**: Error about unknown type

**Cause**: Referencing a type before it's defined

**Solution**: Ensure Types.xml loads before content that references those types

```xml
<!-- Wrong order -->
<Units>
    <Row UnitType="UNIT_MY_UNIT" ... />  <!-- References type not yet defined -->
</Units>

<!-- Correct: Define type first -->
<Types>
    <Row Type="UNIT_MY_UNIT" Kind="KIND_UNIT"/>
</Types>
<Units>
    <Row UnitType="UNIT_MY_UNIT" ... />
</Units>
```

### Problem: Duplicate Primary Key

**Symptom**: Database constraint error

**Cause**: Two mods define the same type/ID

**Solution**: Use unique prefixes for your mod

```xml
<!-- Avoid generic names -->
<Types>
    <Row Type="UNIT_KNIGHT" Kind="KIND_UNIT"/>  <!-- May conflict! -->
</Types>

<!-- Use prefix -->
<Types>
    <Row Type="UNIT_MYMOD_KNIGHT" Kind="KIND_UNIT"/>  <!-- Unique -->
</Types>
```

### Problem: Modifier Not Applying

**Symptom**: Modifier exists but has no effect

**Cause**: Modifier loaded before its effect type is defined

**Solution**: Load GameEffects after base definitions

### Problem: Localization Missing

**Symptom**: Raw LOC_ tags displayed

**Cause**: Text loaded in wrong language group or missing entirely

**Solution**: Verify language code and file loading

```xml
<LocalizedText>
    <Text id="my-text">
        <Items>
            <File>Text/MyMod_Text.xml</File>
        </Items>
        <Criteria>
            <LanguageId>en_US</LanguageId>
        </Criteria>
    </Text>
</LocalizedText>
```

## Compatibility Best Practices

### 1. Use Unique Identifiers

Prefix all IDs with your mod name:

```xml
<Types>
    <Row Type="MYMOD_UNIT_WARRIOR" Kind="KIND_UNIT"/>
    <Row Type="MYMOD_BUILDING_BARRACKS" Kind="KIND_BUILDING"/>
    <Row Type="MYMOD_TRAIT_BONUS" Kind="KIND_TRAIT"/>
</Types>
```

### 2. Prefer Updates Over Replacements

Modify specific values instead of replacing entire rows:

```xml
<!-- Good: Targeted update -->
<Units>
    <Update>
        <Where UnitType="UNIT_WARRIOR"/>
        <Set BaseCombat="22"/>
    </Update>
</Units>

<!-- Avoid: Full replacement (may lose other mod changes) -->
<Units>
    <Row UnitType="UNIT_WARRIOR" BaseCombat="22" Cost="40" ... />
</Units>
```

### 3. Document Dependencies

In your mod description:
- List required mods
- List incompatible mods
- Note load order requirements

### 4. Test with Popular Mods

Before release:
- Test with common mods
- Check for error messages
- Verify all features work

### 5. Provide Compatibility Patches

For known conflicts:

```xml
<!-- Separate compatibility patch mod -->
<Mod id="mymod-othermod-compat" version="1.0">
    <Dependencies>
        <Mod id="my-mod"/>
        <Mod id="other-mod"/>
    </Dependencies>

    <Components>
        <UpdateDatabase id="compat-fixes">
            <Items>
                <File>Data/CompatFixes.xml</File>
            </Items>
        </UpdateDatabase>
    </Components>
</Mod>
```

## Debugging Load Issues

### Enable Debug Logging

Add to launch options:
```
-logfile debug.log -loglevel 4
```

### Check Database.log

Look for:
- Duplicate key errors
- Missing reference errors
- Schema violations

### Common Error Messages

| Error | Cause | Solution |
|-------|-------|----------|
| `FOREIGN KEY constraint failed` | Reference to non-existent row | Check dependency order |
| `UNIQUE constraint failed` | Duplicate primary key | Use unique ID prefix |
| `no such table` | Table not yet created | Load schema first |
| `no such column` | Wrong column name | Check column spelling |

### Verify Load Order

Log your mod's load:

```javascript
// In your mod's script
console.log("MyMod loaded at: " + new Date().toISOString());
```

## Complete Example: Multi-File Mod

### ModInfo with Proper Load Order

```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="complete-civ-mod" version="1.0">
    <Properties>
        <Name>Complete Civilization Mod</Name>
        <Description>Adds a new civilization with all components</Description>
    </Properties>

    <!-- Dependencies -->
    <Dependencies>
        <Mod id="base-game" title="Civilization VII"/>
    </Dependencies>

    <!-- Load after these mods if present -->
    <LoadAfter>
        <Mod id="community-patch"/>
    </LoadAfter>

    <Components>
        <!-- 1. Types first (LoadOrder 1) -->
        <UpdateDatabase id="types" LoadOrder="1">
            <Items>
                <File>Data/Types.xml</File>
            </Items>
        </UpdateDatabase>

        <!-- 2. Database content (LoadOrder 2) -->
        <UpdateDatabase id="content" LoadOrder="2">
            <Items>
                <File>Data/Civilizations.xml</File>
                <File>Data/Leaders.xml</File>
                <File>Data/Units.xml</File>
                <File>Data/Buildings.xml</File>
            </Items>
        </UpdateDatabase>

        <!-- 3. Icons and art defs (LoadOrder 3) -->
        <UpdateDatabase id="art" LoadOrder="3">
            <Items>
                <File>Data/Icons.xml</File>
                <File>Data/ArtDefs.xml</File>
            </Items>
        </UpdateDatabase>

        <!-- 4. Modifiers (LoadOrder 4) -->
        <UpdateDatabase id="modifiers" LoadOrder="4">
            <Items>
                <File>Data/TraitModifiers.xml</File>
            </Items>
        </UpdateDatabase>

        <!-- 5. GameEffects -->
        <GameEffects id="effects">
            <Items>
                <File>Data/Effects.xml</File>
            </Items>
        </GameEffects>

        <!-- 6. Localization -->
        <LocalizedText>
            <Text id="text-en">
                <Items>
                    <File>Text/en_US/Text.xml</File>
                </Items>
            </Text>
        </LocalizedText>

        <!-- 7. UI (optional) -->
        <UserInterface id="ui">
            <Items>
                <File>UI/CustomPanel.blp</File>
                <File>UI/CustomPanel.js</File>
            </Items>
        </UserInterface>
    </Components>

    <Files>
        <File>Art/Icons.dds</File>
    </Files>
</Mod>
```

## Recommended File Structure

```
MyMod/
├── MyMod.modinfo
├── Data/
│   ├── Types.xml           # Type definitions (load first)
│   ├── Civilizations.xml   # Civ definitions
│   ├── Leaders.xml         # Leader definitions
│   ├── Units.xml           # Unit definitions
│   ├── Buildings.xml       # Building definitions
│   ├── TraitModifiers.xml  # Trait-modifier links
│   ├── Effects.xml         # GameEffects modifiers
│   ├── Icons.xml           # Icon definitions
│   └── ArtDefs.xml         # Art definitions
├── Text/
│   ├── en_US/
│   │   └── Text.xml
│   └── fr_FR/
│       └── Text.xml
├── Art/
│   └── Icons.dds
└── UI/
    ├── CustomPanel.blp
    └── CustomPanel.js
```

## Related Documentation

- [ModInfo Files](../Modinfo-Files.md)
- [Types and Kinds](../database/Types-and-Kinds.md)
- [Getting Started](../Getting-Started.md)
- [Modifier System](../modifiers/Overview.md)
