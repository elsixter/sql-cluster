# Práctica: Failover y replicación con Patroni

## 1. Objetivo

Configurar y comprobar un clúster de PostgreSQL administrado
por Patroni, verificando la replicación entre dos nodos y
la recuperación después de detener al líder.

## 2. Entorno de laboratorio

- Sistema operativo: Ubuntu Linux.
- PostgreSQL: versión 16.
- Patroni: versión 3.2.2.
- etcd: versión 3.4.30.
- Clúster: `postgres-lab`.
- Nodo 1: `10.0.2.15:5438`.
- Nodo 2: `10.0.2.15:5439`.
- Coordinación: etcd en `127.0.0.1:2379`.

Este laboratorio utiliza dos nodos en una misma máquina virtual.
Por tanto, simula el failover entre procesos, no la caída de un
servidor físico o virtual completo.

## 3. Estado inicial

Inicialmente, `node1` era el líder y `node2` funcionaba como réplica.

Se utilizó el siguiente comando para consultar los roles y el
estado de los nodos:

```bash
sudo -u postgres patronictl -c /etc/patroni-lab/node1.yml list
```

## 4. Verificación de la replicación

Se creó la tabla `public.prueba_replicacion` y se insertaron
registros de prueba en el líder.

```sql
SELECT * FROM public.prueba_replicacion ORDER BY id;
```

Se verificó que los registros estuvieran disponibles en la réplica.

## 5. Prueba de failover

Se detuvo el proceso de Patroni del nodo líder `node1` para
simular su indisponibilidad.

Patroni coordinó la elección de `node2` como nuevo líder.
El nodo `node2` quedó operativo y aceptó nuevas escrituras.

La elección se reflejó en la línea de tiempo 2 de PostgreSQL.

## 6. Reincorporación de node1

Se volvió a iniciar Patroni para `node1`.

Durante el arranque se detectó un error de permisos porque un
proceso secundario intentaba acceder al directorio `/home/JUAREZ`.

Se inició Patroni desde `/tmp`, un directorio accesible para el
usuario `postgres`, con el siguiente comando:

```bash
sudo -u postgres sh -c 'cd /tmp && exec /usr/bin/patroni /etc/patroni-lab/node1.yml'
```

Después, `node1` volvió a incorporarse como réplica.

## 7. Estado final

Se consultó el estado del clúster con:

```bash
sudo -u postgres patronictl -c /etc/patroni-lab/node2.yml list
```

Resultado verificado:

- `node1`: Replica, estado `streaming`, timeline 2.
- `node2`: Leader, estado `running`, timeline 2.
- Retraso de replicación reportado: 0 MB.

Se ejecutó la consulta de verificación en ambos nodos:

```sql
SELECT * FROM public.prueba_replicacion ORDER BY id;
```

Ambos devolvieron los mismos tres registros de prueba.

## 8. Conclusión

La práctica permitió comprobar el cambio de líder, la continuidad
del servicio de escritura en el nuevo líder y la reincorporación
de la réplica sincronizada.

La prueba se realizó en una sola máquina virtual, por lo que no
representa alta disponibilidad ante la pérdida completa del host.

## 9. Evidencias

- `estado_final.txt`: salida guardada del estado del clúster.
- Las capturas de pantalla pueden añadirse a esta carpeta cuando
  estén disponibles.

