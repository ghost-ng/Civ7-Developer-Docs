# Tutorial: Creating UI Extensions

This tutorial walks through creating custom UI elements for Civilization VII, including new panels, extending existing screens, adding hotkeys, and creating custom widgets.

## What You'll Learn

- Creating custom UI panels with BLP and JavaScript
- Subscribing to game events
- Adding keyboard shortcuts
- Extending existing game screens
- Creating reusable UI components

## Example: Empire Statistics Panel

We'll create a custom panel that shows empire statistics, toggleable with a hotkey.

## File Structure

```
my-stats-panel/
├── ui/
│   ├── EmpireStats.blp
│   ├── EmpireStats.js
│   └── EmpireStats.css
├── text/
│   └── text.xml
└── my-stats-panel.modinfo
```

## Step 1: Create the .modinfo File

**my-stats-panel.modinfo**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-stats-panel" version="1" xmlns="ModInfo">
    <Properties>
        <Name>Empire Statistics Panel</Name>
        <Description>Adds a panel showing empire statistics. Press F8 to toggle.</Description>
        <Authors>Your Name</Authors>
        <AffectsSavedGames>0</AffectsSavedGames>
    </Properties>
    <Dependencies>
    </Dependencies>
    <ActionCriteria>
        <Criteria id="always">
            <AlwaysMet></AlwaysMet>
        </Criteria>
    </ActionCriteria>
    <ActionGroups>
        <ActionGroup id="ui-scripts" scope="game" criteria="always">
            <Actions>
                <UIScripts>
                    <Item>ui/EmpireStats.js</Item>
                </UIScripts>
                <ImportFiles>
                    <Item>ui/EmpireStats.blp</Item>
                    <Item>ui/EmpireStats.css</Item>
                </ImportFiles>
                <UpdateText>
                    <Item>text/text.xml</Item>
                </UpdateText>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

## Step 2: Create the UI Definition (BLP)

