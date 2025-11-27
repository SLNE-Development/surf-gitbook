---
icon: bolt
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
metaLinks:
  alternates:
    - https://app.gitbook.com/s/yE16Xb3IemPxJWydtPOj/getting-started/quickstart
---

# Versionsformat

{% tabs %}
{% tab title="Minecraft Plugins" %}
Surf Minecraft Plugins werden in der Regel in Anlehnung an die Minecraft Versionen versioniert. Das Format ist wie folgt:

```
<MC_VERSION>-<MAJOR>.<MINOR>.<PATCH>-<SUFFIX>
```
{% endtab %}

{% tab title="Andere Projekte" %}
Für andere Projekte, wie z.B. Bibliotheken, die nicht an Minecraft Versionen gebunden sind, wird das folgende Format genutzt:

```
<MAJOR>.<MINOR>.<PATCH>-<SUFFIX>
```
{% endtab %}
{% endtabs %}

| Teil              | Beispiel | Bedeutung                              | Verändert                          |
|-------------------|----------|----------------------------------------|------------------------------------|
| Minecraft Version | 1.21.10  | Die Minecraft Version dieses Projektes | bei Minecraft Update               |
| Major Version     | 1        | Die Major Version des Projektes        | bei Rewrite/komplett neuem Projekt |
| Minor Version     | 3        | Die Minor Version des Projektes        | Bei neuen Features                 |
| Patch Version     | 23       | Die Patch Version des Projektes        | Bei Bugfixes, kleine Features      |
| Suffix            | SNAPSHOT | Der Suffix, meist SNAPSHOT - Optional. | Verändert bei Release/Update       |

## 


