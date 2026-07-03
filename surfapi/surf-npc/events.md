---
description: >-
  Das NPC-Event-System erlaubt es, auf Interaktionen und Lebenszyklusereignisse
  von NPCs zu reagieren – entweder über Bukkit-Events oder direkt am NPC-Objekt.
icon: bell
---

# Events

surf-npc stellt zwei Wege zur Verfügung, um auf NPC-Ereignisse zu reagieren:

1. **Bukkit-Events** – globale Paper-Events, die für alle NPCs ausgelöst werden
2. **NPC-Event-Handler** – direkt am Npc-Objekt registrierte Handler für einen einzelnen NPC

---

## Bukkit-Events

Alle NPC-Events erben von `NpcEvent`. Spielerbezogene Events erben zusätzlich von `NpcPlayerEvent`.

### Übersicht der Events

| Event                | Beschreibung                                              |
| -------------------- | --------------------------------------------------------- |
| `NpcCreateEvent`     | Wird ausgelöst, wenn ein NPC erstellt wird                |
| `NpcDeleteEvent`     | Wird ausgelöst, wenn ein NPC gelöscht wird                |
| `NpcShowEvent`       | Wird ausgelöst, wenn ein NPC einem Spieler angezeigt wird |
| `NpcHideEvent`       | Wird ausgelöst, wenn ein NPC vor einem Spieler verborgen wird |
| `NpcInteractEvent`   | Wird ausgelöst, wenn ein Spieler mit einem NPC interagiert |
| `NpcCollisionEvent`  | Wird ausgelöst, wenn ein Spieler in die Nähe eines NPCs läuft |

### Beispiel: Interaktion abfangen

```kotlin
import dev.slne.surf.npc.api.event.NpcInteractEvent
import org.bukkit.event.EventHandler
import org.bukkit.event.Listener

class NpcListener : Listener {

    @EventHandler
    fun onInteract(event: NpcInteractEvent) {
        val npc    = event.npc
        val player = event.player

        player.sendMessage("Du hast mit ${npc.uniqueName} interagiert!")
    }
}
```

### Beispiel: NPC-Erstellung beobachten

```kotlin
@EventHandler
fun onNpcCreate(event: NpcCreateEvent) {
    val npc = event.npc
    println("Neuer NPC erstellt: ${npc.uniqueName} (ID: ${npc.id})")
}
```

---

## NPC-Event-Handler (lokal)

Event-Handler können direkt am Npc-Objekt registriert werden. Sie werden nur für diesen spezifischen NPC ausgelöst.

### Handler registrieren (reified)

```kotlin
import dev.slne.surf.npc.api.event.NpcInteractEvent

myNpc.addEventHandler<NpcInteractEvent> { event ->
    event.player.sendMessage("Willkommen beim Shop!")
}
```

### Handler registrieren (mit Klasse)

```kotlin
myNpc.addEventHandler(NpcInteractEvent::class) { event ->
    // Handler-Logik
}
```

### Handler entfernen

```kotlin
val handler: (NpcInteractEvent) -> Unit = { event -> /* ... */ }

myNpc.addEventHandler<NpcInteractEvent>(handler)

// Später entfernen
myNpc.removeEventHandler(NpcInteractEvent::class, handler)
```

### Handler in der DSL registrieren

Beim Erstellen eines NPCs können Handler direkt in der DSL angegeben werden:

```kotlin
val npc = npc {
    uniqueName = "quest-npc"
    // ... weitere Einstellungen ...

    withEventHandler<NpcInteractEvent> { event ->
        event.player.sendMessage("Starte deine Quest!")
    }
}
```

{% hint style="info" %}
Lokale Event-Handler werden **nur** für den NPC ausgelöst, an dem sie registriert sind. Für übergreifende Logik (alle NPCs) nutze Bukkit-Events.
{% endhint %}

## Externe Links

* [GitHub – Event-Verzeichnis](https://github.com/SLNE-Development/surf-npc/tree/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/event)
* [GitHub – NpcInteractEvent.kt](https://github.com/SLNE-Development/surf-npc/blob/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/event/NpcInteractEvent.kt)
