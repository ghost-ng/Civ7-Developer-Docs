# Government System

This document covers the government system in Civilization VII, including government types, golden ages, and their bonuses.

## Overview

In Civ VII, governments are major choices that affect your entire empire. Each Age offers different government options, and each government has associated Golden Ages that can be triggered during celebratory periods.

## Core Tables

| Table | Purpose |
|-------|---------|
| `Types` | Register governments as `KIND_GOVERNMENT` |
| `Governments` | Define government details |
| `StartingGovernments` | Available governments per Age |
| `GovernmentModifiers` | Permanent bonuses from governments |
| `GovernmentAttributes` | Attribute bonuses from governments |
| `GoldenAges` | Define golden age bonuses |
| `GoldenAgeModifiers` | Link golden ages to effects |
| `Government_ValidGoldenAges` | Which golden ages each government can use |

## Defining a Government

### Step 1: Register the Type

```xml
<Types>
    <Row Type="GOVERNMENT_MY_GOVERNMENT" Kind="KIND_GOVERNMENT"/>
</Types>
```

### Step 2: Define Government Details

```xml
<Governments>
    <Row GovernmentType="GOVERNMENT_MY_GOVERNMENT"
         Name="LOC_GOVERNMENT_MY_GOVERNMENT_NAME"
         Description="LOC_GOVERNMENT_MY_GOVERNMENT_DESCRIPTION"
         CelebrationName="LOC_GOVERNMENT_MY_GOVERNMENT_CELEBRATION_NAME"/>
</Governments>
```

### Governments Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `GovernmentType` | TEXT | Unique government identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |
| `CelebrationName` | TEXT | Localization key for celebration/golden age period name |

### Step 3: Make Available in Age

```xml
<StartingGovernments>
    <Row AgeType="AGE_ANTIQUITY" GovernmentType="GOVERNMENT_MY_GOVERNMENT"/>
</StartingGovernments>
```

### StartingGovernments Columns

| Column | Type | Description |
|--------|------|-------------|
| `AgeType` | TEXT | Age this government is available in |
| `GovernmentType` | TEXT | Government reference |

## Government Modifiers

Add permanent bonuses that apply while using this government:

```xml
<GovernmentModifiers>
    <Row GovernmentType="GOVERNMENT_MY_GOVERNMENT" ModifierId="MY_GOVERNMENT_MOD_BONUS"/>
</GovernmentModifiers>
```

Define the modifier in GameEffects:

```xml
<GameEffects xmlns="GameEffects">
    <Modifier id="MY_GOVERNMENT_MOD_BONUS"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_CULTURE</Argument>
        <Argument name="Amount">2</Argument>
    </Modifier>
</GameEffects>
```

## Government Attributes

Link governments to attribute bonuses:

```xml
<GovernmentAttributes>
    <Row GovernmentType="GOVERNMENT_MY_GOVERNMENT" AttributeType="ATTRIBUTE_CULTURAL"/>
</GovernmentAttributes>
```

## Golden Ages

Golden Ages are special bonus periods tied to governments. Each government can have multiple golden age options.

### Step 1: Register Golden Age Type

```xml
<Types>
    <Row Type="GOLDEN_AGE_MY_GOVERNMENT_1" Kind="KIND_GOLDEN_AGE"/>
    <Row Type="GOLDEN_AGE_MY_GOVERNMENT_2" Kind="KIND_GOLDEN_AGE"/>
</Types>
```

### Step 2: Define Golden Ages

```xml
<GoldenAges>
    <Row GoldenAgeType="GOLDEN_AGE_MY_GOVERNMENT_1"
         Name="LOC_GOLDEN_AGE_MY_GOVERNMENT_NAME"
         Description="LOC_GOLDEN_AGE_MY_GOVERNMENT_1_DESCRIPTION"/>
    <Row GoldenAgeType="GOLDEN_AGE_MY_GOVERNMENT_2"
         Name="LOC_GOLDEN_AGE_MY_GOVERNMENT_NAME"
         Description="LOC_GOLDEN_AGE_MY_GOVERNMENT_2_DESCRIPTION"/>
</GoldenAges>
```

### GoldenAges Table Columns

| Column | Type | Description |
|--------|------|-------------|
| `GoldenAgeType` | TEXT | Unique golden age identifier |
| `Name` | TEXT | Localization key for name |
| `Description` | TEXT | Localization key for description |

### Step 3: Link Golden Ages to Modifiers

```xml
<GoldenAgeModifiers>
    <Row GoldenAgeType="GOLDEN_AGE_MY_GOVERNMENT_1" ModifierID="MOD_GOLDEN_AGE_MY_GOVERNMENT_CULTURE"/>
    <Row GoldenAgeType="GOLDEN_AGE_MY_GOVERNMENT_2" ModifierID="MOD_GOLDEN_AGE_MY_GOVERNMENT_PRODUCTION"/>
</GoldenAgeModifiers>
```

