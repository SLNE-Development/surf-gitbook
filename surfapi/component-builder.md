---
description: Erstelle Components mit den Standartfarben
icon: message-text
---

# Component Builder

Der Component Builder wird in verschiedenen Funktionen benutzt, beispielsweise beim senden von Nachrichten oder anderen Anzeigemöglichkeiten.

**Manueller Component Builder**

```kotlin
buildText { 
    appendPrefix()
    success("Du wurdest")
    appendSpace()
    variableValue("zur Whitelist")
    appendSpace()
    success("hinzugefügt.")
}
```

**Component Builder beim Nachrichten senden**

```kotlin
player.sendText {
    appendPrefix()
    info("Das ist eine Information.")
}
```

{% code title="Nachricht mit Hover-Text" %}
```kotlin
buildText { 
    appendPrefix()
    success("Du wurdest")
    appendSpace()
    variableValue("zur Whitelist")
    appendSpace()
    success("hinzugefügt.")
    hoverEvent(buildText {
        success("Es hat geklappt!")
    })
}
```
{% endcode %}

{% code title="Nachricht mit Hover-Event und Klick-Action" %}
```kotlin
buildText { 
    appendPrefix()
    success("Du wurdest")
    appendSpace()
    variableValue("zur Whitelist")
    appendSpace()
    success("hinzugefügt.")
    hoverEvent(buildText {
        error("Klicke, um dich wieder zu entfernen.")
    })
    clickRunsCommand("/whitelist remove $playerName")
}
```
{% endcode %}

Jede [Standartfarbe ](farbdesign.md)besitzt eine zugehörige Farb-Funktion welche im Component Builder genutzt werden.

| Methode                  | Farbe               |
| ------------------------ | ------------------- |
| primary(text: Any)       | Primary (#3b92d1)   |
| secondary(text: Any)     | Secondary (#5b5b5b) |
| info(text: Any)          | Info (#40d1db)      |
| success(text: Any)       | Success (#65ff64)   |
| warning(text: Any)       | Warning (#f9c353)   |
| error(text: Any)         | Error (#ee3d51)     |
| variableKey(text: Any)   | Info (#40d1db)      |
| variableValue(text: Any) | Warning (#f9c353)   |
| spacer(text: Any)        | Gray (#AAAAAA)      |
| darkSpacer(text: Any)    | Dark Gray (#555555) |

Außerdem gibt es Zusatz-Funktionen:

<table><thead><tr><th width="229">Methode</th><th width="139">Argumente</th><th>Funktion</th></tr></thead><tbody><tr><td>appendPrefix()</td><td>/</td><td>Fügt den Plugin-Prefix ein</td></tr><tr><td>append()</td><td>Component</td><td>Setzt einen Component ein</td></tr><tr><td>appendNewLine()</td><td>/</td><td>Startet eine neue Zeile</td></tr><tr><td>appendNewPrefixedLine()</td><td>/</td><td>Startet eine neue Zeile mit Prefix</td></tr><tr><td>text()</td><td>Component Builder, Farbe, Decoration</td><td>Fügt Text hinzu, mit einer Farbe und Dekoration</td></tr><tr><td>ellipsis()</td><td>Farbe (Optional)</td><td>Fügt eine Ellipse ein</td></tr><tr><td>appendDiscordLink()</td><td>/</td><td>Fügt den Discord Link zum CastCrafter Discord ein</td></tr><tr><td>appendDisconnectFooter()</td><td>/</td><td>Fügt den Disconnect Footer ein</td></tr><tr><td>appendDisconnectFooterTryAgainLater()</td><td>Is issue?</td><td>Fügt den Disconnect Footer mit extra Nachricht ein</td></tr><tr><td>content()</td><td>Text (Content)</td><td>Fügt Text ohne Farbe hinzu</td></tr><tr><td>decorate()</td><td>Decoration</td><td>Ändert die Dekoration</td></tr><tr><td>decorationIfAbsent()</td><td>Decoration</td><td>Ändert die Dekoration, wenn nicht vorhanden</td></tr><tr><td>font()</td><td>Font Key</td><td>Ändert die Text-Font</td></tr><tr><td>hoverEvent()</td><td>HoverEvent</td><td>Fügt ein Hover-Event ein</td></tr><tr><td>clickEvent()</td><td>ClickEvent</td><td>Fügt ein Click-Event ein</td></tr><tr><td>style()</td><td>Style</td><td>Ändert den Stil des Textes</td></tr><tr><td>shadowColor()</td><td>ARGBLike</td><td>Setzt die Schattenfarbe </td></tr></tbody></table>

