Para tenerlo al inicio y siempre configurado usar keychain:
Meter en el ~/.bashrc
eval $(keychain -q -Q --eval --agents ssh ~/.ssh/tdaf.pem ~/.ssh/cyclops ~/.ssh/dsmctools.pem)

  necesitará esos ficheros y sus correspondientes .pub (el mismo nombre acabado en .pub)



Arrancar un "agente" ssh para poder ir pasando nuestra clave pública entre máquinas

1.- Arrancar el agente
eval `ssh-agent`

2.- Meter la clave al agente
ssh-add clave.pem

3.- Arrancar ssh con el agente (-A). Estamos pasando nuestras claves ssh a la maquina a la que estamos conectando
    Si host key falla (.ssh/known_hosts) no forwardeara las claves ssh por seguridad (podrian robarlas)
ssh -A maquina

4.- Al saltar desde 'maquina' a 'otramaquina' se usará clave.pem
maquina$ ssh otramaquina


Tambien nos sirve para meter distintas claves en el agente de distintas ubicaciones, y luego siempre que usemos -A probará todas esas claves.



Podemos usar gnome keychain para cargarlas las claves automáticamente al inicio:
~/.bashrc
keychain --agents ssh ~/.ssh/tdaf.pem ~/.ssh/cyclops ~/.ssh/dsmctools.pem



# Arrancar el agente
El comando ssh-agent lo que hace es exportar a la variable de entorno SSH_AUTH_SOCK la localización del socket donde escucha.

# Listar identidades
ssh-add -L
ssh-add -l
  más resumido, con el sha256sum de la clave (creo)

# Borrar identidades
ssh-add -D
  borra todas

ssh-add -d clave
  borrar una clave


# Elegir una única clave para usar
http://serverfault.com/questions/599560/use-a-specific-forwarded-key-from-ssh-agent

Obtener la clave pública y apuntar con el Identities a esa clave pública

ssh-add -L | grep /Users/doxna/.ssh/id_rsa.github > ~/.ssh/id_rsa.github.pub
ssh -v -o IdentitiesOnly=yes -i .ssh/id_rsa.github.pub git@github.com

# No usar el agente
SSH_AUTH_SOCK="" ssh -i clave.pem user@host
