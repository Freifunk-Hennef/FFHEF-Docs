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

Signieren der Images (Sysupgrade)
---------------------------------

Damit das Upgrade auf eine neue Version erfolgt, ist i.d.R. eine (oder mehrere) Signatur(en) der entsprechenden Imagedateien im sysupgrade-Unterordner erforderlich. Für diese Dokumentation wird davon ausgegangen, dass die ecdsautils aus den Quellen erzeugt wurden, und somit als single binary vorliegen. Andernfalls unterscheidet sich die Syntax auch nur marginal. Die Scripte sign.sh und signtest.sh sind entsprechend anzupassen.

Erzeugen der Schlüssel
++++++++++++++++++++++

Soweit noch nicht erfolgt, erzeugt man mittels ``ecdsautil generate-key > private_key`` einen entsprechenden Schlüssel zum signieren. Dieser Schlüssel sollte gut verwahrt werden.

Den Publickey kann man sich durch ``ecdsautil show-key < private_key`` anzeigen lassen. Dieser ghört zum einen in die entsprechende site.conf (der Autoupdater nutzt ihn, um die Signatur(en) zu prüfen), zum Anderen kann man hiermit selbst prüfen, ob die Firmware korrekt signiert wurde.

Durchführen der Signierung
++++++++++++++++++++++++++

Nach dem Bau der Firmware und einem ``make manifest ...`` findet sich im sysupgrade-Unterverzeichnis eine entsprechendes Manifest, je nach Branch stable.- beta.- oder experimental.manifest. Hierin befinden sich die namen der Firmwareimages samt der zugehörigen Hashwerte der Binaries.

Per ``/pfad/zu/sign.sh /wo/auch/immer/private_key experimental.manifest`` wird selbiges signiert. Die erzeugte Signatur findet sich am Ende des Manifestes wieder.

Prüfen der Signatur
+++++++++++++++++++

Um sicherzugehen, dass die Signatur korrekt erfolgt ist, kann folgende Zeile verwendet werden. Der **öffentliche** Schlüssel muss hierbei im Klartext angegeben werden:

  ``/pfad/zu/sigtest.sh 7880319051562a(...)a2dc4f47328 experimental.manifest ; echo $?``

Erscheint als Rückgabewert eine ``0``, ist alles in Ordnung. Jeder andere Wert bedeutet, dass es bei der Signaturprüfung einen Fehler gab, und dass die Firmware so nicht durch den Autoupdater eingespielt werden kann.
