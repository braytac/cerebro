Información de la CPU

lscpu

cpuid



# Real mode
https://en.wikipedia.org/wiki/Real_mode

Real mode, also called real address mode, is an operating mode of all x86-compatible CPUs. Real mode is characterized by a 20-bit segmented memory address space (giving exactly 1 MiB of addressable memory) and unlimited direct software access to all addressable memory, I/O addresses and peripheral hardware

Es el estado en el que arranca la CPU.

Para acceder a la memoria se usa un selector de segmento (los segmentos son de 64KB) y otro registro para el offset.
De este modo conseguimos 1MB de memoria addressable.

# Protected mode
https://0xax.gitbooks.io/linux-insides/content/Booting/linux-bootstrap-2.html

En este modo ya tenemos acceso a 4GB de RAM (gracias a un nuevo bus de 32 bits)

Para acceder a la memoria se usa una estructura llamada Global Descriptor Table (GDT). Esta tabla se almacena en memoria y el registro GDTR es quien tiene la dirección de memoria de la tabla.

En la tabla se almacenan estructuras de 64 bits.
  20 bits son para definir el tamaño al que apunta
  32 bits es para apuntar el comienzo de la posición de memoria
  El flag S define si el segmento es de sistema (0), o si es de tipo código o datos
  Para distinguir si es code o data chequearemos el bit 43.
  Con otros bits se definirá específicamente que tipo de registro es:
    Data/Code
    Read-Only  Read/Write  Execute-Only  Execute/Read
    accessed, expand-down (http://www.sudleyplace.com/dpmione/expanddown.html), conforming (si es 1, puede ser ejecutado por lower-level, user-level)



# CPU class, temperatura
https://www.apple.com/newsroom/2020/03/apple-unveils-new-ipad-pro-with-lidar-scanner-and-trackpad-support-in-ipados/

U/Y/H. This letter determines the TDP (thermal design power/point) them machine is designed to run at. The TDP will govern the setting for the base clock speed, and, just as importantly, the throttling behabior under load.
Processor series TDPs are Y: 4.5W, U: 15W, H: 45W.

The new MacBook Air appears to have a Y series processor, like the MacBook, which means it will be aggressively throttled to keep power consumption and heat generation low.
Practically, that means that the new Air will not be capable of running sustained workloads much above its base clock speed, which makes it unsuitable for many programming-related tasks.
