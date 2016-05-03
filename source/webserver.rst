Webserver
=========

Alle http(s)-Dienste terminieren auf dem VM Host und werden dort über ein nginx-Reverse-Proxy auf die eigentliche VM weitergeleitet.

Beispiel:

git.freifunk-hennef.de -> srv03.freifunk-hennef.de (nginx) -> git01.freifunk-hennef.de

nginx
-----

Die nginx-Konfiguration erfolgt durch Ansible.

group_vars/vmhosts/nginx.yml


letsencrypt
-----------

Die letsencrypt-Konfiguration erfolgt durch Ansible.

group_vars/vmhosts/nginx.yml

