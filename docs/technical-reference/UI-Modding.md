# UI Modding

This document covers UI modding in Civilization VII, including screens, widgets, icons, and JavaScript-based UI customization.

## Overview

Civ VII's UI is built with:

- **JavaScript** - UI logic and event handling
- **HTML/CSS-like Markup** - Layout and styling
- **BLP Files** - UI definition files
- **DDS Textures** - UI graphics

## UI System Architecture

```
UI Component
├── Definition (.blp) - Layout and structure
├── Script (.js) - Logic and behavior
├── Style (.css) - Visual styling
└── Assets (.dds) - Graphics and icons
```

## Creating UI Elements

### Basic UI Registration

In your modinfo:

```xml
<Mod id="my-ui-mod" version="1.0">
    <Components>
        <UserInterface id="my-ui">
            <Items>
                <File>UI/MyScreen.blp</File>
                <File>UI/MyScreen.js</File>
            </Items>
        </UserInterface>
    </Components>
</Mod>
```

### UI Definition File (.blp)

BLP files define UI structure:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context Name="MyCustomPanel">
    <Container ID="MainContainer" Size="400,300" Anchor="C,C">
        <Grid ID="Background" Size="parent" Style="Panel_Background"/>

        <Stack ID="ContentStack" Anchor="C,C" StackGrowth="Bottom" Padding="10">
            <Label ID="TitleLabel"
                   String="LOC_MY_PANEL_TITLE"
                   Style="HeaderText"/>

            <Button ID="ActionButton"
                    String="LOC_MY_BUTTON_TEXT"
                    Size="150,40"
                    Style="StandardButton"/>
        </Stack>

        <Button ID="CloseButton"
                Anchor="TR"
                Offset="-5,5"
                Size="30,30"
                Style="CloseButton"/>
    </Container>
</Context>
```

### Common UI Elements

| Element | Purpose |
|---------|---------|
| `Container` | Generic container for grouping |
| `Stack` | Arranges children vertically/horizontally |
| `Grid` | Background or structured layout |
| `Label` | Text display |
| `Button` | Clickable button |
| `Image` | Static image display |
| `TextInput` | Text entry field |
| `Slider` | Value slider |
| `ScrollPanel` | Scrollable content area |
| `List` | Dynamic list of items |

### Element Attributes

| Attribute | Description |
|-----------|-------------|
| `ID` | Unique identifier for JavaScript access |
| `Size` | Width,Height in pixels or "parent" |
| `Anchor` | Position anchor (TL, TC, TR, CL, C, CR, BL, BC, BR) |
| `Offset` | Pixel offset from anchor |
| `Style` | CSS-like style reference |
| `String` | Localization key for text |
| `Visible` | Initial visibility (true/false) |
| `Enabled` | Initial enabled state |

## UI JavaScript

### Basic Screen Controller

```javascript
// MyScreen.js
var MyScreen = {
    // Called when the screen is initialized
    Initialize: function() {
        // Get UI elements
        this.m_MainContainer = document.getElementById("MainContainer");
        this.m_TitleLabel = document.getElementById("TitleLabel");
        this.m_ActionButton = document.getElementById("ActionButton");
        this.m_CloseButton = document.getElementById("CloseButton");

        // Set up event handlers
        this.m_ActionButton.addEventListener("click", this.OnActionClick.bind(this));
        this.m_CloseButton.addEventListener("click", this.OnClose.bind(this));

        // Subscribe to game events
        Events.CitySelectionChanged.add(this.OnCitySelected.bind(this));
    },

    // Button click handler
    OnActionClick: function() {
        // Perform action
        console.log("Action button clicked!");

        // Play sound
        UI.PlaySound("Click");

        // Update UI
        this.m_TitleLabel.textContent = Locale.Lookup("LOC_ACTION_COMPLETE");
    },

    // Close button handler
    OnClose: function() {
        UI.CloseScreen("MyScreen");
    },

    // Game event handler
    OnCitySelected: function(playerID, cityID) {
        if (cityID != null) {
            var city = Cities.GetCity(playerID, cityID);
            this.m_TitleLabel.textContent = city.GetName();
        }
    },

    // Called when screen is shown
    OnShow: function() {
        this.m_MainContainer.style.visibility = "visible";
    },

    // Called when screen is hidden
    OnHide: function() {
        this.m_MainContainer.style.visibility = "hidden";
    }
};

// Initialize when loaded
MyScreen.Initialize();
```

### Common UI Functions

```javascript
// Show/hide elements
element.style.visibility = "visible"; // or "hidden"

// Update text
element.textContent = "New Text";
element.textContent = Locale.Lookup("LOC_KEY");
element.textContent = Locale.Compose("LOC_KEY", param1, param2);

// Enable/disable
element.disabled = true; // or false

// Change style
element.className = "NewStyleClass";
element.style.color = "red";

// Play sounds
UI.PlaySound("Click");
UI.PlaySound("Notification");

// Open/close screens
UI.OpenScreen("ScreenName");
UI.CloseScreen("ScreenName");

