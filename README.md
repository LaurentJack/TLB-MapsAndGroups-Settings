# TLB Maps And Groups : réglages

[English version](SETTINGS_EN.md)

Ce document décrit tous les réglages du mod **TLB Maps And Groups** pour DayZ.

Les fichiers du serveur sont dans `<dossier profil du serveur>/TLB_MapsAndGroups/`. Ils sont créés au premier
démarrage avec les valeurs par défaut ci-dessous, puis réécrits à chaque démarrage pour ajouter les nouveaux réglages.

**À savoir avant de modifier un fichier**

- Chaque ligne des fichiers porte un commentaire (`// ...`) qui reprend l'explication de ce document.
  Le mod ignore ces commentaires et les réécrit : ne modifiez que les valeurs.
- Langue des commentaires : `ConfigCommentsLanguage` dans `ServerConfig.json` (`"fr"`, `"en"`, `"auto"`, ou `""` pour aucun).
- Oui / non : `1` = oui, `0` = non.
- Couleurs : nombre ARGB en décimal signé. Sa valeur hexadécimale est indiquée entre parenthèses ici et entre
  crochets dans les fichiers (`#FFFF0000` = rouge opaque).
- Rangs : `0` recrue, `1` membre, `2` officier, `3` chef.
- Si un fichier contient une erreur de syntaxe, le serveur l'indique dans son log, utilise les valeurs par défaut et
  ne réécrit pas le fichier, pour que vous puissiez le corriger.
- Pour une liste (`[]`), les champs de chaque élément sont décrits sous la forme `Liste[].Champ`.

## Fichiers

