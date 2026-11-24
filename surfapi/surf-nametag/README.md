---
description: >-
  surf-nametag steuert die Namensschilder (Nametags) von Spielern auf
  Packet-Ebene. Die API ist Teil von surf-player.
icon: tag
---

# surf-nametag

surf-nametag ist Teil von **surf-player** (Monorepo [SLNE-Development/surf-player](https://github.com/SLNE-Development/surf-player)). Die API stellt Funktionen bereit, um die Namensschilder von Spielern zu steuern: Aktualisierungen anhalten und die Sichtbarkeit für alle Betrachter überschreiben.

{% hint style="warning" %}
Die frühere API für einzelne Betrachter (`showNametag`, `hideNametag`, `setNametag`, `resetNametag`) und das Artefakt `dev.slne.surf:surf-nametag-api` gibt es nicht mehr. Nametags sind auf **Paper** und **Minestom** verfügbar, nicht auf Velocity.
{% endhint %}

## Installation

```kotlin
repositories {
    maven("https://reposilite.slne.dev/releases")
}

dependencies {
    compileOnly("dev.slne.surf.player:surf-player-api:1.0.3")
}
```

Damit das Plugin vor deinem geladen wird, deklariere eine Abhängigkeit auf `surf-player-paper` (Paper) bzw. `surf-player-minestom` (Minestom). Die Einrichtung ist unter [Installation von surf-chat](../../surf-chat/getting-started/installation.md) beschrieben und gilt für alle Features von surf-player.

## API-Übersicht

Die API ist über das Companion-Objekt von `SurfNametagApi` direkt erreichbar:

```kotlin
import dev.slne.surf.player.api.nametag.SurfNametagApi
import dev.slne.surf.player.api.nametag.nametag.NametagVisibility
```

Alles, was einen einzelnen Spieler betrifft, ist zusätzlich über die `NametagCapability` erreichbar (`SurfPlayerApi.player(uuid)`).

### Aktualisierungen anhalten

```kotlin
fun preventNametagUpdates(playerUuid: UUID)
fun allowNametagUpdates(playerUuid: UUID)
```

`preventNametagUpdates` hält die Aktualisierung des Namensschilds von `playerUuid` an, bis `allowNametagUpdates` aufgerufen wird. Beide Funktionen geben `Unit` zurück (früher `Boolean`).

### Sichtbarkeit überschreiben

```kotlin
fun setNametagVisibility(playerUuid: UUID, visibility: NametagVisibility)
fun resetNametagVisibility(playerUuid: UUID)
```

`setNametagVisibility` zeigt das Namensschild von `playerUuid` für **alle** Betrachter mit der angegebenen Sichtbarkeit an, statt der konfigurierten. Betrachter, die das Schild schon sehen, werden sofort aktualisiert. Die Überschreibung gilt bis `resetNametagVisibility` aufgerufen wird oder der Spieler den Server verlässt.

| `NametagVisibility`    | Bedeutung                                           |
| ---------------------- | --------------------------------------------------- |
| `ALWAYS`               | Das Namensschild ist immer sichtbar                 |
| `NEVER`                | Das Namensschild ist nie sichtbar                   |
| `HIDE_FOR_OTHER_TEAMS` | Für Spieler anderer Teams ausgeblendet              |
| `HIDE_FOR_OWN_TEAM`    | Für Spieler des eigenen Teams ausgeblendet          |

## Beispiel

```kotlin
val player = SurfPlayerApi.player(targetUuid)

// Namensschild für alle ausblenden
player.setNametagVisibility(NametagVisibility.NEVER)

// Zurück zur konfigurierten Sichtbarkeit
player.resetNametagVisibility()

// Dasselbe ohne SurfCommunityPlayer
SurfNametagApi.setNametagVisibility(targetUuid, NametagVisibility.NEVER)
SurfNametagApi.resetNametagVisibility(targetUuid)
```

{% hint style="info" %}
Spieler können fremde Namensschilder über die Einstellung `SettingKeys.SHOW_NAMETAGS` (`nametag:show-nametags`) selbst ausblenden. Diese Einstellung wird jetzt tatsächlich ausgewertet; zuvor wurde sie von surf-nametag nie gelesen. Die Einstellungen sind unter [surf-settings](../surf-settings/README.md) beschrieben.
{% endhint %}

## Befehle

Das Plugin kann mit `/surfplayer nametag reload` neu geladen werden (Permission `surf.player.admin.nametag.reload`).

## Externe Links

* [GitHub – surf-player Repository](https://github.com/SLNE-Development/surf-player)
* [GitHub – Nametag-API](https://github.com/SLNE-Development/surf-player/tree/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/nametag)
* [GitHub – SurfNametagApi.kt](https://github.com/SLNE-Development/surf-player/blob/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/nametag/SurfNametagApi.kt)
