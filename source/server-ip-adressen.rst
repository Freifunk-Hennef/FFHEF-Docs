Server und IP-Adressen
======================

.. nwdiag::
   :desctable:

    nwdiag {
      network Internet {
          srv03 [ address = "176.9.38.45", description = "Virtual Machine Host bei Hetzner" ];
          srv02 [ address = "37.221.194.208", description = "Netcup vServer" ];
          srv01 [ address = "46.38.236.237", description = "Netcup vServer" ];
      }

      network vmbr0 {
          address = "10.10.0.0/24";

          srv03 [ address = "10.10.0.1, 2a01:4f8:150:4426::2" ];
          git01 [ address = "10.10.0.?, 2a01:4f8:150:4426::?", description = "Gogs für git.freifunk-hennef.de"];
          ci01 [ address = "10.10.0.?, 2a01:4f8:150:4426::?", description = "Drone.io für ci.freifunk-hennef.de" ];
          concentrator01 [ address = "10.10.0.?, 2a01:4f8:150:4426::?", description = "Gateway zum FFRL" ];
          supernode01 [ address = "10.10.0.?, 2a01:4f8:150:4426::?", description = "Fastd Endpunkt" ];
      }

      network meshbr {
          supernode01 [ address = "10.?.?.?, ?::?" ];
      }

    }

Server
------

+---------------------+------------------------------+---------------------+-----------------+-------------------------+
| Host                | DNS                          | Name                | Public IPv4     | Public IPv6             |
+=====================+==============================+=====================+=================+=========================+
| Netcup vServer      | srv01.freifunk-hennef.de     | v22015082981327390  | 46.38.236.237   | 2a03:4000:2:ae::/64     |
+                     +------------------------------+---------------------+-----------------+-------------------------+
|                     | srv02.freifunk-hennef.de     | v22015123182829981  | 37.221.194.208  | 2a03:4000:8:6d::/64     |
+---------------------+------------------------------+---------------------+-----------------+-------------------------+
| Hetzner RootServer  | srv03.freifunk-hennef.de     |                     | 176.9.38.45     | 2a01:4f8:150:4426::2/64 |
+---------------------+------------------------------+---------------------+-----------------+-------------------------+


Dienste
-------
+---------------------+------------------------------+
| Name                | DNS                          |
+=====================+==============================+
| Mapserver           | map.freifunk-hennef.de       |
+---------------------+------------------------------+
| Dokumentation       | docs.freifunk-hennef.de      |
+---------------------+------------------------------+
| Hopglass Map        | hopglass.freifunk-hennef.de  |
+---------------------+------------------------------+



Supernodes
----------

===== =========================  ============ ====== =============  ====================== ============  ===========  ==============================
Host  DNS                        ServerName   Port   Public IPv4    Public IPv6            Mesh IPv4      Mesh IPv6     DHCP Bereich
===== =========================  ============ ====== =============  ====================== ============  ===========  ==============================
FFRL  0.wupper.ffrl.de           wupper0      53773  151.80.64.176  2001:41d0:c:95c::176   10.186.0.240               10.186.224.1 - 10.186.255.254
FFRL  1.wupper.ffrl.de           wupper1      53773                                        10.186.0.241               "
FFRL  2.wupper.ffrl.de           wupper2      53773                                        10.186.0.242               "
FFRL  3.wupper.ffrl.de           wupper3      53773                                        10.186.0.243               "
FFRL  4.wupper.ffrl.de           wupper4      53773                                        10.186.0.244               "
FFRL  5.wupper.ffrl.de           wupper5      53773                                        10.186.0.245               "
FFRL  6.wupper.ffrl.de           wupper6      53773                                        10.186.0.246               "
FFRL  7.wupper.ffrl.de           wupper7      53773                                        10.186.0.247               "
FFRL  8.wupper.ffrl.de           wupper8      53773                                        10.186.0.248               "
FFRL  9.wupper.ffrl.de           wupper9      53773                                        10.186.0.249               "
===== =========================  ============ ====== =============  ====================== ============  ===========  ==============================
