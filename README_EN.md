# TLB Maps And Groups: settings

[Version française](SETTINGS.md)

This document describes every setting of the **TLB Maps And Groups** mod for DayZ.

Server files are in `<server profile folder>/TLB_MapsAndGroups/`. They are created on first start with the
default values below, then rewritten on every start to add new settings.

**Before editing a file**

- Every line of the files carries a comment (`// ...`) with the explanation from this document.
  The mod ignores these comments and rewrites them: only edit the values.
- Comment language: `ConfigCommentsLanguage` in `ServerConfig.json` (`"fr"`, `"en"`, `"auto"`, or `""` for none).
- Yes / no: `1` = yes, `0` = no.
- Colors: ARGB number as a signed decimal. Its hexadecimal value is shown in parentheses here and in brackets in
  the files (`#FFFF0000` = opaque red).
- Ranks: `0` recruit, `1` member, `2` officer, `3` leader.
- If a file has a syntax error, the server says so in its log, uses the default values and does not rewrite the
  file, so you can fix it.
- For a list (`[]`), the fields of each element are written as `List[].Field`.

## Files

- [`ServerConfig.json`](#serverconfig): All general settings: groups, levels, map, ping, compass, GPS, Info tab, chat, events, PvE / PvP zones, territory.
- [`Permissions.json`](#permissions): The mod admins and their permissions.
- [`NoBuildZones.json`](#nobuildzones): No build, PvE and PvP zones. Easier to edit in game (menu, Zones tab).
- [`ServerMarkers.json`](#servermarkers): Permanent markers visible to everyone (also editable in game by admins).
- [`Webhooks.json`](#webhooks): Sending events to Discord.
- [`ClanClothing.json`](#clanclothing): Clothing with group-specific textures.
- [`Groups.json`](#groups): Server groups (data managed by the mod; edit with the server stopped).
- [`ClientSettings.json`](#clientsettings): The player's own settings (also editable in the menu, Settings tab).

## Server side

### ServerConfig

`ServerConfig.json` — All general settings: groups, levels, map, ping, compass, GPS, Info tab, chat, events, PvE / PvP zones, territory.

| Setting | Default | Explanation |
|---|---|---|
| `ConfigVersion` | `6` | File format version, managed by the mod: do not edit |
| `ConfigCommentsLanguage` | `"auto"` | Language of these comments: "fr", "en", "auto" (server language) or "" (no comments) |
| `GroupNameMinLength` | `3` | Minimum number of characters in a group name |
| `GroupNameMaxLength` | `24` | Maximum number of characters in a group name |
| `GroupTagMinLength` | `2` | Minimum number of characters in the group tag ([TAG]) |
| `GroupTagMaxLength` | `5` | Maximum number of characters in the group tag |
| `InactiveGroupDeleteDays` | `30` | Delete a group when no member has logged in for X days (0 = never) |
| `AllowTemporaryInvites` | `1` | 1 = allow "session" invites (the player leaves the group at the next server restart) |
| `InviteTimeoutSeconds` | `120` | How long an invitation stays valid, in seconds |
| `PositionUpdateInterval` | `2.0` | Seconds between two teammate position updates (lower = smoother, more network) |
| `RankNames` | `Recruit`, `Member`, `Officer`, `Leader` | Names of the 4 ranks, lowest to highest (rank 0 to 3) |
| `MinRankInvite` | `2` | Minimum rank to invite (0 recruit, 1 member, 2 officer, 3 leader) |
| `MinRankKick` | `2` | Minimum rank to kick a lower-ranked member |
| `MinRankPromote` | `2` | Minimum rank to promote / demote |
| `MinRankMarkers` | `1` | Minimum rank to place group markers |
| `MinRankSubGroups` | `2` | Minimum rank to create / delete subgroups |
| `MinRankUpgrade` | `3` | Minimum rank to upgrade the group level |
| `AllowPlayerUpgrade` | `1` | 1 = players can raise the group level by paying UpgradeCost; 0 = admins only |
| `Levels` | _list_ | Group levels: the first block is level 0 (starting level), then 1, 2... |
| `Levels[].MaxMembers` | `3` | Maximum number of members at this level |
| `Levels[].MaxMarkers` | `10` | Maximum number of group markers at this level |
| `Levels[].MaxSubGroups` | `1` | Maximum number of subgroups at this level |
| `Levels[].UpgradeCost` | `[]` | Items to carry (consumed) to reach this level |
| `AllowPrivateMarkers` | `1` | 1 = players can place private markers (stored on their PC) |
| `AllowGroupMarkers` | `1` | 1 = groups can place shared markers |
| `MaxPrivateMarkers` | `50` | Maximum number of private markers per player |
| `ShowMembersOnMap` | `1` | 1 = show teammates on the map |
| `ShowMembers3D` | `1` | 1 = show teammates in 3D in the world |
| `ShowOfflineMembers` | `1` | 1 = show the last position of offline teammates |
| `Member3DMaxDistance` | `5000.0` | Max distance (m) for 3D teammate display |
| `Offline3DMaxDistance` | `20.0` | Max distance (m) for 3D display of offline teammates |
| `Marker3DMaxDistance` | `15000.0` | Max distance (m) for 3D marker display |
| `AllowDragMarkers` | `1` | 1 = markers can be dragged with the mouse on the map |
| `EnablePing` | `1` | 1 = enable the ping (middle click) shown to the group |
| `PingDurationSeconds` | `30.0` | How long a ping stays visible, in seconds |
| `PingMaxDistance` | `2000.0` | Max distance (m) you can ping at |
| `EnableCompass` | `1` | 1 = enable the compass at the top of the screen (key H) |
| `CompassRequiresItem` | `0` | 1 = the compass only shows if the player carries an item from CompassItems |
| `CompassItems` | `Compass`, `OrienteeringCompass`, `GPSReceiver` | Items that give the compass (if CompassRequiresItem = 1) |
| `EnableGPS` | `1` | 1 = enable the GPS minimap (key B) |
| `GPSRequiresItem` | `0` | 1 = the GPS only shows if the player carries an item from GPSItems |
| `GPSItems` | `GPSReceiver` | Items that give the GPS (if GPSRequiresItem = 1) |
| `GPSInVehicles` | `1` | 1 = being in a vehicle gives the GPS even without an item |
| `GPSOnlyInVehicle` | `0` | 1 = the GPS only works in vehicles |
| `GPSShowCoordinates` | `1` | 1 = show coordinates on the GPS |
| `GPSShowDirection` | `1` | 1 = show the heading on the GPS |
| `GPSShowSpeed` | `1` | 1 = show the speed on the GPS |
| `ServerName` | `"TLB Server"` | Server name shown at the top of the menu |
| `LogoPath` | `"TLB_MapsAndGroups/gui/icons/logo.paa"` | Menu logo: path to a .paa / .edds image (can come from another mod) |
| `ServerTimeLocation` | `"Paris"` | Label shown next to the server time (e.g. Paris) |
| `InfoButtons` | _list_ | Buttons of the Info tab (6 maximum) |
| `InfoButtons[].Text` | `"Discord"` | Button text (empty = hidden) |
| `InfoButtons[].SubText` | `"discord.gg/xxxx"` | Small text under the button |
| `InfoButtons[].URL` | `"https://discord.gg/"` | Link opened on click (https://...) |
| `EnableGlobalChat` | `1` | 1 = enable the global chat channel |
| `EnableGroupChat` | `1` | 1 = enable the group chat channel |
| `DefaultChannel` | `1` | Default chat channel: 0 direct, 1 global, 2 group |
| `ShowGroupTagInChat` | `1` | 1 = show the group [TAG] before names in chat |
| `CensorEnabled` | `0` | 1 = replace words from BadWords with **** |
| `BadWords` | `badword1`, `badword2` | Censored words (case-insensitive) |
| `ChatMaxLength` | `200` | Maximum message length |
| `ChatCooldownMs` | `800` | Minimum delay between two messages from a player, in milliseconds (anti-spam) |
| `GlobalChatColor` | `-1` (#FFFFFFFF) | Global chat color |
| `GroupChatColor` | `-10027162` (#FF66FF66) | Group chat color |
| `DirectChatColor` | `-2236963` (#FFDDDDDD) | Direct chat color |
| `ServerMessageColor` | `-13312` (#FFFFCC00) | Server message color |
| `ChatRoles` | _list_ | Chat roles (admin, VIP...): prefix and color for some SteamIDs |
| `ChatRoles[].Name` | `"Admin"` | Internal role name |
| `ChatRoles[].Prefix` | `"[Admin]"` | Text shown before the name (e.g. [ADMIN]) |
| `ChatRoles[].Color` | `-48060` (#FFFF4444) | Message color for this role |
| `ChatRoles[].Highlight` | `1` | 1 = message highlighted with a background |
| `ChatRoles[].SteamIds` | `[]` | SteamID64 of the players with this role |
| `EventMarkersEnabled` | `1` | 1 = automatically show events (airdrops, crashes...) on the map |
| `ImportExpansionMarkers` | `1` | 1 = copy event markers from the Expansion mod if loaded |
| `ImportExpansionStaticMarkers` | `0` | 1 = also copy Expansion static markers (traders...) |
| `ExpansionEventPvPRadius` | `200.0` | Radius (m) of the PvP zone around imported Expansion events (0 = no zone) |
| `EventMarkers` | _list_ | Objects shown on the map when they spawn |
| `EventMarkers[].Enabled` | `1` | 1 = rule enabled |
| `EventMarkers[].ClassName` | `"ExpansionAirdropContainerBase"` | Object class (inherited classes count too) |
| `EventMarkers[].Name` | `"Airdrop"` | Marker name (empty = object name) |
| `EventMarkers[].Icon` | `"box"` | Marker icon (see the list in the README) |
| `EventMarkers[].Color` | `-29670` (#FFFF8C1A) | Marker color |
| `EventMarkers[].DurationMinutes` | `0` | Display time in minutes (0 = as long as the object exists) |
| `EventMarkers[].Inaccuracy` | `0.0` | Random offset in meters to hide the exact position (0 = exact) |
| `EventMarkers[].PvPRadius` | `200.0` | Radius (m) of the PvP zone around the event, used when ZoneDefaultMode = 1 (0 = no zone) |
| `ZoneDefaultMode` | `0` | Rule outside drawn zones: 0 = PvP everywhere, 1 = PvE everywhere (then draw PvP zones) |
| `PvEBlockIfEitherInside` | `1` | 1 = no damage if the attacker OR the victim is in PvE; 0 = only the victim position counts |
| `PvEProtectVehicles` | `1` | 1 = players cannot damage vehicles in PvE zones |
| `PvEProtectBases` | `1` | 1 = players cannot damage buildings in PvE zones |
| `PvEBlockRestrain` | `1` | 1 = players cannot be restrained in PvE zones |
| `ForceFirstPersonInPvP` | `0` | 1 = first-person view forced in PvP zones (third person comes back when leaving) |
| `SafeZoneRemoveZombies` | `1` | 1 = infected entering a safe zone are removed |
| `SafeZoneRemoveAnimals` | `0` | 1 = animals entering a safe zone are removed |
| `SafeZoneRemoveAI` | `0` | 1 = AI (Expansion AI, AI Patrols...) entering a safe zone are removed; 0 = they stay but deal no damage |
| `UnlimitedStamina` | `0` | Unlimited stamina: 0 = off (vanilla), 1 = everywhere, 2 = only in PvE zones |
| `PvEBlockedDamageSources` | `Grenade_Base`, `ExplosivesBase`, `ClaymoreMine`, `LandMineTrap`, `TrapBase` | Explosives and traps whose damage is also blocked in PvE (the owner is unknown) |
| `TerritoryEnabled` | `1` | 1 = enable territory rules around flags |
| `TerritoryRadius` | `60.0` | Territory radius around a flag, in meters |
| `TerritoryHeight` | `50.0` | Territory height (m) above and below the pole |
| `MinRankBuild` | `0` | Minimum rank to build on your group territory |
| `HostileCannotBuild` | `1` | 1 = forbid building on another group territory |
| `RequireFlagToBuild` | `0` | 1 = a flag of your group is required to build |
| `InstantFlagBuild` | `0` | 1 = the flag pole builds itself when the kit is placed |
| `AutoAttachFlag` | `1` | 1 = a flag is attached to the pole automatically |
| `AutoFlagClassName` | `"Flag_DayZ"` | Class of the automatically attached flag |
| `AutoFlagMarker` | `1` | 1 = create a "Territory" group marker on the flag |
| `AdminBypassBuild` | `1` | 1 = admins with the BypassBuild permission ignore build rules (except flags in no build zones) |
| `MaxFlagsPerGroup` | `1` | Maximum number of flags per group (0 = unlimited) |
| `BuildExceptions` | `Plastic_Explosive`, `ClaymoreMine`, `LandMineTrap`, `ImprovisedExplosive` | Items always allowed, even on enemy territory (explosives...) |
| `RestrictedItems` | `KitBase`, `TentBase`, `CodeLock`, `CombinationLock` | Items subject to territory and no build rules (inherited classes included) |
| `RaidHoursEnabled` | `0` | 1 = raid hours: outside the RaidWindows slots, bases take no damage and only members of the flag's group can destroy a part or dial a lock |
| `RaidAnnounce` | `1` | 1 = chat message when raid time starts and ends |
| `RaidProtectedItems` | `TentBase`, `CombinationLock` | Items protected outside raid hours, on top of base building (inherited classes too) |
| `RaidWindows` | _list_ | Time slots when raids are allowed, in the server's local time |
| `RaidWindows[].Days` | `5`, `6`, `7` | Days of the slot: 1 = Monday, 2 = Tuesday ... 7 = Sunday |
| `RaidWindows[].Start` | `"18:00"` | Start time "HH:MM" |
| `RaidWindows[].End` | `"23:00"` | End time "HH:MM" (earlier than the start = ends the next day; same as the start = whole day) |
| `MapBuildingsEnabled` | `1` | 1 = building icons (wells, police stations...) on the players' map |
| `MapRoadsEnabled` | `1` | 1 = roads drawn over the satellite map (found together with the buildings) |
| `MapBuildings` | _list_ | Building types shown on the map. The server finds them on first start and keeps the result in MapBuildings_<map>.json (searched again if this list changes) |
| `MapBuildings[].Enabled` | `1` | 1 = type shown |
| `MapBuildings[].Name` | `"Water well"` | Type name (for your reference) |
| `MapBuildings[].Icon` | `"map_fountain"` | Icon (see the list in the README) |
| `MapBuildings[].Color` | `-11549705` (#FF4FC3F7) | Icon color |
| `MapBuildings[].Classes` | `Land_Misc_Well_Pump_Yellow`, `Land_Misc_Well_Pump_Blue` | Building classes (inherited classes count too) |
| `DeathMarkerMinutes` | `60` | Private marker placed where the player died, for X minutes (0 = off) |

### Permissions

`Permissions.json` — The mod admins and their permissions.

| Setting | Default | Explanation |
|---|---|---|
| `Admins` | _list_ | List of the mod admins |
| `Admins[].SteamId` | `"76561190000000000"` | Admin SteamID64 (17 digits) |
| `Admins[].Name` | `"Example admin (replace the SteamID)"` | Name for your reference (not used by the game) |
| `Admins[].Permissions` | `*` | "*" = everything; otherwise: ViewGroups, EditGroups, DeleteGroups, JoinGroups, ServerMarkers, NoBuildZones, Mute, BypassBuild, ViewFlags |

### NoBuildZones

`NoBuildZones.json` — No build, PvE and PvP zones. Easier to edit in game (menu, Zones tab).

| Setting | Default | Explanation |
|---|---|---|
| `Enabled` | `1` | 1 = no build zones enabled |
| `PvPRules` | `1` | 1 = PvE / PvP zones enabled |
| `Zones` | _list_ | Drawn zones (easier to edit in game: menu > Zones tab) |
| `Zones[].Name` | `""` | Zone name (shown on the map) |
| `Zones[].Type` | `0` | 0 = no build, 1 = PvE, 2 = PvP, 3 = safe (no damage, zombies removed) |
| `Zones[].Form` | `0` | 0 = circle, 1 = polygon |
| `Zones[].Pos` | `0`, `0`, `0` | Zone center [x, height, z] |
| `Zones[].Radius` | `300` | Radius in meters (circle only) |
| `Zones[].Points` | `[]` | Polygon vertices [x, 0, z] (polygon only) |

### ServerMarkers

`ServerMarkers.json` — Permanent markers visible to everyone (also editable in game by admins).

| Setting | Default | Explanation |
|---|---|---|
| `Markers` | _list_ | Permanent markers visible to everyone (also editable in game by admins) |
| `Markers[].Name` | `"Trader"` | Displayed name |
| `Markers[].Icon` | `"shop"` | Icon (see the list in the README) |
| `Markers[].Color` | `-1` (#FFFFFFFF) | Marker color |
| `Markers[].Pos` | `0`, `0`, `0` | Position [x, height, z] |

### Webhooks

`Webhooks.json` — Sending events to Discord.

| Setting | Default | Explanation |
|---|---|---|
| `Enabled` | `0` | 1 = send events to Discord (webhooks) |
| `DefaultURL` | `""` | Webhook URL used when an event URL is empty |
| `GroupCreated` | `""` | Webhook: group created (empty = DefaultURL) |
| `GroupDeleted` | `""` | Webhook: group deleted |
| `MemberJoined` | `""` | Webhook: a player joins a group |
| `MemberLeft` | `""` | Webhook: a player leaves a group |
| `MemberKicked` | `""` | Webhook: a player is kicked |
| `RankChanged` | `""` | Webhook: rank change |
| `GroupLevel` | `""` | Webhook: group level change |
| `GlobalChat` | `""` | Webhook: global chat messages |
| `GroupChat` | `""` | Webhook: group chat messages |
| `AdminAction` | `""` | Webhook: admin actions |
| `FlagPlaced` | `""` | Webhook: flag placed |
| `MarkerCreated` | `""` | Webhook: server marker created |
| `DisabledEvents` | `[]` | Events never sent (e.g. "GlobalChat") |

### ClanClothing

`ClanClothing.json` — Clothing with group-specific textures.

| Setting | Default | Explanation |
|---|---|---|
| `Enabled` | `0` | 1 = enable clan clothing (group-specific textures) |
| `Sets` | _list_ | One block per group |
| `Sets[].GroupTag` | `"TAG"` | Tag of the group |
| `Sets[].Items` | _list_ | Retextured clothes for this group |
| `Sets[].Items[].ClassName` | `"TShirt_White"` | Clothing class |
| `Sets[].Items[].Textures` | `MyClanMod/data/tshirt_tag_co.paa` | One texture per hiddenSelection, in order (empty = unchanged) |

### Groups

`Groups.json` — Server groups (data managed by the mod; edit with the server stopped).

| Setting | Default | Explanation |
|---|---|---|
| `NextGroupId` | `2` | Next group number (managed by the mod) |
| `Groups` | _list_ | Server groups (edit with the server stopped, or use the admin menu) |
| `Groups[].Id` | `1` | Unique group number |
| `Groups[].Name` | `"Example"` | Group name |
| `Groups[].Tag` | `"EX"` | Group tag |
| `Groups[].Level` | `0` | Group level (index in ServerConfig Levels) |
| `Groups[].Created` | `0` | Creation date (seconds since 2000) |
| `Groups[].LastActive` | `0` | Last activity (seconds since 2000) |
| `Groups[].HasTagColor` | `0` (#00000000) | 1 = the tag has a custom chat color |
| `Groups[].TagColor` | `-1` (#FFFFFFFF) | Tag color in chat |
| `Groups[].NextMarkerId` | `2` | Next marker number (managed by the mod) |
| `Groups[].Members` | _list_ | Group members |
| `Groups[].Members[].SteamId` | `"7656119..."` | Member SteamID64 |
| `Groups[].Members[].Name` | `"Player"` | Player name |
| `Groups[].Members[].Rank` | `3` | Rank: 0 recruit, 1 member, 2 officer, 3 leader |
| `Groups[].Members[].SubGroup` | `0` | Subgroup number (index in SubGroups) |
| `Groups[].Members[].Temporary` | `0` | 1 = session-only member (removed at restart) |
| `Groups[].Members[].LastPos` | `0`, `0`, `0` | Last known position |
| `Groups[].Members[].LastSeen` | `0` | Last login (seconds since 2000) |
| `Groups[].Members[].PingIcon` | `"ping"` | Player ping icon |
| `Groups[].SubGroups` | `Alpha` | Subgroup names |
| `Groups[].Markers` | _list_ | Group markers |
| `Groups[].Markers[].Id` | `1` | Marker number (managed by the mod) |
| `Groups[].Markers[].Name` | `"Base"` | Displayed name |
| `Groups[].Markers[].Icon` | `"house"` | Icon (see the list in the README) |
| `Groups[].Markers[].Color` | `-1` (#FFFFFFFF) | Marker color |
| `Groups[].Markers[].Pos` | `0`, `0`, `0` | Position [x, height, z] |
| `Groups[].Markers[].Type` | `2` | 0 server, 1 temporary server, 2 group, 3 private |
| `Groups[].Markers[].Creator` | `"Player"` | Player who created the marker |
| `Groups[].Markers[].PvPRadius` | `0` | Radius (m) of the PvP zone around the marker (0 = none, managed by the mod) |

## Player side (in each player's game profile)

### ClientSettings

`ClientSettings.json` — The player's own settings (also editable in the menu, Settings tab).

| Setting | Default | Explanation |
|---|---|---|
| `Version` | `0` | File version, managed by the mod: do not edit |
| `ColPlayer3D` | `-10040065` (#FF66CCFF) | 3D teammate marker color |
| `ColOwnMarker` | `-49088` (#FFFF4040) | Color of your own position on the map |
| `ColPlayerOnline` | `-12264124` (#FF44DD44) | Online teammates color |
| `ColPlayerOffline` | `-2276284` (#FFDD4444) | Offline teammates color |
| `ColPing` | `-8960` (#FFFFDD00) | Ping color |
| `ColCompass` | `-1` (#FFFFFFFF) | Compass color |
| `ColCompassLine` | `-8960` (#FFFFDD00) | Compass center line color |
| `ColListFull` | `-12264124` (#FF44DD44) | Team list: full health color |
| `ColListZero` | `-2285022` (#FFDD2222) | Team list: zero health color |
| `ColListBorder` | `-16777216` (#FF000000) | Team list: border color |
| `PlayerListX` | `0.012` | Team list horizontal position (0 = left, 1 = right) |
| `PlayerListY` | `0.3` | Team list vertical position (0 = top, 1 = bottom) |
| `MinimapX` | `0.835` | GPS horizontal position (0 to 1) |
| `MinimapY` | `0.015` | GPS vertical position (0 to 1) |
| `ChatX` | `0.005` | Chat horizontal position (0 to 1) |
| `ChatY` | `0.7` | Chat vertical position (0 to 1) |
| `ShowPlayerList` | `1` | 1 = show the team list (key U) |
| `ShowCompass` | `1` | 1 = show the compass (key H) |
| `ShowGPS` | `1` | 1 = show the GPS (key B) |
| `ShowTagInChat` | `1` | 1 = show group tags in chat |
| `UseClanTextures` | `1` | 1 = show clan clothing |
| `StreamerMode` | `0` | 1 = hide names and coordinates |
| `ShowNoBuildZones` | `1` | 1 = show zones on the map |
| `ShowSatellite` | `1` | 1 = satellite map; 0 = vanilla map |
| `ShowZoneIndicator` | `1` | 1 = show the PvE / PvP indicator at the top of the screen |
| `ShowMapBuildings` | `1` | 1 = building icons (wells, police stations...) on the satellite map |
| `ShowMapRoads` | `1` | 1 = roads drawn on the satellite map |
| `PlayerListStyle` | `0` | Team list style: 0 normal, 1 compact |
| `GPSSize` | `1` | GPS size: 0 small, 1 normal, 2 large |
| `GPSZoom` | `0.15` | GPS zoom (small = close) |
| `MarkerPosition` | `0` | Height of 3D teammate markers: 0 head, 1 stomach, 2 feet |
| `Marker3DIcon` | `1` | 1 = icon on 3D markers |
| `Marker3DName` | `1` | 1 = name on 3D markers |
| `Marker3DDistance` | `1` | 1 = distance on 3D markers |
| `Player3DName` | `1` | 1 = teammate names shown above their head |
| `PingIcon` | `"ping"` | Icon of your pings |
| `PingIconSize` | `24` | Ping icon size |
| `PlayerIcon` | `"person"` | Teammate icon on the map |
| `PlayerIconSize` | `16` | Teammate icon size |
| `ChatTextSize` | `15` | Chat text size |
| `Preset3D` | `0` | 3D markers (key K): 0 all, 1 no server, 2 private + group, 3 private, 4 none |
| `ChatChannelIdx` | `-1` | Last chat channel used (-1 = server default) |
| `HiddenCategories` | `[]` | Categories hidden on the map |
| `Hidden3DCategories` | `[]` | Categories hidden in 3D |
| `HiddenMarkers` | `[]` | Markers hidden on the map |
| `Hidden3DMarkers` | `[]` | Markers hidden in 3D |
| `CollapsedCategories` | `[]` | Collapsed categories in the marker list |

## Admin permissions (`Permissions.json`)

`*`, `ViewGroups`, `EditGroups`, `DeleteGroups`, `JoinGroups`, `ServerMarkers`, `NoBuildZones`, `Mute`, `BypassBuild`, `ViewFlags`

## Webhook events (`Webhooks.json`)

`GroupCreated`, `GroupDeleted`, `MemberJoined`, `MemberLeft`, `MemberKicked`, `RankChanged`, `GroupLevel`, `GlobalChat`, `GroupChat`, `AdminAction`, `FlagPlaced`, `MarkerCreated`

## Available icons

`pin`, `circle`, `ring`, `square`, `triangle`, `diamond`, `star`, `flag`, `house`, `tent`, `castle`, `skull`, `radiation`, `medical`, `car`, `heli`, `boat`, `crosshair`, `shield`, `money`, `shop`, `tower`, `bolt`, `fire`, `water`, `tree`, `mountain`, `question`, `exclamation`, `eye`, `box`, `wrench`, `food`, `ping`, `person`, `arrow`, `gun`, `tag`
