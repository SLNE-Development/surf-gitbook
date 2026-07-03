---
description: >-
  surf-nametag ermöglicht die individuelle Steuerung von Namensschildern (Nametags)
  für Spieler auf Packet-Ebene – pro Betrachter steuerbar.
icon: tag
---

# surf-nametag

surf-nametag stellt eine schlanke API zur Verfügung, um Namensschilder von Spielern für einzelne Betrachter gezielt anzuzeigen, auszublenden oder mit individuellem Text zu überschreiben.

{% hint style="warning" %}
surf-nametag ist nur auf der **Paper/Bukkit**-Plattform verfügbar.
{% endhint %}

## Installation

```kotlin
dependencies {
    compileOnly("dev.slne.surf:surf-nametag-api:+")
}
```

## API-Übersicht

Die API ist über das Companion-Objekt von `SurfNametagApi` direkt erreichbar:

```kotlin
import dev.slne.surf.nametag.api.SurfNametagApi
```

### Nametag anzeigen

```kotlin
fun showNametag(player: UUID, viewer: UUID)
```

Zeigt das Namensschild von `player` für `viewer` an (falls es zuvor ausgeblendet wurde).

### Nametag ausblenden

```kotlin
fun hideNametag(player: UUID, viewer: UUID)
```

Blendet das Namensschild von `player` für `viewer` aus. Andere Spieler sehen das Schild weiterhin.

### Nametag überschreiben

```kotlin
fun setNametag(player: UUID, viewer: UUID, nametag: Component)
```

Ersetzt das Namensschild von `player` für `viewer` mit einem eigenen [Adventure-Component](https://jd.advntr.dev/api/4.17.0/net/kyori/adventure/text/Component.html).

```kotlin
SurfNametagApi.setNametag(
    player = player.uniqueId,
    viewer = viewer.uniqueId,
    nametag = Component.text("[Admin] ").color(NamedTextColor.RED)
        .append(Component.text(player.name))
)
```

### Nametag zurücksetzen

```kotlin
fun resetNametag(player: UUID, viewer: UUID)
```

Stellt das Standard-Namensschild von `player` für `viewer` wieder her.

## Vollständiges Beispiel

```kotlin
import dev.slne.surf.nametag.api.SurfNametagApi
import net.kyori.adventure.text.Component
import net.kyori.adventure.text.format.NamedTextColor

// Nametag für einen bestimmten Spieler ausblenden
SurfNametagApi.hideNametag(
    player = targetPlayer.uniqueId,
    viewer = observingPlayer.uniqueId
)

// Individuelles Nametag setzen (z. B. für eine Admin-Kennzeichnung)
SurfNametagApi.setNametag(
    player = targetPlayer.uniqueId,
    viewer = observingPlayer.uniqueId,
    nametag = Component.text("★ ").color(NamedTextColor.GOLD)
        .append(Component.text(targetPlayer.name).color(NamedTextColor.WHITE))
)

// Zurücksetzen auf Standard
SurfNametagApi.resetNametag(
    player = targetPlayer.uniqueId,
    viewer = observingPlayer.uniqueId
)
```

{% hint style="info" %}
Die Nametag-Sichtbarkeit kann auch über die Spielereinstellung `SettingKeys.SHOW_NAMETAGS` (surf-settings) gesteuert werden.
{% endhint %}

## Externe Links

* [GitHub – surf-nametag API](https://github.com/SLNE-Development/surf-nametag/tree/version/26.1/surf-nametag-api/src/main/kotlin/dev/slne/surf/nametag/api)
* [GitHub – SurfNametagApi.kt](https://github.com/SLNE-Development/surf-nametag/blob/version/26.1/surf-nametag-api/src/main/kotlin/dev/slne/surf/nametag/api/SurfNametagApi.kt)
