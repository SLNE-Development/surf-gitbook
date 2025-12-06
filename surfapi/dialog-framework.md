---
description: >-
  Das Dialog-System stellt eine Kotlin-DSL zur Erstellung aller Paper-Dialogs
  bereit. Es bietet vollständige Kontrolle über Titel, Bodies, Items, Inputs,
  Actions und Dialog-Typen.
icon: window-frame
---

# Dialog Framework

{% hint style="danger" %}
Dieser Eintrag ist noch unvollständig in wird derzeit erarbeitet.
{% endhint %}

## Grundstruktur

Ein Dialog besteht immer aus:

1. **Base**
   * Titel
   * Bodies (Text, Items, Inputs, etc.)
   * Externe Titel
2. **Type**
   * Notice
   * Dialog List
   * Confirmation
   * Multi Action
   * Server Links
3. **Actions**
   * Klickaktionen
   * Commands, Suggest, Konfiguration, Teleport, Permission Checks
4. **Builder-DSL**
   * Einheitliche, intuitive Kotlin-DSL

```kotlin
val dialog = dialog {
    base {
        title { text("Titel") }
        body {
            plainMessage { text("Textinhalt") }
        }
    }
    type { notice() }
}
```

***

## Notice Dialog

Ein Notice Dialog zeigt eine einfache Information mit optionalem Button.\
Er eignet sich für kurze Hinweise oder Benachrichtigungen.

***

### Typdefinition

```kotlin
type { notice() }
type { notice(button) }
type { notice { ... } }
```

### Beispiel: Einfacher Hinweis

```kotlin
val dialog = noticeDialog(
    title = text("Information"),
    notice = text("Dies ist ein Hinweis.")
)
```

***

### Beispiel: Notice mit Button und Action

```kotlin
val dialog = dialog {
    base {
        title { text("Achtung!") }
        body {
            plainMessage { text("Du betrittst jetzt eine PvP-Zone.") }
        }
    }
    type {
        notice {
            label { text("Ok") }
        }
    }
}
```

***

### Beispiel: Komplette Individualisierung

```kotlin
val dialog = dialog {
    base {
        title { text("Neuigkeit") }
        body {
            plainMessage(200) {
                text("Du hast 500 Coins erhalten!")
            }
        }
    }
    type { notice() }
}
```

## Dialog List

Ein DialogList-Dialog zeigt eine Liste klickbarer Dialogeinträge.\
Damit können Menüstrukturen, Kategorien oder Untermenüs erstellt werden.

***

### Typdefinition

```kotlin
type {
    dialogList {
        dialog(dialogA)
        dialog(dialogB)
        columns(2)
        exitAction { ... }
    }
}
```

***

### Beispiel: Hauptmenü

```kotlin
val settingsDialog = dialog { /* ... */ }
val profileDialog = dialog { /* ... */ }

val menu = dialog {
    base { title { text("Hauptmenü") } }

    type {
        dialogList(settingsDialog, profileDialog) {
            columns(1)
            buttonWidth(200)
            exitAction {
                label { text("Schließen") }
            }
        }
    }
}
```

***

### Beispiel: Dynamische Spieler-Liste

```kotlin
val playerDialogs = players.map { p ->
    dialog {
        base { title { text(p.name) } }
        type { notice() }
    }
}

val menu = dialog {
    base { title { text("Spieler online") } }
    type {
        dialogList {
            addAll(playerDialogs)
            columns(3)
        }
    }
}
```

## Confirmation Dialog

Ein Confirmation Dialog besteht aus zwei Buttons, typischerweise „Ja“ und „Nein“.

***

### Typdefinition

```kotlin
type {
    confirmation {
        yes { ... }
        no { ... }
    }
}
```

Jede Option besitzt:

* `label { ... }`
* `action { ... }`

***

### Beispiel: Spieler bannen

```kotlin
val dialog = dialog {
    base {
        title { text("Spieler bannen") }
        body {
            plainMessage { text("Soll dieser Spieler gebannt werden?") }
        }
    }
    type {
        confirmation {
            yes {
                label { text("Ja") }
            }
            no {
                label { text("Nein") }
            }
        }
    }
}
```

## Multi Action Dialog

Ein MultiAction-Dialog bietet mehrere Buttons gleichzeitig.\
Ideal für Werkzeuge, Verwaltungsmenüs oder Aktionsübersichten.

***

### Typdefinition

```kotlin
type {
    multiAction {
        action { ... }
        action { ... }
        columns(2)
        exitAction { ... }
    }
}
```

***

### Beispiel: Administrator-Tools

```kotlin
val adminTools = dialog {
    base { title { text("Verwaltung") } }

    type {
        multiAction {
            columns(2)

            action {
                label { text("Teleport") }
            }

            action {
                label { text("Inventar") }
            }

            exitAction {
                label { text("Schließen") }
            }
        }
    }
}
```

## Server Links Dialog

Ein ServerLinks-Dialog zeigt Buttons mit externen Webseiten, Discord, Shop oder anderen URLs.

***

### Typdefinition

```kotlin
type {
    serverLinks {
        columns(2)
        buttonWidth(150)
        exitAction { ... }
    }
}
```

***

### Beispiel: Link-Übersicht

```kotlin
val dialog = dialog {
    base { title { text("Server Links") } }
    type {
        serverLinks {
            columns(2)
            buttonWidth(120)

            exitAction {
                label { text("Zurück") }
            }
        }
    }
}
```
