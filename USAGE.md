# projectMSDL Configuration Guide

projectMSDL is the SDL2-based test UI for projectM. It uses a configuration file rather than command-line arguments for customization.

## Command-Line Options

**projectMSDL does NOT support command-line arguments.** All configuration is done via the config file.

To run projectMSDL:

```bash
projectMSDL
```

## Configuration File

### Location

The configuration file is searched for in this order:

1. **`~/.projectM/config.inp`** (user home directory) - Created automatically if it doesn't exist
2. **`/usr/local/share/projectM/config.inp`** (installation directory fallback)

On first run, projectMSDL will:
- Create the `~/.projectM/` directory if it doesn't exist
- Copy the default config from the installation directory to your home directory

### File Format

The configuration file uses a simple key-value format:
- **Delimiter**: `=` (equals sign)
- **Comments**: Lines starting with `#` are ignored
- **Whitespace**: Leading/trailing whitespace is trimmed

## Configuration Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **Mesh X** | uint32 | 708 | Width of the per-pixel equation mesh (affects performance and smoothness) |
| **Mesh Y** | uint32 | 400 | Height of the per-pixel equation mesh (affects performance and smoothness) |
| **FPS** | int32 | 60 | Target frames per second |
| **Smooth Transition Duration** | double | 1 | Duration of smooth preset transitions in seconds |
| **Preset Duration** | double | 30 | How long each preset displays before auto-switching (seconds) |
| **Hard Cuts Enabled** | bool | false | Enable hard cuts (sudden transitions triggered by bass hits) |
| **Hard Cut Duration** | double | 60 | Minimum seconds between hard cuts |
| **Hard Cut Sensitivity** | float | 1.0 | Volume threshold for hard cuts to trigger (higher = more sensitive) |
| **Beat Sensitivity** | float | 1.0 | Audio reactivity of visualizations (0-5 scale: 0="dead", 5="VERY reactive") |
| **Window Width** | uint32 | 512 | Initial window width in pixels |
| **Window Height** | uint32 | 512 | Initial window height in pixels |
| **Fullscreen** | bool | false | Start in fullscreen mode |
| **Easter Egg Parameter** | float | 1 | Easter egg setting |
| **Aspect Correction** | bool | true | Enable aspect ratio correction for custom shapes |
| **Preset Path** | string | presets | Path to preset directory (relative to installation data directory) |
| **Title Font** | string | Vera.ttf | Font file for title display |
| **Menu Font** | string | VeraMono.ttf | Font file for menu display |

## Example Configuration File

```ini
# config.inp
# Configuration File for projectM

# Mesh Settings (affects performance and visual smoothness)
Mesh X  = 708                   # Width of PerPixel Equation mesh
Mesh Y  = 400                   # Height of PerPixel Equation mesh

# Display and Performance
FPS     = 60                    # Frames Per Second
Window Width  = 1024            # Startup window width in pixels
Window Height = 768             # Startup window height in pixels
Fullscreen  = false             # Start in fullscreen mode

# Preset Transitions
Smooth Transition Duration = 1  # Duration of smooth transitions (seconds)
Preset Duration = 10            # How long each preset displays (seconds)

# Hard Cuts (sudden transitions on bass hits)
Hard Cuts Enabled = false       # Enable hard cuts
Hard Cut Duration = 60          # Minimum seconds between hard cuts
Hard Cut Sensitivity = 1.0      # Volume sensitivity (higher = more sensitive)

# Audio Reactivity
Beat Sensitivity = 1.0          # Audio reactivity (0 = dead, 5 = VERY reactive)

# Visual Settings
Aspect Correction = true        # Aspect ratio correction for custom shapes
Easter Egg Parameter = 1        # Easter egg setting

# Preset and Font Paths
Preset Path = presets           # Preset directory location
Title Font = Vera.ttf           # Font for titles
Menu Font = VeraMono.ttf        # Font for menus
```

## Performance Tuning

### For Lower-End Systems
- Reduce **Mesh X** and **Mesh Y** (e.g., 512x300)
- Reduce **FPS** to 30 or 45
- Disable **Hard Cuts Enabled** or increase **Hard Cut Duration**

### For Higher-End Systems
- Increase **Mesh X** and **Mesh Y** for smoother effects (e.g., 1024x768)
- Set **FPS** to 120+ if your monitor supports it
- Increase **Beat Sensitivity** for more responsive visuals

## Audio Reactivity Tips

| Beat Sensitivity | Use Case |
|---|---|
| 0 - 0.5 | Calming, slow music; subtle effects |
| 0.5 - 1.5 | Standard settings; most music genres |
| 1.5 - 3.0 | Bass-heavy music; electronic/EDM |
| 3.0 - 5.0 | Very reactive; experimental/extreme settings |

## Preset Switching

Presets will auto-switch based on:
1. **Preset Duration**: How long each preset displays
2. **Hard Cut Duration & Sensitivity**: Allow sudden switches on bass hits
3. **Smooth Transition Duration**: How long the blend between presets takes

To manually switch presets in the UI, use keyboard controls (see projectMSDL help menu).

## Troubleshooting

**Config file not being read?**
- Ensure file is located at `~/.projectM/config.inp`
- Check file permissions (should be readable by your user)
- Verify syntax (key-value pairs, `=` delimiter, `#` comments)
- Restart projectMSDL to reload the config

**Settings not applying?**
- projectMSDL loads config on startup only
- Editing config.inp while running won't apply changes until next launch

**Low FPS or stuttering?**
- Reduce **Mesh X** and **Mesh Y** values
- Lower **FPS** target
- Close other applications
- Check GPU/CPU usage during playback