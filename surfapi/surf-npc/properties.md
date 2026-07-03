---
description: >-
  NPC-Properties sind typsichere Schlüssel-Wert-Paare, die zusätzliche
  Eigenschaften eines NPCs beschreiben und persistent gespeichert werden.
icon: wrench
---

# Properties

Jeder NPC kann beliebig viele **Properties** tragen – typisierte Schlüssel-Wert-Paare, die interne Zustände wie Position, Skin oder Rotation, aber auch eigene Plugin-Daten speichern.

## Datenstruktur

Eine Property besteht aus:

| Feld    | Typ               | Beschreibung                               |
| ------- | ----------------- | ------------------------------------------ |
| `key`   | `String`          | Eindeutiger Bezeichner der Property        |
| `value` | `Any`             | Aktueller Wert (typisiert über `type`)     |
| `type`  | `NpcPropertyType` | Serialisierungs- und Deserialisierungslogik|

## Interne Properties

Das System verwendet folgende interne Schlüssel (aus `NpcProperty.Internal`):

| Schlüssel       | Beschreibung                         |
| --------------- | ------------------------------------ |
| `displayname`   | Angezeigter Name des NPCs            |
| `skin_data`     | Skin-Daten des NPCs                  |
| `location`      | Position in der Welt                 |
| `rotation_type` | Rotationstyp (`FIXED` / `PER_PLAYER`)|
| `persistence`   | Ob der NPC persistent ist            |
| `scale`         | Skalierungsfaktor                    |
| `pose`          | Körperpose                           |

{% hint style="warning" %}
Interne Schlüssel sollten nicht manuell überschrieben werden. Nutze stattdessen die dafür vorgesehenen Methoden (z. B. `setLocation`, `setPose`).
{% endhint %}

## Eigene Properties hinzufügen

```kotlin
import dev.slne.surf.npc.api.npc.property.NpcProperty
import dev.slne.surf.npc.api.npc.property.NpcPropertyType

// Einfache Property hinzufügen
myNpc.addProperty(
    NpcProperty(
        key   = "my-plugin:quest-npc",
        value = true,
        type  = NpcPropertyType.Types.BOOLEAN_TYPE
    )
)

// Mehrere Properties auf einmal
myNpc.addProperties(
    Triple("my-plugin:level", 5, NpcPropertyType.Types.INT_TYPE),
    Triple("my-plugin:name",  "Händler", NpcPropertyType.Types.STRING_TYPE)
)
```

## Properties lesen

```kotlin
// Property-Objekt abrufen
val prop: NpcProperty? = myNpc.getProperty("my-plugin:quest-npc")

// Wert typsicher lesen
val isQuestNpc: Boolean? = myNpc.getPropertyValue("my-plugin:quest-npc", Boolean::class)

// Existenz prüfen
val hatProperty: Boolean = myNpc.hasProperty("my-plugin:quest-npc")
```

## Properties entfernen

```kotlin
// Einzelne Property entfernen
myNpc.removeProperty("my-plugin:quest-npc")

// Alle Properties löschen
myNpc.clearProperties()
```

## Property-Typen (`NpcPropertyType.Types`)

| Konstante              | Typ             |
| ---------------------- | --------------- |
| `BOOLEAN_TYPE`         | `Boolean`       |
| `INT_TYPE`             | `Int`           |
| `LONG_TYPE`            | `Long`          |
| `STRING_TYPE`          | `String`        |
| `DOUBLE_TYPE`          | `Double`        |
| `FLOAT_TYPE`           | `Float`         |
| `UUID_TYPE`            | `UUID`          |
| `NAMED_TEXT_COLOR_TYPE`| `NamedTextColor`|
| `COMPONENT_TYPE`       | `Component`     |
| `SKIN_DATA_TYPE`       | `NpcSkin`       |
| `LOCATION_TYPE`        | `Location`      |
| `ROTATION_TYPE_TYPE`   | `NpcRotationType`|
| `NPC_POSE`             | `NpcPose`       |

## DSL-Helper

```kotlin
import dev.slne.surf.npc.api.dsl.npcProperty

val prop = npcProperty {
    key   = "my-plugin:flag"
    value = true
    type  = NpcPropertyType.Types.BOOLEAN_TYPE
}
myNpc.addProperty(prop)
```

## Externe Links

* [GitHub – NpcProperty.kt](https://github.com/SLNE-Development/surf-npc/blob/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/npc/property/NpcProperty.kt)
* [GitHub – NpcPropertyType.kt](https://github.com/SLNE-Development/surf-npc/blob/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/npc/property/NpcPropertyType.kt)
