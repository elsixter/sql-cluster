# Configuración de PostgreSQL en Linux

## Archivo principal

El archivo `postgresql.conf` contiene parámetros que controlan
el funcionamiento del servidor PostgreSQL.

En el clúster manual se encuentra en:

`/var/lib/postgresql/manual/postgresql.conf`

## Parámetros revisados

- `port`: puerto utilizado por el servidor. En nuestro clúster es `5437`.
- `listen_addresses`: direcciones IP en las que el servidor escucha.
- `max_connections`: límite de conexiones simultáneas.
- `shared_buffers`: memoria compartida utilizada para almacenar páginas de datos.
- `logging_collector`: controla la captura de mensajes de registro.
- `log_directory`: directorio para los registros cuando se utiliza el recolector.

## Consultar valores efectivos

Los valores configurados en el archivo pueden diferir de los que utiliza
el servidor. Para consultar los valores efectivos se pueden usar:

```sql
SHOW port;
SHOW max_connections;
SHOW shared_buffers;
