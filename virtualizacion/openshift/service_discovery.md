A parte de las variables de entorno (mirar kubernetes/service_discovery.md) tenemos un servidor de DNS (skydns) donde poder resolver las aplicaciones.

Por ejemplo:
nodejs-ex.myproject.svc.cluster.local


OpenShift Container Platform has a built-in DNS so that the services can be reached by the service DNS as well as the service IP/port. OpenShift Container Platform supports split DNS by running SkyDNS (distributed service for announcement and discovery of services built on top of etcd) on the master that answers DNS queries for services. The master listens to port 53 by default.


Configuracion de DNS necesarias para la instalación
https://docs.openshift.com/enterprise/latest/install_config/install/prerequisites.html#prereq-dns


SkyDNS corre debajo del openshift master api.

Comprobar que resuelve:
/usr/lib64/nagios/plugins/check_dig -H 127.0.0.1 -p 8053 -l kubernetes.default.svc.cluster.local


# Funcionamiento
dnsmasq hace la division para enviar trafico hacia servidores DNS upstream y los .cluster.local lo reenvia a SkyDNS