**ui/EmpireStats.blp**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context Name="EmpireStatsPanel">
    <!-- Main Panel Container -->
    <Container ID="StatsPanel" Size="320,400" Anchor="TR" Offset="-20,80" Visible="false">
        <!-- Background -->
        <Grid ID="PanelBackground" Size="parent" Style="StandardPanelBackground"/>

        <!-- Header -->
        <Container ID="HeaderContainer" Size="320,40" Anchor="T">
            <Label ID="TitleLabel"
                   String="LOC_EMPIRE_STATS_TITLE"
                   Style="PanelHeaderText"
                   Anchor="CL"
                   Offset="15,0"/>

            <Button ID="CloseButton"
                    Size="30,30"
                    Anchor="CR"
                    Offset="-5,0"
                    Style="CloseButtonStyle"/>
        </Container>

        <!-- Content Area -->
        <ScrollPanel ID="ContentScroll" Size="300,340" Anchor="B" Offset="0,-10">
            <Stack ID="ContentStack" StackGrowth="Bottom" Padding="8">

                <!-- Yields Section -->
                <Container ID="YieldsSection" Size="280,120">
                    <Label ID="YieldsSectionLabel"
                           String="LOC_STATS_YIELDS_HEADER"
                           Style="SectionHeaderText"
                           Anchor="TL"/>

                    <Grid ID="YieldsGrid" Size="280,90" Anchor="B" Columns="2" Rows="3">
                        <Container ID="GoldRow" Size="140,30">
                            <Image ID="GoldIcon" Size="24,24" Icon="ICON_YIELD_GOLD" Anchor="L"/>
                            <Label ID="GoldValue" Anchor="R" Style="YieldValueText"/>
                        </Container>
                        <Container ID="ScienceRow" Size="140,30">
                            <Image ID="ScienceIcon" Size="24,24" Icon="ICON_YIELD_SCIENCE" Anchor="L"/>
                            <Label ID="ScienceValue" Anchor="R" Style="YieldValueText"/>
                        </Container>
                        <Container ID="CultureRow" Size="140,30">
                            <Image ID="CultureIcon" Size="24,24" Icon="ICON_YIELD_CULTURE" Anchor="L"/>
                            <Label ID="CultureValue" Anchor="R" Style="YieldValueText"/>
                        </Container>
                        <Container ID="FoodRow" Size="140,30">
                            <Image ID="FoodIcon" Size="24,24" Icon="ICON_YIELD_FOOD" Anchor="L"/>
                            <Label ID="FoodValue" Anchor="R" Style="YieldValueText"/>
                        </Container>
                        <Container ID="ProductionRow" Size="140,30">
                            <Image ID="ProductionIcon" Size="24,24" Icon="ICON_YIELD_PRODUCTION" Anchor="L"/>
                            <Label ID="ProductionValue" Anchor="R" Style="YieldValueText"/>
                        </Container>
                        <Container ID="HappinessRow" Size="140,30">
                            <Image ID="HappinessIcon" Size="24,24" Icon="ICON_YIELD_HAPPINESS" Anchor="L"/>
                            <Label ID="HappinessValue" Anchor="R" Style="YieldValueText"/>
                        </Container>
                    </Grid>
                </Container>

                <!-- Military Section -->
                <Container ID="MilitarySection" Size="280,80">
                    <Label ID="MilitarySectionLabel"
                           String="LOC_STATS_MILITARY_HEADER"
                           Style="SectionHeaderText"
                           Anchor="TL"/>

                    <Stack ID="MilitaryStack" Anchor="B" StackGrowth="Bottom" Padding="4">
                        <Container ID="UnitCountRow" Size="260,24">
                            <Label ID="UnitCountLabel" String="LOC_STATS_TOTAL_UNITS" Anchor="L"/>
                            <Label ID="UnitCountValue" Anchor="R" Style="StatValueText"/>
                        </Container>
                        <Container ID="MilitaryStrengthRow" Size="260,24">
                            <Label ID="MilitaryStrengthLabel" String="LOC_STATS_MILITARY_STRENGTH" Anchor="L"/>
                            <Label ID="MilitaryStrengthValue" Anchor="R" Style="StatValueText"/>
                        </Container>
                    </Stack>
                </Container>

                <!-- Cities Section -->
                <Container ID="CitiesSection" Size="280,80">
                    <Label ID="CitiesSectionLabel"
                           String="LOC_STATS_CITIES_HEADER"
                           Style="SectionHeaderText"
                           Anchor="TL"/>

                    <Stack ID="CitiesStack" Anchor="B" StackGrowth="Bottom" Padding="4">
                        <Container ID="CityCountRow" Size="260,24">
                            <Label ID="CityCountLabel" String="LOC_STATS_TOTAL_CITIES" Anchor="L"/>
                            <Label ID="CityCountValue" Anchor="R" Style="StatValueText"/>
                        </Container>
                        <Container ID="PopulationRow" Size="260,24">
                            <Label ID="PopulationLabel" String="LOC_STATS_TOTAL_POPULATION" Anchor="L"/>
                            <Label ID="PopulationValue" Anchor="R" Style="StatValueText"/>
                        </Container>
                    </Stack>
                </Container>

                <!-- Refresh Button -->
                <Button ID="RefreshButton"
                        String="LOC_STATS_REFRESH"
                        Size="120,35"
                        Style="StandardButton"/>
            </Stack>
        </ScrollPanel>
    </Container>
</Context>
```

### BLP Element Reference

| Element | Purpose |
|---------|---------|
| `Container` | Generic grouping element |
| `Stack` | Arranges children vertically/horizontally |
| `Grid` | Grid layout with rows/columns |
| `ScrollPanel` | Scrollable content area |
| `Label` | Text display |
| `Button` | Clickable button |
| `Image` | Image/icon display |

### Anchor Values

| Anchor | Position |
|--------|----------|
| `TL` | Top-Left |
| `TC` or `T` | Top-Center |
| `TR` | Top-Right |
| `CL` or `L` | Center-Left |
| `C` | Center |
| `CR` or `R` | Center-Right |
| `BL` | Bottom-Left |
| `BC` or `B` | Bottom-Center |
| `BR` | Bottom-Right |

## Step 3: Create the JavaScript Controller

**ui/EmpireStats.js**
```javascript
/**
 * Empire Statistics Panel
 * Shows various empire statistics in a toggleable panel
 */