// Get localized text
var text = Locale.Lookup("LOC_MY_TEXT");
var formatted = Locale.Compose("LOC_MY_FORMAT", value1, value2);
```

### Event System

```javascript
// Subscribe to game events
Events.TurnBegin.add(function(turn) {
    console.log("Turn " + turn + " started");
});

Events.CityProductionCompleted.add(function(playerID, cityID, itemType) {
    // Handle production complete
});

Events.TechnologyResearched.add(function(playerID, techType) {
    // Handle tech researched
});

Events.UnitMoved.add(function(playerID, unitID) {
    // Handle unit movement
});

// Unsubscribe from events
var handler = function() { /* ... */ };
Events.TurnBegin.add(handler);
// Later:
Events.TurnBegin.remove(handler);
```

### Common Events

| Event | Parameters | Trigger |
|-------|------------|---------|
| `TurnBegin` | turn | New turn starts |
| `TurnEnd` | turn | Turn ends |
| `CitySelectionChanged` | playerID, cityID | City selection changes |
| `UnitSelectionChanged` | playerID, unitID | Unit selection changes |
| `CityProductionCompleted` | playerID, cityID, itemType | Production finishes |
| `TechnologyResearched` | playerID, techType | Tech completed |
| `CivicCompleted` | playerID, civicType | Civic completed |
| `WarDeclared` | attackerID, defenderID | War declared |
| `PeaceMade` | player1ID, player2ID | Peace made |

## Modifying Existing UI

### Extending Base Screens

```javascript
// Hook into existing screen
(function() {
    // Save original function
    var originalInitialize = CityPanel.Initialize;

    // Replace with extended version
    CityPanel.Initialize = function() {
        // Call original
        originalInitialize.call(this);

        // Add custom functionality
        this.AddCustomButton();
    };

    CityPanel.AddCustomButton = function() {
        // Create new button
        var button = document.createElement("button");
        button.id = "MyCustomButton";
        button.textContent = Locale.Lookup("LOC_MY_BUTTON");
        button.addEventListener("click", this.OnMyButtonClick.bind(this));

        // Add to existing container
        var container = document.getElementById("CityPanelContainer");
        container.appendChild(button);
    };

    CityPanel.OnMyButtonClick = function() {
        // Custom functionality
        console.log("Custom button clicked!");
    };
})();
```

### Adding Context Menu Items

```javascript
// Add item to unit context menu
ContextMenu.AddUnitAction({
    ID: "MY_CUSTOM_ACTION",
    Name: "LOC_MY_ACTION_NAME",
    Tooltip: "LOC_MY_ACTION_TOOLTIP",
    Icon: "ICON_MY_ACTION",
    Condition: function(unit) {
        // Return true if action should be shown
        return unit.GetMoves() > 0;
    },
    Execute: function(unit) {
        // Perform the action
        console.log("Custom action on unit " + unit.GetID());
    }
});
```

## Custom Icons in UI

### Using Icon Tags in Text

```javascript
// Icons display automatically in localized text
var text = Locale.Lookup("LOC_YIELD_TEXT");
// "LOC_YIELD_TEXT" = "+5 [ICON_GOLD] Gold"
```

### Setting Image Sources

```javascript
// Reference icon from atlas
element.style.backgroundImage = "url('Icons/MyIcon.dds')";

// Use icon definition
Image.SetIcon(imageElement, "ICON_MY_CUSTOM");
```

## UI Styles

### Common Style Classes

| Class | Usage |
|-------|-------|
| `Panel_Background` | Standard panel background |
| `HeaderText` | Large header text |
| `BodyText` | Regular body text |
| `StandardButton` | Default button style |
| `CloseButton` | X close button |
| `TooltipBox` | Tooltip container |
| `ListItem` | List item row |
| `SelectedItem` | Selected list item |

### Custom Styles

```css
/* In style file */
.MyCustomStyle {
    background-color: rgba(0, 0, 0, 0.8);
    border: 2px solid #gold;
    padding: 10px;
    font-size: 14px;
    color: white;
}

.MyCustomButton {
    background-image: url('UI/ButtonBackground.dds');
    border: none;
    padding: 5px 15px;
}

.MyCustomButton:hover {
    background-image: url('UI/ButtonBackgroundHover.dds');
}
```

## Complete Example: Custom Info Panel

### UI Definition (CustomInfoPanel.blp)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context Name="CustomInfoPanel">
    <Container ID="PanelContainer" Size="300,200" Anchor="BR" Offset="-20,-100">
        <Grid ID="Background" Size="parent" Style="Panel_Background"/>

        <Stack ID="ContentStack" Anchor="T,C" Offset="0,10"
               StackGrowth="Bottom" Padding="5">

            <Label ID="PanelTitle"
                   String="LOC_CUSTOM_PANEL_TITLE"
                   Style="HeaderText"/>

            <Container ID="YieldRow" Size="280,30">
                <Label ID="GoldLabel" Anchor="L" Size="140,30"/>
                <Label ID="ScienceLabel" Anchor="R" Size="140,30"/>
            </Container>

            <Container ID="UnitRow" Size="280,30">
                <Label ID="UnitCountLabel" Anchor="L" Size="140,30"/>
                <Label ID="CityCountLabel" Anchor="R" Size="140,30"/>
            </Container>

            <Button ID="DetailsButton"
                    String="LOC_VIEW_DETAILS"
                    Size="120,30"
                    Style="StandardButton"/>
        </Stack>

        <Button ID="CloseButton"
                Anchor="TR" Offset="-5,5"
                Size="24,24"
                Style="CloseButton"/>
    </Container>
</Context>
```

