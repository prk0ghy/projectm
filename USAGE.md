# projectM SDL Configuration and Usage Guide

projectM is a **music visualization library** that transforms audio input into stunning, dynamic visual effects. This guide explains how to use projectM with presets and configure it to your preferences.

## What is projectM?

projectM is an open-source music visualizer that reads audio input and produces mesmerizing visuals by:
1. **Analyzing audio**: Detecting tempo, analyzing frequency data (FFT), and determining beat patterns
2. **Running preset equations**: Executing Milkdrop-style pixel shaders and mathematical equations from `.milk` preset files
3. **Rendering in real-time**: Creating fluid, responsive visualizations synchronized to the music

## Getting Started

### Prerequisites

- An audio input source (e.g., music player, system audio loopback, microphone)
- A graphical display (monitor)
- projectM installed on your system

### Running projectM SDL

projectM SDL is the SDL2-based test UI for projectM. To start it:

```bash
projectMSDL
```

**Note:** projectMSDL does NOT support command-line arguments. All configuration is done via the config file.

## Understanding Presets (.milk files)

### What is a .milk preset?

A `.milk` preset is a text-based configuration file that defines a visualization style. It contains:
- **Pixel shader code** (GLSL) - Controls what each pixel looks like
- **Per-frame equations** - Mathematical expressions that run once per frame
- **Per-pixel equations** - Mathematical expressions that run for each pixel
- **Shape definitions** - Geometric shapes like waveforms, custom shapes, and motion effects
- **Texture references** - References to texture files used in the visualization

Presets are typically named descriptively (e.g., `Flexi-Chrome.milk`, `Goo.milk`) and are the creative core of projectM visualizations.

### Where to Get Presets

projectM does not ship with presets by default. Download preset packs from the official repositories:

