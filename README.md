# N8N
Contenedor de n8n Actualizable sin perder la data.

#Proceso de actualizacion.
Descargar la nueva imagen:


docker-compose pull n8n

Relanzar el contenedor: Docker detectará la nueva imagen y recreará el contenedor, pero mantendrá intacta tu carpeta n8n_data.
Bash

docker-compose up -d --force-recreate
¡Y listo! Tu n8n estará actualizado y todos tus flujos y credenciales seguirán ahí porque viven fuera del contenedor.
