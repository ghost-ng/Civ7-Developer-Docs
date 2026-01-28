# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a **community-maintained documentation wiki** for Civilization VII modding APIs. It documents undocumented database schemas, modifier systems, JavaScript APIs, and modding patterns. The content is written in Markdown for GitHub wiki compatibility.

## Key Civ VII Modding Concepts

### Critical Differences from Civ VI
- **JavaScript instead of Lua** for all scripting (UI, gameplay, map generation)
- **HTML/CSS/JavaScript** for UI instead of Lua+XML
- **Age-based content system**: Antiquity, Exploration, Modern - mods specify which ages via ActionCriteria in .modinfo
- **Two database scopes**: Shell (frontend/menus) and Game (active gameplay)

### The Modifier System
The modifier system is central to all game effects. Core structure:
- **Owner**: What the modifier is attached to
- **Subjects**: What it affects (determined by Collection type)
- **Effect**: What it does
- **Requirements**: Conditions for activation (OwnerRequirements and SubjectRequirements)

Two definition methods:
1. Database tables (SQL/XML) - multiple tables: Types, DynamicModifiers, Modifiers, ModifierArguments
2. GameEffects XML (recommended) - simplified `<GameEffects xmlns="GameEffects">` format that auto-generates tables

### Game Module Structure
```
Base/modules/
├── core/           - Core game systems, fonts, base UI
├── base-standard/  - Standard game rules and content
├── age-antiquity/  - Antiquity Age civilizations, units, buildings
├── age-exploration/- Exploration Age content
└── age-modern/     - Modern Age content
```

## Documentation Structure

This repo functions as a **GitHub wiki**. Key files:

- `Home.md` - Wiki home page (entry point)
- `_Sidebar.md` - Navigation sidebar for all pages
- `README.md` - Repository readme
- `docs/` - All detailed documentation
  - `database/` - Entity schemas (Units, Buildings, Civilizations, etc.)
  - `modifiers/` - Effect system (Overview, Effect-Types, Collection-Types, Requirement-Types)
  - `javascript/` - JavaScript API reference
  - `narrative/` - Event system
  - `civilization-creation/` - Civ/Leader creation guides
  - `technology-civics/` - Tech/Civic tree systems
  - `technical-reference/` - Localization, Art, UI, Load Order
  - `tutorials/` - Step-by-step modding guides
  - `Index.md` - Cross-reference lookup for all tables, types, effects
- `examples/` - Code snippets and templates
- `progress.md` - Documentation progress tracker

## Tutorials (Practical Guides)

Complete step-by-step tutorials with working code examples:

| Tutorial | Creates |
|----------|---------|
| `tutorials/Adding-Policies.md` | New traditions with modifiers |
| `tutorials/Creating-a-Unit.md` | Custom units with abilities |
| `tutorials/Creating-a-Civilization.md` | Full civ with leader, traits, abilities |
| `tutorials/Creating-a-Map-Generator.md` | JavaScript map generation script |
| `tutorials/Creating-UI-Extensions.md` | Custom UI panels with hotkeys |

## Game File Locations (Windows)

| Location | Path |
|----------|------|
| Game Files | `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\` |
| Dev Tools | `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII Development Tools\` |
| User Mods | `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Mods\` |
| Debug Logs | `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Logs\` |

## Writing Documentation

- Use GitHub-flavored Markdown
- Internal links use relative paths: `[Types](database/Types-and-Kinds.md)`
- Include working XML/SQL examples when documenting database tables
- Cross-reference related documentation in "See Also" sections
- When documenting modifier-related content, show both database table and GameEffects XML approaches
