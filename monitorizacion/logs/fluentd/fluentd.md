http://fluentd.org/

"Fluentd" is an open-source tool to collect events and logs. 150+ plugins instantly enables you to store the massive data for Log Search, Big Data Analytics, and Archiving (MongoDB, S3, Hadoop).
Escrito en ruby.


http://es.slideshare.net/treasure-data/fluentd-loves-mongodb-at-mongosv-july172012
Comparación contra Scribe y Flume
Otra opción es logplex (de heroku, escrito en erlang)


http://docs.fluentd.org/articles/free-alternative-to-splunk-by-fluentd
fluentd + elasticsearch + kibana

http://tech.riywo.com/blog/2013/12/20/fluentd-on-mesos-plus-docker-plus-marathon/


OpenShift usa fluentd para recoger los logs de la plataforma y los pods y enviarlos a un Elasticsearch para posterior consulta.


Puede leer logs de journald: https://github.com/reevoo/fluent-plugin-systemd
Para instalar esta gema en alpine, necesitaremos el apk libffi-dev
La imagen de fluentd no vale para el plugin de systemd, porque falta la lib libsystemd.so.0 y alpine no soporta systemd asi que no se espera esa lib.

La otra opción es usar una imagen basada en debian:

Crear imagen de docker custom: https://github.com/fluent/fluentd-docker-image