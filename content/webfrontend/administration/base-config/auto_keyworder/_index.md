---
title: "64 - Auto Keyworder"
menu:
  main:
    name: "Auto Keyworder"
    identifier: "webfrontend/administration/base-config/auto_keyworder"
    parent: "webfrontend/administration/base-config"
---
# Auto Keyworder

> HINWEIS: Dieses Plugin wird als separates Modul lizensiert. Bitte überprüfen Sie im Zweifel Ihren Lizenzvertrag.



Das Auto Keyworder Plugin ist ein Prozess-Plugin (im Hintergrund), das periodisch Bilddaten von Objekten an Online-KI-Dienste sendet, um den Bildinhalt zu erkennen und Objekte mit automatisch generierten Schlagwörtern und Themen zu aktualisieren. Weitere Informationen zur Einrichtung des Dienstes sind bei den [weiteren Funktionen](../../../datamanagement/features/keyword_plugin/) zu finden. 

Derzeit sind die folgenden KI-Dienste implementiert:

- Cloudsight: https://cloudsight.ai



Das Plugin prüft die Basis-Konfiguration auf Veränderungen. Dies geschieht mit einer Verzögerung von `baseconfig_poll_interval_sec` Sekunden, nachdem ein momentan laufender Prozess beendet wurde. Dieser wird in der Server-Konfiguration definiert.

Folgende Einstellungen stehen zur Verfügung:

| Option                                                  | Beschreibung                                                 |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| **Dienst aktiviert**                                    | Aktiviert/Deaktiviert den Auto-Keyworder                     |
| **Update-Prozess nach dem Speichern starten**           | Aktivieren Sie dies, um den Update-Prozess direkt zu starten nachdem die Basis-Konfiguration gespeichert wurde. Alternativ wird bis zum nächsten konfigurierten Zeitpunkt gewartet.<br> <ul> <li>Interne Representation: Wert `start_now` , welcher ebenso bei einem API-Aufruf bestimmt werden kann</li> <li>Dieser Wert ist **nicht anhaltend** und greift nur einmalig. Jedes Mal wenn der Wert zu `True` gesetzt wird, setzt das Plugin nach dem Lauf den Wert wieder auf `False`. </ul> |
| **easydb API Nutzer** und **easydb API Nutzerpasswort** | <ul><li> _verpflichtend_ (wenn **easydb API Session Token** nicht gesetzt ist) </li><li> Login und Passwort eines bestimmten Nutzers, der Suchen und Updates in easydb durchführen darf</li><li> Dieses Plugin nutzt die easydb-Endpunkte `/api/v1/search` und `/api/v1/db` </li><li> Der Nutzer benötigt mindestens eines der folgenden Rechte für Objekttypen (bzw. Pools):<ul><li> `write`-Recht für alle Objekttypen, die zum Updaten konfiguriert wurden </li><li> `mask`-Recht auf eine Maske, die das Bearbeiten von allen Feldern erlaubt </li><li> `asset_show`-Recht für die ausgewählten Asset-Felder, sodass das Plugin die Bilddaten an den KI-Service übermitteln kann </li><li> `bag_read` für den Pool, wenn die Objekte Poolmanagement besitzen </li><li> wenn die Schlagwörter in verlinkten Objekttypen verwaltet werden, braucht der Nutzer darüber hinaus die folgenden Rechte für die verlinkte Objekttypen: <ul><li> `read`-Recht , um nach existierenden verlinkten Objekten zu suchen </li><li> `mask`-Recht für eine Maske, die das Schreiben und Lesen innerhalb des Schlagwort-Textfeldes erlaubt </li><li> `create`-Recht, um neue verlinkte Objekte anzulegen</ul></ul></ul> |
| **easydb API Session Token**                            | <ul><li> _verpflichtend_ (wenn **easydb API Nutzer** und **easydb API Nutzerpasswort** nicht gesetzt sind)</li><li> Authentifiziert den Token des Nutzers, der Suchen und Updates in easydb durchführen darf</li><li> Wenn dies gesetzt ist, werden **easydb API Nutzer** und **easydb API Nutzerpasswort** ignoriert</ul> |

### Konfiguration für verschiedene Dienste


Konfigurationen für verschiedene Dienste und Objekttyp-Setups sind in mehreren verschiedenen Konfigurationsblöcken gespeichert:

