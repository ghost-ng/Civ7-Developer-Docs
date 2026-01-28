# Localization System

This document covers the localization system in Civilization VII, including text keys, translations, formatting, and how to add multi-language support to mods.

## Overview

Civ VII's localization system allows mods to provide text in multiple languages:

- **LocalizedText Table** - Store all translatable strings
- **Text Tags** - Unique identifiers for each string (LOC_*)
- **Language Codes** - ISO language identifiers
- **Text Formatting** - Icons, colors, and dynamic values
- **Gender/Plural Support** - Grammar handling for different languages

## Core Tables

| Table | Purpose |
|-------|---------|
| `LocalizedText` | Primary table for all translated strings |
| `LocalizedTextTags` | Register custom text tag patterns |
| `Languages` | Define supported languages |
| `FontStyleSheets` | Font configurations per language |

## LocalizedText Table

### Basic Structure

```xml
<LocalizedText>
    <Row Tag="LOC_MY_TEXT_KEY" Language="en_US">
        <Text>This is my English text.</Text>
    </Row>
    <Row Tag="LOC_MY_TEXT_KEY" Language="fr_FR">
        <Text>Ceci est mon texte français.</Text>
    </Row>
    <Row Tag="LOC_MY_TEXT_KEY" Language="de_DE">
        <Text>Dies ist mein deutscher Text.</Text>
    </Row>
</LocalizedText>
```

### LocalizedText Columns

| Column | Type | Description |
|--------|------|-------------|
| `Tag` | TEXT | Unique text identifier (must start with LOC_) |
| `Language` | TEXT | Language code (e.g., en_US, fr_FR) |
| `Text` | TEXT | The actual translated string |
| `Gender` | TEXT | Gender variant (Male, Female, Neutral) |
| `Plurality` | INTEGER | Plural form number |

## Supported Languages

| Code | Language |
|------|----------|
| `en_US` | English (US) |
| `fr_FR` | French |
| `de_DE` | German |
| `es_ES` | Spanish |
| `it_IT` | Italian |
| `ja_JP` | Japanese |
| `ko_KR` | Korean |
| `pl_PL` | Polish |
| `pt_BR` | Portuguese (Brazil) |
| `ru_RU` | Russian |
| `zh_Hans_CN` | Chinese (Simplified) |
| `zh_Hant_HK` | Chinese (Traditional) |

## Text Tag Naming Conventions

Follow these conventions for consistent localization:

### Standard Prefixes

| Prefix | Usage |
|--------|-------|
| `LOC_` | All localization tags must start with this |
| `LOC_UNIT_` | Unit names and descriptions |
| `LOC_BUILDING_` | Building names and descriptions |
| `LOC_TECH_` | Technology names and descriptions |
| `LOC_CIVIC_` | Civic names and descriptions |
| `LOC_LEADER_` | Leader names and dialogue |
| `LOC_CIV_` | Civilization names |
| `LOC_TRAIT_` | Trait names and descriptions |
| `LOC_ABILITY_` | Ability names and descriptions |
| `LOC_MODIFIER_` | Modifier descriptions |
| `LOC_PEDIA_` | Civilopedia entries |

### Suffixes

| Suffix | Usage |
|--------|-------|
| `_NAME` | Display name |
| `_DESCRIPTION` | Full description |
| `_SHORT_DESCRIPTION` | Abbreviated description |
| `_QUOTE` | Flavor quote |
| `_TOOLTIP` | Hover tooltip |
| `_HELP` | Help text |

### Example Naming

```xml
<LocalizedText>
    <!-- Unit localization -->
    <Row Tag="LOC_UNIT_MY_WARRIOR_NAME" Language="en_US">
        <Text>Elite Warrior</Text>
    </Row>
    <Row Tag="LOC_UNIT_MY_WARRIOR_DESCRIPTION" Language="en_US">
        <Text>A powerful melee unit with bonus combat strength.</Text>
    </Row>

    <!-- Building localization -->
    <Row Tag="LOC_BUILDING_MY_TEMPLE_NAME" Language="en_US">
        <Text>Grand Temple</Text>
    </Row>
    <Row Tag="LOC_BUILDING_MY_TEMPLE_DESCRIPTION" Language="en_US">
        <Text>A magnificent temple that provides +3 [ICON_FAITH] Faith.</Text>
    </Row>
</LocalizedText>
```

## Text Formatting

### Icons

Use icon tags to display game icons in text:

