# Autenticación de PostgreSQL con pg_hba.conf

## Objetivo

Comprender cómo PostgreSQL controla las conexiones mediante el archivo `pg_hba.conf` y configurar la autenticación mediante contraseña en un clúster manual de Linux.

## Ubicación del archivo

En el clúster manual utilizado para esta práctica, el archivo se encuentra en:

`/var/lib/postgresql/manual/pg_hba.conf`

Para consultar su ubicación efectiva:

```sql
SHOW hba_file;
```

## Estructura de las reglas

Las reglas utilizan las siguientes columnas:

- `TYPE`: tipo de conexión, como `local` o `host`.
- `DATABASE`: bases de datos a las que aplica la regla.
- `USER`: roles de PostgreSQL a los que aplica.
- `ADDRESS`: dirección IP de origen para conexiones de tipo `host`.
- `METHOD`: método de autenticación.

## Métodos de autenticación

### trust

Permite conectarse sin comprobar una contraseña. No es apropiado para entornos que requieren autenticación segura.

### scram-sha-256

Permite autenticar al usuario mediante una contraseña utilizando el mecanismo SCRAM-SHA-256.

## Práctica realizada

Se configuró una contraseña para el rol `postgres` y se modificó la regla IPv4 de conexiones normales:

Antes:

```text
host    all    all    127.0.0.1/32    trust
```

Después:

```text
host    all    all    127.0.0.1/32    scram-sha-256
```

La regla de replicación se dejó sin cambios.

## Aplicar cambios

Se utilizó `pg_ctl` para recargar la configuración sin detener el servidor:

```bash
sudo -u postgres /usr/lib/postgresql/16/bin/pg_ctl \
  -D /var/lib/postgresql/manual reload
```

## Verificación

Se probó una conexión TCP/IP solicitando la contraseña:

```bash
psql -h 127.0.0.1 -p 5437 -U postgres -d postgres -W
```

Después se verificó el usuario y la base de datos:

```sql
SELECT current_user, current_database();
```

La conexión fue exitosa. Para demostrar que la contraseña se verifica realmente, también se puede realizar una prueba controlada con una contraseña incorrecta y comprobar que PostgreSQL rechaza el acceso.

## Precauciones

- Hacer una copia de seguridad de `pg_hba.conf` antes de modificarlo.
- Cambiar solo la regla necesaria.
- Revisar la configuración antes de recargarla.
- No utilizar `trust` indiscriminadamente en entornos de producción.
