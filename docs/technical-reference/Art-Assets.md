# Art Asset Requirements

This document covers art asset requirements and formats for Civilization VII modding, including icons, 3D models, textures, and UI elements.

## Overview

Civ VII uses various art asset types:

- **Icons** - 2D images for UI elements (units, buildings, techs, etc.)
- **Portraits** - Leader and civilization imagery
- **3D Models** - Units, buildings, improvements on the map
- **Textures** - Surface details for 3D models
- **UI Elements** - Interface components and backgrounds
- **Animations** - Unit and leader animations

## File Formats

### Supported Formats

| Asset Type | Format | Notes |
|------------|--------|-------|
| Icons | `.dds`, `.png` | DDS preferred for performance |
| Textures | `.dds` | Compressed texture format |
| 3D Models | `.gr2` | Granny 3D format |
| Animations | `.gr2` | Included in model files |
| Audio | `.wem` | Wwise audio format |
| Video | `.bk2` | Bink video format |

### DDS Format Details

DDS (DirectDraw Surface) is the primary texture format:

| Compression | Usage |
|-------------|-------|
| `DXT1` / `BC1` | RGB without alpha (smallest) |
| `DXT5` / `BC3` | RGBA with alpha (icons) |
| `BC7` | High quality RGBA |
| `Uncompressed` | Source editing only |

## Icon Assets

### Icon Categories

| Category | Database Column | Typical Size |
|----------|-----------------|--------------|
| Unit Icons | `IconAtlas` | 256x256 |
| Building Icons | `IconAtlas` | 256x256 |
| Technology Icons | `IconAtlas` | 128x128 |
| Civic Icons | `IconAtlas` | 128x128 |
| Resource Icons | `IconAtlas` | 64x64 |
| Leader Portraits | `Portrait` | 512x512 |
| Civ Icons | `IconAtlas` | 256x256 |

### Icon Atlas System

Icons are organized into atlases (sprite sheets):

```xml
<IconTextureAtlases>
    <Row Name="ICON_ATLAS_MY_MOD"
         IconsPerRow="4"
         IconsPerColumn="4"
         IconSize="256"
         Filename="MyMod_Icons.dds"/>
</IconTextureAtlases>
```

### IconTextureAtlases Columns

| Column | Type | Description |
|--------|------|-------------|
| `Name` | TEXT | Atlas identifier |
| `IconsPerRow` | INTEGER | Icons horizontally |
| `IconsPerColumn` | INTEGER | Icons vertically |
| `IconSize` | INTEGER | Size of each icon in pixels |
| `Filename` | TEXT | Path to DDS file |

### Mapping Icons to Content

```xml
<IconDefinitions>
    <Row Name="ICON_UNIT_MY_WARRIOR"
         Atlas="ICON_ATLAS_MY_MOD"
         Index="0"/>
    <Row Name="ICON_BUILDING_MY_TEMPLE"
         Atlas="ICON_ATLAS_MY_MOD"
         Index="1"/>
    <Row Name="ICON_TECH_MY_TECH"
         Atlas="ICON_ATLAS_MY_MOD"
         Index="2"/>
</IconDefinitions>
```

### IconDefinitions Columns

| Column | Type | Description |
|--------|------|-------------|
| `Name` | TEXT | Icon identifier (ICON_*) |
| `Atlas` | TEXT | Reference to IconTextureAtlas |
| `Index` | INTEGER | Position in atlas (0-based, left-to-right, top-to-bottom) |

### Linking Icons to Database Entries

```xml
<!-- Unit with custom icon -->
<Units>
    <Row UnitType="UNIT_MY_WARRIOR"
         Name="LOC_UNIT_MY_WARRIOR_NAME"
         Icon="ICON_UNIT_MY_WARRIOR"/>
</Units>

<!-- Building with custom icon -->
<Buildings>
    <Row BuildingType="BUILDING_MY_TEMPLE"
         Name="LOC_BUILDING_MY_TEMPLE_NAME"
         Icon="ICON_BUILDING_MY_TEMPLE"/>
</Buildings>

<!-- Technology with custom icon -->
<Technologies>
    <Row TechnologyType="TECH_MY_TECH"
         Name="LOC_TECH_MY_TECH_NAME"
         Icon="ICON_TECH_MY_TECH"/>
</Technologies>
```

