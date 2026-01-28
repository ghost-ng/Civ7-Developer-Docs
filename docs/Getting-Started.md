# Getting Started with Civ VII Modding

This guide will help you set up your modding environment and create your first Civilization VII mod.

## Prerequisites

1. **Civilization VII** installed via Steam
2. **Civilization VII Development Tools** (available on Steam as free DLC)
3. **A Code Editor** - Visual Studio Code recommended

## Important File Locations

### Windows
| Location | Path |
|----------|------|
| Game Files | `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII\` |
| Dev Tools | `C:\Program Files (x86)\Steam\steamapps\common\Sid Meier's Civilization VII Development Tools\` |
| User Mods | `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Mods\` |
| Debug Logs | `%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Logs\` |

### macOS
| Location | Path |
|----------|------|
| User Mods | `~/Library/Application Support/Civilization VII/Mods/` |

### Linux/Steam Deck
| Location | Path |
|----------|------|
| User Mods | `~/My Games/Sid Meier's Civilization VII/Mods/` |

## Recommended Settings

Edit `AppOptions.txt` in your user data folder:

```
; Enable database export for debugging
CopyDatabasesToDisk 1

; Enable FireTuner debugging tool
EnableTuner 1

; Enable in-game debug panels (backtick key)
EnableDebugPanels 1

; Enable Chrome DevTools for UI debugging
UIDebugger 1

; Auto-reload UI files when edited
UIFileWatcher 1
```

## Game Module Structure

The game content is organized into modules:

```
Base/modules/
├── core/              # Core game systems
│   ├── data/          # Core data definitions
│   ├── scripts/       # JavaScript helpers
│   ├── ui/            # UI components
│   └── core.modinfo   # Module metadata
├── base-standard/     # Standard game rules
│   ├── data/          # Game data (units, buildings, etc.)
│   ├── text/          # Localization
│   └── base-standard.modinfo
├── age-antiquity/     # Antiquity Age content
├── age-exploration/   # Exploration Age content
└── age-modern/        # Modern Age content
```

## Creating Your First Mod

### Step 1: Create the Mod Folder

Create a new folder in your Mods directory:
```
%LOCALAPPDATA%\Firaxis Games\Sid Meier's Civilization VII\Mods\my-first-mod\
```

### Step 2: Create the .modinfo File

Create `my-first-mod.modinfo` in your mod folder:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Mod id="my-first-mod" version="1" xmlns="ModInfo">
    <Properties>
        <Name>My First Mod</Name>
        <Description>This is my first Civ VII mod.</Description>
        <Authors>Your Name</Authors>
        <AffectsSavedGames>1</AffectsSavedGames>
    </Properties>
    <Dependencies>
    </Dependencies>
    <ActionCriteria>
    </ActionCriteria>
    <ActionGroups>
    </ActionGroups>
</Mod>
```

### Step 3: Add Content

See the following guides for adding specific content:
- [Database Modding](Database-Modding.md) - Add units, buildings, civilizations
- [Modifier System](Modifier-System.md) - Add game effects
- [modinfo Files](Modinfo-Files.md) - Configure your mod

## Key Differences from Civ VI

| Aspect | Civilization VI | Civilization VII |
|--------|----------------|------------------|
| Scripting | Lua | JavaScript |
| UI | Lua + XML panels | HTML/CSS/JavaScript |
| Ages | Single era progression | Distinct Ages (Antiquity, Exploration, Modern) |
| Database Scopes | GameplayDatabase | Shell (frontend) + Game (gameplay) |
| Effects System | Modifiers | Modifiers (similar, with GameEffects XML) |

## Debugging Tips

1. **Check the Logs** - `Database.log` in the Logs folder shows errors
2. **Use FireTuner** - Connect to running game for live debugging
3. **Export Database** - With `CopyDatabasesToDisk 1`, view SQLite files in debug folder
4. **Validate XML** - XML syntax errors will prevent mod loading

## Next Steps

- [Database Modding](Database-Modding.md)
- [The Modifier System](Modifier-System.md)
- [modinfo Files](Modinfo-Files.md)
