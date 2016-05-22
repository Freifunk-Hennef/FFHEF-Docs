Git und CI
==========

Git Repository
--------------

Wir setzen die Software `Gogs <http://gogs.io>`_ für unser Git Repository ein.

Das Repository ist unter `<https://git.freifunk-hennef.de/>`_ erreichbar. Die Authentifizierung erfolgt per PAM, d.h. die Zugangsdaten entsprechen den lokalen auf dem System (die in der Regel per Ansible gepflegt werden).


Continious Integration Server
-----------------------------

Wir setzen die Software `Drone <http://drone.io>`_ für unsere Continious Integration ein.

Der Continious Integration Server ist unter `<https://ci.freifunk-hennef.de/>`_ erreichbar. Die Authentifizierung erfolgt gegen unser Git Repository (siehe `Git Repository`_).

Funktionsweise und Einrichtung
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Über die Weboberfläche des CI-Servers können in unserem Git vorhandene Repositories hinzugefügt werden. Hierbei wird in Gogs ein Webhook eingerichtet, der den CI-Server bei jedem Push kontaktiert; dieser prüft daraufhin die Existenz der Datei ``.drone.yml``, in der Anweisungen für Build und Deploment stehen. Diese werden daraufhin von Drone automatisch ausgeführt.

.. seqdiag::
  :scale: 50

  seqdiag {
    user  -> gogs [label = "git push"];
    gogs -> drone [label = "push webhook"];
    drone -> build_job [label = "start build job"];
    drone <-- build_job [label = "log output"];
    drone -> cache_job [label = "cache build output"];
    drone <-- cache_job [label = "save cache to /var/lib/drone/cache"];
    drone -> deploy_job [label = "start deploy job"];
    deploy_job -> some_system [label = "deploy (scp/rsync...)"];
    deploy_job <-- some_system [label = "log output"];
    drone <-- deploy_job [label = "log output"];
  }

