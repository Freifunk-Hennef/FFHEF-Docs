Configuration Management
========================

Zur Konfiguration und zum Deployment von Servern verwenden wir `Ansible <www.ansible.com>`_.

Ansible Vault
-------------

Sensible Informationen, z.B. Keys, werden in Ansible verschlüsselt gespeichert.

Verschlüsselte Dateien editiert man mit `ansible-vault edit <DATEINAME>`. Das Passwort für Vault wird standardmässig aus `.vault-password` gelesen. Auf srv03.freifunk-hennef.de liegt in `/etc/ansible/.vault-password` das aktuelle Passwort, das man sich in seine lokale Umgebung kopieren kann, um mit den verschlüsselten Dateien umgehen zu können.

