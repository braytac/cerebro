https://github.com/sadovnychyi/gae-console/

pip install gae-console --target .
Añadir a app.yml
includes:
- gae_console/include.yaml

dev_appserver.py .
localhost:8080/_ah/console

Control+b para ejecutar

Para ejecutar el programa principal:
import main
main.funcion()

Se guarda el estado entre ejecucciones