### JavaScript Controller (CustomInfoPanel.js)

```javascript
var CustomInfoPanel = {
    m_Container: null,
    m_GoldLabel: null,
    m_ScienceLabel: null,
    m_UnitCountLabel: null,
    m_CityCountLabel: null,

    Initialize: function() {
        // Get elements
        this.m_Container = document.getElementById("PanelContainer");
        this.m_GoldLabel = document.getElementById("GoldLabel");
        this.m_ScienceLabel = document.getElementById("ScienceLabel");
        this.m_UnitCountLabel = document.getElementById("UnitCountLabel");
        this.m_CityCountLabel = document.getElementById("CityCountLabel");

        var detailsButton = document.getElementById("DetailsButton");
        var closeButton = document.getElementById("CloseButton");

        // Event handlers
        detailsButton.addEventListener("click", this.OnDetailsClick.bind(this));
        closeButton.addEventListener("click", this.OnClose.bind(this));

        // Game events
        Events.TurnBegin.add(this.UpdatePanel.bind(this));
        Events.PlayerYieldChanged.add(this.UpdateYields.bind(this));

        // Initial update
        this.UpdatePanel();
    },

    UpdatePanel: function() {
        var localPlayer = Players.GetLocalPlayer();
        if (!localPlayer) return;

        this.UpdateYields();
        this.UpdateCounts();
    },

    UpdateYields: function() {
        var localPlayer = Players.GetLocalPlayer();
        if (!localPlayer) return;

        var treasury = localPlayer.GetTreasury();
        var gold = treasury.GetGoldPerTurn();

        var science = localPlayer.GetTechs().GetSciencePerTurn();

        this.m_GoldLabel.textContent = Locale.Compose(
            "LOC_GOLD_PER_TURN", gold
        );
        this.m_ScienceLabel.textContent = Locale.Compose(
            "LOC_SCIENCE_PER_TURN", science
        );
    },

    UpdateCounts: function() {
        var localPlayer = Players.GetLocalPlayer();
        if (!localPlayer) return;

        var units = localPlayer.GetUnits().GetCount();
        var cities = localPlayer.GetCities().GetCount();

        this.m_UnitCountLabel.textContent = Locale.Compose(
            "LOC_UNIT_COUNT", units
        );
        this.m_CityCountLabel.textContent = Locale.Compose(
            "LOC_CITY_COUNT", cities
        );
    },

    OnDetailsClick: function() {
        UI.PlaySound("Click");
        UI.OpenScreen("Reports");
    },

    OnClose: function() {
        UI.PlaySound("Click");
        this.m_Container.style.visibility = "hidden";
    },

    Show: function() {
        this.UpdatePanel();
        this.m_Container.style.visibility = "visible";
    },

    Toggle: function() {
        if (this.m_Container.style.visibility === "visible") {
            this.OnClose();
        } else {
            this.Show();
        }
    }
};

CustomInfoPanel.Initialize();

// Register keyboard shortcut
Input.RegisterKeyHandler("I", function() {
    CustomInfoPanel.Toggle();
    return true;
});
```

### Localization (CustomInfoPanel_Text.xml)

```xml
<LocalizedText>
    <Row Tag="LOC_CUSTOM_PANEL_TITLE" Language="en_US">
        <Text>Empire Overview</Text>
    </Row>
    <Row Tag="LOC_GOLD_PER_TURN" Language="en_US">
        <Text>[ICON_GOLD] {1} per turn</Text>
    </Row>
    <Row Tag="LOC_SCIENCE_PER_TURN" Language="en_US">
        <Text>[ICON_SCIENCE] {1} per turn</Text>
    </Row>
    <Row Tag="LOC_UNIT_COUNT" Language="en_US">
        <Text>Units: {1}</Text>
    </Row>
    <Row Tag="LOC_CITY_COUNT" Language="en_US">
        <Text>Cities: {1}</Text>
    </Row>
    <Row Tag="LOC_VIEW_DETAILS" Language="en_US">
        <Text>View Details</Text>
    </Row>
</LocalizedText>
```

## Best Practices

1. **Use Localization** - Never hardcode text strings
2. **Handle Null Checks** - Game objects may be null
3. **Unsubscribe Events** - Clean up event handlers when screen closes
4. **Match Game Style** - Use existing style classes for consistency
5. **Test All Resolutions** - UI should work at different screen sizes
6. **Use Anchoring** - Position elements relative to screen edges
7. **Provide Feedback** - Play sounds on user interactions
8. **Performance** - Don't update UI every frame unnecessarily

## Related Documentation

- [JavaScript API](../javascript/Overview.md)
- [Art Assets](Art-Assets.md)
- [Localization](Localization.md)
- [ModInfo Files](../Modinfo-Files.md)
