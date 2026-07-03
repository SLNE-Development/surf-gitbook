---
description: >-
  SurfSettingsApi ist die zentrale Schnittstelle zum Lesen, Schreiben und
  Verwalten von Spielereinstellungen.
icon: gear
---

# SurfSettingsApi

`SurfSettingsApi` ist das Hauptinterface von surf-settings. Es erlaubt den typsicheren Zugriff auf Spielereinstellungen, das persistente Speichern von Werten sowie das Öffnen des Einstellungs-GUIs.

## Zugriff

Die API ist über das Companion-Objekt direkt erreichbar:

```kotlin
import dev.slne.surf.settings.api.SurfSettingsApi

val value = SurfSettingsApi.getSettingValue(player.uniqueId, SettingKeys.CHAT_PINGS)
```

## Methoden

### Wert lesen (synchron, gecacht)

```kotlin
fun <T : Any> getSettingValue(playerUuid: UUID, key: SettingKey<T>): T
```

Gibt den aktuell gecachten Wert der Einstellung zurück. Ist kein Wert vorhanden, wird der `defaultValue` des [`SettingKey`](setting-key.md) zurückgegeben.

{% hint style="info" %}
Diese Methode ist synchron und greift nur auf den In-Memory-Cache zu. Für Offline-Spieler sollte `getCachedValueOrLoad` verwendet werden.
{% endhint %}

### Wert lesen (suspend, mit Fallback auf Datenbank)

```kotlin
suspend fun <T : Any> getCachedValueOrLoad(playerUuid: UUID, key: SettingKey<T>): T
```

Gibt den gecachten Wert zurück, oder lädt ihn aus der Datenbank, falls er nicht im Cache ist. Nützlich für Offline-Spieler.

### Wert speichern

```kotlin
suspend fun <T : Any> saveSetting(playerUuid: UUID, key: SettingKey<T>, value: T)
```

Speichert einen Einstellungswert persistent für einen Spieler.

```kotlin
// Beispiel: Chat-Pings für einen Spieler deaktivieren
SurfSettingsApi.saveSetting(player.uniqueId, SettingKeys.CHAT_PINGS, false)
```

{% hint style="warning" %}
`saveSetting` ist eine Suspend-Funktion und muss aus einem Coroutine-Kontext aufgerufen werden.
{% endhint %}

### Einstellung erstellen (registrieren)

```kotlin
suspend fun <T : Any> createSetting(key: SettingKey<T>): Setting
```

Registriert eine neue Einstellung anhand eines [`SettingKey`](setting-key.md). Muss beim Start des Plugins aufgerufen werden, bevor die Einstellung gelesen oder gesetzt wird.

```kotlin
// Beim Plugin-Start
SurfSettingsApi.createSetting(MY_CUSTOM_KEY)
```

### Einstellungsdefinition abrufen

```kotlin
fun <T : Any> getSetting(key: SettingKey<T>): Setting?
```

Gibt die `Setting`-Definition zurück, sofern sie registriert ist.

### Alle Einstellungen abrufen

```kotlin
fun getSettings(): ObjectSet<Setting>
```

Gibt alle registrierten `Setting`-Definitionen zurück.

### Einstellungs-GUI öffnen

```kotlin
fun openSettingsGui(playerUuid: UUID)
```

Öffnet das integrierte Einstellungs-GUI für den angegebenen Spieler.

```kotlin
// GUI für den anfragenden Spieler öffnen
SurfSettingsApi.openSettingsGui(player.uniqueId)
```

## Vollständiges Beispiel

```kotlin
import dev.slne.surf.settings.api.SurfSettingsApi
import dev.slne.surf.settings.api.setting.SettingKey
import dev.slne.surf.settings.api.setting.SettingKeys
import net.kyori.adventure.key.Key

// Eigenen Key definieren
val MY_FEATURE_KEY = SettingKey.ofBoolean(
    Key.key("myplugin", "feature-enabled"),
    defaultValue = true
)

// Beim Start registrieren
suspend fun onEnable() {
    SurfSettingsApi.createSetting(MY_FEATURE_KEY)
}

// Wert lesen
fun isFeatureEnabled(playerUuid: UUID): Boolean {
    return SurfSettingsApi.getSettingValue(playerUuid, MY_FEATURE_KEY)
}

// Wert speichern
suspend fun disableFeature(playerUuid: UUID) {
    SurfSettingsApi.saveSetting(playerUuid, MY_FEATURE_KEY, false)
}
```

## Externe Links

* [GitHub – SurfSettingsApi.kt](https://github.com/SLNE-Development/surf-settings/blob/version/26.1/surf-settings-api/src/main/kotlin/dev/slne/surf/settings/api/SurfSettingsApi.kt)
