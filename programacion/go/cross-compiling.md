http://kelcecil.com/golang/2015/10/24/cross-compiling-in-go-1.5.html
https://coderwall.com/p/pnfwxg/cross-compiling-golang

En un mac generar un binario lixux:
GOOS=linux GOARCH=amd64 go build -o hello main.go

En linux generar mac:
GOOS=darwin GOARCH=amd64 go build/install...

Para raspberry pi zero:
GOOS=linux GOARCH=arm GOARM=6

GOOS=windows GOARCH=amd64 go build/install

32bits -> 386

Siempre poner las dos variables!


# Diferentes builds por plataforma
https://dave.cheney.net/2013/10/12/how-to-use-conditional-compilation-with-the-go-build-tool

Podemos definir determinados ficheros que solo se compilarán si goos y/o goarch son los definidos.

Esta selección podemos hacerla poniendo una tag al comienzo del fichero (luego hay que dejar una línea en blanco):
// +build darwin dragonfly freebsd linux netbsd openbsd

O nombrando el fichero con el os/arch:
mypkg_linux.go         // only builds on linux systems
mypkg_windows_amd64.go // only builds on windows 64bit platforms

Podemos usar negación:
// +build !darwin,!linux,!freebsd,!openbsd,!windows


Si queremos usar una función que no existe en un grupo de máquinas, definiremos la misma función en dos ficheros, unix (por ejemplo) y fallback (donde no existe).
La función retornará vació para el grupo que no funciona.