### Step 4: Link Golden Ages to Government

```xml
<Government_ValidGoldenAges>
    <Row GovernmentType="GOVERNMENT_MY_GOVERNMENT" GoldenAgeType="GOLDEN_AGE_MY_GOVERNMENT_1"/>
    <Row GovernmentType="GOVERNMENT_MY_GOVERNMENT" GoldenAgeType="GOLDEN_AGE_MY_GOVERNMENT_2"/>
</Government_ValidGoldenAges>
```

## Governments by Age

### Antiquity Age

| Government | Description |
|------------|-------------|
| `GOVERNMENT_CLASSICAL_REPUBLIC` | Citizen-led governance emphasizing culture |
| `GOVERNMENT_DESPOTISM` | Centralized autocratic rule |
| `GOVERNMENT_OLIGARCHY` | Rule by a small elite group |

### Exploration Age

| Government | Description |
|------------|-------------|
| `GOVERNMENT_FEUDAL_MONARCHY` | Hierarchical noble system |
| `GOVERNMENT_PLUTOCRACY` | Rule by the wealthy |
| `GOVERNMENT_THEOCRACY` | Religious-based governance |
| `GOVERNMENT_REVOLUTIONARY_AUTHORITARIANISM` | Revolutionary centralized power |
| `GOVERNMENT_CONSTITUTIONAL_MONARCHY` | Monarchy with legal limits |
| `GOVERNMENT_REVOLUTIONARY_REPUBLIC` | Revolutionary democratic system |

### Modern Age

| Government | Description |
|------------|-------------|
| `GOVERNMENT_AUTHORITARIANISM` | Modern autocratic rule |
| `GOVERNMENT_BUREAUCRATIC_MONARCHY` | Administrative monarchy |
| `GOVERNMENT_ELECTIVE_REPUBLIC` | Democratic republic |
| `GOVERNMENT_REVOLUCION` | Revolutionary government |

## Antiquity Golden Ages

Each Antiquity government has two golden age options:

### Classical Republic Golden Ages
| Type | Effect |
|------|--------|
| `GOLDEN_AGE_CLASSICAL_REPUBLIC_1` | Culture bonus |
| `GOLDEN_AGE_CLASSICAL_REPUBLIC_2` | Wonder production bonus |

### Despotism Golden Ages
| Type | Effect |
|------|--------|
| `GOLDEN_AGE_DESPOTISM_1` | Science bonus |
| `GOLDEN_AGE_DESPOTISM_2` | Unit production bonus |

### Oligarchy Golden Ages
| Type | Effect |
|------|--------|
| `GOLDEN_AGE_OLIGARCHY_1` | Food bonus |
| `GOLDEN_AGE_OLIGARCHY_2` | Building production bonus |

## Complete Example: New Government

### Database XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <!-- 1. Register types -->
    <Types>
        <Row Type="GOVERNMENT_MERCHANT_REPUBLIC" Kind="KIND_GOVERNMENT"/>
        <Row Type="GOLDEN_AGE_MERCHANT_REPUBLIC_1" Kind="KIND_GOLDEN_AGE"/>
        <Row Type="GOLDEN_AGE_MERCHANT_REPUBLIC_2" Kind="KIND_GOLDEN_AGE"/>
    </Types>

    <!-- 2. Define government -->
    <Governments>
        <Row GovernmentType="GOVERNMENT_MERCHANT_REPUBLIC"
             Name="LOC_GOVERNMENT_MERCHANT_REPUBLIC_NAME"
             Description="LOC_GOVERNMENT_MERCHANT_REPUBLIC_DESCRIPTION"
             CelebrationName="LOC_GOVERNMENT_MERCHANT_REPUBLIC_CELEBRATION_NAME"/>
    </Governments>

    <!-- 3. Make available in Exploration Age -->
    <StartingGovernments>
        <Row AgeType="AGE_EXPLORATION" GovernmentType="GOVERNMENT_MERCHANT_REPUBLIC"/>
    </StartingGovernments>

    <!-- 4. Add permanent modifiers -->
    <GovernmentModifiers>
        <Row GovernmentType="GOVERNMENT_MERCHANT_REPUBLIC" ModifierId="MERCHANT_REPUBLIC_MOD_TRADE_GOLD"/>
    </GovernmentModifiers>

    <!-- 5. Define golden ages -->
    <GoldenAges>
        <Row GoldenAgeType="GOLDEN_AGE_MERCHANT_REPUBLIC_1"
             Name="LOC_GOLDEN_AGE_MERCHANT_REPUBLIC_NAME"
             Description="LOC_GOLDEN_AGE_MERCHANT_REPUBLIC_1_DESCRIPTION"/>
        <Row GoldenAgeType="GOLDEN_AGE_MERCHANT_REPUBLIC_2"
             Name="LOC_GOLDEN_AGE_MERCHANT_REPUBLIC_NAME"
             Description="LOC_GOLDEN_AGE_MERCHANT_REPUBLIC_2_DESCRIPTION"/>
    </GoldenAges>

    <!-- 6. Link golden ages to modifiers -->
    <GoldenAgeModifiers>
        <Row GoldenAgeType="GOLDEN_AGE_MERCHANT_REPUBLIC_1" ModifierID="MOD_GOLDEN_AGE_MERCHANT_TRADE"/>
        <Row GoldenAgeType="GOLDEN_AGE_MERCHANT_REPUBLIC_2" ModifierID="MOD_GOLDEN_AGE_MERCHANT_GOLD"/>
    </GoldenAgeModifiers>

    <!-- 7. Link golden ages to government -->
    <Government_ValidGoldenAges>
        <Row GovernmentType="GOVERNMENT_MERCHANT_REPUBLIC" GoldenAgeType="GOLDEN_AGE_MERCHANT_REPUBLIC_1"/>
        <Row GovernmentType="GOVERNMENT_MERCHANT_REPUBLIC" GoldenAgeType="GOLDEN_AGE_MERCHANT_REPUBLIC_2"/>
    </Government_ValidGoldenAges>
