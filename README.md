# SQL y Administración de PostgreSQL

Repositorio de aprendizaje práctico enfocado en SQL, administración de PostgreSQL en Linux y gestión de clústeres de bases de datos. Documenta ejercicios, comandos, configuraciones, pruebas de funcionamiento y procedimientos realizados en un entorno de laboratorio con Ubuntu.

## Objetivo

Desarrollar habilidades para consultar y manipular datos con SQL, administrar instancias de PostgreSQL, configurar clústeres, gestionar usuarios y permisos, y comprender los mecanismos de replicación y recuperación ante fallos.

## Temas y prácticas realizadas

### 1. SQL y consultas a bases de datos

- Consultas con `SELECT`, filtros y operadores lógicos.
- Uso de `AND`, `OR`, subconsultas y operadores de comparación.
- Agrupaciones con `GROUP BY` y filtros con `HAVING`.
- Funciones de agregación y consultas con varias condiciones.
- Operaciones CRUD: `INSERT`, `UPDATE` y `DELETE`.
- Administración de roles, usuarios y permisos en PostgreSQL.

### 2. Administración de clústeres PostgreSQL en Linux

- Exploración de clústeres y procesos de PostgreSQL en Ubuntu.
- Inicialización de un clúster con `initdb`.
- Inicio, detención y consulta del estado con `pg_ctl`.
- Inspección de directorios de datos y archivos de configuración.
- Configuración de `postgresql.conf`, puertos y parámetros del servidor.
- Revisión de autenticación y acceso mediante `pg_hba.conf`.
- Consulta del estado de los clústeres y diagnóstico de errores de arranque.

### 3. Alta disponibilidad y replicación con Patroni

Práctica de laboratorio con dos nodos PostgreSQL administrados por Patroni y coordinados mediante etcd.

**Entorno utilizado:**
- Ubuntu Linux.
- PostgreSQL 16.15.
- Patroni 3.2.2.
- etcd 3.4.30.

**Configuración del laboratorio:**
- `node1`: puerto PostgreSQL `5438`.
- `node2`: puerto PostgreSQL `5439`.
- Clúster Patroni: `postgres-lab`.
- Coordinación mediante etcd en `127.0.0.1:2379`.

**Pruebas realizadas:**
- Configuración de un nodo líder y una réplica.
- Verificación de la replicación mediante una tabla de prueba.
- Simulación de la indisponibilidad del proceso Patroni del líder.
- Elección de `node2` como nuevo líder (*failover*).
- Comprobación de escrituras en el nuevo líder.
- Reincorporación de `node1` como réplica.
- Verificación del estado `streaming`, la línea de tiempo 2 y el retraso reportado de 0 MB.
- Comparación de los registros de prueba en ambos nodos después de la recuperación.

**Resultado:** ambos nodos quedaron operativos, con `node2` como líder y `node1` como réplica sincronizada.

> El laboratorio utiliza dos nodos en una misma máquina virtual. Por ello, permite practicar el failover entre procesos, pero no demuestra tolerancia a la caída completa del servidor anfitrión.

## Documentación y evidencias

### Administración manual de PostgreSQL

Los procedimientos de administración se documentan en `docs/postgresql/`:

- `manual-postgresql-cluster-linux.md`: creación manual de un clúster.
- `cluster-manual-linux.md`: ciclo de vida y operación del clúster.
- `configuracion-postgresql.md`: parámetros y configuración del servidor.

### Laboratorio Patroni

La documentación y las evidencias de la práctica de failover se encuentran en `evidencias/patroni/`:

- `practica_failover.md`: procedimiento, pruebas y resultados.
- `estado_final.txt`: salida de `patronictl list` con el estado final de los nodos.

## Herramientas utilizadas

- **Sistema operativo:** Ubuntu Linux.
- **Gestor de bases de datos:** PostgreSQL.
- **Administración de clústeres:** `initdb`, `pg_ctl` y `patronictl`.
- **Alta disponibilidad:** Patroni y etcd.
- **Control de versiones:** Git y GitHub.

## Enfoque de aprendizaje

El repositorio registra el progreso mediante ejercicios SQL, prácticas de administración, configuración de servicios, diagnóstico de errores y pruebas reproducibles. Cada práctica busca explicar no solo los comandos utilizados, sino también su propósito y los resultados obtenidos.

Las actividades se desarrollan en un entorno de laboratorio y se documentan progresivamente a medida que se incorporan nuevos conocimientos.

