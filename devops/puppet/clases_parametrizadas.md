Nos permiten modificar las variables definidas en esa clase

Si no le damos valor por defecto, hay que pasarle el parametro obligatoriamente.
class motd(
  $hola,
  $var = 'mundo',
  $otra = undef,
)
{
  file {'/etc/motd':
    ensure => present,
    content => template('motd/motd.erb'),
  }
}


# Ahora podemos llamar a la clase como si fuese un recurso:
class { 'motd': 
  hola => "pepe",
  var => 'GRILLO',
  param => inline_template("<%= @var || 'por defecto' %>"),
}

No llamarlas con "include motd", ya que los parámetros no tomarán sus valores por defecto.


Para ejecutarlas localmente:
puppet apply -e "class {'nombre': }"


Podemos hacerun chequeo de los parámetros con los métodos 'validate_' de stdlib: http://forge.puppetlabs.com/puppetlabs/stdlib


Si definimos una variable como undef, podremos llamar a la clase sin estar obligados a definir la varible, pero la variable seguirá sin estar definida al llamar a la clase.


No se puede redefinir variables:
class prueba (
  $var = undef,
) {
  $var = "nuevo"
  notify {"var: $var":}
}
Error: Cannot reassign variable var at /tmp/pruebas/test.pp:20 on node host