| Icon Tag | Display |
|----------|---------|
| `[ICON_FOOD]` | Food icon |
| `[ICON_PRODUCTION]` | Production icon |
| `[ICON_GOLD]` | Gold icon |
| `[ICON_SCIENCE]` | Science icon |
| `[ICON_CULTURE]` | Culture icon |
| `[ICON_FAITH]` | Faith icon |
| `[ICON_HAPPINESS]` | Happiness icon |
| `[ICON_DIPLOMACY]` | Diplomacy icon |
| `[ICON_STRENGTH]` | Combat strength icon |
| `[ICON_MOVEMENT]` | Movement icon |
| `[ICON_RANGE]` | Range icon |
| `[ICON_CAPITAL]` | Capital icon |
| `[ICON_CITIZEN]` | Citizen/population icon |
| `[ICON_TURN]` | Turn icon |
| `[ICON_LEGACY]` | Legacy point icon |

### Example with Icons

```xml
<Row Tag="LOC_MY_BUILDING_DESCRIPTION" Language="en_US">
    <Text>Provides +2 [ICON_FOOD] Food and +1 [ICON_PRODUCTION] Production. Requires 100 [ICON_GOLD] Gold maintenance per turn.</Text>
</Row>
```

### Colors

Use color tags for emphasis:

```xml
<Row Tag="LOC_MY_WARNING_TEXT" Language="en_US">
    <Text>[COLOR_RED]Warning:[ENDCOLOR] This action cannot be undone.</Text>
</Row>

<Row Tag="LOC_MY_BONUS_TEXT" Language="en_US">
    <Text>[COLOR_GREEN]+50%[ENDCOLOR] Production bonus active.</Text>
</Row>
```

### Common Color Tags

| Tag | Color |
|-----|-------|
| `[COLOR_RED]` | Red (warnings, negative) |
| `[COLOR_GREEN]` | Green (positive, bonuses) |
| `[COLOR_BLUE]` | Blue (links, info) |
| `[COLOR_YELLOW]` | Yellow (highlights) |
| `[COLOR_GREY]` | Grey (disabled, secondary) |
| `[COLOR_FLOAT_FOOD]` | Food color |
| `[COLOR_FLOAT_PRODUCTION]` | Production color |
| `[COLOR_FLOAT_GOLD]` | Gold color |
| `[COLOR_FLOAT_SCIENCE]` | Science color |
| `[COLOR_FLOAT_CULTURE]` | Culture color |
| `[ENDCOLOR]` | End color formatting |

### Line Breaks

Use `[NEWLINE]` for line breaks:

```xml
<Row Tag="LOC_MY_MULTILINE_TEXT" Language="en_US">
    <Text>First line of text.[NEWLINE]Second line of text.[NEWLINE][NEWLINE]Paragraph after blank line.</Text>
</Row>
```

## Dynamic Text

### Variable Substitution

Use curly braces for dynamic values:

```xml
<Row Tag="LOC_MY_DYNAMIC_TEXT" Language="en_US">
    <Text>You have earned {1_Amount} [ICON_GOLD] Gold from {2_Source}.</Text>
</Row>
```

### Numbered Parameters

| Pattern | Usage |
|---------|-------|
| `{1_Name}` | First parameter with label |
| `{2_Name}` | Second parameter with label |
| `{Amount}` | Named parameter |
| `{1}` | Simple numbered parameter |

### Example: Modifier Description

```xml
<Row Tag="LOC_MOD_BONUS_YIELD_DESCRIPTION" Language="en_US">
    <Text>+{1_Amount} {2_YieldIcon} {3_YieldName} in this city.</Text>
</Row>
```

## Gender and Plurality

### Gender Variants

Some languages require gender-specific text:

```xml
<!-- German example with gender -->
<Row Tag="LOC_LEADER_GREETING" Language="de_DE" Gender="Male">
    <Text>Willkommen, mein Freund.</Text>
</Row>
<Row Tag="LOC_LEADER_GREETING" Language="de_DE" Gender="Female">
    <Text>Willkommen, meine Freundin.</Text>
</Row>
```

### Plural Forms

Handle singular/plural variations:

```xml
<!-- Plurality: 1 = singular, 2 = plural -->
<Row Tag="LOC_UNIT_COUNT" Language="en_US" Plurality="1">
    <Text>{1_Count} Unit</Text>
</Row>
<Row Tag="LOC_UNIT_COUNT" Language="en_US" Plurality="2">
    <Text>{1_Count} Units</Text>
</Row>

<!-- Russian has more plural forms -->
<Row Tag="LOC_UNIT_COUNT" Language="ru_RU" Plurality="1">
    <Text>{1_Count} юнит</Text>
</Row>
<Row Tag="LOC_UNIT_COUNT" Language="ru_RU" Plurality="2">
    <Text>{1_Count} юнита</Text>
</Row>
<Row Tag="LOC_UNIT_COUNT" Language="ru_RU" Plurality="5">
    <Text>{1_Count} юнитов</Text>
</Row>
```

