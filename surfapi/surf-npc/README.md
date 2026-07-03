---
description: >-
  surf-npc ist eine vollständige NPC-API für Paper mit Kotlin-DSL, Skin-Support,
  Rotationssteuerung, Properties und einem Event-System.
icon: person-walking
---

# surf-npc

surf-npc ermöglicht die Erstellung und Verwaltung von NPCs (Non-Player Characters) auf Paper-Servern. Die API bietet eine Kotlin-DSL für bequeme NPC-Erstellung, typsichere Properties, Skin-Verwaltung, Rotationstypen, Posen und ein integriertes Event-System.

{% hint style="warning" %}
surf-npc ist nur auf der **Paper/Bukkit**-Plattform verfügbar.
{% endhint %}

### Übersicht

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>🏗️ <strong>NPC erstellen</strong></td><td>NPCs mit der Kotlin-DSL anlegen und verwalten</td><td><a href="npc-erstellen.md">npc-erstellen.md</a></td></tr><tr><td>🎨 <strong>Skin & Aussehen</strong></td><td>Spieler-Skins laden und Posen setzen</td><td><a href="skin-und-pose.md">skin-und-pose.md</a></td></tr><tr><td>🔧 <strong>Properties</strong></td><td>Typsichere NPC-Eigenschaften verwalten</td><td><a href="properties.md">properties.md</a></td></tr><tr><td>📡 <strong>Events</strong></td><td>Auf NPC-Interaktionen und Ereignisse reagieren</td><td><a href="events.md">events.md</a></td></tr></tbody></table>

### Installation

```kotlin
dependencies {
    compileOnly("dev.slne.surf:surf-npc-api:+")
}
```

### Externe Links

* [GitHub – surf-npc API](https://github.com/SLNE-Development/surf-npc/tree/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api)
