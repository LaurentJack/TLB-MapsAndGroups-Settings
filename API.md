# TLB Maps And Groups — API pour les mods d'événements

[English version](API_EN.md)

Les mods d'événements (KOTH, airdrops, missions IA…) peuvent afficher leurs marqueurs sur notre carte. L'API suit
la même logique que celle des autres mods de carte : on crée un marqueur, on garde l'objet renvoyé pour le modifier,
puis on le retire. Tout se fait **côté serveur** ; les joueurs reçoivent les changements en moins de 3 secondes.

Le mod déclare le define `TLB_MAPSANDGROUPS`, pour que l'intégration ne compile que s'il est chargé :

```c
#ifdef TLB_MAPSANDGROUPS
TLBMG_ServerMarker m_Marker;

void CreateMarker(vector pos)
{
	m_Marker = TLBMG_MarkerManager.Get().AddTempServerMarker("[KOTH] Tisy", pos, "flag", ARGB(255, 255, 255, 0));
	if (m_Marker)
		m_Marker.SetRadius(40, ARGB(90, 255, 255, 0), true);	// cercle hachuré de 40 m
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

`TLBMG_MarkerManager.Get()` :

| Fonction | Rôle |
|---|---|
| `AddTempServerMarker(nom, position, icône, couleur, editable = false, show3D = true, showMap = true, showGPS = true)` | crée un marqueur visible par tous, renvoie un `TLBMG_ServerMarker` (null hors serveur ou avant le démarrage du mod). `editable` est ignoré : seuls les admins modifient les marqueurs serveur |
| `RemoveServerMarker(marqueur)` / `RemoveServerMarkerByUid(uid)` | retire le marqueur (renvoie false s'il n'existe plus) |
| `GetServerMarker(uid)`, `GetServerMarkers()`, `Count()`, `RemoveAllServerMarkers()` | accès aux marqueurs créés par l'API |
| `IsReady()` (statique) | vrai une fois le mod démarré sur le serveur |

`TLBMG_ServerMarker` : champ `uid`, `IsValid()`, `GetName/GetPosition/GetIcon/GetColor/GetRadius()`, et
`SetName`, `SetPosition`, `SetIcon`, `SetColor`, `SetRadius(rayon, couleur = 0, hachuré = false)` (cercle visuel,
couleur 0 = celle du marqueur), `SetPvPRadius(rayon)` (zone PvP, suit `ZoneDefaultMode`), `SetShow3D(bool)`,
`SetShowMap(bool)`. Les `Set…` s'enchaînent : `m.SetColor(c).SetName("…")`.

Icône : un nom d'icône du mod (`flag`, `skull`, `box`, `heli`, `crosshair`…), une icône de la carte vanilla
(`map_police`…) ou le chemin d'une image `.paa` de ton mod (`MonMod\gui\icone.paa`). Un nom inconnu devient `exclamation`.

`TLBMG_GroupManager.Get()` (groupes, par Steam ID) : `HasGroup`, `GetPlayersGroupName`, `GetPlayersGroupTag`,
`GetPlayersGroupId` (-1 sans groupe), `AreInSameGroup(a, b)`, `GetGroupMembers` (liste de Steam ID).

Les marqueurs de l'API disparaissent au redémarrage du serveur : le mod d'événement doit les recréer.

