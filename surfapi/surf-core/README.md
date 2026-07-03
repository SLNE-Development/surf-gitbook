---
description: >-
  surf-core ist die zentrale Infrastruktur-Bibliothek des Surf-Netzwerks. Sie
  stellt netzwerkweite Spielerverwaltung, Serververwaltung und ein plattformübergreifendes
  Event-System bereit.
icon: server
---

# surf-core

surf-core ist das Herzstück des Surf-Netzwerks. Es stellt grundlegende Dienste bereit, die von allen anderen Modulen genutzt werden: netzwerkweite Spieler- und Serververwaltung, plattformübergreifende Events und Spielerweiterleitung zwischen Backend-Servern.

### Übersicht

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>🌐 <strong>SurfCoreApi</strong></td><td>Spieler, Server und Events netzwerkweit verwalten</td><td><a href="surf-core-api.md">surf-core-api.md</a></td></tr><tr><td>👤 <strong>SurfPlayer</strong></td><td>Netzwerkweite Spieler-Repräsentation</td><td><a href="surf-player.md">surf-player.md</a></td></tr></tbody></table>

### Installation

surf-core wird über das surf-api-Gradle-Plugin automatisch als `compileOnly`-Abhängigkeit eingebunden (wenn `dev.slne.surf.api.gradle.core` oder `paper-plugin` verwendet wird). Eine manuelle Abhängigkeit ist in der Regel nicht nötig.

Für explizite Abhängigkeit:

```kotlin
dependencies {
    compileOnly("dev.slne.surf:surf-core-api-common:+")
}
```

### Externe Links

* [GitHub – surf-core API](https://github.com/SLNE-Development/surf-core/tree/version/26.2/surf-core-api)