// Panel controller object
var EmpireStatsPanel = {
    // Element references
    m_Panel: null,
    m_GoldValue: null,
    m_ScienceValue: null,
    m_CultureValue: null,
    m_FoodValue: null,
    m_ProductionValue: null,
    m_HappinessValue: null,
    m_UnitCountValue: null,
    m_MilitaryStrengthValue: null,
    m_CityCountValue: null,
    m_PopulationValue: null,

    // State
    m_IsVisible: false,

    /**
     * Initialize the panel
     */
    Initialize: function() {
        console.log("Initializing Empire Stats Panel...");

        // Get element references
        this.m_Panel = document.getElementById("StatsPanel");
        this.m_GoldValue = document.getElementById("GoldValue");
        this.m_ScienceValue = document.getElementById("ScienceValue");
        this.m_CultureValue = document.getElementById("CultureValue");
        this.m_FoodValue = document.getElementById("FoodValue");
        this.m_ProductionValue = document.getElementById("ProductionValue");
        this.m_HappinessValue = document.getElementById("HappinessValue");
        this.m_UnitCountValue = document.getElementById("UnitCountValue");
        this.m_MilitaryStrengthValue = document.getElementById("MilitaryStrengthValue");
        this.m_CityCountValue = document.getElementById("CityCountValue");
        this.m_PopulationValue = document.getElementById("PopulationValue");

        // Set up button handlers
        var closeButton = document.getElementById("CloseButton");
        var refreshButton = document.getElementById("RefreshButton");

        if (closeButton) {
            closeButton.addEventListener("click", this.OnClose.bind(this));
        }
        if (refreshButton) {
            refreshButton.addEventListener("click", this.RefreshData.bind(this));
        }

        // Subscribe to game events
        this.SubscribeToEvents();

        // Register hotkey (F8)
        this.RegisterHotkey();

        console.log("Empire Stats Panel initialized!");
    },

    /**
     * Subscribe to relevant game events
     */
    SubscribeToEvents: function() {
        // Update when turn begins
        if (typeof Events !== 'undefined') {
            Events.TurnBegin.add(this.OnTurnBegin.bind(this));
            Events.CityProductionCompleted.add(this.OnCityChanged.bind(this));
            Events.CityPopulationChanged.add(this.OnCityChanged.bind(this));
            Events.UnitCreated.add(this.OnUnitChanged.bind(this));
            Events.UnitKilled.add(this.OnUnitChanged.bind(this));
        }
    },

    /**
     * Register the F8 hotkey
     */
    RegisterHotkey: function() {
        // Listen for keyboard events
        window.addEventListener("keydown", function(event) {
            // F8 key
            if (event.key === "F8" || event.keyCode === 119) {
                EmpireStatsPanel.Toggle();
                event.preventDefault();
            }
        });
    },

    /**
     * Toggle panel visibility
     */
    Toggle: function() {
        if (this.m_IsVisible) {
            this.Hide();
        } else {
            this.Show();
        }
    },

    /**
     * Show the panel
     */
    Show: function() {
        if (!this.m_Panel) return;

        this.RefreshData();
        this.m_Panel.style.display = "block";
        this.m_IsVisible = true;

        // Play sound
        if (typeof UI !== 'undefined' && UI.PlaySound) {
            UI.PlaySound("UI_Panel_Open");
        }
    },

    /**
     * Hide the panel
     */
    Hide: function() {
        if (!this.m_Panel) return;

        this.m_Panel.style.display = "none";
        this.m_IsVisible = false;

        if (typeof UI !== 'undefined' && UI.PlaySound) {
            UI.PlaySound("UI_Panel_Close");
        }
    },

    /**
     * Close button handler
     */
    OnClose: function() {
        this.Hide();
    },

    /**
     * Refresh all statistics
     */
    RefreshData: function() {
        var localPlayer = this.GetLocalPlayer();
        if (!localPlayer) {
            console.warn("No local player found");
            return;
        }

        this.UpdateYields(localPlayer);
        this.UpdateMilitary(localPlayer);
        this.UpdateCities(localPlayer);
    },

    /**
     * Get the local player object
     */
    GetLocalPlayer: function() {
        if (typeof Players === 'undefined') return null;
        if (typeof GameContext === 'undefined') return null;

        return Players.get(GameContext.localObserverID);
    },

    /**
     * Update yield displays
     */
    UpdateYields: function(player) {
        // Get treasury for gold
        var treasury = player.Treasury;
        if (treasury && this.m_GoldValue) {
            var goldPerTurn = treasury.getGoldBalance ? treasury.getGoldBalance() : 0;
            this.m_GoldValue.textContent = this.FormatNumber(goldPerTurn);
        }

        // Get yields from cities
        var totalFood = 0;
        var totalProduction = 0;
        var totalScience = 0;
        var totalCulture = 0;
        var totalHappiness = 0;

        var cities = player.Cities;
        if (cities) {
            var cityList = cities.getCities ? cities.getCities() : [];
            for (var i = 0; i < cityList.length; i++) {
                var city = cityList[i];
                if (city && city.getYieldPerTurn) {
                    totalFood += city.getYieldPerTurn("YIELD_FOOD") || 0;
                    totalProduction += city.getYieldPerTurn("YIELD_PRODUCTION") || 0;
                    totalScience += city.getYieldPerTurn("YIELD_SCIENCE") || 0;
                    totalCulture += city.getYieldPerTurn("YIELD_CULTURE") || 0;
                    totalHappiness += city.getYieldPerTurn("YIELD_HAPPINESS") || 0;
                }
            }
        }

        if (this.m_FoodValue) this.m_FoodValue.textContent = this.FormatNumber(totalFood);
        if (this.m_ProductionValue) this.m_ProductionValue.textContent = this.FormatNumber(totalProduction);
        if (this.m_ScienceValue) this.m_ScienceValue.textContent = this.FormatNumber(totalScience);
        if (this.m_CultureValue) this.m_CultureValue.textContent = this.FormatNumber(totalCulture);
        if (this.m_HappinessValue) this.m_HappinessValue.textContent = this.FormatNumber(totalHappiness);
    },

    /**
     * Update military displays
     */
    UpdateMilitary: function(player) {
        var units = player.Units;
        var unitCount = 0;
        var militaryStrength = 0;

        if (units) {
            var unitIds = units.getUnitIds ? units.getUnitIds() : [];
            unitCount = unitIds.length;

            // Calculate total military strength
            for (var i = 0; i < unitIds.length; i++) {
                var unit = Units.get(unitIds[i]);
                if (unit && unit.getCombat) {
                    militaryStrength += unit.getCombat() || 0;
                }
            }
        }

        if (this.m_UnitCountValue) {
            this.m_UnitCountValue.textContent = unitCount.toString();
        }
        if (this.m_MilitaryStrengthValue) {
            this.m_MilitaryStrengthValue.textContent = this.FormatNumber(militaryStrength);
        }
    },

    /**
     * Update city displays
     */
    UpdateCities: function(player) {
        var cities = player.Cities;
        var cityCount = 0;
        var totalPopulation = 0;

        if (cities) {
            var cityList = cities.getCities ? cities.getCities() : [];
            cityCount = cityList.length;

            for (var i = 0; i < cityList.length; i++) {
                var city = cityList[i];
                if (city && city.population) {
                    totalPopulation += city.population;
                }
            }
        }

        if (this.m_CityCountValue) {
            this.m_CityCountValue.textContent = cityCount.toString();
        }
        if (this.m_PopulationValue) {
            this.m_PopulationValue.textContent = totalPopulation.toString();
        }
    },

    /**
     * Format numbers with commas
     */
    FormatNumber: function(num) {
        if (num === null || num === undefined) return "0";
        return Math.floor(num).toLocaleString();
    },

    // Event handlers
    OnTurnBegin: function(turn) {
        if (this.m_IsVisible) {
            this.RefreshData();
        }
    },

    OnCityChanged: function() {
        if (this.m_IsVisible) {
            this.RefreshData();
        }
    },

    OnUnitChanged: function() {
        if (this.m_IsVisible) {
            this.RefreshData();
        }
    }
};

