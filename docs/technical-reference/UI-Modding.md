# UI Modding

This document covers UI modding in Civilization VII, including the HTML/JavaScript architecture, custom components, and how to extend the game's interface.

## Overview

Civ VII's UI is built on **Coherent Labs Gameface** (Cohtml), which renders standard web technologies:

- **HTML5** - UI structure and layout
- **CSS** - Styling and animations
- **JavaScript** - Logic and event handling
- **Web Components** - Custom reusable UI elements

> **Note:** Unlike some earlier documentation suggested, Civ VII does NOT use "BLP" files. The UI is standard HTML/CSS/JavaScript.

## UI Architecture

```
UI Component
├── HTML (.html) - Structure and layout
├── JavaScript (.js) - Logic and behavior
├── CSS (.css) - Visual styling
└── Assets (.dds) - Graphics and icons
```

### Key Files

| File | Purpose |
|------|---------|
| `root-shell.html` | Main menu/shell UI root |
| `root-game.html` | In-game UI root |
| `cohtml.js` | Coherent Labs engine interface |

## HTML Structure

Civ VII UI uses standard HTML5 with custom web components:

### Example: root-shell.html

```html
<!doctype html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Civilization VII</title>
    <script src="fs://game/core/ui/cohtml.js"></script>
    <script type="module" src="fs://game/core/ui/shell/root-shell.js"></script>
    <link rel="stylesheet" href="fs://game/core/ui/shell/root-shell.css">
    <link rel="stylesheet" href="fs://game/core/ui/themes/default/default.css">
</head>
<body class="font-body text-base text-accent-2">
    <div id="roots" class="fullscreen"></div>
    <div class="fxs-popups fullscreen"></div>
    <nav-tray></nav-tray>
    <div id="tooltips" class="absolute pointer-events-none"></div>
</body>
</html>
```

### Custom Web Components

The game uses custom HTML elements (web components) for UI panels:

```html
<!-- Navigation tray -->
<nav-tray></nav-tray>

<!-- Yield banner -->
<panel-yield-banner></panel-yield-banner>

<!-- City banner manager -->
<city-banner-manager></city-banner-manager>

<!-- Slots for UI placement -->
<fxs-slot name="top-center" class="top center">
    <panel-yield-banner></panel-yield-banner>
</fxs-slot>
```

## JavaScript UI Controllers

### Basic Component Pattern

```javascript
// my-panel.js
class MyPanel extends HTMLElement {
    constructor() {
        super();
        this.handles = [];
    }

    connectedCallback() {
        // Called when element is added to DOM
        this.render();
        this.setupEventListeners();
    }

    disconnectedCallback() {
        // Called when element is removed - clean up
        this.handles.forEach(h => h.clear());
        this.handles = [];
    }

    render() {
        this.innerHTML = `
            <div class="panel-container">
                <h2 class="panel-title">${Locale.Lookup("LOC_MY_PANEL_TITLE")}</h2>
                <div class="panel-content" id="content"></div>
                <button class="btn-close" id="close-btn">X</button>
            </div>
        `;
    }

    setupEventListeners() {
        // DOM events
        this.querySelector("#close-btn").addEventListener("click", () => {
            this.hide();
        });

        // Game events via engine.on()
        this.handles.push(
            engine.on("TurnBegin", this.onTurnBegin.bind(this))
        );
        this.handles.push(
            engine.on("CitySelectionChanged", this.onCitySelected.bind(this))
        );
    }

    onTurnBegin(turn) {
        this.querySelector("#content").textContent = `Turn: ${turn}`;
    }

    onCitySelected(playerID, cityID) {
        if (cityID !== -1) {
            const city = Cities.get(cityID);
            this.querySelector("#content").textContent = city.name;
        }
    }

    show() {
        this.style.display = "block";
    }

    hide() {
        this.style.display = "none";
    }
}

// Register the custom element
customElements.define("my-panel", MyPanel);
```

### Registering Components

```javascript
// Use Controls.define() for Firaxis-style components
Controls.define("my-panel", {
    createInstance: (container) => {
        return new MyPanel();
    }
});
```

## Event Handling

### Engine Events (Game Events)

All gameplay events use `engine.on()`:

