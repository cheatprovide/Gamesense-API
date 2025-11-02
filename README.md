# Gamesense Lua API Documentation

---

## 📋 Table of Contents

1. [Overview](#overview)
2. [Core Modules](#core-modules)
3. [Getting Started](#getting-started)
4. [API Reference](#api-reference)
5. [Code Examples](#code-examples)
6. [Best Practices](#best-practices)

---

## Overview

The Gamesense Lua API﻿ provides an extensive toolkit specifically tailored for creating advanced scripts. These API give deep access to the game state, rendering overlays for enhanced visibility, and user interface controls for customizable cheat features. The API facilitates the development scripts for gameplay advantage, including anti-aim, resolver, and visual feautures, while also enabling debugging and fine-tuning of these cheat components to maximize efficacy.

### Key Features

- **Event-Driven Architecture** — Callback system for real-time event handling
- **Screen & World Rendering** — Draw text, shapes, and textures on screen
- **Entity Manipulation** — Access and modify player and game entity data
- **Advanced UI Controls** — Create custom menu items and interface elements
- **Persistent Storage** — Database system for saving configuration data
- **Network Information** — Monitor latency and command queue state
- **Collision Detection** — Hitbox tracing and visibility checks

---

## Core Modules

| Module | Purpose |
|--------|---------|
| **client** | Core client functions (events, logging, tracing) |
| **entity** | Player and entity manipulation |
| **globals** | Game variables and timing information |
| **ui** | Menu controls and interface elements |
| **renderer** | Screen rendering and graphics drawing |
| **config** | Configuration and preset management |
| **cvar** | Console variable access and control |
| **database** | Persistent data storage system |
| **materialsystem** | Material and shader manipulation |
| **plist** | Player list management utilities |
| **panorama** | UI panel control and JavaScript execution |

---

## Getting Started

### Basic Script Structure

Every Gamesense script follows this fundamental pattern:

```lua
-- Create a UI element for user control
local my_feature = ui.new_checkbox("MISC", "Main", "Enable My Feature")

-- Register an event callback
client.set_event_callback("paint", function()
    -- Check if the feature is enabled
    if ui.get(my_feature) then
        -- Your feature logic here
        client.log("Feature is active!")
    end
end)

-- Optional: Log that the script loaded
client.log("Script loaded successfully!")
```

### Understanding the Event System

All runtime code execution happens through event callbacks:

```lua
client.set_event_callback(event_name, callback_function)
```

**Common Events:**
- `paint` — Called every frame for rendering and real-time updates
- Game events — Custom events triggered by game state changes

To remove a callback later:

```lua
client.unset_event_callback(event_name, callback_function)
```

---

## API Reference

### Client Functions

#### Event Management

```lua
client.set_event_callback(event_name, callback)
```
Register a callback function to be executed when an event occurs.

```lua
client.unset_event_callback(event_name, callback)
```
Remove a previously registered event callback.

---

#### Logging System

```lua
client.log(msg, ...)
```
Write a message to the console. Supports multiple arguments for concatenation.

```lua
client.color_log(r, g, b, msg, ...)
```
Write a colored message to the console.
- Parameters: `r, g, b` — RGB color values (0-255)

```lua
client.error_log(msg)
```
Write an error message to the console with special formatting.

---

#### Console Commands

```lua
client.exec(cmd, ...)
```
Execute one or more console commands. Multiple arguments are concatenated.

Example:
```lua
client.exec("say", " Hello world!")
```

---

#### User Input Detection

```lua
client.key_state(virtual_key)
```
Check if a keyboard key is currently pressed. Returns `true` or `false`.

---

#### Timing Functions

```lua
client.latency()
```
Returns the current network latency in seconds.

```lua
client.timestamp()
```
Returns a high-precision timestamp in milliseconds.

```lua
client.system_time()
```
Returns current system time as four values: `hour, minute, seconds, milliseconds`

```lua
client.unix_time()
```
Returns Unix timestamp as four values: `hour, minute, seconds, milliseconds`

---

#### Camera Functions

```lua
client.camera_angles()
```
Returns current camera viewing angles: `pitch, yaw, roll`

```lua
client.camera_position()
```
Returns camera world coordinates: `x, y, z`

```lua
client.eye_position()
```
Returns local player's eye position in world coordinates: `x, y, z` (or `nil` if not available)

```lua
client.screen_size()
```
Returns screen dimensions: `width, height`

---

#### Visibility & Tracing

```lua
client.visible(x, y, z)
```
Check if a world position is visible and rendered on screen. Returns `true` or `false`.

```lua
client.trace_line(skip_entindex, from_x, from_y, from_z, to_x, to_y, to_z)
```
Trace a line from point A to point B and detect intersections.

**Returns:** `fraction, entindex`
- `fraction` — Hit percentage (0.0 = immediate hit, 1.0 = no hit)
- `entindex` — Index of entity hit, or -1 if none

```lua
client.trace_bullet(from_player, from_x, from_y, from_z, to_x, to_y, to_z, skip_players)
```
Trace a bullet trajectory using a specific player's current weapon.

**Parameters:**
- `from_player` — Entity index of the player whose weapon to use
- `skip_players` — Optional boolean to skip expensive hitbox checks

**Returns:** `entindex, damage`

---

#### Damage Calculation

```lua
client.scale_damage(entindex, hitgroup, damage)
```
Calculate adjusted damage based on the hit group and armor.

**Parameters:**
- `hitgroup` — Integer index of the hit group
- `damage` — Base damage value

**Returns:** Adjusted damage value

---

#### Debug Drawing

```lua
client.draw_debug_text(x, y, z, line_offset, duration, r, g, b, a, ...)
```
Draw debug text at a world position with automatic screen projection.

**Parameters:**
- `x, y, z` — World position
- `line_offset` — Vertical line spacing for multi-line text
- `duration` — How long to display the text (in seconds)
- `r, g, b, a` — RGBA color values (0-255)

**Important:** Do not call during the `paint` event.

```lua
client.draw_hitboxes(entindex, duration, hitboxes, r, g, b, a, tick)
```
Draw visual overlays for entity hitboxes.

**Parameters:**
- `hitboxes` — Specific hitbox index, array of indices, or 19 for all hitboxes
- `duration` — Display duration in seconds

**Important:** Do not call during the `paint` event.

---

#### Random Number Generation

```lua
client.random_int(minimum, maximum)
```
Generate a random integer between minimum and maximum (inclusive).

```lua
client.random_float(minimum, maximum)
```
Generate a random float between minimum and maximum.

---

#### Miscellaneous Client Functions

```lua
client.userid_to_entindex(userid)
```
Convert a user ID to an entity index. Returns entity index or 0 on failure.

```lua
client.set_clan_tag(...)
```
Set the player's clan tag. Pass no arguments or an empty string to remove the tag.

```lua
client.delay_call(delay, callback, ...)
```
Execute a function after a specified delay (in seconds).

```lua
client.reload_active_scripts()
```
Reload all active scripts on the next frame.

---

#### Low-Level Interface Access

```lua
client.create_interface(module_name, interface_name)
```
Get a pointer to a game interface by name. Returns pointer or `nil`.

```lua
client.find_signature(module_name, pattern)
```
Find a memory signature pattern within a game module.

**Pattern Format:** Use `\xCC` as a wildcard. Example: `'\x01\x02\xCC\x03'`

**Returns:** Memory address or `nil`

```lua
client.get_model_name(model_index)
```
Get the file path of a model by its index. Returns string or `nil`.

```lua
client.register_esp_flag(flag, r, g, b, callback)
```
Register a custom ESP information flag. Requires "Flags" to be enabled in Player ESP settings.

---

### Entity Functions

#### Getting Entities

```lua
entity.get_local_player()
```
Get the local player's entity index. Returns index or `nil`.

```lua
entity.get_all(classname)
```
Get all entities of a specific class.

**Parameters:**
- `classname` — Optional class name filter (e.g., "CCSPlayer", "CWeaponM4A1")

**Returns:** Array of entity indices

```lua
entity.get_players(enemies_only)
```
Get player entities (only active, alive players).

**Parameters:**
- `enemies_only` — Optional boolean to filter only enemies

**Returns:** Array of player entity indices

```lua
entity.get_game_rules()
```
Get the game rules entity index.

```lua
entity.get_player_resource()
```
Get the player resource entity index.

---

#### Entity Properties

```lua
entity.get_classname(ent)
```
Get the class name of an entity.

```lua
entity.get_prop(ent, propname, array_index)
```
Get the value of a networked property.

**Parameters:**
- `array_index` — Optional array index for array-type properties

**Returns:** Value or `nil`. Vectors and angles return 3 separate values.

```lua
entity.set_prop(ent, propname, value, array_index)
```
Set the value of a networked property.

**Note:** For vectors and angles, separate components with commas.

```lua
entity.get_origin(player)
```
Get world coordinates of an entity: `x, y, z`

```lua
entity.get_esp_data(player)
```
Get ESP information table containing `alpha`, `health`, and `weapon_id`.

---

#### Player Information

```lua
entity.get_player_name(ent)
```
Get a player's in-game name or "unknown" if unavailable.

```lua
entity.get_player_weapon(ent)
```
Get the entity index of the player's active weapon, or `nil`.

```lua
entity.get_steam64(player)
```
Get a player's SteamID64 or `nil`.

```lua
entity.is_enemy(ent)
```
Check if an entity is on the opposing team. Returns `true` or `false`.

```lua
entity.is_alive(ent)
```
Check if a player is alive. Returns `true` or `false`.

```lua
entity.is_dormant(ent)
```
Check if a player is currently active (not dormant/not streamed out). Returns `true` or `false`.

---

#### Hitbox Operations

```lua
entity.hitbox_position(player, hitbox)
```
Get world coordinates of a specific hitbox.

**Parameters:**
- `hitbox` — Hitbox name as string or integer index

**Returns:** `x, y, z` or `nil`

```lua
entity.get_bounding_box(player)
```
Get the 2D screen bounding box of a player.

**Returns:** `x1, y1, x2, y2, alpha_multiplier`
- `alpha_multiplier` of 0 indicates invalid/off-screen box

---

### Rendering Functions

**Important:** Most rendering functions can only be called from the `paint` event callback.

#### Text Rendering

```lua
renderer.text(x, y, r, g, b, a, flags, max_width, ...)
```
Draw text on the screen.

**Parameters:**
- `flags` — String of formatting flags:
  - `"+"` — Large text
  - `"-"` — Small text
  - `"c"` — Center aligned
  - `"r"` — Right aligned
  - `"b"` — Bold
  - `"d"` — High DPI scaling
- `max_width` — Text width clipping (0 for no limit)

```lua
renderer.measure_text(flags, ...)
```
Calculate the dimensions of rendered text.

**Returns:** `width, height`

```lua
renderer.indicator(r, g, b, a, ...)
```
Draw text as an indicator (vertically offset for stacking).

**Returns:** Y coordinate or `nil`

---

#### Shape Drawing

```lua
renderer.rectangle(x, y, w, h, r, g, b, a)
```
Draw a filled rectangle.

```lua
renderer.line(xa, ya, xb, yb, r, g, b, a)
```
Draw a line between two points.

```lua
renderer.gradient(x, y, w, h, r1, g1, b1, a1, r2, g2, b2, a2, ltr)
```
Draw a gradient-filled rectangle.

**Parameters:**
- `ltr` — `true` for left-to-right (horizontal), `false` for top-to-bottom (vertical)

```lua
renderer.triangle(x0, y0, x1, y1, x2, y2, r, g, b, a)
```
Draw a filled triangle.

```lua
renderer.circle(x, y, r, g, b, a, radius, start_degrees, percentage)
```
Draw a filled circle or arc.

**Parameters:**
- `start_degrees` — Starting angle (0=right, 90=bottom, 180=left, 270=top)
- `percentage` — Portion of circle (0.0-1.0)

```lua
renderer.circle_outline(x, y, r, g, b, a, radius, start_degrees, percentage, thickness)
```
Draw a circle outline.

---

#### Texture & Image Rendering

```lua
renderer.texture(id, x, y, w, h, r, g, b, a, mode)
```
Draw a loaded texture on screen.

**Parameters:**
- `mode` — Rendering mode:
  - `"f"` — Fill/stretch
  - `"r"` — Repeat/tile
  - `nil` — Auto mode

```lua
renderer.load_svg(contents, width, height)
```
Load SVG data as a texture. Returns texture ID or `nil`.

```lua
renderer.load_png(contents, width, height)
```
Load PNG image data as a texture. Returns texture ID or `nil`.

```lua
renderer.load_jpg(contents, width, height)
```
Load JPG image data as a texture. Returns texture ID or `nil`.

```lua
renderer.load_rgba(contents, width, height)
```
Load raw RGBA buffer data as a texture. Returns texture ID or `nil`.

---

#### Coordinate Conversion

```lua
renderer.world_to_screen(x, y, z)
```
Convert world coordinates to screen coordinates.

**Returns:** `screen_x, screen_y` or `nil` if position is off-screen

---

### UI Functions

#### Creating UI Elements

```lua
ui.new_checkbox(tab, container, name)
```
Create a checkbox (true/false toggle).

```lua
ui.new_slider(tab, container, name, min, max, init_value, show_tooltip, unit, scale, tooltips)
```
Create a slider (numeric input with range).

```lua
ui.new_combobox(tab, container, name, ...)
```
Create a dropdown menu with options.

```lua
ui.new_multiselect(tab, container, name, ...)
```
Create a multi-select menu.

```lua
ui.new_hotkey(tab, container, name, inline, default_hotkey)
```
Create a hotkey/keybind input.

```lua
ui.new_button(tab, container, name, callback)
```
Create a clickable button.

```lua
ui.new_color_picker(tab, container, name, r, g, b, a)
```
Create a color picker.

```lua
ui.new_textbox(tab, container, name)
```
Create a text input field.

```lua
ui.new_listbox(tab, container, name, items)
```
Create a list selection widget.

```lua
ui.new_label(tab, container, name)
```
Create a read-only label.

```lua
ui.new_string(name, value)
```
Create a persistent string variable.

**Available Tabs:** `RAGE`, `AA`, `LEGIT`, `VISUALS`, `MISC`, `SKINS`, `PLAYERS`, `LUA`

---

#### UI Control

```lua
ui.get(item)
```
Get the current value of a UI element.

**Return Types:**
- Checkbox: `true` or `false`
- Slider: Integer
- Combobox: String
- Multiselect: Array of strings
- Hotkey: `true` if pressed, `false` otherwise
- Color Picker: `r, g, b, a`
- Textbox: String

```lua
ui.set(item, value, ...)
```
Set the value of a UI element.

```lua
ui.set_callback(item, callback)
```
Set a function to execute when the UI element changes.

```lua
ui.set_visible(item, visible)
```
Show or hide a UI element.

```lua
ui.reference(tab, container, name)
```
Get a reference to a built-in menu item for manipulation.

---

#### UI Status

```lua
ui.is_menu_open()
```
Check if the menu is currently open. Returns `true` or `false`.

```lua
ui.mouse_position()
```
Get current mouse coordinates: `x, y`

```lua
ui.menu_position()
```
Get menu window position: `x, y`

```lua
ui.menu_size()
```
Get menu dimensions: `width, height`

```lua
ui.name(item)
```
Get the display name of a UI element.

---

### Configuration System

```lua
config.load(name, tab_name, container_name)
```
Load a saved configuration preset.

**Usage Examples:**
```lua
config.load('MyConfig')                          -- Load entire config
config.load('MyConfig', 'VISUALS')              -- Load specific tab
config.load('MyConfig', 'VISUALS', 'Main')      -- Load specific container
```

```lua
config.export()
```
Export current configuration as a string.

---

### Console Variables (CVars)

```lua
cvar.set_string(value)
cvar.get_string()
cvar.set_float(value)
cvar.set_raw_float(value)
cvar.get_float()
cvar.set_int(value)
cvar.set_raw_int(value)
cvar.get_int()
cvar.invoke_callback(...)
```

**Usage Example:**
```lua
cvar.cl_interp_ratio:set_float(1)
cvar.snd_setmixer:invoke_callback("Ambient", "vol", "0")
```

---

### Global Variables

```lua
globals.realtime()
```
Get local system time in seconds (as float).

```lua
globals.curtime()
```
Get game simulation time in seconds (synchronized with server).

```lua
globals.frametime()
```
Get elapsed time during the last frame in seconds.

```lua
globals.absoluteframetime()
```
Get absolute frame time in seconds.

```lua
globals.maxplayers()
```
Get maximum player slots on the server.

```lua
globals.tickcount()
```
Get total elapsed game ticks since map start.

```lua
globals.tickinterval()
```
Get duration of one server tick in seconds.

```lua
globals.framecount()
```
Get total frames rendered since game start.

```lua
globals.mapname()
```
Get current map name or `nil` if in menu.

```lua
globals.lastoutgoingcommand()
```
Get last outgoing user command number.

```lua
globals.oldcommandack()
```
Get previous server-acknowledged command number.

```lua
globals.commandack()
```
Get most recent server-acknowledged command number.

```lua
globals.chokedcommands()
```
Get number of currently choked (unsent) commands.

---

### Material System

```lua
materialsystem.find_material(path, force_load)
```
Find a material by file path. Returns material reference or `nil`.

```lua
materialsystem.find_materials(partial_path, force_load)
```
Find all materials matching a partial path. Returns table of material references.

```lua
materialsystem.find_texture(path)
```
Find a texture by file path. Returns texture reference.

```lua
materialsystem.get_model_materials(entindex)
```
Get all materials used by an entity model. Returns table of material references.

#### Material Properties

```lua
materialsystem.get_name()
materialsystem.reload()
materialsystem.color_modulate(r, g, b)
materialsystem.alpha_modulate(alpha)
materialsystem.get_shader_param(param_name)
materialsystem.set_shader_param(param_name, value, force)
materialsystem.get_material_var_flag(index)
materialsystem.set_material_var_flag(index, enabled)
```

#### Special Materials

```lua
materialsystem.arms_material()
```
Get the hands/arms material (when viewmodel arms are enabled).

```lua
materialsystem.chams_material()
```
Get the player chams (character model) material.

---

### Database (Persistent Storage)

```lua
database.write(key, value)
```
Save data persistently to the database.

**Parameters:**
- `key` — Unique string identifier for the data
- `value` — Value or table to save

```lua
database.read(key)
```
Load persistent data from the database.

**Returns:** Table/value or `nil` if not found

---

### Panorama UI

```lua
panorama.open(panel)
```
Open a UI panel.

```lua
panorama.loadstring(js_code, panel)
```
Execute JavaScript code within a panel.

---

### Player List Management

```lua
plist.set(entindex, field, value)
plist.get(entindex, field)
```
Set or get a value in the player list for an entity.

---

## Code Examples

### Example 1: Simple Wallhack Toggle

```lua
-- Create a checkbox in the menu
local wallhack_enabled = ui.new_checkbox("VISUALS", "World", "Wallhack")

-- Set up rendering on each frame
client.set_event_callback("paint", function()
    -- Skip if disabled
    if not ui.get(wallhack_enabled) then return end
    
    -- Loop through all enemy players
    for _, player in ipairs(entity.get_players(true)) do
        -- Only process alive players
        if entity.is_alive(player) then
            -- Get player position and render it
            local x, y, z = entity.get_origin(player)
            renderer.world_to_screen(x, y, z)
        end
    end
end)
```

### Example 2: Hotkey Handler

```lua
-- Create a hotkey input
local my_hotkey = ui.new_hotkey("MISC", "Main", "Toggle Feature", true)

-- Check for hotkey press each frame
client.set_event_callback("paint", function()
    if ui.get(my_hotkey) then
        client.log("Hotkey was pressed!")
    end
end)
```

### Example 3: On-Screen Debug Info

```lua
client.set_event_callback("paint", function()
    -- Get mouse and screen info
    local mouse_x, mouse_y = ui.mouse_position()
    local screen_w, screen_h = ui.screen_size()
    
    -- Display mouse position
    renderer.text(10, 10, 255, 255, 255, 255, nil, 0, 
        "Mouse: " .. mouse_x .. ", " .. mouse_y)
    
    -- Display screen resolution
    renderer.text(10, 30, 255, 255, 255, 255, nil, 0, 
        "Resolution: " .. screen_w .. "x" .. screen_h)
    
    -- Display current FPS
    renderer.text(10, 50, 255, 255, 255, 255, nil, 0, 
        "FPS: " .. math.floor(1 / globals.frametime()))
end)
```

### Example 4: Player Names on ESP

```lua
client.set_event_callback("paint", function()
    -- Get the local player
    local me = entity.get_local_player()
    if not me then return end
    
    -- Loop through all enemies
    for _, player in ipairs(entity.get_players(true)) do
        if entity.is_alive(player) then
            -- Get player name and position
            local name = entity.get_player_name(player)
            local enemy_x, enemy_y, enemy_z = entity.get_origin(player)
            
            -- Convert to screen coordinates
            local screen_x, screen_y = renderer.world_to_screen(
                enemy_x, enemy_y, enemy_z
            )
            
            -- Draw name if on screen
            if screen_x then
                renderer.text(screen_x, screen_y, 255, 0, 0, 255, "c", 0, name)
            end
        end
    end
end)
```

### Example 5: Data Persistence

```lua
-- Save settings to persistent storage
database.write("my_settings", {
    feature_enabled = true,
    sensitivity = 42,
    color = {255, 128, 0}
})

-- Load settings from persistent storage
local saved_settings = database.read("my_settings")
if saved_settings then
    client.log("Loaded sensitivity: " .. saved_settings.sensitivity)
end
```

---

## Best Practices

### 1. Always Check for Nil Values

```lua
local pos = entity.get_origin(player)
if pos then
    -- Safe to use pos
    local x, y, z = pos
end
```

### 2. Use Event Callbacks Properly

```lua
-- Store callback for later removal
local my_callback = function()
    -- Event code here
end

client.set_event_callback("paint", my_callback)

-- Clean up when done
client.unset_event_callback("paint", my_callback)
```

### 3. Optimize Paint Callback Operations

```lua
client.set_event_callback("paint", function()
    -- Use framecount to throttle expensive operations
    if globals.framecount() % 10 == 0 then
        -- This runs every 10 frames instead of every frame
    end
    
    -- Or use delay_call for timed operations
    client.delay_call(1, function()
        -- This runs after 1 second
    end)
end)
```

### 4. Persist Important Data

```lua
-- Save user configuration when it changes
ui.set_callback(my_setting, function()
    database.write("user_settings", {
        my_setting = ui.get(my_setting)
    })
end)
```

### 5. Use Color Constants

```lua
local colors = {
    red = {255, 0, 0, 255},
    green = {0, 255, 0, 255},
    blue = {0, 0, 255, 255},
    white = {255, 255, 255, 255}
}

renderer.text(x, y, colors.red[1], colors.red[2], colors.red[3], colors.red[4], nil, 0, "Error")
```

### 6. Validate User Input

```lua
local slider_value = ui.get(my_slider)
if slider_value >= 0 and slider_value <= 100 then
    -- Value is valid, use it
else
    client.error_log("Invalid slider value!")
end
```

### 7. Use Tracing for Visibility

```lua
-- Check direct line of sight
local fraction, hit = client.trace_line(0, x1, y1, z1, x2, y2, z2)
if fraction == 1.0 then
    -- Direct line of sight, no obstruction
else
    -- Something is blocking the view
end
```

### 8. Handle Entity Availability

```lua
-- Many entities can be nil
local local_player = entity.get_local_player()
if not local_player then
    return  -- Skip if local player not available
end

-- Always check before using entity data
for _, player in ipairs(entity.get_players()) do
    if entity.is_alive(player) and not entity.is_dormant(player) then
        -- Safe to access player data
    end
end
```

---

*Having any issues? Discord: discord.gg/vusCGG8BUB*