// Initialize when DOM is ready
if (document.readyState === "loading") {
    document.addEventListener("DOMContentLoaded", function() {
        EmpireStatsPanel.Initialize();
    });
} else {
    EmpireStatsPanel.Initialize();
}
```

## Step 4: Create Styles (CSS)

**ui/EmpireStats.css**
```css
/* Panel background */
.StandardPanelBackground {
    background-color: rgba(20, 20, 30, 0.95);
    border: 2px solid #8b7355;
    border-radius: 4px;
}

/* Header text */
.PanelHeaderText {
    font-size: 18px;
    font-weight: bold;
    color: #ffd700;
    text-shadow: 1px 1px 2px black;
}

/* Section headers */
.SectionHeaderText {
    font-size: 14px;
    font-weight: bold;
    color: #c0c0c0;
    border-bottom: 1px solid #555;
    padding-bottom: 4px;
    margin-bottom: 8px;
}

/* Yield value text */
.YieldValueText {
    font-size: 14px;
    color: #ffffff;
    font-weight: bold;
}

/* Stat value text */
.StatValueText {
    font-size: 13px;
    color: #90ee90;
}

/* Close button */
.CloseButtonStyle {
    background-image: url('UI/Icons/CloseButton.dds');
    background-size: contain;
    border: none;
    cursor: pointer;
}

.CloseButtonStyle:hover {
    filter: brightness(1.2);
}

/* Standard button */
.StandardButton {
    background-color: #4a4a5a;
    border: 1px solid #8b7355;
    color: white;
    padding: 8px 16px;
    cursor: pointer;
    border-radius: 3px;
}

.StandardButton:hover {
    background-color: #5a5a6a;
}

