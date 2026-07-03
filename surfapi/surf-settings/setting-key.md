---
description: >-
  Ein SettingKey ist ein typsicherer Bezeichner für eine Spielereinstellung.
  Er verbindet einen Namespace-Key mit einem konkreten Werttyp.
icon: key
---

# SettingKey

Ein `SettingKey<T>` verknüpft einen namespaced [Key](https://jd.advntr.dev/key/4.17.0/net/kyori/adventure/key/Key.html) (aus Adventure) mit einem bestimmten Werttyp `T`. Er enthält außerdem den Standardwert sowie Serialisierungs- und Deserialisierungslogik für die Persistenz.

## Klasse

```kotlin
class SettingKey<T : Any>(
    val key: Key,
    val defaultValue: T,
    val serializer: (T) -> String,
    val deserializer: (String) -> T
)
```

| Eigenschaft     | Beschreibung                                              |
| --------------- | --------------------------------------------------------- |
| `key`           | Namespaced Key (z. B. `chat:pings`)                       |
| `defaultValue`  | Standardwert, wenn kein Spielerwert gesetzt ist           |
| `serializer`    | Wandelt `T` in einen `String` um (für die Datenbank)      |
| `deserializer`  | Wandelt einen `String` zurück in `T`                      |
| `name`          | String-Darstellung des Keys (`key.asString()`)            |

## Factory-Methoden

Für die gängigsten Typen stellt `SettingKey` statische Hilfsmethoden bereit:

```kotlin
// Boolean-Einstellung (Standard: false)
val key = SettingKey.ofBoolean(Key.key("chat", "pings"), defaultValue = true)

// Int-Einstellung (Standard: 0)
val key = SettingKey.ofInt(Key.key("ui", "volume"), defaultValue = 100)

// Double-Einstellung (Standard: 0.0)
val key = SettingKey.ofDouble(Key.key("ui", "brightness"), defaultValue = 1.0)

// String-Einstellung (Standard: "")
val key = SettingKey.ofString(Key.key("profile", "status"), defaultValue = "Keine Angabe")
```

{% hint style="info" %}
Für eigene Typen kannst du `SettingKey` direkt mit einem eigenen Serializer/Deserializer instanziieren.
{% endhint %}

## Eigene Typen

```kotlin
enum class Language { DE, EN, FR }

val LANGUAGE_KEY = SettingKey(
    key          = Key.key("profile", "language"),
    defaultValue = Language.DE,
    serializer   = { it.name },
    deserializer = { Language.valueOf(it) }
)
```

## Gleichheit

Zwei `SettingKey`-Instanzen gelten als gleich, wenn ihr `key` identisch ist. Der Werttyp wird dabei **nicht** verglichen.

## Externe Links

* [GitHub – SettingKey.kt](https://github.com/SLNE-Development/surf-settings/blob/version/26.1/surf-settings-api/src/main/kotlin/dev/slne/surf/settings/api/setting/SettingKey.kt)
