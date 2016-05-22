Images bauen
============

Site-Konfiguration aus Git Repository auschecken/aktualisieren
--------------------------------------------------------------

Erstmalig auschecken
  ``git clone https://git.freifunk-hennef.de/Freifunk-Hennef/site.git``

Aktualisieren
  Wenn man die Sourcen bereits früher ausgecheckt hat, in das Verzeichnis wechseln und das lokale Git Repository aktualisieren:

  ``git pull``

Änderungen pushen und Image bauen
---------------------------------

Nachdem man Änderungen durchgeführt und geprüft hat, müssen diese noch ins Git Repository eingecheckt werden. Durch die Integration in unseren CI-Server werden die Firmware-Images automatisch gebaut.

Änderungen prüfen
  Anzeigen der geänderten Dateien: ``git status``

  Anzeigen der Änderungen: ``git diff``

Dateien dem Git Commit hinzufügen
  Einzelne Dateien dem Commit hinzufügen: ``git add <DATEINAME>``

  Alle Dateien dem Commit hinzufügen: ``git add .``

Commit durchführen
  ``git commit -m "<ZUSAMMENFASSUNG DER ÄNDERUNG"``

In Git Repository pushen
  ``git push``

Nach dem Pushen beginnt das CI-System mit dem Erzeugen der Images. Je nachdem, ob bereits gecachte Toolchains vorhanden sind oder nicht kann dieser Build bis zu 2 Stunden dauern. Anschließend werden die (unsignierten) Images in das Verzeichnis ``/var/www/images.freifunk-hennef.de/unsigned/$$BRANCH/`` (wobei ``$$BRANCH`` durch den Git-Branch ersetzt wird, in den gepusht wurde) auf ``web01.freifunk-hennef.de`` synchronisiert.

Die Ausgabe des serverseitigen Builds kann man im CI-System prüfen: `<https://ci.freifunk-hennef.de/Freifunk-Hennef/site>`_.