## File Organization

### Recommended Structure

```
MyMod/
├── Text/
│   ├── en_US/
│   │   ├── MyMod_Text.xml
│   │   ├── MyMod_Units_Text.xml
│   │   └── MyMod_Buildings_Text.xml
│   ├── fr_FR/
│   │   ├── MyMod_Text.xml
│   │   └── ...
│   └── de_DE/
│       └── ...
└── MyMod.modinfo
```

### ModInfo Configuration

```xml
<Mod id="my-mod" version="1.0">
    <Properties>
        <Name>My Mod</Name>
    </Properties>

    <LocalizedText>
        <!-- English (required as fallback) -->
        <Text id="my-mod-text-en">
            <Items>
                <File>Text/en_US/MyMod_Text.xml</File>
            </Items>
        </Text>

        <!-- Optional translations -->
        <Text id="my-mod-text-fr">
            <Items>
                <File>Text/fr_FR/MyMod_Text.xml</File>
            </Items>
            <Criteria>
                <LanguageId>fr_FR</LanguageId>
            </Criteria>
        </Text>

        <Text id="my-mod-text-de">
            <Items>
                <File>Text/de_DE/MyMod_Text.xml</File>
            </Items>
            <Criteria>
                <LanguageId>de_DE</LanguageId>
            </Criteria>
        </Text>
    </LocalizedText>
</Mod>
```

## Complete Example: Localized Civilization

### English Text (Text/en_US/MyCiv_Text.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <!-- Civilization -->
        <Row Tag="LOC_CIV_MY_CIVILIZATION_NAME" Language="en_US">
            <Text>Atlantis</Text>
        </Row>
        <Row Tag="LOC_CIV_MY_CIVILIZATION_DESCRIPTION" Language="en_US">
            <Text>The legendary island civilization of Atlantis.</Text>
        </Row>
        <Row Tag="LOC_CIV_MY_CIVILIZATION_ADJECTIVE" Language="en_US">
            <Text>Atlantean</Text>
        </Row>

        <!-- Leader -->
        <Row Tag="LOC_LEADER_MY_LEADER_NAME" Language="en_US">
            <Text>Poseidon</Text>
        </Row>
        <Row Tag="LOC_LEADER_MY_LEADER_SUBTITLE" Language="en_US">
            <Text>God-King of Atlantis</Text>
        </Row>

        <!-- Unique Ability -->
        <Row Tag="LOC_TRAIT_MY_ABILITY_NAME" Language="en_US">
            <Text>Masters of the Sea</Text>
        </Row>
        <Row Tag="LOC_TRAIT_MY_ABILITY_DESCRIPTION" Language="en_US">
            <Text>Naval units receive +5 [ICON_STRENGTH] Combat Strength. Coastal cities gain +2 [ICON_FOOD] Food and +2 [ICON_GOLD] Gold.</Text>
        </Row>

        <!-- Unique Unit -->
        <Row Tag="LOC_UNIT_ATLANTEAN_TRIREME_NAME" Language="en_US">
            <Text>Atlantean Trireme</Text>
        </Row>
        <Row Tag="LOC_UNIT_ATLANTEAN_TRIREME_DESCRIPTION" Language="en_US">
            <Text>Unique Atlantean naval unit. Stronger than the regular Trireme and heals in ocean tiles.</Text>
        </Row>

        <!-- Unique Building -->
        <Row Tag="LOC_BUILDING_LIGHTHOUSE_OF_ATLANTIS_NAME" Language="en_US">
            <Text>Lighthouse of Atlantis</Text>
        </Row>
        <Row Tag="LOC_BUILDING_LIGHTHOUSE_OF_ATLANTIS_DESCRIPTION" Language="en_US">
            <Text>Unique Atlantean building. Provides +3 [ICON_GOLD] Gold and +1 [ICON_SCIENCE] Science. Naval units trained here start with a free promotion.</Text>
        </Row>

        <!-- Civilopedia -->
        <Row Tag="LOC_PEDIA_CIV_MY_CIVILIZATION_CHAPTER_HISTORY" Language="en_US">
            <Text>Atlantis, as described by Plato, was a powerful island nation that existed around 9,000 years before his time. According to the legend, the Atlanteans possessed advanced technology and a sophisticated culture that rivaled any civilization of the ancient world.[NEWLINE][NEWLINE]The island was said to be larger than Libya and Asia Minor combined, located beyond the Pillars of Hercules (the Strait of Gibraltar). Its capital city featured concentric rings of water and land, connected by bridges and tunnels.</Text>
        </Row>
    </LocalizedText>
