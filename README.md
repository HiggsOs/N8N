# N8N
Contenedor de n8n Actualizable sin perder la data.

# Proceso de actualizacion.
Descargar la nueva imagen:


docker-compose pull n8n

Relanzar el contenedor: Docker detectará la nueva imagen y recreará el contenedor, pero mantendrá intacta tu carpeta n8n_data.
Bash

docker-compose up -d --force-recreate
¡Y listo! Tu n8n estará actualizado y todos tus flujos y credenciales seguirán ahí porque viven fuera del contenedor.

# Issues
Al lanzar la primera vez el contenedor no se va a lanzar correctamente porque se deben cambiar los permisos de la carpeta donde se va a guardar la informacion.

# solucion

Plan de Acción Definitivo
Paso 1: Detener todo completamente
Asegúrate de que no haya ningún contenedor de n8n corriendo.

Bash

docker-compose down
Si te da algún error, no te preocupes, el siguiente paso lo arreglará.

Paso 2: Verificar los permisos actuales (Diagnóstico)
Vamos a ver quién es el propietario actual de la carpeta n8n_data. Ejecuta este comando desde tu directorio ~/n8n-docker:

Bash

ls -ld ./n8n_data
Lo más seguro es que la salida se vea así, mostrando que root es el propietario:
drwxr-xr-x 2 root root 4096 Jun 17 10:30 ./n8n_data

Esto confirma que el usuario node (ID 1000) del contenedor no puede escribir aquí.

Paso 3: Cambiar el propietario de la carpeta (La Solución)
Ahora, ejecuta el comando para cambiar el propietario. Esto le dará el control total de la carpeta al usuario que el contenedor de n8n necesita.

Bash

sudo chown -R 1000:1000 ./n8n_data
Paso 4: Verificar los permisos de nuevo (Confirmación)
Ejecuta el mismo comando del paso 2 para confirmar que el cambio se aplicó:

Bash

ls -ld ./n8n_data
La salida ahora debería mostrar 1000 como el propietario y el grupo:
drwxr-xr-x 2 1000 1000 4096 Jun 17 10:30 ./n8n_data

Si ves 1000 1000, ¡perfecto! El problema de permisos en el servidor está resuelto.


Paso 5: Lanzar n8n
Ahora sí, con los permisos de la carpeta corregidos en el servidor, lanza el contenedor:

Bash

docker-compose up -d
Paso 6: Revisar los logs por última vez
Bash

docker-compose logs -f n8n
Esta vez, el error EACCES: permission denied debería haber desaparecido y deberías ver los mensajes de n8n indicando que se ha iniciado correctamente. Puede que veas un mensaje como Editor is now available on..., esa es la señal de que todo ha ido bien.

