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