```javascript
// Subscribe to game events
engine.on("TurnBegin", (turn) => {
    console.log("Turn " + turn + " started");
});

engine.on("CitySelectionChanged", (playerID, cityID) => {
    // Handle city selection
});

engine.on("UnitMoved", (playerID, unitID) => {
    // Handle unit movement
});

// Unsubscribe
const handle = engine.on("TurnBegin", callback);
handle.clear(); // Later, to unsubscribe
```

### DOM Events

Standard DOM events for UI interaction:

```javascript
element.addEventListener("click", (event) => {
    // Handle click
});

element.addEventListener("keydown", (event) => {
    if (event.key === "Escape") {
        this.hide();
    }
});
```

## Styling

### CSS Structure

```css
/* my-panel.css */
.panel-container {
    background-color: rgba(0, 0, 0, 0.8);
    border: 2px solid var(--color-gold);
    padding: 1rem;
    border-radius: 4px;
}

.panel-title {
    font-size: 1.5rem;
    color: var(--color-accent);
    margin-bottom: 1rem;
}

.btn-close {
    position: absolute;
    top: 0.5rem;
    right: 0.5rem;
    background: none;
    border: none;
    color: white;
    cursor: pointer;
}

.btn-close:hover {
    color: var(--color-gold);
}
```

### Theme Variables

The game uses CSS custom properties for theming:

```css
:root {
    --color-gold: #c9a227;
    --color-accent: #ffffff;
    --color-background: rgba(20, 20, 20, 0.9);
}
```

## ModInfo Registration

### Loading UI Scripts

```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-ui-mod" version="1" xmlns="ModInfo">
    <Properties>
        <Name>My UI Mod</Name>
        <Description>Adds custom UI panels.</Description>
    </Properties>
    <ActionCriteria>
        <Criteria id="always"><AlwaysMet/></Criteria>
    </ActionCriteria>
    <ActionGroups>
        <ActionGroup id="game-ui" scope="game" criteria="always">
            <Actions>
                <UIScripts>
                    <Item>ui/my-panel.js</Item>
                    <Item>ui/my-panel.css</Item>
                </UIScripts>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

## Common UI Functions

### Localization

```javascript
// Get localized string
const text = Locale.Lookup("LOC_MY_TEXT_KEY");

// Get localized string with parameters
const formatted = Locale.Compose("LOC_MY_FORMAT", value1, value2);
```

### Game Data Access

```javascript
// Get local player
const localPlayer = Players.get(GameContext.localPlayerID);

// Get cities
const city = Cities.get(cityID);

// Get units
const unit = Units.get(unitID);

// Game info database
const unitInfo = GameInfo.Units.lookup("UNIT_WARRIOR");
```

### Playing Sounds

```javascript
// Play UI sound
engine.call("PlaySound", "UI_Click");
```

## Complete Example: Custom Info Panel

### File Structure

```
my-info-panel/
├── ui/
│   ├── info-panel.js
│   └── info-panel.css
├── text/
│   └── en_us/
│       └── info-panel-text.xml
└── my-info-panel.modinfo
```

### info-panel.js

```javascript
class InfoPanel extends HTMLElement {
    constructor() {
        super();
        this.handles = [];
        this.isVisible = false;
    }

    connectedCallback() {
        this.render();
        this.setupEvents();
    }

    disconnectedCallback() {
        this.handles.forEach(h => h.clear());
    }

    render() {
        this.innerHTML = `
            <div class="info-panel" id="panel">
                <div class="info-panel__header">
                    <h2>${Locale.Lookup("LOC_INFO_PANEL_TITLE")}</h2>
                    <button class="info-panel__close" id="close">X</button>
                </div>
                <div class="info-panel__content">
                    <div class="info-row">
                        <span class="info-label">${Locale.Lookup("LOC_GOLD_LABEL")}</span>
                        <span class="info-value" id="gold-value">0</span>
                    </div>
                    <div class="info-row">
                        <span class="info-label">${Locale.Lookup("LOC_SCIENCE_LABEL")}</span>
                        <span class="info-value" id="science-value">0</span>
                    </div>
                    <div class="info-row">
                        <span class="info-label">${Locale.Lookup("LOC_UNITS_LABEL")}</span>
                        <span class="info-value" id="units-value">0</span>
                    </div>
                </div>
            </div>
        `;
    }

    setupEvents() {
        // Close button
        this.querySelector("#close").addEventListener("click", () => {
            this.hide();
        });

        // Keyboard shortcut
        window.addEventListener("keydown", (e) => {
            if (e.key === "F8") {
                this.toggle();
            }
        });

        // Game events
        this.handles.push(
            engine.on("TurnBegin", () => this.update())
        );
        this.handles.push(
            engine.on("PlayerYieldChanged", () => this.update())
        );
    }

