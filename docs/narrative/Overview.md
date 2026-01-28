# Narrative Events System

Narrative Events are pop-up events that add storytelling and choices to Civilization VII gameplay.

## What are Narrative Events?

Narrative Events appear as pop-ups during gameplay, presenting:
- Story text and flavor
- Choices with different outcomes
- Quests with objectives
- Discoveries (goody huts)

![Narrative Event Structure](../images/narrative-structure.png)

## Event Structure

Most pop-ups are composed of **multiple Narrative Events**:

1. **Parent Event** - The pop-up itself with title and body text
2. **Child Events** - Each button/choice is a separate linked event

```
Event A (Pop-up)
├── Event B (Choice 1 button)
├── Event C (Choice 2 button)
└── Event D (Choice 3 button)
```

## Text Elements

### Pop-up Text (Parent Event)
- **StoryTitle** - Title at top of pop-up
- **Completion** - Body/flavor text

### Button Text (Child Events)
- **Name** - Main button text
- **Imperative** - Quest objective (optional)
- **Description** - Tooltip showing rewards/effects

## Creating a Basic Event

### 1. Register Types

```xml
<Types>
    <Row Type="EVENT_MY_POPUP" Kind="KIND_NARRATIVE_STORY"/>
    <Row Type="EVENT_MY_CHOICE_A" Kind="KIND_NARRATIVE_STORY"/>
    <Row Type="EVENT_MY_CHOICE_B" Kind="KIND_NARRATIVE_STORY"/>
</Types>
```

### 2. Define NarrativeStories

```xml
<NarrativeStories>
    <!-- Parent Event (the pop-up) -->
    <Row
        NarrativeStoryType="EVENT_MY_POPUP"
        Name="LOC_EVENT_MY_POPUP_NAME"
        Description="LOC_EVENT_MY_POPUP_DESCRIPTION"
        Completion="LOC_EVENT_MY_POPUP_COMPLETION"
        StoryTitle="LOC_EVENT_MY_POPUP_STORYTITLE"
        Age="AGE_ANTIQUITY"
        Activation="REQUISITE"
        ActivationRequirementSetId="REQSET_MY_ACTIVATION"
        RequirementSetId="REQSET_MY_TRIGGER"
        UIActivation="STANDARD"
    />
    <!-- Choice A -->
    <Row
        NarrativeStoryType="EVENT_MY_CHOICE_A"
        Name="LOC_EVENT_MY_CHOICE_A_NAME"
        Description="LOC_EVENT_MY_CHOICE_A_DESCRIPTION"
        Completion="LOC_EVENT_MY_CHOICE_A_COMPLETION"
        Age="AGE_ANTIQUITY"
        Activation="LINKED"
        RequirementSetId="Met"
        UIActivation="STANDARD"
    />
    <!-- Choice B -->
    <Row
        NarrativeStoryType="EVENT_MY_CHOICE_B"
        Name="LOC_EVENT_MY_CHOICE_B_NAME"
        Description="LOC_EVENT_MY_CHOICE_B_DESCRIPTION"
        Completion="LOC_EVENT_MY_CHOICE_B_COMPLETION"
        Age="AGE_ANTIQUITY"
        Activation="LINKED"
        RequirementSetId="Met"
        UIActivation="STANDARD"
    />
</NarrativeStories>
```

### 3. Link Events

```xml
<NarrativeStory_Links>
    <Row
        FromNarrativeStoryType="EVENT_MY_POPUP"
        ToNarrativeStoryType="EVENT_MY_CHOICE_A"
        Name="LOC_EVENT_MY_CHOICE_A_NAME"
        Description="LOC_EVENT_MY_CHOICE_A_DESCRIPTION"
    />
    <Row
        FromNarrativeStoryType="EVENT_MY_POPUP"
        ToNarrativeStoryType="EVENT_MY_CHOICE_B"
        Name="LOC_EVENT_MY_CHOICE_B_NAME"
        Description="LOC_EVENT_MY_CHOICE_B_DESCRIPTION"
    />
</NarrativeStory_Links>
```

## Key Columns

### NarrativeStories

| Column | Required | Description |
|--------|----------|-------------|
| `NarrativeStoryType` | Yes | Unique identifier |
| `Name` | Yes | Button text (can be dummy for pop-ups) |
| `Description` | Yes | Tooltip text (can be dummy for pop-ups) |
| `Completion` | No | Pop-up body text |
| `StoryTitle` | No | Pop-up title |
| `Age` | Yes | `AGE_ANTIQUITY`, `AGE_EXPLORATION`, `AGE_MODERN` |
| `Activation` | Yes | How event activates (see below) |
| `ActivationRequirementSetId` | No | One-time eligibility check |
| `RequirementSetId` | Yes | Trigger/completion condition |
| `UIActivation` | Yes | Visual style |
| `IsQuest` | No | Mark as quest (`TRUE`/`FALSE`) |
| `Hidden` | No | No pop-up, just button (`TRUE`/`FALSE`) |

