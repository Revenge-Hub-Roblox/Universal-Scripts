# Utils Module — Documentation

## 🗂️ Table of Contents

### 🔧 Core Setup
- [Initialization](#️-initialization) - How to set up the module
- [Services Integration](#-services-integration) - Automatic Roblox service handling

### 🎮 Character & Player
- [Targeting](#-utilsgetclosestplayermaxdistance) - Find closest players
- [Vision](#️-utilslookattargetposition) - Control character facing
- [Movement](#-movement-api) - Walk, jump, sit, fling, nudge

### 📊 System & Performance
- [Performance Metrics](#-performance-metrics) - FPS, Ping, TPS monitoring
- [Team Detection](#-team-detection) - Get player's current team

### ⚙️ Advanced
- [Service Management](#-services-integration) - Direct Roblox service access
- [Event Handling](#-utilsonrespawncb) - Respawn callbacks

---

## 📌 Features

* Automatic LocalPlayer, Character, RootPart, and Humanoid caching
* Auto-refresh on player death
* Raycasting helpers (including wall-bypass "weapon" raycast)
* Targeting utilities (closest player, lookAt)
* Movement API (walk, jump, sit, fling, etc.)
* Performance metrics (FPS, Ping, TPS)
* Auto-team detection
* Global access via `_G` or `getgenv()` after initialization

---

# ⚙️ Initialization

Load the Utils module using your loader:

```lua
local Utils = loadstring(game:HttpGet("https://raw.githubusercontent.com/volmaksDev/My-Scripts/refs/heads/main/AdvancedUtils.lua"))()
Utils.init()
```

After calling `Utils.init()`, the following globals become available:

```lua
player
character
hrp
humanoid
```

(These are synced automatically when you respawn.)

---

# 📚 API Reference

Below are all currently available methods and their usage examples.

---

## 🔄 `Utils.init()`

Initializes the module and registers global references.

```lua
Utils.init()
print(player, character, hrp)
```

---

## 🎯 `Utils.getClosestPlayer(maxDistance?)`

Returns the player instance closest to you.

```lua
local target = Utils.getClosestPlayer(200) -- optional range limit
if target then
    print("Closest:", target.Name)
end
```

---

## 👁️ `Utils.lookAt(targetPosition)`

Rotates your HumanoidRootPart to face a world position.

```lua
Utils.lookAt(Vector3.new(0,5,0))
```

Or to face another player:

```lua
local enemy = Utils.getClosestPlayer()
if enemy then
    Utils.lookAt(enemy.Character.HumanoidRootPart.Position)
end
```

---

## 🧭 Movement API

A collection of methods for controlling your character.

### `Utils.moveTo(position)`

```lua
Utils.moveTo(Vector3.new(10,0,10))
```

### `Utils.jump()`

```lua
Utils.jump()
```

### `Utils.sit()`

```lua
Utils.sit()
```

### `Utils.nudge(directionVector)`

Pushes your character slightly.

```lua
Utils.nudge(Vector3.new(0, 0, -10))
```

### `Utils.fling(power)`

Applies a high-velocity fling.

```lua
Utils.fling(5000)
```


---

## 📊 Performance Metrics

### `Utils.getFPS()`

```lua
print("FPS:", Utils.getFPS())
```

### `Utils.getPing()`

```lua
print("Ping:", Utils.getPing())
```

### `Utils.getTPS()`

```lua
print("TPS:", Utils.getTPS())
```

---

## 🏳 Team Detection

### `Utils.getTeam()`

Returns the LocalPlayer’s current team name.

```lua
local team = Utils.getTeam()
print("Team:", team)
```

# 🔧 Services Integration

The module now includes a robust service management system that automatically handles all Roblox services with caching and error protection.

## 🎯 Automatic Service Loading

After calling `Utils.init()`, all Roblox services become available as global variables:

```lua
Utils.init()

-- All services are now available globally:
RunService.Heartbeat:Connect(function()
    -- Your code here
end)

TweenService:Create(part, tweenInfo, goals)
UserInputService.InputBegan:Connect(callback)
```

## 📋 Available Services

All major Roblox services are supported:

```lua
-- Core services
Players
Workspace
Lighting
ReplicatedStorage
RunService
TweenService

-- Input & UI
UserInputService
ContextActionService
StarterGui
GuiService
TextService

-- Networking & Data
HttpService
TeleportService
MarketplaceService
MemoryStoreService

-- Gameplay
PathfindingService
Terrain
CollectionService
SoundService
Teams

-- And many more...
```

## 🔧 Manual Service Access

You can also access services manually through the Services table:

```lua
-- Method 1: Direct global access (recommended)
RunService.Heartbeat:Connect(callback)

-- Method 2: Through Utils.Services
Utils.Services.RunService.Heartbeat:Connect(callback)

-- Method 3: Dynamic service loading
local MyService = Utils.Services.AnyServiceName
```

## ⚡ Features

- **Automatic Caching**: Services are cached on first access for better performance
- **Error Protection**: Invalid service names won't crash your script
- **Lazy Loading**: Services are only loaded when actually used
- **Global Exposure**: All services become available globally after initialization

## 💡 Usage Example

```lua
local Utils = loadstring(game:HttpGet("https://raw.githubusercontent.com/volmaksDev/My-Scripts/refs/heads/main/AdvancedUtils.lua"))()
Utils.init()

-- Now use any service directly:
RunService.Heartbeat:Connect(function()
    if character then
        -- Update character every frame
    end
end)

-- Create tweens easily
local tween = TweenService:Create(part, TweenInfo.new(1), {Position = targetPos})
tween:Play()

-- Handle user input
UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.E then
        Utils.jump()
    end
end)
```

## 🛡️ Error Handling

If you try to access a non-existent service, you'll get a warning instead of an error:

```lua
local fakeService = NonExistentService -- Warning: 'NonExistentService' is not a valid Roblox service
```

This prevents scripts from breaking when service names are misspelled or unavailable.

---

# ❗ Notes

* Globals (`player`, `character`, `hrp`, etc.) auto-refresh after respawn
* This module is safe to call multiple times
* Intended for exploit-side scripting (loadstring-compatible)
* All helper functions avoid expensive operations unless necessary

---

# 📄 License

MIT License
You are free to use, modify and distribute the module.
