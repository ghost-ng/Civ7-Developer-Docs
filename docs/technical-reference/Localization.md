# Localization System

This document covers the localization system in Civilization VII, including text keys, translations, formatting, and how to add multi-language support to mods.

## Overview

Civ VII's localization system allows mods to provide text in multiple languages:

- **Language-specific Text Tables** - Store translatable strings per language
- **Text Tags** - Unique identifiers for each string (LOC_*)
- **Language Codes** - ISO language identifiers
- **Text Formatting** - Icons, colors, and dynamic values
- **Gender/Plural Support** - Grammar handling for different languages

## Core Structure

Unlike some earlier documentation may suggest, Civ VII uses **language-specific table names** rather than a single `LocalizedText` table with a `Language` column.

### Correct Text File Format

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <EnglishText>
        <Row Tag="LOC_MY_TEXT_KEY">
            <Text>This is my English text.</Text>
        </Row>
    </EnglishText>
</Database>
```

> **Important:** The table name (e.g., `EnglishText`) determines the language. Do NOT use a `Language` attribute on rows.

### Language Table Names

| Language | Table Name |
|----------|------------|
| English (US) | `EnglishText` |
| French | `FrenchText` |
| German | `GermanText` |
| Spanish | `SpanishText` |
| Italian | `ItalianText` |
| Japanese | `JapaneseText` |
| Korean | `KoreanText` |
| Polish | `PolishText` |
| Portuguese (Brazil) | `PortugueseText` |
| Russian | `RussianText` |
| Chinese (Simplified) | `SimplifiedChineseText` |
| Chinese (Traditional) | `TraditionalChineseText` |

## Supported Language Codes

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

## File Organization

### Recommended Structure

```
MyMod/
├── text/
│   ├── en_us/
│   │   ├── MyMod_Text.xml
│   │   ├── MyMod_Units_Text.xml
│   │   └── MyMod_Buildings_Text.xml
│   ├── fr_fr/
│   │   └── MyMod_Text.xml
│   └── de_de/
│       └── MyMod_Text.xml
└── my-mod.modinfo
```

### ModInfo Configuration

```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-mod" version="1" xmlns="ModInfo">
    <Properties>
        <Name>My Mod</Name>
    </Properties>
    <ActionCriteria>
        <Criteria id="always"><AlwaysMet/></Criteria>
    </ActionCriteria>
    <ActionGroups>
        <ActionGroup id="game" scope="game" criteria="always">
            <Actions>
                <!-- English (required as fallback) -->
                <UpdateText>
                    <Item>text/en_us/MyMod_Text.xml</Item>
                </UpdateText>

                <!-- Optional: Localized versions using locale attribute -->
                <UpdateText>
                    <Item locale="fr_FR">text/fr_fr/MyMod_Text.xml</Item>
                    <Item locale="de_DE">text/de_de/MyMod_Text.xml</Item>
                </UpdateText>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

## Text Tag Naming Conventions

### Standard Prefixes

| Prefix | Usage |
|--------|-------|
| `LOC_` | All localization tags must start with this |
| `LOC_UNIT_` | Unit names and descriptions |
| `LOC_BUILDING_` | Building names and descriptions |
| `LOC_TECH_` | Technology names and descriptions |
| `LOC_CIVIC_` | Civic names and descriptions |
| `LOC_LEADER_` | Leader names and dialogue |
| `LOC_CIV_` or `LOC_CIVILIZATION_` | Civilization names |
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

## Complete Example: Localized Civilization

### English Text (text/en_us/MyCiv_Text.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <EnglishText>
        <!-- Civilization -->
        <Row Tag="LOC_CIVILIZATION_MY_ATLANTIS_NAME">
            <Text>Atlantis</Text>
        </Row>
        <Row Tag="LOC_CIVILIZATION_MY_ATLANTIS_DESCRIPTION">
            <Text>The legendary island civilization of Atlantis.</Text>
        </Row>
        <Row Tag="LOC_CIVILIZATION_MY_ATLANTIS_ADJECTIVE">
            <Text>Atlantean</Text>
        </Row>

        <!-- Leader -->
        <Row Tag="LOC_LEADER_MY_POSEIDON_NAME">
            <Text>Poseidon</Text>
        </Row>
        <Row Tag="LOC_LEADER_MY_POSEIDON_SUBTITLE">
            <Text>God-King of Atlantis</Text>
        </Row>

        <!-- Unique Ability -->
        <Row Tag="LOC_TRAIT_MY_ABILITY_NAME">
            <Text>Masters of the Sea</Text>
        </Row>
        <Row Tag="LOC_TRAIT_MY_ABILITY_DESCRIPTION">
            <Text>Naval units receive +5 [icon:YIELD_STRENGTH] Combat Strength. Coastal cities gain +2 [icon:YIELD_FOOD] Food and +2 [icon:YIELD_GOLD] Gold.</Text>
        </Row>

        <!-- Unique Unit -->
        <Row Tag="LOC_UNIT_ATLANTEAN_TRIREME_NAME">
            <Text>Atlantean Trireme</Text>
        </Row>
        <Row Tag="LOC_UNIT_ATLANTEAN_TRIREME_DESCRIPTION">
            <Text>Unique Atlantean naval unit. Stronger than the regular Trireme and heals in ocean tiles.</Text>
        </Row>
    </EnglishText>
