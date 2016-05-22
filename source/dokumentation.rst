Dokumentation pflegen
=====================

Sphinx-Sourcen aus Git Repository auschecken/aktualisieren
----------------------------------------------------------

Erstmalig auschecken
  ``git clone https://git.freifunk-hennef.de/Freifunk-Hennef/ffhef-docs.git``

Aktualisieren
  Wenn man die Sourcen bereits früher ausgecheckt hat, in das Verzeichnis wechseln und das lokale Git Repository aktualisieren:

  ``git pull``

Dokumentation editieren
-----------------------

Die Quellen für die Dokumentation liegen im Verzeichnis ``source/``. Dort mit einem einfachen Texteditor bzw. bevorzugt mit einem Editor, der Unterstützung für ReStructeredText beinhaltet, die ``.rst``-Dateien editieren.

Die Dateien haben das Format `ReStructeredText <https://de.wikipedia.org/wiki/ReStructuredText>`_. Einige hilfreiche Informationen dazu:

* `Quick reStructuredText <http://docutils.sourceforge.net/docs/user/rst/quickref.html>`_

Dokumentation lokal bauen
-------------------------

Um die Dokumentation lokal mit Hilfe von Sphinx zu bauen, müssen mindestens folgende Tools installiert sein:

* Sphinx `<http://www.sphinx-doc.org/en/stable/install.html>`_
* sphinxcontrib-nwdiag `<http://blockdiag.com/en/nwdiag/sphinxcontrib.html#install>`_
* sphinxcontrib-seqdiag `<http://blockdiag.com/en/seqdiag/sphinxcontrib.html#install>`_


Dokumentation bauen
  ``make html``

  Dies funktioniert sowohl unter Linux und anderen unixoiden Betriebssystemen, als auch unter Windows. Als Ergebnis erhält man - wenn keine Fehler aufgetreten sind - die Dokumentation im HTML-Format im Verzeichnis ``build/html/``. Die Startseite findet man unter ``build/html/index.html``.

Dokumentation aktualisieren
---------------------------

Nachdem man Änderungen durchgeführt und geprüft hat, müssen diese noch ins Git Repository eingecheckt werden. Durch die Integration in unseren CI-Server wird die Webseite `<https://docs.freifunk-hennef.de>`_ automatisch aktualisiert.

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

Kurze Zeit später sollte `<https://docs.freifunk-hennef.de>`_ aktualisiert sein. Die Ausgabe des serverseitigen Builds kann man im CI-System prüfen: `<https://ci.freifunk-hennef.de/Freifunk-Hennef/ffhef-docs>`_.
