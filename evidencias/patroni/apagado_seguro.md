# Apagado seguro del laboratorio Patroni postgres-lab

## 1. Objetivo

Detener de forma segura el laboratorio Patroni `postgres-lab`, conservando intactos los demás servicios PostgreSQL del equipo.

## 2. Servicios identificados

| Servicio | Puerto | Resultado |
|---|---:|---|
| PostgreSQL main | 5435 | Permaneció activo |
| PostgreSQL Docker | 5432 | Permaneció activo |
| Patroni node1 | 5438 | Detenido |
| Patroni node2 | 5439 | Detenido |
| etcd del laboratorio | 2379 | Detenido |

El clúster PostgreSQL `manual` no se modificó.

## 3. Inspección previa

Antes de detener los procesos, se revisaron los procesos activos, los puertos y el estado de etcd:

```bash
ps -eo pid,ppid,user,args | grep -E '[p]atroni|[p]ostgres.*patroni-lab|[e]tcd'
sudo ss -ltnp | grep -E ':(5432|5435|5438|5439|2379)\b'
systemctl is-active etcd
```

Esto permitió identificar los procesos del laboratorio y distinguirlos de los servicios que debían permanecer activos.

## 4. Apagado de los nodos Patroni

Se utilizó `Ctrl+C` en las terminales correspondientes a los procesos de Patroni. No se utilizaron comandos generales como `pkill postgres` ni se detuvo el servicio global de PostgreSQL.

Después se verificó que los puertos 5438 y 5439 dejaran de escuchar y que no quedaran procesos de Patroni ni procesos PostgreSQL asociados a `patroni-lab`.

## 5. Apagado de etcd

Se revisó la configuración del servicio:

```bash
sudo systemctl status etcd --no-pager
sudo systemctl cat etcd
sudo cat /etc/default/etcd
```

Se identificaron estos parámetros:

- Nombre: `patroni-lab-etcd`.
- Directorio de datos: `/var/lib/etcd/patroni-lab`.
- URL de clientes: `http://127.0.0.1:2379`.
- Token inicial: `patroni-lab`.

Una vez detenidos los nodos