</Database>
```

### French Translation (Text/fr_FR/MyCiv_Text.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_CIV_MY_CIVILIZATION_NAME" Language="fr_FR">
            <Text>Atlantide</Text>
        </Row>
        <Row Tag="LOC_CIV_MY_CIVILIZATION_DESCRIPTION" Language="fr_FR">
            <Text>La légendaire civilisation insulaire de l'Atlantide.</Text>
        </Row>
        <Row Tag="LOC_CIV_MY_CIVILIZATION_ADJECTIVE" Language="fr_FR">
            <Text>Atlante</Text>
        </Row>

        <Row Tag="LOC_LEADER_MY_LEADER_NAME" Language="fr_FR">
            <Text>Poséidon</Text>
        </Row>
        <Row Tag="LOC_LEADER_MY_LEADER_SUBTITLE" Language="fr_FR">
            <Text>Dieu-Roi de l'Atlantide</Text>
        </Row>

        <Row Tag="LOC_TRAIT_MY_ABILITY_NAME" Language="fr_FR">
            <Text>Maîtres des Mers</Text>
        </Row>
        <Row Tag="LOC_TRAIT_MY_ABILITY_DESCRIPTION" Language="fr_FR">
            <Text>Les unités navales reçoivent +5 [ICON_STRENGTH] Force de combat. Les villes côtières gagnent +2 [ICON_FOOD] Nourriture et +2 [ICON_GOLD] Or.</Text>
        </Row>
    </LocalizedText>
</Database>
```

### German Translation (Text/de_DE/MyCiv_Text.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_CIV_MY_CIVILIZATION_NAME" Language="de_DE">
            <Text>Atlantis</Text>
        </Row>
        <Row Tag="LOC_CIV_MY_CIVILIZATION_DESCRIPTION" Language="de_DE">
            <Text>Die legendäre Inselzivilisation von Atlantis.</Text>
        </Row>
        <Row Tag="LOC_CIV_MY_CIVILIZATION_ADJECTIVE" Language="de_DE">
            <Text>Atlantisch</Text>
        </Row>

        <Row Tag="LOC_LEADER_MY_LEADER_NAME" Language="de_DE">
            <Text>Poseidon</Text>
        </Row>
        <Row Tag="LOC_LEADER_MY_LEADER_SUBTITLE" Language="de_DE">
            <Text>Gottkönig von Atlantis</Text>
        </Row>

        <Row Tag="LOC_TRAIT_MY_ABILITY_NAME" Language="de_DE">
            <Text>Meister der Meere</Text>
        </Row>
        <Row Tag="LOC_TRAIT_MY_ABILITY_DESCRIPTION" Language="de_DE">
            <Text>Marineeinheiten erhalten +5 [ICON_STRENGTH] Kampfstärke. Küstenstädte erhalten +2 [ICON_FOOD] Nahrung und +2 [ICON_GOLD] Gold.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Accessing Text in JavaScript

When using the JavaScript API, access localized text:

```javascript
// Get localized string
const text = Locale.Lookup("LOC_MY_TEXT_KEY");

// Get localized string with parameters
const formattedText = Locale.Compose("LOC_MY_DYNAMIC_TEXT", amount, sourceName);

// Check if a localization key exists
const exists = Locale.HasText("LOC_MY_TEXT_KEY");

// Get current language
const currentLanguage = Locale.GetCurrentLanguage();
```

## Best Practices

1. **Always Use LOC_ Prefix** - All text keys must start with LOC_
2. **English First** - Always provide English (en_US) as the base/fallback language
3. **Consistent Naming** - Follow naming conventions for easy maintenance
4. **Separate Files** - Organize text by category (units, buildings, etc.)
5. **Test All Languages** - Verify text displays correctly in different languages
6. **Handle Length** - Some languages produce longer text; design UI accordingly
7. **Use Icons** - Icons are universal and reduce translation needs
8. **Provide Context** - Use clear, descriptive tag names for translators
9. **Avoid Concatenation** - Don't build sentences from multiple tags; word order varies
10. **Test Special Characters** - Ensure proper encoding for non-ASCII characters

## Troubleshooting

### Missing Text

If you see raw LOC_ tags in game:
- Verify the tag name matches exactly (case-sensitive)
- Check that the language code is correct
- Ensure the XML file is loaded in modinfo
- Verify XML syntax is valid

### Encoding Issues

Always use UTF-8 encoding:
```xml
<?xml version="1.0" encoding="utf-8"?>
```

### Icon Not Displaying

- Check icon tag spelling (case-sensitive)
- Verify the icon exists in the game

## Related Documentation

- [ModInfo Files](../Modinfo-Files.md)
- [Getting Started](../Getting-Started.md)
- [Civilizations](../database/Civilizations.md)
- [Leaders](../database/Leaders.md)