1. **[Cream of the Crop Pack](https://github.com/projectM-visualizer/presets-cream-of-the-crop)** (Recommended)
   - ~10,000 curated presets selected by Jason Fletcher
   - Contains only the best presets from all released Milkdrop packs
   - Default preset pack for most projectM installations
   - **File size**: ~500 MB

2. **[Milkdrop Texture Pack](https://github.com/projectM-visualizer/presets-milkdrop-texture-pack)** (Required)
   - Contains all original Milkdrop textures
   - **Install this alongside any preset pack** for proper texture support

3. **[Classic projectM Presets](https://github.com/projectM-visualizer/presets-projectm-classic)**
   - ~4,200 presets used in projectM versions up to 3.1.12

4. **[Original Milkdrop Presets](https://github.com/projectM-visualizer/presets-milkdrop-original)**
   - The original preset collection from the last official Milkdrop release

5. **Large Collections**
   - [130k+ Preset MegaPack](https://drive.google.com/file/d/1DlszoqMG-pc5v1Bo9x4NhemGPiwT-0pv/view) (4.08 GB zipped with textures)

### Installing Presets

1. Download and extract a preset pack
2. Place the `.milk` files in a directory (e.g., `~/.projectM/presets/` on Linux/macOS or `%APPDATA%\projectM\presets\` on Windows)
3. Place texture `.tga` and `.png` files in the textures directory (same parent directory or as specified in the preset)
4. Configure the preset path in `config.inp` (see below)

### Selecting Presets

Once projectM is running:

**Keyboard Controls:**
- **Space** or **Right Arrow** - Jump to next preset
- **Left Arrow** - Jump to previous preset
- **L** - Lock current preset (disable auto-switching)
- **H** - Toggle hard cuts (sudden transitions on bass hits)

The window title displays the current preset name. Presets auto-switch based on:
- **Preset Duration** - How long each preset displays (configurable)
- **Hard Cut events** - Sudden switches triggered by loud bass hits (configurable)

## Configuration File

### File Location

The configuration file is searched for in this order:

1. **`~/.projectM/config.inp`** (user home directory) - Created automatically on first run
2. **`/usr/local/share/projectM/config.inp`** (system installation directory fallback)

On first run, projectMSDL will:
- Create the `~/.projectM/` directory if it doesn't exist
- Copy the default config file from the installation directory to your home directory

### File Format

The configuration file uses a simple key-value format:
- **Delimiter**: `=` (equals sign)
- **Comments**: Lines starting with `#` are ignored
- **Whitespace**: Leading/trailing whitespace is trimmed

## Configuration Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **Preset Path** | string | `presets` | Path to preset directory (relative to installation or absolute) |
| **Mesh X** | uint32 | 708 | Width of per-pixel equation mesh (affects visual smoothness and performance) |
| **Mesh Y** | uint32 | 400 | Height of per-pixel equation mesh (affects visual smoothness and performance) |
| **FPS** | int32 | 60 | Target frames per second |
| **Window Width** | uint32 | 512 | Initial window width in pixels |
| **Window Height** | uint32 | 512 | Initial window height in pixels |
| **Fullscreen** | bool | false | Start in fullscreen mode |
| **Smooth Transition Duration** | double | 1 | Duration of smooth blend between presets in seconds |
| **Preset Duration** | double | 30 | How long each preset displays before auto-switching (seconds) |
| **Hard Cuts Enabled** | bool | false | Enable sudden preset transitions on bass hits |
| **Hard Cut Duration** | double | 60 | Minimum seconds between hard cuts |
| **Hard Cut Sensitivity** | float | 1.0 | Volume threshold multiplier for hard cuts (higher = more sensitive) |
| **Beat Sensitivity** | float | 1.0 | Audio reactivity of visualizations (0-5 scale: 0="dead", 5="VERY reactive") |
| **Aspect Correction** | bool | true | Enable aspect ratio correction for custom shapes |
| **Title Font** | string | `Vera.ttf` | Font file for title display |
| **Menu Font** | string | `VeraMono.ttf` | Font file for menu display |
| **Easter Egg Parameter** | float | 1 | Easter egg setting |

## Example Configuration File

```ini
# config.inp - Configuration for projectM SDL

# Preset and Texture Paths
Preset Path = ~/.projectM/presets   # Location of .milk preset files

# Display Settings
Window Width = 1920                 # Initial window width
Window Height = 1080                # Initial window height
Fullscreen = false                  # Start windowed (true = start fullscreen)

# Performance (Mesh controls visual smoothness and FPS)
Mesh X = 708                        # Width of pixel equation mesh
Mesh Y = 400                        # Height of pixel equation mesh
FPS = 60                            # Target frames per second

# Preset Transitions
Preset Duration = 30                # Duration each preset displays (seconds)
Smooth Transition Duration = 1      # Smooth blend duration between presets (seconds)

# Hard Cuts (sudden transitions on bass hits)
Hard Cuts Enabled = false           # Enable hard cuts
Hard Cut Duration = 60              # Minimum seconds between hard cuts
Hard Cut Sensitivity = 1.0          # Volume threshold (higher = more sensitive)

# Audio Reactivity
Beat Sensitivity = 1.5              # Visual reactivity to audio (0-5)

# Visual Adjustments
Aspect Correction = true            # Correct aspect ratio for shapes
Easter Egg Parameter = 1            # Easter egg setting

# Font Files
Title Font = Vera.ttf               # Font for preset title display
Menu Font = VeraMono.ttf            # Font for UI menus
```

## Performance Tuning

### For Lower-End Systems (laptops, older GPUs)

```ini
Mesh X = 512
Mesh Y = 300
FPS = 30
Hard Cuts Enabled = false
```

**What these changes do:**
- Reduces mesh resolution → fewer pixels calculated per frame
- Lowers FPS → less frequent rendering
- Less visual smoothness but better battery life and CPU/GPU usage

### For Higher-End Systems (gaming rigs, modern GPUs)

```ini
Mesh X = 1024
Mesh Y = 768
FPS = 144
Beat Sensitivity = 2.0
```

**What these changes do:**
- Increases mesh resolution → smoother, more detailed effects
- Higher FPS → smoother animations (if monitor supports 144Hz)
- Increased beat sensitivity → more responsive audio effects

### For Medium Systems (standard laptops/desktops)

```ini
Mesh X = 708
Mesh Y = 400
FPS = 60
Beat Sensitivity = 1.0
```

This is the default and works well for most systems.

## Audio Reactivity Guide

The **Beat Sensitivity** setting controls how responsive visuals are to audio. Adjust based on music style:

| Beat Sensitivity | Best For | Behavior |
|---|---|---|
| 0.0 - 0.5 | Ambient, lo-fi, calm music | Subtle, gentle effects; minimal audio response |
| 0.5 - 1.5 | General purpose, rock, pop | Balanced responsiveness to beats |
| 1.5 - 3.0 | Bass-heavy, electronic, EDM | Very reactive to drums and bass |
| 3.0 - 5.0 | Experimental, extreme genres | Highly sensitive; may be overwhelming |

## Preset Switching & Transitions

### Automatic Switching

Presets automatically cycle based on these settings:

1. **Preset Duration** - Sets how long each preset displays
   - `Preset Duration = 30` means each preset shows for 30 seconds
   - `Preset Duration = 0` disables auto-switching (manual only)

2. **Hard Cuts** - Optional sudden transitions on bass hits
   - When enabled, loud bass can trigger instant preset changes
   - `Hard Cut Sensitivity` controls how loud the bass needs to be
   - `Hard Cut Duration` prevents cuts from happening too frequently

3. **Smooth Transition** - Blend between presets
   - `Smooth Transition Duration = 1` creates a 1-second blend effect
   - `Smooth Transition Duration = 0` causes instant hard-cut transitions

### Manual Switching

In projectM SDL:
- **Right Arrow** or **Space** - Next preset
- **Left Arrow** - Previous preset
- **L** - Lock preset (stop auto-switching, unlock with L again)

## Troubleshooting

### Config File Not Being Read

**Problem**: Settings aren't applying to projectM

**Solutions**:
1. Ensure file is located at `~/.projectM/config.inp`
2. Check file permissions (must be readable by your user)
3. Verify syntax: each parameter uses format `Key = value`
4. Verify parameter names match exactly (case-sensitive in some cases)
5. Restart projectMSDL to reload config (changes require restart)

### No Presets Loading / "Preset Not Found" Error

**Problem**: Presets directory can't be found

**Solutions**:
1. Download presets from the repositories listed above
2. Verify `Preset Path` in config.inp points to correct directory
   - Use absolute paths (e.g., `/home/user/.projectM/presets`)
   - Use tilde expansion (e.g., `~/.projectM/presets`)
3. Ensure `.milk` files exist in the directory
4. Check file permissions (directory and files must be readable)

### Missing Textures in Presets

**Problem**: Presets display but textures appear corrupted or missing

**Solutions**:
1. Download and install the [Milkdrop Texture Pack](https://github.com/projectM-visualizer/presets-milkdrop-texture-pack)
2. Extract texture files to the same directory as presets
3. Some presets have textures in subdirectories - ensure directory structure is preserved

### Low FPS or Stuttering

**Problem**: Visualizer is choppy or slow

**Solutions** (in order of impact):
1. **Reduce Mesh Resolution**:
   ```ini
   Mesh X = 512
   Mesh Y = 300
   ```
2. **Lower FPS Target**:
   ```ini
   FPS = 45
   ```
3. **Close other applications** (especially those using GPU/audio)
4. **Check GPU/CPU usage** during playback using system monitor
5. **Update graphics drivers** to latest version

### Audio Not Responding to Visualizer

**Problem**: Visualizer shows but doesn't react to music

**Solutions**:
1. Ensure audio input is configured correctly
   - Consult your OS audio settings for loopback/capture device setup
2. Increase **Beat Sensitivity** to make effects more prominent
3. Try a preset known to be audio-reactive (most are)
4. Test with high-volume audio first to verify audio input is working

### Window Sizing or Fullscreen Issues

**Problem**: Window appears wrong size or fullscreen isn't working

**Solutions**:
1. Verify window dimensions in config:
   ```ini
   Window Width = 1920
   Window Height = 1080
   ```
2. For fullscreen issues, try toggling manually with **F** key in projectM
3. On multi-monitor setups, check `stretchMonitors` setting in advanced configs

## Resources

- **Official Repository**: [projectM-visualizer/projectm](https://github.com/projectM-visualizer/projectm)
- **Preset Repositories**: [projectM Organization](https://github.com/projectM-visualizer)
- **Documentation**: [GitHub Wiki](https://github.com/projectM-visualizer/projectm/wiki)
- **Discord Community**: [Join the chat](https://discord.gg/mMrxAqaa3W)

## Tips for Best Results

1. **Start with Cream of the Crop pack** - Contains the best presets
2. **Always install the Milkdrop Texture Pack** - Many presets depend on these textures
3. **Use audio loopback** - Capture system audio instead of mic for better results
4. **Experiment with Beat Sensitivity** - Different music genres work better at different settings
5. **Try different presets** - Each preset has unique visual style and audio reactivity
6. **Adjust for your hardware** - Use performance tuning guides above to optimize for your system