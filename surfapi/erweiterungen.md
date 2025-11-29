---
icon: plus
---

# Erweiterungen

#### **Globale Erweiterungen**

**Location Extensions**

* `Location#distanceSquared`: Distanz von einem Ort zu einem anderen, quadriert
* <kbd>`Location#distance`</kbd>: Distanz von einem Ort zum anderen

Datum Extensions

* <kbd>`dateTimeFormatter`</kbd>: Globaler Zeit & Datum Formatter nach Europäischem Format
* `currentDateTimeFormatted`: Aktuelle Uhrzeit & Datum formatiert mit dem dateTimeFormatter

Service Extensions

* `requiredService<Typ>`: Sucht nach Fallback Services des angegeben Types

FastUtil Extensions

* `mutableObjectSetOf`: Erstellt ein veränderbares ObjectSet
* `objectSetOf`: Erstellt ein unveränderbares ObjectSet
* `mutableObjectListOf`: Erstellt eine veränderbare ObjectList
* `objectListOf`: Erstellt eine unveränderbare ObjectList
* `mutableObject2ObjectMapOf`: Erstellt eine veränderbare Object2ObjectMap
* `object2ObjectMapOf`: Erstellt eine unveränderbare Object2ObjectMap&#x20;

Andere Extensions

* `random`: Globales SecureRandom
* `String#toSmallCaps`: Setzt die Font des Strings zu Smallcaps

#### platformspezifische Erweiterungen (Paper)

* `key(name: String)`: Erstellt einen NameSpacedKey vom ProvidingPlugin mit dem angegeben Namen
* `forEachPlayer`: Geht alle Spieler des Servers durch und führt die angegebene Aktion mit ihnen aus
* `Location#chuckX`: Berechnet die Chunk X Koordinate des Ortes
* `Location#chuckZ`: Berechnet die Chunk Z Koordinate des Ortes
* `Location#toVector3d`: Wandelt die Location in einen drei dimensionalen Vector um
* `Listener#register`: Registriert den Listener