    update() {
        if (!this.isVisible) return;

        const player = Players.get(GameContext.localPlayerID);
        if (!player) return;

        // Update values
        this.querySelector("#gold-value").textContent =
            player.Treasury?.GetGoldPerTurn() || 0;
        this.querySelector("#science-value").textContent =
            player.Techs?.GetSciencePerTurn() || 0;
        this.querySelector("#units-value").textContent =
            player.Units?.getUnitIds().length || 0;
    }

    show() {
        this.querySelector("#panel").style.display = "block";
        this.isVisible = true;
        this.update();
    }

    hide() {
        this.querySelector("#panel").style.display = "none";
        this.isVisible = false;
    }

    toggle() {
        if (this.isVisible) {
            this.hide();
        } else {
            this.show();
        }
    }
}

customElements.define("info-panel", InfoPanel);

// Add to document when ready
document.addEventListener("DOMContentLoaded", () => {
    const panel = document.createElement("info-panel");
    document.body.appendChild(panel);
});
```

### info-panel.css

```css
.info-panel {
    position: fixed;
    right: 20px;
    bottom: 100px;
    width: 280px;
    background: rgba(20, 20, 30, 0.95);
    border: 2px solid #c9a227;
    border-radius: 8px;
    padding: 16px;
    display: none;
    z-index: 1000;
}

.info-panel__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 16px;
    border-bottom: 1px solid #555;
    padding-bottom: 8px;
}

.info-panel__header h2 {
    margin: 0;
    font-size: 18px;
    color: #c9a227;
}

.info-panel__close {
    background: none;
    border: none;
    color: #888;
    font-size: 18px;
    cursor: pointer;
}

.info-panel__close:hover {
    color: #fff;
}

.info-row {
    display: flex;
    justify-content: space-between;
    padding: 8px 0;
    border-bottom: 1px solid #333;
}

.info-label {
    color: #aaa;
}

.info-value {
    color: #fff;
    font-weight: bold;
}
```

### info-panel-text.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <EnglishText>
        <Row Tag="LOC_INFO_PANEL_TITLE">
            <Text>Empire Overview</Text>
        </Row>
        <Row Tag="LOC_GOLD_LABEL">
            <Text>Gold per Turn</Text>
        </Row>
        <Row Tag="LOC_SCIENCE_LABEL">
            <Text>Science per Turn</Text>
        </Row>
        <Row Tag="LOC_UNITS_LABEL">
            <Text>Total Units</Text>
        </Row>
    </EnglishText>
</Database>
```

### my-info-panel.modinfo

```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-info-panel" version="1" xmlns="ModInfo">
    <Properties>
        <Name>Info Panel</Name>
        <Description>Adds an empire overview panel (F8 to toggle).</Description>
        <Authors>Your Name</Authors>
    </Properties>
    <ActionCriteria>
        <Criteria id="always"><AlwaysMet/></Criteria>
    </ActionCriteria>
    <ActionGroups>
        <ActionGroup id="game" scope="game" criteria="always">
            <Actions>
                <UIScripts>
                    <Item>ui/info-panel.js</Item>
                    <Item>ui/info-panel.css</Item>
                </UIScripts>
                <UpdateText>
                    <Item>text/en_us/info-panel-text.xml</Item>
                </UpdateText>
            </Actions>
        </ActionGroup>
    </ActionGroups>
</Mod>
```

## Best Practices

1. **Use Localization** - Never hardcode text strings
2. **Handle Null Checks** - Game objects may be null
3. **Clean Up Events** - Use `handle.clear()` when components are destroyed
4. **Use Web Components** - Encapsulate functionality in custom elements
5. **Test All Resolutions** - UI should work at different screen sizes
6. **Use CSS Variables** - Match the game's theme system
7. **Provide Feedback** - Play sounds on user interactions
8. **Performance** - Don't update UI every frame unnecessarily

## Related Documentation

- [JavaScript Events](../javascript/Events.md) - Event subscription reference
- [Coherent Labs Engine](../javascript/Coherent-Labs-Engine.md) - Engine API details
- [Localization](Localization.md) - Text and translation
- [ModInfo Files](../Modinfo-Files.md) - Mod configuration