## Creating Icon Atlases

### Step 1: Create Individual Icons

Create square icons at the required size:
- Use PNG format for editing
- Maintain transparency for backgrounds
- Follow game's art style

### Step 2: Arrange into Atlas

Combine icons into a single image:
```
+-------+-------+-------+-------+
| Icon0 | Icon1 | Icon2 | Icon3 |  <- Row 0
+-------+-------+-------+-------+
| Icon4 | Icon5 | Icon6 | Icon7 |  <- Row 1
+-------+-------+-------+-------+
```

### Step 3: Convert to DDS

Use tools like:
- **NVIDIA Texture Tools** - Command line or GUI
- **Intel Texture Works** - Photoshop plugin
- **GIMP with DDS Plugin** - Free option
- **Texconv** - Microsoft command-line tool

Example conversion:
```bash
texconv -f BC3_UNORM -o output/ MyMod_Icons.png
```

### Step 4: Register in Database

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <IconTextureAtlases>
        <Row Name="ICON_ATLAS_MY_MOD"
             IconsPerRow="4"
             IconsPerColumn="2"
             IconSize="256"
             Filename="Art/Icons/MyMod_Icons.dds"/>
    </IconTextureAtlases>

    <IconDefinitions>
        <Row Name="ICON_UNIT_MY_UNIT_1" Atlas="ICON_ATLAS_MY_MOD" Index="0"/>
        <Row Name="ICON_UNIT_MY_UNIT_2" Atlas="ICON_ATLAS_MY_MOD" Index="1"/>
        <Row Name="ICON_BUILDING_MY_BUILDING_1" Atlas="ICON_ATLAS_MY_MOD" Index="2"/>
        <Row Name="ICON_BUILDING_MY_BUILDING_2" Atlas="ICON_ATLAS_MY_MOD" Index="3"/>
        <Row Name="ICON_TECH_MY_TECH_1" Atlas="ICON_ATLAS_MY_MOD" Index="4"/>
        <Row Name="ICON_TECH_MY_TECH_2" Atlas="ICON_ATLAS_MY_MOD" Index="5"/>
    </IconDefinitions>
</Database>
```

## Leader Portraits

### Portrait Requirements

| Type | Size | Format |
|------|------|--------|
| Full Portrait | 512x512 | DDS (BC3) |
| Background | 1024x512 | DDS (BC1) |
| Scene | 2048x1024 | DDS (BC3) |

### Portrait Database Entry

```xml
<LeaderPortraits>
    <Row LeaderType="LEADER_MY_LEADER"
         Portrait="Art/Leaders/MyLeader_Portrait.dds"
         Background="Art/Leaders/MyLeader_Background.dds"/>
</LeaderPortraits>
```

## Civilization Icons

### Civ Icon Requirements

| Asset | Size | Usage |
|-------|------|-------|
| Main Icon | 256x256 | Selection screen, diplomacy |
| Small Icon | 64x64 | Unit flags, UI elements |
| Background | 512x256 | Leader scene background |

### Civilization Art Definition

```xml
<CivilizationArtDefs>
    <Row CivilizationType="CIVILIZATION_MY_CIV"
         IconAtlas="ICON_ATLAS_MY_CIV"
         IconIndex="0"
         BackgroundImage="Art/Civs/MyCiv_Background.dds"
         ColorPrimary="COLOR_MY_CIV_PRIMARY"
         ColorSecondary="COLOR_MY_CIV_SECONDARY"/>
</CivilizationArtDefs>

<Colors>
    <Row ColorType="COLOR_MY_CIV_PRIMARY">
        <Red>0.8</Red><Green>0.2</Green><Blue>0.2</Blue><Alpha>1</Alpha>
    </Row>
    <Row ColorType="COLOR_MY_CIV_SECONDARY">
        <Red>1.0</Red><Green>0.8</Green><Blue>0.2</Blue><Alpha>1</Alpha>
    </Row>
