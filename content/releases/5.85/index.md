---
menu:
  main:
    name: "5.85 (Juni 2021)"
    identifier: "5.85"
    parent: "releases"
    weight: -585
---

> Für dieses Release ist **kein Re-Index** nötig. 

> Diese Version enthält das update auf elasticsearch Version 7.11. Diese versteht die Einstellung `reclaim_deletes_weight` nicht mehr. Bei Problemen befolgen Sie bitte [diese Aleitung](/en/technical/elasticsearch/updates/version_7.11) zum Entfernen der Einstellung.

# Version 5.85.2

*Veröffentlicht am 30.06.2021*

## Webfrontend

### Behoben

* Falsche Maskenauswahl bei Verknüpfung im Editor behoben

## Plugins

### Behoben

* Kodierung bei Abfrage im IUCN-Plugin korrigiert

# Version 5.85.1

*Veröffentlicht am 22.06.2021*

## Webfrontend

### Behoben

* Download von Präsentationen korrigiert

# Version 5.85.0

*Veröffentlicht am 16.06.2021*

## Webfrontend

### Neu

* neuer Systemrecht-Parameter `enable_ignore_linked_objects_filter` (siehe Server-Teil) wird verwendet, um Filter für verlinkte Objekte ausblenden zu können
* neue Option, um nur den ersten Eintrag eines Mehrfachfelds im PDF-Creator zu verwenden

### Verbessert

* Speichern-Button wird deaktiviert, wenn in den Workflows eine E-Mail-Aktion ohne Empfänger konfiguriert wurde
* Verbesserung der Barrierefreiheit
* Masken werden im Datenmodell-Editor alphabetisch sortiert
* erster Buchstabe eines Tags wird angezeigt, wenn der Tag kein Icon konfiguriert hat
* "standard"-Unterstützung für den Custom-Datentyp "location"

### Behoben

* Sichtbarkeit verlinkter Objekte mit Hierarchien korrigiert
* Filter für verlinkte Objekte in Expertensuche
* Encodsng-Fix für Asset-Upload im CSV-Importer
* Feldsichtbarkeit in Filtern korrigiert
* Systemnutzer werden in E-Mail-Aktionen in Workflows ausgeblendet
* Speichern von Assets in verschachtelten Mehrfachfeldern korrigiert
* Öffnen von Detail-Deeplinks korrigiert
* Problem beim Umschalten zwischen Objekten in Vollbild-Detailanzeige korrigiert

## Server

### Neu

* neuer Parameter `enable_ignore_linked_objects_filter` für Systemrecht `system.frontend_features` (ersetzt das Systemrecht `system.disable_linked_objects_filter`).
* Lösch-Simulation für Assets (standardmäßig deaktiviert). Kann Fehler finden, bevor Assets wirklich gelöscht werden.

### Verbessert

* Asset-URL im CSV-Export
* bessere Fehlerbehandlung für Node.js-Skriptaufruf

### Behoben

* bei Mail-Versand an Gruppen wird nicht versucht, diese an archivierte Nutzer zu schicken (das schlug fehl)
* unnötige Daten aus Kurz-Format von Benutzern (eingebettet in andere Objekte) entfernt
* verwendete Assets werden in Vorbereitung auf das Aufräumen verlinkt. Dies geschieht einmalig beim Start, im Fall eines Fehlers kann dieses Update mit `debug.disable_collect_missing_asset_links: true` deaktiviert werden.
* interne Daten in Objektausgabe entfernt.

# Prüfsummen

Hier die Prüfsummen unserer Docker-Images (neueste Version): 

```ini
docker.easydb.de/pf/chrome               sha256:effcf603c923c8cafc5b302b717353bb43a447a9df858ce0e66e263fae4f93f3
docker.easydb.de/pf/eas                  sha256:3f5d195b23c53768d860d60b343358497296f8f78d5db918cd032fcb80882e74
docker.easydb.de/pf/elasticsearch        sha256:8ed3f3d5a05436c8297b2bf3aa1d359aa1256dc89ceaa429b1daa7c11e4f1ea4
docker.easydb.de/pf/fylr                 sha256:766441fba764067ab9b2aa6674490cbe53f74a2db70a5fd436b80b7fd7ce297b
docker.easydb.de/pf/postgresql-11        sha256:88f53efde21bbf527ae3aea5022f5657c89d7ac8fa75a11c22ffa955ce207012
docker.easydb.de/pf/server-base          sha256:564e41d86bcbaaf3275e3957d58e1b34e6ab93a4454c31fe8040657e7e2ae1bd
docker.easydb.de/pf/webfrontend          sha256:ea21e1fef1e4761fcebfe62660d9c39514f83bf00b3cf3a89deb17447bb62c6b
```
