No puede correr al mismo tiempo que KVM.


VERR_NO_MEMORY
Si da al intentar abrir un .ova:
tar xf fichero.ova
Abrir con virtualbox el .ovf


Casque al montar una copia de un .vdi de otra VM
Es porque el uuid ya está siendo utilizando. Tendremos que cambiarlo
http://stackoverflow.com/questions/17803331/how-to-change-uuid-in-virtual-box

xManage internalcommands sethduuid imagen.vdi




Protesta por ciertos modulos:
NO instalar los paquetes virtualbox-guest-*
Si los teniamos instalados, revisar que hemos borrrado los modules que metia.


VT-x is disabled in the BIOS for all CPU modes (VERR_VMX_MSR_ALL_VMX_DISABLED)
Activar este modo en la bios

wget https://bazaar.launchpad.net/~cpu-checker-dev/cpu-checker/trunk/download/head:/kvmok-20100309223031-mmnp5uz31a5de08g-1/kvm-ok
chmod a+x kvm-ok
./kvm-ok

Si está desactivada en la bios, en el dmesg veremos mensajes:
kvm: disabled by bios