- [`ServerConfig.json`](#serverconfig) : Tous les réglages généraux : groupes, niveaux, carte, ping, boussole, GPS, onglet Infos, chat, événements, zones PvE / PvP, territoire.
- [`Permissions.json`](#permissions) : Les admins du mod et leurs permissions.
- [`NoBuildZones.json`](#nobuildzones) : Zones de construction interdite, PvE et PvP. Plus simple à modifier en jeu (menu, onglet Zones).
- [`ServerMarkers.json`](#servermarkers) : Marqueurs permanents visibles par tous (aussi modifiables en jeu par les admins).
- [`Webhooks.json`](#webhooks) : Envoi des événements vers Discord.
- [`ClanClothing.json`](#clanclothing) : Vêtements aux textures propres à un groupe.
- [`Groups.json`](#groups) : Les groupes du serveur (données gérées par le mod ; à modifier serveur arrêté).
- [`ClientSettings.json`](#clientsettings) : Réglages personnels du joueur (aussi modifiables dans le menu, onglet Réglages).

## Côté serveur

### ServerConfig

`ServerConfig.json` — Tous les réglages généraux : groupes, niveaux, carte, ping, boussole, GPS, onglet Infos, chat, événements, zones PvE / PvP, territoire.

| Réglage | Par défaut | Explication |
|---|---|---|
| `ConfigVersion` | `6` | Version du format de ce fichier, gérée par le mod : ne pas modifier |
| `ConfigCommentsLanguage` | `"auto"` | Langue de ces commentaires : "fr", "en", "auto" (langue du serveur) ou "" (aucun commentaire) |
| `GroupNameMinLength` | `3` | Nombre minimum de caractères du nom de groupe |
| `GroupNameMaxLength` | `24` | Nombre maximum de caractères du nom de groupe |
| `GroupTagMinLength` | `2` | Nombre minimum de caractères du tag (abréviation affichée [TAG]) |
| `GroupTagMaxLength` | `5` | Nombre maximum de caractères du tag |
| `InactiveGroupDeleteDays` | `30` | Supprime un groupe dont aucun membre ne s'est connecté depuis X jours (0 = jamais) |
| `AllowTemporaryInvites` | `1` | 1 = autorise les invitations "session" (le joueur quitte le groupe au redémarrage du serveur) |
| `InviteTimeoutSeconds` | `120` | Durée de validité d'une invitation, en secondes |
| `PositionUpdateInterval` | `2.0` | Secondes entre deux envois des positions des coéquipiers (plus bas = plus fluide, plus de réseau) |
| `RankNames` | `Recruit`, `Member`, `Officer`, `Leader` | Noms des 4 rangs, du plus bas au plus haut (rang 0 à 3) |
| `MinRankInvite` | `2` | Rang minimum pour inviter (0 recrue, 1 membre, 2 officier, 3 chef) |
| `MinRankKick` | `2` | Rang minimum pour exclure un membre de rang inférieur |
| `MinRankPromote` | `2` | Rang minimum pour promouvoir / rétrograder |
| `MinRankMarkers` | `1` | Rang minimum pour poser des marqueurs de groupe |
| `MinRankSubGroups` | `2` | Rang minimum pour créer / supprimer des sous-groupes |
| `MinRankUpgrade` | `3` | Rang minimum pour améliorer le niveau du groupe |
| `AllowPlayerUpgrade` | `1` | 1 = les joueurs peuvent monter le niveau du groupe en payant UpgradeCost ; 0 = seulement les admins |
| `Levels` | _liste_ | Niveaux de groupe : le premier bloc est le niveau 0 (niveau de départ), puis 1, 2... |
| `Levels[].MaxMembers` | `3` | Nombre maximum de membres à ce niveau |
| `Levels[].MaxMarkers` | `10` | Nombre maximum de marqueurs de groupe à ce niveau |
| `Levels[].MaxSubGroups` | `1` | Nombre maximum de sous-groupes à ce niveau |
| `Levels[].UpgradeCost` | `[]` (vide) | Objets à avoir sur soi (consommés) pour atteindre ce niveau |
| `AllowPrivateMarkers` | `1` | 1 = les joueurs peuvent poser des marqueurs privés (stockés chez eux) |
| `AllowGroupMarkers` | `1` | 1 = les groupes peuvent poser des marqueurs partagés |
| `MaxPrivateMarkers` | `50` | Nombre maximum de marqueurs privés par joueur |
| `ShowMembersOnMap` | `1` | 1 = affiche les coéquipiers sur la carte |
| `ShowMembers3D` | `1` | 1 = affiche les coéquipiers en 3D dans le monde |
| `ShowOfflineMembers` | `1` | 1 = affiche la dernière position des coéquipiers déconnectés |
| `Member3DMaxDistance` | `5000.0` | Distance max (m) d'affichage 3D des coéquipiers |
| `Offline3DMaxDistance` | `20.0` | Distance max (m) d'affichage 3D des coéquipiers déconnectés |
| `Marker3DMaxDistance` | `15000.0` | Distance max (m) d'affichage 3D des marqueurs |
| `AllowDragMarkers` | `1` | 1 = les marqueurs peuvent être déplacés à la souris sur la carte |
| `EnablePing` | `1` | 1 = active le ping (clic molette) visible par le groupe |
| `PingDurationSeconds` | `30.0` | Durée d'affichage d'un ping, en secondes |
| `PingMaxDistance` | `2000.0` | Distance max (m) à laquelle on peut pinger |
| `EnableCompass` | `1` | 1 = active la boussole en haut de l'écran (touche H) |
| `CompassRequiresItem` | `0` | 1 = la boussole ne s'affiche que si le joueur porte un objet de CompassItems |
| `CompassItems` | `Compass`, `OrienteeringCompass`, `GPSReceiver` | Objets qui donnent la boussole (si CompassRequiresItem = 1) |
| `EnableGPS` | `1` | 1 = active la minicarte GPS (touche B) |
| `GPSRequiresItem` | `0` | 1 = le GPS ne s'affiche que si le joueur porte un objet de GPSItems |
| `GPSItems` | `GPSReceiver` | Objets qui donnent le GPS (si GPSRequiresItem = 1) |
| `GPSInVehicles` | `1` | 1 = un véhicule donne le GPS même sans objet |
| `GPSOnlyInVehicle` | `0` | 1 = le GPS ne marche qu'en véhicule |
| `GPSShowCoordinates` | `1` | 1 = affiche les coordonnées sur le GPS |
| `GPSShowDirection` | `1` | 1 = affiche l'orientation sur le GPS |
| `GPSShowSpeed` | `1` | 1 = affiche la vitesse sur le GPS |
| `ServerName` | `"TLB Server"` | Nom du serveur affiché en haut du menu |
| `LogoPath` | `"TLB_MapsAndGroups/gui/icons/logo.paa"` | Logo du menu : chemin d'une image .paa / .edds (peut venir d'un autre mod) |
| `ServerTimeLocation` | `"Paris"` | Texte affiché à côté de l'heure du serveur (ex. Paris) |
| `InfoButtons` | _liste_ | Boutons de l'onglet Infos (6 maximum) |
| `InfoButtons[].Text` | `"Discord"` | Texte du bouton (vide = bouton masqué) |
| `InfoButtons[].SubText` | `"discord.gg/xxxx"` | Petit texte sous le bouton |
| `InfoButtons[].URL` | `"https://discord.gg/"` | Lien ouvert au clic (https://...) |
| `EnableGlobalChat` | `1` | 1 = active le canal de chat global |
| `EnableGroupChat` | `1` | 1 = active le canal de chat de groupe |
| `DefaultChannel` | `1` | Canal de chat par défaut : 0 proximité, 1 global, 2 groupe |
| `ShowGroupTagInChat` | `1` | 1 = affiche le [TAG] du groupe devant le nom dans le chat |
| `CensorEnabled` | `0` | 1 = remplace les mots de BadWords par des **** |
| `BadWords` | `badword1`, `badword2` | Mots censurés (sans tenir compte des majuscules) |
| `ChatMaxLength` | `200` | Longueur maximum d'un message |
| `ChatCooldownMs` | `800` | Délai minimum entre deux messages d'un joueur, en millisecondes (anti-spam) |
| `GlobalChatColor` | `-1` (#FFFFFFFF) | Couleur du chat global |
| `GroupChatColor` | `-10027162` (#FF66FF66) | Couleur du chat de groupe |
| `DirectChatColor` | `-2236963` (#FFDDDDDD) | Couleur du chat de proximité |
| `ServerMessageColor` | `-13312` (#FFFFCC00) | Couleur des messages du serveur |
| `ChatRoles` | _liste_ | Rôles du chat (admin, VIP...) : préfixe et couleur pour certains SteamID |
| `ChatRoles[].Name` | `"Admin"` | Nom interne du rôle |
| `ChatRoles[].Prefix` | `"[Admin]"` | Texte affiché devant le nom (ex. [ADMIN]) |
| `ChatRoles[].Color` | `-48060` (#FFFF4444) | Couleur des messages de ce rôle |
| `ChatRoles[].Highlight` | `1` | 1 = message mis en évidence par un fond |
| `ChatRoles[].SteamIds` | `[]` (vide) | SteamID64 des joueurs qui ont ce rôle |
| `EventMarkersEnabled` | `1` | 1 = affiche automatiquement les événements (airdrops, épaves...) sur la carte |
| `ImportExpansionMarkers` | `1` | 1 = recopie les marqueurs d'événements du mod Expansion s'il est chargé |
| `ImportExpansionStaticMarkers` | `0` | 1 = recopie aussi les marqueurs fixes d'Expansion (traders...) |
| `ExpansionEventPvPRadius` | `200.0` | Rayon (m) de la zone PvP autour des événements Expansion importés (0 = pas de zone) |
| `EventMarkers` | _liste_ | Objets à signaler sur la carte quand ils apparaissent |
| `EventMarkers[].Enabled` | `1` | 1 = règle active |
| `EventMarkers[].ClassName` | `"ExpansionAirdropContainerBase"` | Classe de l'objet (les classes héritées comptent aussi) |
| `EventMarkers[].Name` | `"Airdrop"` | Nom du marqueur (vide = nom de l'objet) |
| `EventMarkers[].Icon` | `"box"` | Icône du marqueur (voir la liste dans le README) |
| `EventMarkers[].Color` | `-29670` (#FFFF8C1A) | Couleur du marqueur |
| `EventMarkers[].DurationMinutes` | `0` | Durée d'affichage en minutes (0 = tant que l'objet existe) |
| `EventMarkers[].Inaccuracy` | `0.0` | Décalage aléatoire en mètres pour ne pas donner la position exacte (0 = exacte) |
| `EventMarkers[].PvPRadius` | `200.0` | Rayon (m) de la zone PvP autour de l'événement, active quand ZoneDefaultMode = 1 (0 = pas de zone) |
| `ZoneDefaultMode` | `0` | Règle hors des zones tracées : 0 = PvP partout, 1 = PvE partout (on trace alors les zones PvP) |
| `PvEBlockIfEitherInside` | `1` | 1 = pas de dégâts si l'attaquant OU la victime est en PvE ; 0 = seulement la position de la victime compte |
| `PvEProtectVehicles` | `1` | 1 = les joueurs ne peuvent pas abîmer les véhicules en zone PvE |
| `PvEProtectBases` | `1` | 1 = les joueurs ne peuvent pas abîmer les constructions en zone PvE |
| `PvEBlockRestrain` | `1` | 1 = impossible de menotter un joueur en zone PvE |
| `ForceFirstPersonInPvP` | `0` | 1 = vue 1re personne obligatoire dans les zones PvP (la 3e personne revient en sortant) |
| `SafeZoneRemoveZombies` | `1` | 1 = les zombies qui entrent dans une zone safe sont supprimés |
| `SafeZoneRemoveAnimals` | `0` | 1 = les animaux qui entrent dans une zone safe sont supprimés |
| `SafeZoneRemoveAI` | `0` | 1 = les IA (Expansion AI, AI Patrols...) qui entrent dans une zone safe sont supprimées ; 0 = elles restent mais ne font aucun dégât |
| `AutorunEnabled` | `1` | 1 = les joueurs peuvent utiliser l'autorun : une touche (= par défaut, modifiable dans les contrôles) fait avancer le personnage seul |
| `UnlimitedStamina` | `0` | Stamina illimitee : 0 = non (vanilla), 1 = partout, 2 = seulement en zone PvE |
| `PvEBlockedDamageSources` | `Grenade_Base`, `ExplosivesBase`, `ClaymoreMine`, `LandMineTrap`, `TrapBase` | Explosifs et pièges dont les dégâts sont aussi bloqués en PvE (l'auteur est inconnu) |
| `TerritoryEnabled` | `1` | 1 = active les règles de territoire autour des drapeaux |
| `TerritoryRadius` | `60.0` | Rayon du territoire autour d'un drapeau, en mètres |
| `TerritoryHeight` | `50.0` | Hauteur (m) du territoire au-dessus et en dessous du mât |
| `MinRankBuild` | `0` | Rang minimum pour construire sur le territoire de son groupe |
| `HostileCannotBuild` | `1` | 1 = interdit de construire sur le territoire d'un autre groupe |
| `RequireFlagToBuild` | `0` | 1 = il faut un drapeau de son groupe pour construire |
| `InstantFlagBuild` | `0` | 1 = le mât du drapeau se construit tout seul à la pose du kit |
| `AutoAttachFlag` | `1` | 1 = un drapeau est accroché automatiquement au mât |
| `AutoFlagClassName` | `"Flag_DayZ"` | Classe du drapeau accroché automatiquement |
| `AutoFlagMarker` | `1` | 1 = crée un marqueur de groupe "Territory" sur le drapeau |
| `AdminBypassBuild` | `1` | 1 = les admins avec la permission BypassBuild ignorent les règles de construction (sauf drapeau en zone interdite) |
| `MaxFlagsPerGroup` | `1` | Nombre maximum de drapeaux par groupe (0 = illimité) |
| `BuildExceptions` | `Plastic_Explosive`, `ClaymoreMine`, `LandMineTrap`, `ImprovisedExplosive` | Objets toujours autorisés, même en territoire ennemi (explosifs...) |
| `RestrictedItems` | `KitBase`, `TentBase`, `CodeLock`, `CombinationLock` | Objets soumis aux règles de territoire et de zones interdites (héritiers compris) |
| `RaidHoursEnabled` | `0` | 1 = horaires de raid : hors des créneaux de RaidWindows, les bases ne prennent aucun dégât et seuls les membres du groupe du drapeau peuvent détruire une pièce ou composer un cadenas |
| `RaidAnnounce` | `1` | 1 = message dans le chat quand les raids commencent et se terminent |
| `RaidProtectedItems` | `TentBase`, `CombinationLock` | Objets protégés hors des horaires de raid, en plus des constructions (héritiers compris) |
| `RaidWindows` | _liste_ | Créneaux où les raids sont permis, à l'heure locale du serveur |
| `RaidWindows[].Days` | `5`, `6`, `7` | Jours du créneau : 1 = lundi, 2 = mardi ... 7 = dimanche |
| `RaidWindows[].Start` | `"18:00"` | Heure de début "HH:MM" |
| `RaidWindows[].End` | `"23:00"` | Heure de fin "HH:MM" (plus tôt que le début = finit le lendemain ; égale au début = toute la journée) |
| `MapBuildingsEnabled` | `1` | 1 = icônes des bâtiments (puits, commissariats...) sur la carte des joueurs |
| `MapRoadsEnabled` | `1` | 1 = routes tracées par-dessus la carte satellite (trouvées en même temps que les bâtiments) |
| `MapBuildings` | _liste_ | Types de bâtiments affichés sur la carte. Le serveur les cherche au premier démarrage et garde le résultat dans MapBuildings_<carte>.json (recherche refaite si cette liste change) |
| `MapBuildings[].Enabled` | `1` | 1 = type affiché |
| `MapBuildings[].Name` | `"Water well"` | Nom du type (pour vous y retrouver) |
| `MapBuildings[].Icon` | `"map_fountain"` | Icône (voir la liste dans le README) |
| `MapBuildings[].Color` | `-11549705` (#FF4FC3F7) | Couleur de l'icône |
| `MapBuildings[].Classes` | `Land_Misc_Well_Pump_Yellow`, `Land_Misc_Well_Pump_Blue` | Classes des bâtiments (les classes héritées comptent aussi) |
| `DeathMarkerMinutes` | `60` | Marqueur privé posé à l'endroit de la mort du joueur, pendant X minutes (0 = désactivé) |

### Permissions

`Permissions.json` — Les admins du mod et leurs permissions.

| Réglage | Par défaut | Explication |
|---|---|---|
| `Admins` | _liste_ | Liste des admins du mod |
| `Admins[].SteamId` | `"76561190000000000"` | SteamID64 de l'admin (17 chiffres) |
| `Admins[].Name` | `"Example admin (replace the SteamID)"` | Nom pour vous repérer (non utilisé par le jeu) |
| `Admins[].Permissions` | `*` | "*" = tout ; sinon : ViewGroups, EditGroups, DeleteGroups, JoinGroups, ServerMarkers, NoBuildZones, Mute, BypassBuild, ViewFlags |

### NoBuildZones

`NoBuildZones.json` — Zones de construction interdite, PvE et PvP. Plus simple à modifier en jeu (menu, onglet Zones).

| Réglage | Par défaut | Explication |
|---|---|---|
| `Enabled` | `1` | 1 = zones de construction interdite actives |
| `PvPRules` | `1` | 1 = zones PvE / PvP actives |
| `Zones` | _liste_ | Zones tracées (plus simple à éditer en jeu : menu > onglet Zones) |
| `Zones[].Name` | `""` | Nom de la zone (affiché sur la carte) |
| `Zones[].Type` | `0` | 0 = construction interdite, 1 = PvE, 2 = PvP, 3 = safe (aucun dégât, zombies supprimés) |
| `Zones[].Form` | `0` | 0 = cercle, 1 = polygone |
| `Zones[].Pos` | `0`, `0`, `0` | Centre de la zone [x, hauteur, z] |
| `Zones[].Radius` | `300` | Rayon en mètres (cercle seulement) |
| `Zones[].Points` | `[]` (vide) | Sommets du polygone [x, 0, z] (polygone seulement) |

### ServerMarkers

`ServerMarkers.json` — Marqueurs permanents visibles par tous (aussi modifiables en jeu par les admins).

| Réglage | Par défaut | Explication |
|---|---|---|
| `Markers` | _liste_ | Marqueurs permanents visibles par tous (aussi éditables en jeu par les admins) |
| `Markers[].Name` | `"Trader"` | Nom affiché |
| `Markers[].Icon` | `"shop"` | Icône (voir la liste dans le README) |
| `Markers[].Color` | `-1` (#FFFFFFFF) | Couleur du marqueur |
| `Markers[].Pos` | `0`, `0`, `0` | Position [x, hauteur, z] |

### Webhooks

`Webhooks.json` — Envoi des événements vers Discord.

| Réglage | Par défaut | Explication |
|---|---|---|
| `Enabled` | `0` | 1 = envoie les événements vers Discord (webhooks) |
| `DefaultURL` | `""` | URL de webhook utilisée quand celle d'un événement est vide |
| `GroupCreated` | `""` | Webhook : création d'un groupe (vide = DefaultURL) |
| `GroupDeleted` | `""` | Webhook : suppression d'un groupe |
| `MemberJoined` | `""` | Webhook : un joueur rejoint un groupe |
| `MemberLeft` | `""` | Webhook : un joueur quitte un groupe |
| `MemberKicked` | `""` | Webhook : un joueur est exclu |
| `RankChanged` | `""` | Webhook : changement de rang |
| `GroupLevel` | `""` | Webhook : changement de niveau d'un groupe |
| `GlobalChat` | `""` | Webhook : messages du chat global |
| `GroupChat` | `""` | Webhook : messages du chat de groupe |
| `AdminAction` | `""` | Webhook : actions des admins |
| `FlagPlaced` | `""` | Webhook : pose d'un drapeau |
| `MarkerCreated` | `""` | Webhook : création d'un marqueur serveur |
| `DisabledEvents` | `[]` (vide) | Événements à ne jamais envoyer (ex. "GlobalChat") |

### ClanClothing

`ClanClothing.json` — Vêtements aux textures propres à un groupe.

| Réglage | Par défaut | Explication |
|---|---|---|
| `Enabled` | `0` | 1 = active les vêtements de clan (textures propres à un groupe) |
| `Sets` | _liste_ | Un bloc par groupe |
| `Sets[].GroupTag` | `"TAG"` | Tag du groupe concerné |
| `Sets[].Items` | _liste_ | Vêtements retexturés pour ce groupe |
| `Sets[].Items[].ClassName` | `"TShirt_White"` | Classe du vêtement |
| `Sets[].Items[].Textures` | `MyClanMod/data/tshirt_tag_co.paa` | Une texture par hiddenSelection, dans l'ordre (vide = inchangée) |

### Groups

`Groups.json` — Les groupes du serveur (données gérées par le mod ; à modifier serveur arrêté).

| Réglage | Par défaut | Explication |
|---|---|---|
| `NextGroupId` | `2` | Prochain numéro de groupe (géré par le mod) |
| `Groups` | _liste_ | Groupes du serveur (à modifier serveur arrêté, ou via le menu admin) |
| `Groups[].Id` | `1` | Numéro unique du groupe |
| `Groups[].Name` | `"Example"` | Nom du groupe |
| `Groups[].Tag` | `"EX"` | Tag du groupe |
| `Groups[].Level` | `0` | Niveau du groupe (index dans Levels de ServerConfig) |
| `Groups[].Created` | `0` | Date de création (secondes depuis 2000) |
| `Groups[].LastActive` | `0` | Dernière activité (secondes depuis 2000) |
| `Groups[].HasTagColor` | `0` (#00000000) | 1 = le tag a une couleur personnalisée dans le chat |
| `Groups[].TagColor` | `-1` (#FFFFFFFF) | Couleur du tag dans le chat |
| `Groups[].NextMarkerId` | `2` | Prochain numéro de marqueur (géré par le mod) |
| `Groups[].Members` | _liste_ | Membres du groupe |
| `Groups[].Members[].SteamId` | `"7656119..."` | SteamID64 du membre |
| `Groups[].Members[].Name` | `"Player"` | Nom du joueur |
| `Groups[].Members[].Rank` | `3` | Rang : 0 recrue, 1 membre, 2 officier, 3 chef |
| `Groups[].Members[].SubGroup` | `0` | Numéro du sous-groupe (index dans SubGroups) |
| `Groups[].Members[].Temporary` | `0` | 1 = invité pour la session seulement (retiré au redémarrage) |
| `Groups[].Members[].LastPos` | `0`, `0`, `0` | Dernière position connue |
| `Groups[].Members[].LastSeen` | `0` | Dernière connexion (secondes depuis 2000) |
| `Groups[].Members[].PingIcon` | `"ping"` | Icône de ping du joueur |
| `Groups[].SubGroups` | `Alpha` | Noms des sous-groupes |
| `Groups[].Markers` | _liste_ | Marqueurs du groupe |
| `Groups[].Markers[].Id` | `1` | Numéro du marqueur (géré par le mod) |
| `Groups[].Markers[].Name` | `"Base"` | Nom affiché |
| `Groups[].Markers[].Icon` | `"house"` | Icône (voir la liste dans le README) |
| `Groups[].Markers[].Color` | `-1` (#FFFFFFFF) | Couleur du marqueur |
| `Groups[].Markers[].Pos` | `0`, `0`, `0` | Position [x, hauteur, z] |
| `Groups[].Markers[].Type` | `2` | 0 serveur, 1 serveur temporaire, 2 groupe, 3 privé |
| `Groups[].Markers[].Creator` | `"Player"` | Joueur qui a créé le marqueur |
| `Groups[].Markers[].PvPRadius` | `0` | Rayon (m) de la zone PvP autour du marqueur (0 = aucune, géré par le mod) |

## Côté joueur (dans le profil du jeu de chaque joueur)

### ClientSettings

`ClientSettings.json` — Réglages personnels du joueur (aussi modifiables dans le menu, onglet Réglages).

| Réglage | Par défaut | Explication |
|---|---|---|
| `Version` | `0` | Version du fichier, gérée par le mod : ne pas modifier |
| `ColPlayer3D` | `-10040065` (#FF66CCFF) | Couleur du marqueur 3D des coéquipiers |
| `ColOwnMarker` | `-49088` (#FFFF4040) | Couleur de votre position sur la carte |
| `ColPlayerOnline` | `-12264124` (#FF44DD44) | Couleur des coéquipiers connectés |
| `ColPlayerOffline` | `-2276284` (#FFDD4444) | Couleur des coéquipiers déconnectés |
| `ColPing` | `-8960` (#FFFFDD00) | Couleur des pings |
| `ColCompass` | `-1` (#FFFFFFFF) | Couleur de la boussole |
| `ColCompassLine` | `-8960` (#FFFFDD00) | Couleur du repère central de la boussole |
| `ColListFull` | `-12264124` (#FF44DD44) | Liste d'équipe : couleur santé pleine |
| `ColListZero` | `-2285022` (#FFDD2222) | Liste d'équipe : couleur santé nulle |
| `ColListBorder` | `-16777216` (#FF000000) | Liste d'équipe : couleur de la bordure |
| `PlayerListX` | `0.012` | Position horizontale de la liste d'équipe (0 = gauche, 1 = droite) |
| `PlayerListY` | `0.3` | Position verticale de la liste d'équipe (0 = haut, 1 = bas) |
| `MinimapX` | `0.835` | Position horizontale du GPS (0 à 1) |
| `MinimapY` | `0.015` | Position verticale du GPS (0 à 1) |
| `ChatX` | `0.005` | Position horizontale du chat (0 à 1) |
| `ChatY` | `0.7` | Position verticale du chat (0 à 1) |
| `ShowPlayerList` | `1` | 1 = affiche la liste d'équipe (touche U) |
| `ShowCompass` | `1` | 1 = affiche la boussole (touche H) |
| `ShowGPS` | `1` | 1 = affiche le GPS (touche B) |
| `ShowTagInChat` | `1` | 1 = affiche les tags de groupe dans le chat |
| `UseClanTextures` | `1` | 1 = affiche les vêtements de clan |
| `StreamerMode` | `0` | 1 = masque les noms et les coordonnées |
| `ShowNoBuildZones` | `1` | 1 = affiche les zones sur la carte |
| `ShowSatellite` | `1` | 1 = carte satellite ; 0 = carte vanilla |
| `ShowZoneIndicator` | `1` | 1 = affiche l'indicateur PvE / PvP en haut de l'écran |
| `ShowMapBuildings` | `1` | 1 = icônes des bâtiments (puits, commissariats...) sur la carte satellite |
| `ShowMapRoads` | `1` | 1 = routes tracées sur la carte satellite |
| `AutorunSpeed` | `2` | Vitesse de l'autorun : 1 = marche, 2 = course, 3 = sprint (Shift maintenu = sprint) |
| `PlayerListStyle` | `0` | Style de la liste d'équipe : 0 normal, 1 compact |
| `GPSSize` | `1` | Taille du GPS : 0 petit, 1 normal, 2 grand |
| `GPSZoom` | `0.15` | Zoom du GPS (petit = proche) |
| `MarkerPosition` | `0` | Hauteur du marqueur 3D des coéquipiers : 0 tête, 1 ventre, 2 pieds |
| `Marker3DIcon` | `1` | 1 = icône sur les marqueurs 3D |
| `Marker3DName` | `1` | 1 = nom sur les marqueurs 3D |
| `Marker3DDistance` | `1` | 1 = distance sur les marqueurs 3D |
| `Player3DName` | `1` | 1 = pseudo des coéquipiers affiché au-dessus de leur tête |
| `PingIcon` | `"ping"` | Icône de vos pings |
| `PingIconSize` | `24` | Taille de l'icône de ping |
| `PlayerIcon` | `"person"` | Icône des coéquipiers sur la carte |
| `PlayerIconSize` | `16` | Taille de l'icône des coéquipiers |
| `ChatTextSize` | `15` | Taille du texte du chat |
| `Preset3D` | `0` | Marqueurs 3D (touche K) : 0 tous, 1 sans serveur, 2 privés + groupe, 3 privés, 4 aucun |
| `ChatChannelIdx` | `-1` | Dernier canal de chat utilisé (-1 = canal par défaut du serveur) |
| `HiddenCategories` | `[]` (vide) | Catégories masquées sur la carte |
| `Hidden3DCategories` | `[]` (vide) | Catégories masquées en 3D |
| `HiddenMarkers` | `[]` (vide) | Marqueurs masqués sur la carte |
| `Hidden3DMarkers` | `[]` (vide) | Marqueurs masqués en 3D |
| `CollapsedCategories` | `[]` (vide) | Catégories repliées dans la liste des marqueurs |

## Permissions admin (`Permissions.json`)

`*`, `ViewGroups`, `EditGroups`, `DeleteGroups`, `JoinGroups`, `ServerMarkers`, `NoBuildZones`, `Mute`, `BypassBuild`, `ViewFlags`

## Évènements webhook (`Webhooks.json`)

`GroupCreated`, `GroupDeleted`, `MemberJoined`, `MemberLeft`, `MemberKicked`, `RankChanged`, `GroupLevel`, `GlobalChat`, `GroupChat`, `AdminAction`, `FlagPlaced`, `MarkerCreated`

## Icônes disponibles

`pin`, `circle`, `ring`, `square`, `triangle`, `diamond`, `star`, `flag`, `house`, `tent`, `castle`, `skull`, `radiation`, `medical`, `car`, `heli`, `boat`, `crosshair`, `shield`, `money`, `shop`, `tower`, `bolt`, `fire`, `water`, `tree`, `mountain`, `question`, `exclamation`, `eye`, `box`, `wrench`, `food`, `ping`, `person`, `arrow`, `gun`, `tag`