</Database>
```

### French Translation (text/fr_fr/MyCiv_Text.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <FrenchText>
        <Row Tag="LOC_CIVILIZATION_MY_ATLANTIS_NAME">
            <Text>Atlantide</Text>
        </Row>
        <Row Tag="LOC_CIVILIZATION_MY_ATLANTIS_DESCRIPTION">
            <Text>La legendaire civilisation insulaire de l'Atlantide.</Text>
        </Row>
        <Row Tag="LOC_CIVILIZATION_MY_ATLANTIS_ADJECTIVE">
            <Text>Atlante</Text>
        </Row>

        <Row Tag="LOC_LEADER_MY_POSEIDON_NAME">
            <Text>Poseidon</Text>
        </Row>
        <Row Tag="LOC_LEADER_MY_POSEIDON_SUBTITLE">
            <Text>Dieu-Roi de l'Atlantide</Text>
        </Row>

        <Row Tag="LOC_TRAIT_MY_ABILITY_NAME">
            <Text>Maitres des Mers</Text>
        </Row>
        <Row Tag="LOC_TRAIT_MY_ABILITY_DESCRIPTION">
            <Text>Les unites navales recoivent +5 [icon:YIELD_STRENGTH] Force de combat. Les villes cotieres gagnent +2 [icon:YIELD_FOOD] Nourriture et +2 [icon:YIELD_GOLD] Or.</Text>
        </Row>
    </FrenchText>
</Database>
```

## Text Formatting

### Icons

Use icon tags to display game icons in text:

| Icon Tag | Display |
|----------|---------|
| `[icon:YIELD_FOOD]` | Food icon |
| `[icon:YIELD_PRODUCTION]` | Production icon |
| `[icon:YIELD_GOLD]` | Gold icon |
| `[icon:YIELD_SCIENCE]` | Science icon |
| `[icon:YIELD_CULTURE]` | Culture icon |
| `[icon:YIELD_FAITH]` | Faith/Happiness icon |
| `[icon:YIELD_DIPLOMACY]` | Diplomacy icon |
| `[icon:YIELD_STRENGTH]` | Combat strength icon |

### Example with Icons

```xml
<Row Tag="LOC_MY_BUILDING_DESCRIPTION">
    <Text>Provides +2 [icon:YIELD_FOOD] Food and +1 [icon:YIELD_PRODUCTION] Production.</Text>
</Row>
```

### Colors

Use color tags for emphasis:

```xml
<Row Tag="LOC_MY_WARNING_TEXT">
    <Text>[COLOR_RED]Warning:[ENDCOLOR] This action cannot be undone.</Text>
</Row>

<Row Tag="LOC_MY_BONUS_TEXT">
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
| `[ENDCOLOR]` | End color formatting |

### Line Breaks

Use `[N]` or `[NEWLINE]` for line breaks:

```xml
<Row Tag="LOC_MY_MULTILINE_TEXT">
    <Text>First line of text.[N]Second line of text.[N][N]Paragraph after blank line.</Text>
</Row>
```

## Dynamic Text

### Variable Substitution

Use curly braces for dynamic values:

```xml
<Row Tag="LOC_MY_DYNAMIC_TEXT">
    <Text>You have earned {1_Amount} [icon:YIELD_GOLD] Gold from {2_Source}.</Text>
</Row>
```

### Numbered Parameters

| Pattern | Usage |
|---------|-------|
| `{1_Name}` | First parameter with label |
| `{2_Name}` | Second parameter with label |
| `{Amount}` | Named parameter |
| `{1}` | Simple numbered parameter |

## Gender Support

Some languages require gender-specific text. Use the `Gender` child element:

```xml
<Row Tag="LOC_CIVILIZATION_AKSUM_ADJECTIVE">
    <Text>Aksumite</Text>
    <Gender>Masculine:an</Gender>
</Row>
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
- Check that you're using the correct table name (`EnglishText`, not `LocalizedText`)
- Ensure the XML file is loaded in modinfo via `<UpdateText>`
- Verify XML syntax is valid

### Encoding Issues

Always use UTF-8 encoding:
```xml
<?xml version="1.0" encoding="utf-8"?>
```

### Icon Not Displaying

- Check icon tag spelling (case-sensitive)
- Use the correct format: `[icon:YIELD_NAME]`
- Verify the icon exists in the game

## Related Documentation

- [ModInfo Files](../Modinfo-Files.md)
- [Getting Started](../Getting-Started.md)
- [Civilizations](../database/Civilizations.md)
- [Leaders](../database/Leaders.md)
