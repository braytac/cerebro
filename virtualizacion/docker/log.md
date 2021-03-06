https://docs.docker.com/engine/admin/logging/view_container_logs/

Truco redirect syslog:
Una buena idea es coger el /dev/log de la máquina (donde se escriben los logs para syslog) y apuntarlo a nuestra máquina.
Así conseguiremos tener en nuestro /var/log local los logs generados por la app

docker run ... -v /dev/log:/dev/log ...


Existen distintos drivers para los logs. De esta manera los podremos enviar a: syslog, journald, splunk, fluentd, etc


# Logging drivers
Por defecto esta configurado el driver json-file.
Ejemplo de path:
/var/lib/docker/containers/e0e92c39f045c038c221e0162b771e2aa63e124b329399453ae37f2e9e4d4404/e0e92c39f045c038c221e0162b771e2aa63e124b329399453ae37f2e9e4d4404-json.log



Cada container puede correr un driver distinto.
Para conocer cual está usando:
docker inspect -f '{{.HostConfig.LogConfig.Type}}' CONTAINER

O con las opciones del logger:
docker inspect -f '{{.HostConfig.LogConfig}}' CONTAINER


Si queremos cambiar el driver a nivel global:
/etc/docker/daemon.json
{
  "log-driver": "syslog"
}


https://docs.docker.com/config/containers/logging/json-file/
Limitar número y tamaño (no parece que podamos preguntar al docker la running config para ver las log-opts):
docker run --log-opt max-size=10m --log-opt max-file=5 my-app:latest

my-app:
    image: my-app:latest
    logging:
        driver: "json-file"
        options:
            max-file: 5
            max-size: 10m

También a nivel de daemon
/etc/docker/daemon.json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}


Para arrancar un container con un log-driver especifico:
docker run --log-driver journald ...

Journald no funciona bien si se generan muchas trazas. Mejor usar fichero.

Cuidado con enviar muchas trazas a journald. Por defecto journald tiene un límite de 1000 mensajes cada 30"
Se generará un mensaje tipo:
Oct 02 03:17:11 app6 systemd-journal[565]: Suppressed 25193 messages from /system.slice/docker.service

Podemos filtrar estos mensajes con:
journalctl -u systemd-journald



Lista de drivers disponibles:
https://docs.docker.com/engine/admin/logging/overview/#supported-logging-drivers



Fluentd: https://docs.docker.com/engine/admin/logging/fluentd/
Enviar los logs a un container donde este escuchando fluentd.
Si no esta levantado fluentd, no nos deja levantar containers que usen este log driver.


A partir de 17.05 tambien se puede acoplar logging plugins, que extienden el set de logging outputs diponibles:
https://docs.docker.com/engine/admin/logging/plugins/

Con --log-opts podremos pasarle parametros al plugin

Logging plugin para enviar a redis: https://github.com/pressrelations/docker-redis-log-driver
En caso de no poder conectar con redis hace drop del log:
https://github.com/pressrelations/docker-redis-log-driver/blob/master/driver/driver.go#L148



# fluentd leyendo de journald (docker escribiendo en journald)
plugin journald (para que fluentd lea de journald)
https://github.com/reevoo/fluent-plugin-systemd
Hace falta montar /var/log/journal en el container.

# Enviar logs a Elastic Search
Mejor opción, que docker escriba a journald.
Fluentd lea de journald y envie a redis.
Y de redis se lleve a ES con logstash.

De esta manera tenemos una capa de cacheo por si ES esta down.
Y tambien cacheo en journald si fluentd cae.
