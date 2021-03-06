https://forge.puppetlabs.com/puppetlabs/stdlib

Funciones para trabajar con strings, arrays, etc.

join(["uno","dos","tres"],",") => "uno,dos,tres"

$var = join(["uno","dos","tres"],"','")
notify {"llena: '$var'":} => "llena: 'uno','dos','tres'"


parsejson($var) (mirar json.md)

empty
Returns true if the variable is empty.

Para los validate_ mirar validate.md

is_array
is_domain_name
is_float
is_function_available
  This function accepts a string as an argument, determines whether the Puppet runtime has access to a function by that name. It returns a true if the function exists, false if not.
is_hash
is_integer
is_ip_address
is_mac_address
is_numeric
is_string


ensure_packages
Takes a list of packages and only installs them if they don't already exist.



Define un recurso si no está definido ya. Mientras en el defined_with_params no haya un parámetro que contradiga el recurso definido, dara true.
Explicación: si no ponemos parámetros dara true. Si ponemos alguno de los parámetros dara true. Pero si ponemos algún parámetro distinto o que no está, dara false.
file { '/tmp/prueba':
  ensure => file,
  content => "cosas dentro",
}
if defined_with_params(File['/tmp/prueba'], {}) {
  notify{'file prueba creado':}
}


Más compacto:
This example only creates the resource if it does not already exist:
ensure_resource('user', 'dan', {'ensure' => 'present' })

Ejemplo más extenso:
ensure_resource(file, $target, {
  ensure => present,
  owner => $monitorizacion::params::user,
  require => Nagios_hostgroup[$servicename],
})

Si nos da duplicated resource puede ser porque en un sitio tenemos declarado el ensure_resource, y en otro no.
Puppet pasa primero por donde está el ensure, no está el recurso declarado, y se aplica.
Cuando pasa por la declaración del recurso sin ensure, da error.
https://ask.puppetlabs.com/question/2354/duplicate-declaration-error-occured-even-if-i-use-ensure_resource/


El truco para no tener exported resources duplicados es crearnos una clase propia que será la que exportemos.
Dentro de esa clase definimos el recurso que necesitamos con ensure_resource().
De esta manera, dos nodos pueden declarar el mismo exported resource (aunque tienen que tener distinto nombre, el $fqdn por ejemplo), y el host que recoje esos exported resources, usará ensure_resources para evitar duplicidades.
Idea sacada de: http://ttboj.wordpress.com/2013/06/04/collecting-duplicate-resources-in-puppet/


El parámetro alias parece que tiene algun significado especial, porque al definir una defined type con nombre distinto pero igual alias me dice:
Error: Failed to apply catalog: Parameter alias failed on Monitorizacion::Icinga::Hostgroup[m2m-client.com]: Munging failed for value "m2m generic group" in class alias: Cannot alias Monitorizacion::Icinga::Hostgroup[m2m-client.com] to "m2m generic group"; resource ["Monitorizacion::Icinga::Hostgroup", "m2m generic group"] already declared


Convertir un "false" en false
str2bool("false")



# file_line
https://github.com/puppetlabs/puppetlabs-stdlib/blob/master/lib/puppet/type/file_line.rb

Añadir una linea a un fichero.
Se puede hacer que sustituya a una existente (match =>)
'match' tiene que machear 'line' (al menos hasta que lo arreglen: https://github.com/puppetlabs/puppetlabs-stdlib/commit/a06c0d8115892a74666676b50d4282df9850a119#commitcomment-8289511)

A partir de la 4.2.0 existe
"after => "

No existe "before =>"
https://github.com/puppetlabs/puppetlabs-stdlib/pull/256

file_line { 'sudo_rule':
  path => '/etc/sudoers',
  line => '%sudo ALL=(ALL) ALL',
}

file_line { 'change_mount':
  path  => '/etc/fstab',
  line  => '10.0.0.1:/vol/data /opt/data nfs defaults 0 0',
  match => '^172.16.17.2:/vol/old',
}
