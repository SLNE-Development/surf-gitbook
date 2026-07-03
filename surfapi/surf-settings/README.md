---
description: >-
  surf-settings bietet eine typsichere API zur Verwaltung spielerspezifischer
  Einstellungen mit persistenter Datenbankspeicherung und integriertem GUI.
icon: sliders
---

# surf-settings

surf-settings ist ein Plugin-Modul, das eine typsichere und erweiterbare Einstellungsverwaltung für Spieler bereitstellt. Einstellungen werden persistent in einer Datenbank gespeichert und können über ein integriertes GUI oder direkt über die API abgerufen und gesetzt werden.

### Übersicht

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>🔑 <strong>SettingKey</strong></td><td>Typsichere Schlüssel für Einstellungen definieren</td><td><a href="setting-key.md">setting-key.md</a></td></tr><tr><td>📋 <strong>SettingKeys</strong></td><td>Alle vordefinierten Einstellungsschlüssel im Überblick</td><td><a href="setting-keys.md">setting-keys.md</a></td></tr><tr><td>⚙️ <strong>SurfSettingsApi</strong></td><td>Einstellungen lesen, schreiben und das GUI öffnen</td><td><a href="surf-settings-api.md">surf-settings-api.md</a></td></tr></tbody></table>

### Installation

Füge surf-settings als Abhängigkeit in deiner `build.gradle.kts` hinzu:

```kotlin
dependencies {
    compileOnly("dev.slne.surf:surf-settings-api:+")
}
```

{% hint style="info" %}
surf-settings setzt surf-core voraus. Stelle sicher, dass das Plugin auf dem Server läuft, bevor du es als Abhängigkeit verwendest.
{% endhint %}

### Externe Links

* [GitHub – surf-settings API](https://github.com/SLNE-Development/surf-settings/tree/version/26.1/surf-settings-api)
