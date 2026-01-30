# Civilization VII Modding API Reference

Welcome to the comprehensive API reference for Civilization VII modding.

## What's Different in Civ VII?

### JavaScript Instead of Lua

Unlike Civilization VI which used Lua for scripting, **Civilization VII uses JavaScript** for:
- UI modifications
- Gameplay scripting
- Map generation scripts

### Age-Based Content System

Content in Civ VII is organized by Ages:
- **Antiquity Age** - Ancient civilizations (Rome, Egypt, Greece, Han, etc.)
- **Exploration Age** - Medieval/Renaissance (Spain, Ming, Mongolia, etc.)
- **Modern Age** - Industrial to future (America, Prussia, Meiji, etc.)

Mods must specify which Age(s) their content applies to using ActionCriteria in the .modinfo file.

### Two Database Scopes

- **Shell/Frontend** - Main menu and game setup screens
- **Game** - Active gameplay database

## Quick Start

1. **[Setting Up Your Mod](docs/Getting-Started.md)** - Create your first .modinfo file
2. **[modinfo Files Reference](docs/Modinfo-Files.md)** - Complete .modinfo documentation

## Tutorials

Step-by-step guides for common modding tasks:

| Tutorial | Description |
|----------|-------------|
| [Adding Policies](docs/tutorials/Adding-Policies.md) | Create new traditions with modifiers |
| [Creating a Unit](docs/tutorials/Creating-a-Unit.md) | Add custom units with combat abilities |
| [Creating a Civilization](docs/tutorials/Creating-a-Civilization.md) | Build complete civs with leaders and abilities |
| [Creating a Map Generator](docs/tutorials/Creating-a-Map-Generator.md) | Write JavaScript map scripts |
| [Creating UI Extensions](docs/tutorials/Creating-UI-Extensions.md) | Add custom panels and hotkeys |

## Core Documentation

### Database Reference

| Topic | Description |
|-------|-------------|
| [Types and Kinds](docs/database/Types-and-Kinds.md) | All 108 KIND values that power game entities |
| [Tables Reference](docs/database/Tables-Reference.md) | Complete reference for 673 database tables |
| [Units](docs/database/Units.md) | Unit definitions, stats, and abilities |
| [Buildings](docs/database/Buildings.md) | Buildings, improvements, and wonders |
| [Civilizations](docs/database/Civilizations.md) | Civilization definitions and traits |
| [Leaders](docs/database/Leaders.md) | Leader definitions and abilities |
| [Map & Terrain](docs/database/Map-Terrain.md) | Terrain types, biomes, features, natural wonders |

### Modifier System

The modifier system is how all game effects work in Civ VII.

| Topic | Description |
|-------|-------------|
| [Overview](docs/modifiers/Overview.md) | How modifiers work |
| [Effect Types](docs/modifiers/Effect-Types.md) | All 376+ available effect types |
| [Collection Types](docs/modifiers/Collection-Types.md) | All 40 target subject collections |
| [Requirement Types](docs/modifiers/Requirement-Types.md) | All 280+ conditional requirements |

### JavaScript API

Civ VII uses JavaScript for scripting instead of Lua.

| Topic | Description |
|-------|-------------|
| [Overview](docs/javascript/Overview.md) | Global objects, events, and API reference |
| [Events Reference](docs/javascript/Events.md) | All 26+ JavaScript events with parameters |

### Narrative Events

Pop-up events with choices and quests.

| Topic | Description |
|-------|-------------|
| [Overview](docs/narrative/Overview.md) | Event structure, triggers, and rewards |

## Comprehensive Reference

For quick lookups, see the **[Cross-Reference Index](docs/Index.md)** which contains:
- All 108 KIND values organized by category
- All 376+ Effect Types for modifiers
- All 40 Collection Types for targeting
- All 280+ Requirement Types for conditions
- All 673 database tables with descriptions
- Common modding tasks quick reference

## Examples

See the [Examples](examples/README.md) directory for quick reference templates and code snippets.

## Official Resources

The **Civilization VII Development Tools** (available on Steam) includes:
- Official documentation
- Example mods (`fxs-new-policies`, `fxs-new-narrative-events`, etc.)
- FireTuner debug console

Location: `Steam\steamapps\common\Sid Meier's Civilization VII Development Tools\`

## File Locations

| Content | Path |
|---------|------|
| Game Files | `Steam\steamapps\common\Sid Meier's Civilization VII\` |
| Dev Tools | `Steam\steamapps\common\Sid Meier's Civilization VII Development Tools\` |
| User Mods | `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Mods\` |
| Logs | `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Logs\` |

## Contributing

This documentation is community-maintained. Found an error or have additions? The source is available for contributions.