</Colors>
```

## 3D Model Assets

### Model Types

| Type | Usage |
|------|-------|
| Unit Models | In-game unit representation |
| Building Models | City buildings and wonders |
| Improvement Models | Tile improvements |
| Resource Models | Resource icons on map |
| Terrain Features | Trees, rocks, etc. |

### Model File Structure

Civ VII uses Granny 3D (.gr2) format:
- Mesh geometry
- Skeleton/bones
- Animations
- Materials references

### Referencing Existing Models

For mods without custom 3D art, reference existing models:

```xml
<ArtDefines>
    <Row ArtDefineType="ART_DEF_UNIT_MY_WARRIOR"
         BaseArtDefine="ART_DEF_UNIT_SWORDSMAN"/>
</ArtDefines>

<Units>
    <Row UnitType="UNIT_MY_WARRIOR"
         ArtDefine="ART_DEF_UNIT_MY_WARRIOR"/>
</Units>
```

### Unit Art Definitions

```xml
<UnitArtDefines>
    <Row UnitArtDefineType="ART_DEF_UNIT_MY_WARRIOR"
         Model="Units/MyWarrior/MyWarrior.gr2"
         Scale="1.0"
         Formation="FORMATION_CLASS_LAND_COMBAT"/>
</UnitArtDefines>
```

### Building Art Definitions

```xml
<BuildingArtDefines>
    <Row BuildingArtDefineType="ART_DEF_BUILDING_MY_TEMPLE"
         Model="Buildings/MyTemple/MyTemple.gr2"
         Scale="1.0"/>
</BuildingArtDefines>

<Buildings>
    <Row BuildingType="BUILDING_MY_TEMPLE"
         ArtDefine="ART_DEF_BUILDING_MY_TEMPLE"/>
</Buildings>
```

## Texture Assets

### Texture Types

| Type | Suffix | Purpose |
|------|--------|---------|
| Diffuse/Albedo | `_D` | Base color |
| Normal | `_N` | Surface detail |
| Specular | `_S` | Reflectivity |
| Emissive | `_E` | Glow effects |
| Ambient Occlusion | `_AO` | Shadow detail |

### Texture Sizes

| Usage | Recommended Size |
|-------|------------------|
| Unit textures | 512x512 - 1024x1024 |
| Building textures | 1024x1024 - 2048x2048 |
| Terrain textures | 512x512 |
| UI elements | Power of 2 (256, 512, etc.) |

## UI Assets

### UI Image Requirements

```xml
<UITextures>
    <Row TextureName="MY_MOD_BUTTON"
         Filename="Art/UI/MyModButton.dds"
         Width="64"
         Height="32"/>
</UITextures>
```

### Background Images

For custom screens or panels:

```xml
<UIBackgrounds>
    <Row BackgroundName="MY_MOD_BACKGROUND"
         Filename="Art/UI/MyModBackground.dds"
         TileMode="STRETCH"/>
</UIBackgrounds>
```

### Tile Modes

| Mode | Description |
|------|-------------|
| `STRETCH` | Stretch to fill area |
| `TILE` | Repeat texture |
| `CENTER` | Center without scaling |
| `FILL` | Fill maintaining aspect ratio |

## File Organization

### Recommended Structure

```
MyMod/
├── Art/
│   ├── Icons/
│   │   ├── MyMod_Units.dds
│   │   ├── MyMod_Buildings.dds
│   │   └── MyMod_Techs.dds
│   ├── Leaders/
│   │   ├── MyLeader_Portrait.dds
│   │   └── MyLeader_Background.dds
│   ├── Civs/
│   │   └── MyCiv_Background.dds
│   ├── Units/
│   │   └── MyWarrior/
│   │       ├── MyWarrior.gr2
│   │       ├── MyWarrior_D.dds
│   │       └── MyWarrior_N.dds
│   ├── Buildings/
│   │   └── MyTemple/
│   │       └── ...
│   └── UI/
│       ├── MyModButton.dds
│       └── MyModBackground.dds
├── Data/
│   └── MyMod_ArtDefs.xml
└── MyMod.modinfo
```

### ModInfo Art Configuration

```xml
<Mod id="my-mod" version="1.0">
    <Properties>
        <Name>My Mod</Name>
    </Properties>

    <Components>
        <!-- Art definitions -->
        <UpdateDatabase id="my-mod-art">
            <Items>
                <File>Data/MyMod_ArtDefs.xml</File>
            </Items>
        </UpdateDatabase>
    </Components>

    <!-- Import art files -->
    <Files>
        <File>Art/Icons/MyMod_Units.dds</File>
        <File>Art/Icons/MyMod_Buildings.dds</File>
        <File>Art/Leaders/MyLeader_Portrait.dds</File>
        <!-- etc. -->
    </Files>
