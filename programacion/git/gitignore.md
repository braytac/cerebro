Ponemos en el fichero los ficheros que no queremos de los que se haga track (no nos avisará de que no están en el repo cuando hagamos git status)

Ejemplo
.gitignore:
*.pyc
*.swp
~*


Ver que ficheros se están ignorando:
git ls-files --others -i --exclude-standard
