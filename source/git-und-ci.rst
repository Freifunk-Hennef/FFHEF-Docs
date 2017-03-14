Git und CI
==========

Git Repository
--------------

Wir setzen die Software `Gogs <http://gogs.io>`_ für unser Git Repository ein.

Das Repository ist unter `<https://git.freifunk-hennef.de/>`_ erreichbar. Die Authentifizierung erfolgt per PAM, d.h. die Zugangsdaten entsprechen den lokalen auf dem System (die in der Regel per Ansible gepflegt werden). 
Die öffentlichen Anteile hosten wir auf `Github <https://github.com/Freifunk-Hennef/>`.

Continious Integration Server
-----------------------------

Wir setzen die Software `Drone <https://github.com/drone/drone>`_ für unsere Continious Integration ein.

Der Continious Integration Server ist unter `<https://ci.freifunk-hennef.de/>`_ erreichbar. Die Authentifizierung erfolgt gegen unser Git Repository (siehe `Git Repository`_).

Funktionsweise und Einrichtung
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Mit der Version 0.5 der Drone hat sich das Handling stark verändert, zudem haben wir die CI nun auf Github ausgerichtet. 
Über die Weboberfläche des CI-Servers können in unserem Git vorhandene Repositories hinzugefügt werden. Hierbei wird ein Webhook eingerichtet, der den CI-Server bei jedem Push kontaktiert; dieser prüft daraufhin die Existenz der Datei ``.drone.yml``, in der Anweisungen für Build und Deploment stehen. Diese werden daraufhin von Drone automatisch ausgeführt.

.. seqdiag::
  :scale: 50

  seqdiag {
    user  -> github [label = "git push"];
    github -> drone [label = "push webhook"];
    drone -> build_job [label = "start build job"];
    drone <-- build_job [label = "log output"];
    drone -> cache_job [label = "cache build output"];
    drone <-- cache_job [label = "save cache to /var/lib/drone/cache"];
    drone -> deploy_job [label = "start deploy job"];
    deploy_job -> some_system [label = "deploy (scp/rsync...)"];
    deploy_job <-- some_system [label = "log output"];
    drone <-- deploy_job [label = "log output"];
  }

Wichtig zu wissen ist, dass das Einrichten (und die .drone.yml) nicht mehr ganz so intuitiv zu befüllen sind wie in vorherigen Versionen. Stolpersteine sind, u.a.:

* Build-Dauer 
Nur ein Admin kann in den Einstellungen des Repositories in der UI der Drone die maximale Builddauer einstellen. Daher müssen die Adminaccount per Environment angegeben werden: ``DRONE_ADMIN=user,user,...`` 
Dem Drone-Agent-Container muss beim Start per Environment ein weitaus größerer Inaktivitäts-Timeout mitgegeben werden: ``DRONE_TIMEOUT=180m``
* Deployment 
Zuvor hat sich Drone selbst um die Deployment-Keys gekümmert. Dies ist nun nicht mehr der Fall. Somit müssen selbst Deployment-Keys erzeugt und dem Repository hinzugefügt werden. 
Für den Deployment-User wird ein normaler SSH-Key erzeugt: ``sudo -u deployuser ssh-keygen`` 
Der pubkey wird in die authorized_keys geschrieben (``cat key.pub >> authorized_keys``), der private key auf den Server kopiert, wo Drone läuft. 
Hier wird zusätzlich drone als CLI-Tool benötigt, das Binary kann unter `<http://readme.drone.io/usage/getting-started-cli/>`_ heruntergeladen werden. 
Zunächst werden wieder Environment-Variablen benötigt: ``export DRONE_SERVER=<url des servers>`` und ``export DRONE_TOKEN=<usertoken>``. Letzteres findet man in der Drone-UI unter "Account". 
Dann kann man den Key hinzufügen: ``./drone secret add Repo/sitory VARNAME @/pfad/zum/key --skip-verify --conceal``. Im Deployment-Schrit der .drone.yml wird dies dann mit ``key: ${VARNAME}`` referenziert.

Docker-Image für die CI
-----------------------
Zum Bauen der Images wird im Hintergrund ein Docker-Container benötigt.

Diesen basteln wir uns selbst, er steht unter `docker-buildimage <https://hub.docker.com/r/ffhef/docker-buildimage/>`_ zur Verfügung. Das - simple - Dockerfile findet sich in unserem `git <https://github.com/Freifunk-Hennef/docker-buildimage>`_ 
Das Image ist, wie das von tobitheo, recht generisch und kann auch gerne für eigene Konstrukte verwendet werden.
