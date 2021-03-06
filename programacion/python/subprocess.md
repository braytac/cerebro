mirar sh.md

python3 (stderr a su propia pipe, si queremos que valla a stdout poner stderr=subprocess.STDOUT)
>>> subprocess.run(["ls","-53y"], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
CompletedProcess(args=['ls', '-53y'], returncode=2, stdout=b"ls: opci\xc3\xb3n inv\xc3\xa1lida -- '5'\nPruebe 'ls --help' para m\xc3\xa1s informaci\xc3\xb3n.\n")


py2
subprocess.check_output(["ls", "-la"])


python2 (para py2.3 https://docs.python.org/2.3/lib/os-newstreams.html os.popen, putenv para definir variables de entorno)
import subprocess
returncode = subprocess.call(["ls", "-l"])
Esto saca el stdout y stderr directamente, no podemos almacenarlo en una variable.

returncode = subprocess.call("ls -l", shell=True)
  para poder usar redirecciones, pipes, etc
  no recomendado, hace un spawn de una shell


Metodo para obtener la salida del comando (en este caso, stderr y stdout mezcladas), hacer stderr=subprocess.STDOUT

Sacar stdout y stderr en distintas variables

import subprocess
try:
    # CUIDADO con usar shlex, puede hacer que nos escapen el comando y ejeuten otras cosas
    # Mejor meter el comando con un array
    p = subprocess.Popen(shlex.split(command), stdout=subprocess.PIPE, stderr=subprocess.stderr, universal_newlines=True)
    (stdout, stderr) = p.communicate() # Espera a que termine el comando. Si no esperamos, no tendremos el return code definido
    return_code = p.returncode
    message = stdout.strip()
except Exception as e:
    raise Exception("Error executing command: {}".format(str(e)))


Usar https://docs.python.org/3.2/library/subprocess.html
https://pypi.python.org/pypi/subprocess32/
Para usarlo en python2.7
pip install subprocess32

Backport of the subprocess module from Python 3.2/3.3 for use on 2.x.


subprocess.Popen
  ejecuta el programa en un nuevo proceso

from subprocess32 import Popen,PIPE,STDOUT
with Popen(["ls","-a"], stdout=PIPE, stderr=STDOUT) as proc:
    print(proc.stdout.read())

También se le puede pasar
cwd=
env=
etc

proc.pid
prod.returncode
proc.args
...

Escribir en un fichero:
fd = open('output.txt', 'w')
Popen(["ls","-7a"], stdout=fd, stderr=STDOUT)

si queremos sacarlo tambien por stdout
fd.flush()
fd.seek(0)
fd.readlines()



Salida en tiempo real, va sacando cada línea según se va produciendo:
http://stackoverflow.com/questions/18421757/live-output-from-subprocess-command

from subprocess32 import Popen,PIPE
with Popen(["/tmp/script.sh"], stdout=PIPE) as proc:
  for line in iter(proc.stdout.readline, ''):
    print(line.rstrip()) #rstrip() para no pintar lineas en blanco


Reglas para usar subprocess
Never use shell=True. It needlessy invokes an extra shell process to call your program.
When calling processes, arguments are passed around as lists. sys.argv in python is a list, and so is argv in C. So you pass a list to Popen to call subprocesses, not a string.
Don't redirect stderr to a PIPE when you're not reading it.
Don't redirect stdin when you're not writing to it.


# Stdin
Enviando cosas por el stdin
from subprocess import Popen, PIPE, STDOUT

p = Popen(['grep', 'f'], stdout=PIPE, stdin=PIPE, stderr=STDOUT)
grep_stdout = p.communicate(input=b'one\ntwo\nthree\nfour\nfive\nsix\n')[0]
print(grep_stdout.decode())


# Variables de entorno
import subprocess, os
my_env = os.environ.copy()
my_env["PATH"] = "/usr/sbin:/sbin:" + my_env["PATH"]
subprocess.Popen(my_command, env=my_env)



# VIEJO #
https://docs.python.org/2/library/subprocess.html#replacing-older-functions-with-the-subprocess-module

import subprocess
subprocess.call("ls")



from subprocess import Popen, PIPE

p = Popen(["ls", "-l", "file"], stdout=PIPE)
# Al ejecutar Popen se arranca el nuevo proceso en segundo plano
# No termina, se queda en modo defunct.
# Si lo que estamos lanzando es un proceso con sudo, el hijo sera de root y no podremos matarlo con p.kill() o similares
# http://stackoverflow.com/questions/21886828/killing-a-subprocess-started-via-sudo

output = p.communicate()[0]
# Si pedimos el output antes de que termine el programa parara la ejecuccion hasta tener el valor

print(p.returncode)


# Mandar stderr a /dev/null
import os
from subprocess import Popen, PIPE

FNULL = open(os.devnull, 'w')
proc = Popen(["/home/adrian/dsn/platon/src/scripts/inventory.py"], stdout=PIPE, stderr=FNULL)

