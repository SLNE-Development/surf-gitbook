---
description: >-
  surf-settings bietet eine typsichere API zur Verwaltung spielerspezifischer
  Einstellungen mit persistenter Datenbankspeicherung und integriertem GUI.
icon: sliders
---

# surf-settings

surf-settings ist ein Feature von surf-player, das eine typsichere und erweiterbare Einstellungsverwaltung für Spieler bereitstellt. Einstellungen werden persistent in einer Datenbank gespeichert und können über ein integriertes GUI oder direkt über die API abgerufen und gesetzt werden.

### Übersicht

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>🔑 <strong>SettingKey</strong></td><td>Typsichere Schlüssel für Einstellungen definieren</td><td><a href="setting-key.md">setting-key.md</a></td></tr><tr><td>📋 <strong>SettingKeys</strong></td><td>Alle vordefinierten Einstellungsschlüssel im Überblick</td><td><a href="setting-keys.md">setting-keys.md</a></td></tr><tr><td>⚙️ <strong>SurfSettingsApi</strong></td><td>Einstellungen lesen, schreiben und das GUI öffnen</td><td><a href="surf-settings-api.md">surf-settings-api.md</a></td></tr></tbody></table>

### Installation

surf-settings ist Teil von **surf-player** (Monorepo [SLNE-Development/surf-player](https://github.com/SLNE-Development/surf-player)). Füge `surf-player-api` als Abhängigkeit in deiner `build.gradle.kts` hinzu:

```kotlin
repositories {
    maven("https://reposilite.slne.dev/releases")
}

dependencies {
    compileOnly("dev.slne.surf.player:surf-player-api:1.0.3")
}
```

Das Artefakt `dev.slne.surf:surf-settings-api` existiert nicht mehr. Die Klassen liegen jetzt im Paket `dev.slne.surf.player.api.settings`.

{% hint style="info" %}
Das Plugin deiner Plattform muss auf dem Server laufen, bevor du es als Abhängigkeit verwendest: `surf-player-paper`, `surf-player-minestom` oder `surf-player-velocity`. Einstellungen sind auf allen drei Plattformen verfügbar. Die Einrichtung ist unter [Installation von surf-chat](../../surf-chat/getting-started/installation.md) beschrieben.
{% endhint %}

### Befehle

Spieler öffnen das Menü mit `/settings`, setzen Werte mit `/settings toggle <einstellung> [an|aus]` und zeigen Infos mit `/settings info <einstellung>` an. Admins nutzen `/surfplayer settings refresh|list` (Velocity: `/surfplayervelocity settings ...`; früher `/surfSettings`).

### Externe Links

* [GitHub – Settings-API](https://github.com/SLNE-Development/surf-player/tree/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/settings)
* [GitHub – Migration von Befehlen, Permissions und API](https://github.com/SLNE-Development/surf-player/blob/version/26.3/docs/command-migration.md)