.StandardButton:active {
    background-color: #3a3a4a;
}
```

## Step 5: Add Localization

**text/text.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <LocalizedText>
        <Row Tag="LOC_EMPIRE_STATS_TITLE" Language="en_US">
            <Text>Empire Statistics</Text>
        </Row>
        <Row Tag="LOC_STATS_YIELDS_HEADER" Language="en_US">
            <Text>Yields Per Turn</Text>
        </Row>
        <Row Tag="LOC_STATS_MILITARY_HEADER" Language="en_US">
            <Text>Military</Text>
        </Row>
        <Row Tag="LOC_STATS_CITIES_HEADER" Language="en_US">
            <Text>Cities</Text>
        </Row>
        <Row Tag="LOC_STATS_TOTAL_UNITS" Language="en_US">
            <Text>Total Units:</Text>
        </Row>
        <Row Tag="LOC_STATS_MILITARY_STRENGTH" Language="en_US">
            <Text>Military Strength:</Text>
        </Row>
        <Row Tag="LOC_STATS_TOTAL_CITIES" Language="en_US">
            <Text>Cities:</Text>
        </Row>
        <Row Tag="LOC_STATS_TOTAL_POPULATION" Language="en_US">
            <Text>Total Population:</Text>
        </Row>
        <Row Tag="LOC_STATS_REFRESH" Language="en_US">
            <Text>Refresh</Text>
        </Row>
    </LocalizedText>
</Database>
```

## Step 6: Install and Test

1. Copy mod folder to Mods directory
2. Enable the mod
3. Start a game
4. Press **F8** to toggle the panel
5. Verify statistics update correctly

## Advanced: Extending Existing UI

### Hooking Into Base Screens

```javascript
// Extend an existing screen
(function() {
    // Store original function
    var originalFunc = SomeExistingPanel.Initialize;

    // Replace with extended version
    SomeExistingPanel.Initialize = function() {
        // Call original
        originalFunc.call(this);

        // Add your customizations
        this.AddCustomFeature();
    };

    SomeExistingPanel.AddCustomFeature = function() {
        // Your custom code here
    };
})();
```

### Adding Context Menu Options

```javascript
// Add option to city context menu
ContextMenu.AddCityAction({
    ID: "MY_CUSTOM_ACTION",
    Name: "LOC_MY_ACTION_NAME",
    Tooltip: "LOC_MY_ACTION_TOOLTIP",
    Icon: "ICON_MY_ACTION",
    Condition: function(city) {
        // Show only for cities with > 5 population
        return city.population > 5;
    },
    Execute: function(city) {
        // Do something with the city
        console.log("Action on city: " + city.name);
    }
});
```

### Adding Notifications

```javascript
// Show a notification
function ShowCustomNotification(message) {
    if (typeof NotificationManager !== 'undefined') {
        NotificationManager.addNotification({
            type: "NOTIFICATION_GENERIC",
            message: message,
            icon: "ICON_NOTIFICATION_GENERIC"
        });
    }
}
```

## Common UI Patterns

### Modal Dialog

```javascript
function ShowConfirmDialog(title, message, onConfirm) {
    // Create overlay
    var overlay = document.createElement("div");
    overlay.className = "ModalOverlay";

    var dialog = document.createElement("div");
    dialog.className = "ModalDialog";
    dialog.innerHTML = `
        <h2>${title}</h2>
        <p>${message}</p>
        <button id="confirmBtn">Confirm</button>
        <button id="cancelBtn">Cancel</button>
    `;

    overlay.appendChild(dialog);
    document.body.appendChild(overlay);

    document.getElementById("confirmBtn").onclick = function() {
        document.body.removeChild(overlay);
        if (onConfirm) onConfirm();
    };

    document.getElementById("cancelBtn").onclick = function() {
        document.body.removeChild(overlay);
    };
}
```

### Dynamic List

```javascript
function PopulateList(listElement, items, itemTemplate) {
    listElement.innerHTML = "";

    for (var i = 0; i < items.length; i++) {
        var item = items[i];
        var element = document.createElement("div");
        element.className = "ListItem";
        element.innerHTML = itemTemplate(item);
        element.onclick = (function(itemData) {
            return function() {
                OnListItemClick(itemData);
            };
        })(item);
        listElement.appendChild(element);
    }
}
```

## Debugging UI

1. **Enable UIDebugger** in AppOptions.txt:
   ```
   UIDebugger 1
   ```

2. **Enable UIFileWatcher** for live reloading:
   ```
   UIFileWatcher 1
   ```

3. **Use console.log** for debugging
4. **Check browser dev tools** (F12) if UIDebugger is enabled

## Next Steps

- [UI Modding Reference](../technical-reference/UI-Modding.md)
- [JavaScript API Overview](../javascript/Overview.md)
- [Localization Guide](../technical-reference/Localization.md)
