# Creación manual de un clúster PostgreSQL en Linux

## Objetivo

Crear un clúster PostgreSQL de forma manual utilizando `initdb` y administrarlo con `pg_ctl`, sin modificar el clúster existente.

## 1. Preparar el directorio

```bash
sudo install -d -o postgres -g postgres /var/lib/postgresql/manual
```

Crea el directorio y asigna su propiedad al usuario del sistema `postgres`.

## 2. Inicializar el clúster

```bash
sudo -u postgres /usr/lib/postgresql/16/bin/initdb -D /var/lib/postgresql/manual
```

`initdb` genera los archivos y directorios necesarios para que PostgreSQL pueda funcionar como un clúster.

## 3. Configurar el puerto

Archivo de configuración:

`/var/lib/postgresql/manual/postgresql.conf`

Se configura el puerto `5437` para mantener el clúster separado del clúster existente.

```text
port = 5437
```

## 4. Iniciar el servidor

```bash
sudo -u postgres /usr/lib/postgresql/16/bin/pg_ctl \
-D /var/lib/postgresql/manual \
-l /var/lib/postgresql/manual/server.log \
start
```

## 5. Verificar el estado del clúster

```bash
sudo -u postgres /usr/lib/postgresql/16/bin/pg_ctl \
-D /var/lib/postgresql/manual status
```

Permite comprobar si el servidor está en ejecución.

## 6. Conectarse al clúster

```bash
sudo -u postgres psql -p 5437 -d postgres
```

Dentro de `psql`, verificar el puerto y el directorio de datos:

```sql
SHOW port;
SHOW data_directory;
```

Los resultados esperados son `5437` y `/var/lib/postgresql/manual`, respectivamente.

Para salir de `psql`:

```text
\q
```

## 7. Detener el clúster

```bash
sudo -u postgres /usr/lib/postgresql/16/bin/pg_ctl \
-D /var/lib/postgresql/manual stop
```

El comando solicita un apagado controlado del servidor.

## 8. Reiniciar el clúster

```bash
sudo -u postgres /usr/lib/postgresql/16/bin/pg_ctl \
-D /var/lib/postgresql/manual \
-l /var/lib/postgresql/manual/server.log \
start
```

Después del inicio, comprobar nuevamente el estado con `pg_ctl status`.

## Resultado

Se creó y administró un clúster PostgreSQL independiente en `/var/lib/postgresql/manual`, configurado para utilizar el puerto `5437`. Se practicaron las operaciones de inicio, consulta de estado, conexión y apagado controlado sin modificar el clúster existente.


- `-D`: especifica el directorio del clúster.
- `-l`: indica dónde guardar el registro del servidor.
- `start`: inicia PostgreSQL.

## Conceptos aprendidos

- **Clúster:** conjunto de bases de datos administradas por una instancia de PostgreSQL.
- **Directorio de datos:** ubicación de los archivos que conforman el clúster.
- **`initdb`:** inicializa el directorio de datos.
- **`pg_ctl`:** permite iniciar, detener y controlar el servidor.
- **Puerto:** punto de conexión utilizado por los clientes para comunicarse con PostgreSQL.

## Nota

Este clúster se creó manualmente y se administra con `pg_ctl`. No debe confundirse con los clústeres gestionados por las herramientas de Ubuntu, como `pg_createcluster`.

