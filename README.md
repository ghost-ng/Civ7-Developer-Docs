# Civilization VII Modding API Reference

A comprehensive, community-maintained API reference for Civilization VII modders.

## About This Project

This documentation aims to provide modders with a complete reference for the undocumented APIs, database schemas, and systems available in Civilization VII. While Firaxis provides official documentation for basic modding concepts, this project goes deeper into the specific tables, fields, effects, and scripting APIs.

## Key Differences from Civilization VI

**Important**: Civilization VII has a significantly different modding architecture than Civilization VI:

| Aspect | Civ VI | Civ VII |
|--------|--------|---------|
| Scripting Language | Lua | JavaScript |
| UI Framework | Lua + XML | HTML/CSS/JavaScript |
| Database | SQLite | SQLite |
| Data Format | XML/SQL | XML/SQL |

## Documentation Structure

- **[Home](Home.md)** - Overview and getting started
- **[docs/](docs/)** - Detailed API documentation
  - Database Schemas
  - JavaScript APIs
  - Modifier System
  - UI System
- **[examples/](examples/)** - Working code examples

## Quick Links

- [Official Firaxis Documentation](https://github.com/civfanatics/civ7-modding) (external)
- [CivFanatics Civ7 Modding Forum](https://forums.civfanatics.com/) (external)

## Game File Locations

### Windows
- **Game Installation**: `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\`
- **Development Tools**: `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII Development Tools\`
- **User Data/Mods**: `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\`

### macOS
- **User Data/Mods**: `~/Library/Application Support/Civilization VII/`

### Linux/Steam Deck
- **User Data/Mods**: `~/My Games/Sid Meier's Civilization VII/`

## Module Structure

The game content is organized into modules:

```
Base/modules/
    core/           - Core game systems, fonts, base UI
    base-standard/  - Standard game rules and content
    age-antiquity/  - Antiquity Age civilizations, units, buildings
    age-exploration/- Exploration Age content
    age-modern/     - Modern Age content
```

## Contributing

This is a community documentation project. Contributions are welcome via pull requests.

## License

This documentation is provided for educational purposes. Civilization VII is a trademark of Firaxis Games and 2K Games.
