# Port-Mod Patches

This directory contains patches for the `port-mod` submodule that add features not yet merged upstream.

## Applying Patches

Before building, apply the patches to the `port-mod` submodule:

```bash
# From the repository root
git submodule update --init --recursive
git -C port-mod apply ../patches/port-mod/*.patch
```

## Current Patches

### 0001-lan-room-discovery.patch

**What it does:**
- Implements automatic LAN room discovery using UDP broadcast on port 33770
- Hosts broadcast their room information every 2 seconds
- Clients listen for broadcasts and display discovered rooms in the join screen
- Adds a settings toggle `lan_room_discovery_enabled` (default: true)
- Manages Android MulticastLock for receiving broadcasts
- Keeps manual IP entry as a fallback option

**Modified files:**
- `STS2AndroidPortCompat/Patches/LanRoomDiscoveryService.cs` (new)
- `STS2AndroidPortCompat/Patches/LanMultiplayerPatches.cs`
- `STS2AndroidPortCompat/Patches/AndroidInGameSettingsPatches.cs`
- `STS2AndroidPortCompat/Patches/AndroidSettingsMerge.cs`

**Testing:**
To test LAN discovery, you need two Android devices on the same WiFi:
1. Device A: Create a multiplayer host
2. Device B: Open "Join Friend" screen - the host should appear in the discovered rooms list
3. Click on a discovered room to auto-fill its IP and port

**Why a patch?**
This change is in a fork's development branch. The patch format allows:
- Easy review of the exact changes
- Applying to any compatible `port-mod` version
- Testing without maintaining a separate fork of sts2-android-compat
- Potential upstream submission when stable