</Database>
```

### GameEffects XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameEffects xmlns="GameEffects">
    <!-- Permanent government bonus -->
    <Modifier id="MERCHANT_REPUBLIC_MOD_TRADE_GOLD"
              collection="COLLECTION_PLAYER_TRADE_ROUTES"
              effect="EFFECT_TRADE_ROUTE_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="Amount">3</Argument>
    </Modifier>

    <!-- Golden Age 1: Trade Route bonus -->
    <Modifier id="MOD_GOLDEN_AGE_MERCHANT_TRADE"
              collection="COLLECTION_OWNER"
              effect="EFFECT_PLAYER_ADJUST_TRADE_ROUTE_CAPACITY">
        <Argument name="Amount">2</Argument>
    </Modifier>

    <!-- Golden Age 2: Gold per city -->
    <Modifier id="MOD_GOLDEN_AGE_MERCHANT_GOLD"
              collection="COLLECTION_PLAYER_CITIES"
              effect="EFFECT_CITY_ADJUST_YIELD">
        <Argument name="YieldType">YIELD_GOLD</Argument>
        <Argument name="Amount">5</Argument>
    </Modifier>
</GameEffects>
```

### Localization

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_GOVERNMENT_MERCHANT_REPUBLIC_NAME" Language="en_US">
            <Text>Merchant Republic</Text>
        </Row>
        <Row Tag="LOC_GOVERNMENT_MERCHANT_REPUBLIC_DESCRIPTION" Language="en_US">
            <Text>A republic governed by wealthy merchants. Trade routes provide +3 [ICON_GOLD] Gold.</Text>
        </Row>
        <Row Tag="LOC_GOVERNMENT_MERCHANT_REPUBLIC_CELEBRATION_NAME" Language="en_US">
            <Text>Age of Commerce</Text>
        </Row>
        <Row Tag="LOC_GOLDEN_AGE_MERCHANT_REPUBLIC_NAME" Language="en_US">
            <Text>Golden Age of Trade</Text>
        </Row>
        <Row Tag="LOC_GOLDEN_AGE_MERCHANT_REPUBLIC_1_DESCRIPTION" Language="en_US">
            <Text>+2 Trade Route capacity.</Text>
        </Row>
        <Row Tag="LOC_GOLDEN_AGE_MERCHANT_REPUBLIC_2_DESCRIPTION" Language="en_US">
            <Text>+5 [ICON_GOLD] Gold in all cities.</Text>
        </Row>
    </LocalizedText>
</Database>
```

## AI Government Preferences

Make the AI prefer certain governments using AI biases (see [AI Behavior](../civilization-creation/AI-Behavior.md)):

```xml
<AiListTypes>
    <Row ListType="MyCiv Government Bias"/>
</AiListTypes>

<AiLists>
    <Row ListType="MyCiv Government Bias" LeaderType="TRAIT_MY_CIV" System="GovernmentBiases"/>
</AiLists>

<AiFavoredItems>
    <Row ListType="MyCiv Government Bias" Item="GOVERNMENT_MERCHANT_REPUBLIC" Value="50"/>
</AiFavoredItems>
```

## Best Practices

1. **Balance Golden Ages** - Provide meaningful choices between different bonuses
2. **Thematic Consistency** - Match government bonuses to their historical nature
3. **Age Appropriateness** - Only add governments to ages where they make sense
4. **Two Golden Ages** - Follow the pattern of offering two golden age options
5. **Clear Descriptions** - Help players understand what each government provides
6. **AI Biases** - Set appropriate AI preferences for civilizations

## Related Documentation

- [Civic Trees](Civic-Trees.md)
- [AI Behavior](../civilization-creation/AI-Behavior.md)
- [Modifier System](../modifiers/Overview.md)
- [Types and Kinds](../database/Types-and-Kinds.md)
