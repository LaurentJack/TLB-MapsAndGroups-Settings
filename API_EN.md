# TLB Maps And Groups — API for event mods

[Version française](API.md)

Event mods (KOTH, airdrops, AI missions…) can show their markers on our map. The API works like the one of other
map mods: create a marker, keep the returned object to update it, then remove it. Everything runs **server side**;
players receive the changes within 3 seconds.

The mod declares the define `TLB_MAPSANDGROUPS`, so your integration only compiles when it is loaded:

```c
#ifdef TLB_MAPSANDGROUPS
TLBMG_ServerMarker m_Marker;

void CreateMarker(vector pos)
{
	m_Marker = TLBMG_MarkerManager.Get().AddTempServerMarker("[KOTH] Tisy", pos, "flag", ARGB(255, 255, 255, 0));
	if (m_Marker)
		m_Marker.SetRadius(40, ARGB(90, 255, 255, 0), true);	// 40 m hatched circle
}

void OnCaptureStarted()
{
	if (m_Marker)
		m_Marker.SetColor(ARGB(255, 0, 255, 0));
}

void RemoveMarker()
{
	TLBMG_MarkerManager.Get().RemoveServerMarker(m_Marker);
	m_Marker = null;
}
#endif
```

`TLBMG_MarkerManager.Get()`:

| Function | Purpose |
|---|---|
| `AddTempServerMarker(name, position, icon, color, editable = false, show3D = true, showMap = true, showGPS = true)` | creates a marker visible to everyone, returns a `TLBMG_ServerMarker` (null outside the server or before the mod has started). `editable` is ignored: only admins can edit server markers |
| `RemoveServerMarker(marker)` / `RemoveServerMarkerByUid(uid)` | removes the marker (returns false if it no longer exists) |
| `GetServerMarker(uid)`, `GetServerMarkers()`, `Count()`, `RemoveAllServerMarkers()` | access to the markers created through the API |
| `IsReady()` (static) | true once the mod has started on the server |

`TLBMG_ServerMarker`: field `uid`, `IsValid()`, `GetName/GetPosition/GetIcon/GetColor/GetRadius()`, and
`SetName`, `SetPosition`, `SetIcon`, `SetColor`, `SetRadius(radius, color = 0, hatched = false)` (visual circle,
color 0 = marker color), `SetPvPRadius(radius)` (PvP zone, follows `ZoneDefaultMode`), `SetShow3D(bool)`,
`SetShowMap(bool)`. `Set…` calls can be chained: `m.SetColor(c).SetName("…")`.

Icon: one of the mod's icon names (`flag`, `skull`, `box`, `heli`, `crosshair`…), a vanilla map icon
(`map_police`…) or the path of a `.paa` image from your mod (`MyMod\gui\icon.paa`). An unknown name becomes `exclamation`.

`TLBMG_GroupManager.Get()` (groups, by Steam ID): `HasGroup`, `GetPlayersGroupName`, `GetPlayersGroupTag`,
`GetPlayersGroupId` (-1 without a group), `AreInSameGroup(a, b)`, `GetGroupMembers` (list of Steam IDs).

API markers are removed when the server restarts: the event mod must create them again.