### Activation Types

| Type | Use For |
|------|---------|
| `REQUISITE` | Initial events - checks requirements |
| `LINKED` | Button choices - no activation check |
| `LINKED_REQUISITE` | Conditional buttons - checks ActivationRequirementSetId |
| `AUTO` | Discovery events |

### UIActivation Types

| Type | Appearance |
|------|------------|
| `STANDARD` | Normal event pop-up |
| `DISCOVERY` | Discovery/goody hut style |

## Requirements

Use RequirementSets to control when events trigger:

### ActivationRequirementSetId
Checked **once** when event initializes. Failed = event never shows.

Use for: Player civilization, leader, permanent conditions.

### RequirementSetId
Checked **continuously**. When met, event "completes" and shows pop-up.

Use for: Game state conditions, quest objectives.

```xml
<!-- In GameEffects XML -->
<RequirementSet id="REQSET_MY_ACTIVATION" type="REQUIREMENTSET_TEST_ANY">
    <Requirement type="REQUIREMENT_PLAYER_CIVILIZATION_TYPE_MATCHES">
        <Argument name="CivilizationType">CIVILIZATION_ROME</Argument>
    </Requirement>
    <Requirement type="REQUIREMENT_PLAYER_CIVILIZATION_TYPE_MATCHES">
        <Argument name="CivilizationType">CIVILIZATION_GREECE</Argument>
    </Requirement>
</RequirementSet>

<RequirementSet id="REQSET_MY_TRIGGER">
    <Requirement type="REQUIREMENT_PLAYER_HAS_FOUNDED_X_CITIES">
        <Argument name="Amount">3</Argument>
    </Requirement>
</RequirementSet>
```

## Adding Rewards

### 1. Create Modifier

```xml
<Modifier id="MY_EVENT_REWARD_MOD" collection="COLLECTION_OWNER" effect="EFFECT_PLAYER_GRANT_YIELD" permanent="true">
    <Argument name="YieldType">YIELD_GOLD</Argument>
    <Argument name="Amount">100</Argument>
</Modifier>
```

Note: Set `permanent="true"` for rewards that persist.

### 2. Create Reward

```xml
<NarrativeRewards>
    <Row NarrativeRewardType="MY_EVENT_REWARD" ModifierID="MY_EVENT_REWARD_MOD"/>
</NarrativeRewards>
```

### 3. Link to Event

```xml
<NarrativeStory_Rewards>
    <Row
        NarrativeStoryType="EVENT_MY_CHOICE_A"
        NarrativeRewardType="MY_EVENT_REWARD"
        Activation="COMPLETE"
    />
</NarrativeStory_Rewards>
```

Activation options:
- `COMPLETE` - When event completes
- `START` - When event activates

## Adding Icons

```xml
<NarrativeRewardIcons>
    <Row NarrativeStoryType="EVENT_MY_CHOICE_A" RewardIconType="YIELD_GOLD"/>
    <Row NarrativeStoryType="EVENT_MY_CHOICE_A" RewardIconType="QUEST"/>
    <Row NarrativeStoryType="EVENT_MY_CHOICE_B" RewardIconType="YIELD_CULTURE"/>
</NarrativeRewardIcons>
```

Use `Negative="true"` for red/negative icons.

## Dynamic Text

Replace text placeholders at runtime:

```xml
<NarrativeStory_TextReplacements>
    <Row
        NarrativeStoryType="EVENT_MY_POPUP"
        NarrativeStoryTextType="BODY"
        NarrativeTextReplacementType="CAPITAL"
        Priority="1"
    />
</NarrativeStory_TextReplacements>
```

### NarrativeStoryTextType

| Type | Replaces In |
|------|-------------|
| `BODY` | Completion text |
| `REWARD` | Description |
| `IMPERATIVE` | Imperative |
| `OPTION` | Name |

### NarrativeTextReplacementType

| Type | Inserts |
|------|---------|
| `REWARD` | Auto-generated reward text |
| `CAPITAL` | Player's capital city name |
| `CIVILIZATION` | Player's civ name |
| `CIVILIZATION_ADJ` | Civ adjective |
| `INDEPENDENT` | Nearby independent name |

Text format: `{1}` for first replacement, `{2}` for second, etc.

```xml
<LocalizedText>
    <Row Tag="LOC_MY_EVENT_COMPLETION" Language="en_US">
        <Text>The people of {1} celebrate!</Text>
    </Row>
</LocalizedText>
```

## See Also

- [NarrativeStories Reference](NarrativeStories.md)
- [Narrative Requirements](Requirements.md)
- [Narrative Rewards](Rewards.md)
- [Discoveries](Discoveries.md)
- [Modifier System](../modifiers/Overview.md)
