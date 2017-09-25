Lenguaje declarativo (como puppet) para desplegar infraestructura.

Comparado con Ansible veo bastantes mejoras a la hora de desplegar infraestructura.
Por ejemplo, el esquema de terraform es crear recursos e ir pasando sus variables a los recursos de más adelante que lo necesiten.
Esto se puede realizar fácilmente también usando modulos, definiendo parámetros de entrada y de salida de ansible.

En ansible todas las variables se comparten de modo global, por lo que es muy dificil reusar un módulo varia veces (por ejemplo para crear unas redes) y luego usar alguna de las variables generadas por ese módulo para otra parte (no sabremos si esa variable es de la primera ejecucción del rol, de la segunda o cual)

Otra desventaja de Ansible es que si modificamos el playbook de nuestra infraestructura añadirá cosas, en vez de borrar lo antiguo y crear lo nuevo.