| Option                                                       | Beschreibung                                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Diese Konfiguration aktivieren** ( `true/false` )          | Aktiviert/Deaktiviert diese Konfiguration                    |
| **Name dieser Konfiguration**                                | Um das Debuggen zu erleichtern, kann man der Konfiguration einen bestimmten Namen geben (_optional_) |
| **API-Key**                                                  | API-Key des verwendeten Dienstes                             |
| **Dienst**                                                   | Der verwendete KI-Dienstleister - momentan ist nur Cloudsight.ai implementiert. |
| **Objekttyp**                                                | <ul><li>_verpflichtend_</li><li> Objekttyp für die Schlagwort-Aktualisierung</li><li> Nur Objekttypen mit den folgenden Anforderungen können ausgewählt werden:<ul><li> Mindestens ein Asset (Bild)</li><li> Mindestens ein Feld, wo die generierten Schlagwörter gespeichert werden (entweder ein Textfeld innerhalb eines Mehrfachfeldes oder ein Textfeld innerhalb eines verlinkten Objekttyps in einem Mehrfachfeld)</li><li> Mindestens ein Datum-Feld, in dem der Zeitpunkt des zuletzt erfolgreichen Updates des Objekts gespeichert wird</ul></ul> |
| **Datei-Feld**                                               | Feld in den das Bild hochgeladen wird, welches an den KI-Dienst geschickt werden soll |
| **Zielfeld für Bildtitel (Subject)**                         | Text-Feld, in dem der Bild-Titel gespeichert wird (_optional_) |
| **Zielfeld für Schlagwörter**                                | <ul><li>_verpflichtend_</li> <li>Text-Feld, in dem die Schlagwörter gespeichert werden</li><li> Wenn das Feld ein Mehrfachfeld ist, wird jedes Schlagwort in einer neuen Reihe gespeichert, sonst werden die Schlagwörter mit einem Komma getrennt</li><li> Wenn das Feld ein mehrsprachiges Feld ist, werden die Schlagwörter für die bestimmten Sprachen gespeichert (-siehe unten-)</li><li> Wenn das Feld ein verlinkter Objekttyp ist, sucht das Plugin einen Eintrag mit dem gleichen Namen. Wenn dies nicht der Fall ist, wird ein neuer Datensatz angelegt. Dies erfolgt bevor es mit dem Datensatz verlinkt wird, der aktualisiert wird.</ul> |
| **Zielfeld für Menge/Anzahl**                                | Text-Feld, in dem die Menge / Anzahl gespeichert werden soll, siehe auch "Zielfeld für Schlagwörter" |
| **Zielfeld für Geschlecht**                                  | Text-Feld, in dem das Geschlecht gespeichert werden soll, siehe auch "Zielfeld für Schlagwörter" |
| **Zielfeld für Material**                                    | Text-Feld, in dem Materialien gespeichert werden sollen, siehe auch "Zielfeld für Schlagwörter" |
| **Zielfeld für Farben**                                      | Text-Feld, in dem Farben gespeichert werden sollen, siehe auch "Zielfeld für Schlagwörter" |
| **Zielfeld für Zeitpunkt der Verschlagwortung**              | <ul><li>_verpflichtend_</li><li> Es muss ein Datum-Feld sein, um den Zeitpunkt zu speichern</li><li> Nachdem ein Datensatz erfolgreich aktualisiert wurde, wird der Zeitpunkt in diesem Feld gespeichert</li><li> Es werden nur Objekte gesucht, wo dieses Feld keinen Wert hat oder wo der Zeitpunkt älter als das bestimmte maximale Alter ist (-siehe unten-)</ul> |
| **Tagfilter, um Objekte für die automatische Verschlagwortung zu markieren** | <ul><li> _verpflichtend_</li><li> Tag-Filter, um Datensätze zu markieren, die aktualisiert werden sollen</li><li> Es werden nur Datensätze gesucht, wo die Tags gesetzt wurden beziehungsweise nicht gesetzt wurden.</ul> |
| **Mindestdauer seit der letzten automatischen Verschlagwortung** | <ul><li>_verpflichtend_</li><li> Zeit, seitdem der Datensatz das letzte Mal aktualisiert wurde (in Tagen)</li><li> Es werden nur Datensätze gesucht, wo das Feld des Zeitpunktes keinen Wert hat oder wo der Zeitpunkt älter als diese Zeit ist</li><li> Minimum: `1`</li><li> Standard ist `7`</li><li> Wenn man Daten in Datensätzen überschreiben möchte, die erst kürzlich geändert wurden, muss man den Zeitpunkt löschen</ul> |
| **Sprache, für Bildtitel (Subject) und easydb-Sprache für verlinkte Schlagwort-Objekte** | <ul><li>_verpflichtend_</li><li> Sprache, in der die Schlagwörter angefordert werden</li><li> Es kommt auf den KI-Dienst an, welche Sprachen genutzt werden können</li><li> Die folgenden Sprachen können genutzt werden:<ul><li> Deutsch: `de-DE`</li><li> Englisch: `en-US`</li><li> Spanisch: `es-ES`</li><li> Italienisch: `it-IT`</li><li> Arabisch: `ar`</li><li> Tschechisch: `cs-CZ`</li><li> Farsi (Persisch): `fa`</li><li> Französisch: `fr-FR`</li><li> Japanisch: `ja-Jpan`</li><li> Georgisch: `ka-GE`</li><li> Koreanisch: `ko-Kore`</li><li> Niederländisch: `nl-NL`</li><li> Polnisch: `pl-PL`</li><li> Russisch: `ru-RU`</li><li> Chinesisch: `zh-Hans`</ul><li> Sprach-Einstellungen für Cloudsight.ai:<ul><li> Die Sprache wird über die API geschickt</li><li> Der Titel ( `name` ) vom analysierten Bild wird in dieser Sprache zurückgesendet</li><li> Die Schlagwörter werden in der Sprache gesendet, die im Cloudsight-Projekt für den gegebenen API-Key konfiguriert wurde. **Diese Konfiguration ist getrennt und unabhängig von easydb**.</li><li> Für die besten Ergebnisse sollte die Sprache ausgewählt werden, die im Cloudsight-Projekt konfiguriert wurde. So werden die Schlagwörter und der Titel in derselben Sprache gespeichert.</ul></ul> |
