for f in *.png ; do convert $f $(basename $f .png).gif ; done
gifsicle --delay=200 --loop *.gif > anim.gif

sudo pacman -S gifsicle


Varias imagenes a gif:
convert output/* output.gif


En Gimp:
Meter las imágenes en capas.
Exportar como GIF


Crear gif a partir de un video:
mplayer -ao null <video file name> -vo jpeg:outdir=output
