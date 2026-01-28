# Example Mods

This directory contains example code for common modding tasks.

## Official Firaxis Examples

The Civilization VII Development Tools (available on Steam) includes official example mods:

- **fxs-majapahit-traditions** - Adding civilization-specific traditions
- **fxs-new-narrative-events** - Creating narrative events
- **fxs-new-policies** - Adding new policies/traditions

Location: `Steam\steamapps\common\Sid Meier's Civilization VII Development Tools\Examples\`

## Example Files

### [new-policies-example](new-policies-example/)

A complete example mod adding two new policies to the Antiquity Age.

**Files:**
- `my-new-policies.modinfo` - Mod metadata and actions
- `data/traditions.sql` - Database entries
- `data/game-effects.xml` - Modifier definitions
- `text/text.xml` - Localization

See [Tutorial: Adding Policies](../docs/tutorials/Adding-Policies.md) for a detailed walkthrough.

## Quick Reference

### Minimal .modinfo Template

```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-mod-id" version="1" xmlns="ModInfo">
    <Properties>
        <Name>My Mod Name</Name>
        <Description>Description here</Description>
        <Authors>Your Name</Authors>
        <AffectsSavedGames>1</AffectsSavedGames>
    </Properties>
    <ActionGroups>
        <ActionGroup id="main" scope="game">
            <Actions>
                <UpdateDatabase>
                    <Item>data/changes.xml</Item>
                </UpdateDatabase>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

### Age-Specific Loading

```xml
<ActionCriteria>
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
```

### Common Database Operations

**Insert new type:**
```sql
INSERT INTO Types (Type, Kind) VALUES ('MY_TYPE', 'KIND_UNIT');
```

**Modify existing value:**
```sql
UPDATE Units SET BaseMoves = 3 WHERE UnitType = 'UNIT_SCOUT';
```

**Delete an entry:**
```sql
DELETE FROM TypeTags WHERE Type = 'UNIT_WARRIOR' AND Tag = 'UNIT_CLASS_INFANTRY';
```

### Simple Modifier

```xml
<GameEffects xmlns="GameEffects">
    <Modifier id="MY_MOD" collection="COLLECTION_PLAYER_CITIES" effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_FOOD</Argument>
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```