</Mod>
```

## Complete Example: Custom Unit with Icon

### Step 1: Create Icon Atlas (MyMod_Icons.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <IconTextureAtlases>
        <Row Name="ICON_ATLAS_MY_MOD"
             IconsPerRow="2"
             IconsPerColumn="2"
             IconSize="256"
             Filename="Art/Icons/MyMod_Icons.dds"/>
    </IconTextureAtlases>

    <IconDefinitions>
        <Row Name="ICON_UNIT_ELITE_GUARD" Atlas="ICON_ATLAS_MY_MOD" Index="0"/>
        <Row Name="ICON_UNIT_ELITE_GUARD_PORTRAIT" Atlas="ICON_ATLAS_MY_MOD" Index="1"/>
    </IconDefinitions>
</Database>
```

### Step 2: Define Unit Art (MyMod_Units.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Database>
    <Types>
        <Row Type="UNIT_ELITE_GUARD" Kind="KIND_UNIT"/>
    </Types>

    <!-- Use existing model as base -->
    <ArtDefines>
        <Row ArtDefineType="ART_DEF_UNIT_ELITE_GUARD"
             BaseArtDefine="ART_DEF_UNIT_SWORDSMAN"/>
    </ArtDefines>

    <Units>
        <Row UnitType="UNIT_ELITE_GUARD"
             Name="LOC_UNIT_ELITE_GUARD_NAME"
             Description="LOC_UNIT_ELITE_GUARD_DESC"
             Icon="ICON_UNIT_ELITE_GUARD"
             Portrait="ICON_UNIT_ELITE_GUARD_PORTRAIT"
             ArtDefine="ART_DEF_UNIT_ELITE_GUARD"
             BaseMoves="2"
             BaseCombat="40"
             Cost="150"/>
    </Units>
</Database>
```

## Tools and Resources

### Recommended Tools

| Tool | Purpose |
|------|---------|
| **Photoshop/GIMP** | Icon and texture creation |
| **NVIDIA Texture Tools** | DDS conversion |
| **Blender** | 3D modeling (requires export plugin) |
| **Asset Studio** | Extract game assets for reference |
| **ModBuddy** | Mod project management |

### DDS Conversion Commands

Using NVIDIA Texture Tools:
```bash
# Convert PNG to DDS with alpha
nvcompress -bc3 input.png output.dds

# Convert PNG to DDS without alpha
nvcompress -bc1 input.png output.dds
```

Using Microsoft Texconv:
```bash
# High quality with alpha
texconv -f BC7_UNORM -o output/ input.png

# Standard with alpha
texconv -f BC3_UNORM -o output/ input.png

# No alpha
texconv -f BC1_UNORM -o output/ input.png
```

## Best Practices

1. **Use DDS Format** - Better performance than PNG in-game
2. **Power of 2 Sizes** - 64, 128, 256, 512, 1024, 2048
3. **Match Game Style** - Study existing assets for consistency
4. **Optimize File Sizes** - Use appropriate compression
5. **Test All Resolutions** - Verify icons look good at all zoom levels
6. **Provide Fallbacks** - Reference existing art when custom isn't available
7. **Organize Files** - Use clear folder structure
8. **Document Assets** - Note file purposes and requirements
9. **Version Control** - Track source files separately from converted DDS
10. **Consider Performance** - Large textures impact load times

## Troubleshooting

### Missing Icons

- Verify atlas filename and path
- Check IconDefinitions match expected names
- Ensure DDS format is correct (BC3 for transparency)
- Confirm modinfo includes the file

### Corrupted Textures

- Re-export DDS with correct settings
- Verify dimensions are power of 2
- Check compression format matches usage

### Model Not Appearing

- Verify ArtDefine reference is correct
- Check model file path in modinfo
- Try using BaseArtDefine as fallback

## Related Documentation

- [Units](../database/Units.md)
- [Buildings](../database/Buildings.md)
- [Civilizations](../database/Civilizations.md)
- [Leaders](../database/Leaders.md)
- [ModInfo Files](../Modinfo-Files.md)
