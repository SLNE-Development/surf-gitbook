---
description: >-
  NPCs werden über eine Kotlin-DSL oder direkt über SurfNpcApi erstellt.
  Dieser Abschnitt erklärt alle Parameter und zeigt praxisnahe Beispiele.
icon: hammer
---

# NPC erstellen

## Kotlin-DSL (empfohlen)

Die einfachste Methode, einen NPC zu erstellen, ist die `npc { }` DSL-Funktion:

```kotlin
import dev.slne.surf.npc.api.dsl.npc
import dev.slne.surf.npc.api.dsl.fetchedSkin
import dev.slne.surf.npc.api.npc.rotation.NpcRotationType
import org.bukkit.entity.EntityType

val myNpc = npc {
    uniqueName = "shop-npc"
    type       = EntityType.PLAYER

    displayName {
        primary("Shop")
    }

    location {
        world("world")
        x(100.0)
        y(64.0)
        z(200.0)
        yaw(0f)
        pitch(0f)
    }

    skin = fetchedSkin("Notch")   // Skin eines Spielers laden (suspend)

    rotationType = NpcRotationType.PER_PLAYER
    persistent   = true
}
```

### DSL-Eigenschaften

| Eigenschaft                      | Typ                 | Pflicht | Beschreibung                                                                |
| -------------------------------- | ------------------- | ------- | --------------------------------------------------------------------------- |
| `uniqueName`                     | `String`            | ✅       | Eindeutiger interner Name des NPCs                                          |
| `type`                           | `EntityType`        | ✅       | Entity-Typ (z. B. `PLAYER`, `VILLAGER`)                                     |
| `displayName { }`                | `SurfComponentBuilder` | ✅    | Angezeigter Name über dem NPC                                               |
| `location { }`                   | `LocationBuilder`   | ✅       | Position des NPCs in der Welt                                               |
| `skin`                           | `NpcSkin`           | ✅       | Skin-Daten (nur relevant für `PLAYER`-Typ)                                  |
| `rotationType`                   | `NpcRotationType`   | ❌       | `PER_PLAYER` (dreht sich zum Spieler) oder `FIXED` (feste Rotation)         |
| `persistent`                     | `Boolean`           | ❌       | Ob der NPC nach Server-Neustart bestehen bleibt (Standard: `false`)         |
| `scale`                          | `Double`            | ❌       | Skalierungsfaktor des NPCs (Standard: `1.0`)                                |
| `useTransparentNametagBackground`| `Boolean`           | ❌       | Nametag-Hintergrund transparent darstellen (Standard: `false`)              |
| `viewers`                        | `ObjectSet<UUID>?`  | ❌       | Eingeschränkte Betrachter; `null` = alle Spieler                             |
| `glowing`                        | `Boolean`           | ❌       | Leuchteffekt aktivieren (Standard: `false`)                                 |
| `glowingColor`                   | `NamedTextColor`    | ❌       | Farbe des Leuchteffekts (Standard: `WHITE`)                                 |

## Direkte API-Nutzung

Alternativ kann `SurfNpcApi.createNpc(...)` direkt verwendet werden:

```kotlin
import dev.slne.surf.npc.api.SurfNpcApi
import dev.slne.surf.npc.api.npc.rotation.NpcRotationType
import dev.slne.surf.npc.api.npc.skin.NpcSkin

val npc = SurfNpcApi.createNpc(
    displayName             = Component.text("Händler"),
    uniqueName              = "händler-npc",
    type                    = EntityType.PLAYER,
    useTransparentBackground= false,
    location                = Location(world, 0.0, 64.0, 0.0),
    rotationType            = NpcRotationType.PER_PLAYER,
    persistent              = false,
    skin                    = NpcSkin.empty()
)
```

{% hint style="info" %}
`NpcSkin.empty()` verwendet einen Standard-Skin. Für echte Spieler-Skins nutze `SurfNpcApi.fetchSkin(username)` (suspend) oder `fetchedSkin(username)` in der DSL.
{% endhint %}

## NPC verwalten

```kotlin
// NPC anzeigen / ausblenden
myNpc.show()
myNpc.hide()

// NPC zu einem Spieler teleportieren (Spieler zu NPC)
myNpc.teleport(player)

// Anzeigename ändern
myNpc.setDisplayName(Component.text("Neuer Name"))

// Position ändern
myNpc.setLocation(newLocation)

// Skalierung setzen
myNpc.setScale(1.5)

// NPC speichern (Persistenz)
myNpc.save()

// NPC löschen
myNpc.delete()
```

## NPC abrufen

```kotlin
// Über die eindeutige ID
val npc: Npc? = SurfNpcApi.getNpc(id = 42)

// Über den unique Name
val npc: Npc? = SurfNpcApi.getNpc(uniqueName = "shop-npc")

// Alle NPCs abrufen
val allNpcs: ObjectSet<Npc> = SurfNpcApi.getNpcs()
```

## Betrachter steuern

Standardmäßig sehen alle Spieler einen NPC. Mit `viewers` kann die Sichtbarkeit eingeschränkt werden:

```kotlin
// Betrachter hinzufügen
myNpc.addViewer(player.uniqueId)

// Betrachter entfernen
myNpc.removeViewer(player.uniqueId)

// Prüfen
val sieht = myNpc.hasViewer(player.uniqueId)

// Alle Betrachter entfernen
myNpc.clearViewers()

// Über alle Betrachter iterieren
myNpc.forEachViewer { uuid -> /* ... */ }
```

## Ausrüstung setzen

```kotlin
import org.bukkit.inventory.EquipmentSlot

myNpc.setEquipment(EquipmentSlot.HEAD, ItemStack(Material.DIAMOND_HELMET))
myNpc.setEquipment(EquipmentSlot.HAND, ItemStack(Material.IRON_SWORD))
```

## Externe Links

* [GitHub – SurfNpcApi.kt](https://github.com/SLNE-Development/surf-npc/blob/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/SurfNpcApi.kt)
* [GitHub – NpcDslBuilder.kt](https://github.com/SLNE-Development/surf-npc/blob/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/dsl/NpcDslBuilder.kt)
* [GitHub – Npc.kt](https://github.com/SLNE-Development/surf-npc/blob/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/npc/Npc.kt)
