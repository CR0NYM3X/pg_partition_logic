 
# 🚀 pg_partition_logic

**pg_partition_logic** es un framework de **Indexación Lógica** para PostgreSQL diseñado para eliminar el escaneo ineficiente en tablas particionadas de escala masiva (Terabytes).

Resuelve el problema nativo de PostgreSQL donde las búsquedas por columnas no particionadas (como un `id` o otra columna en una tabla particionada por `fecha`) ignoran el **Partition Pruning**, causando latencias críticas.

### 💎 El Diferencial

A diferencia de un índice tradicional, **pg_partition_logic** actúa como un "Índice Global Emulado" mediante una arquitectura de **Shadow Mapping**. Permite:

* **Pruning Forzado:** Reduce el tiempo de consulta de segundos a milisegundos inyectando predicados de partición automáticamente.
* **Esquema Dinámico:** Agrega nuevas columnas de búsqueda (`UUID`, `Emails`, `Tracking Codes`) sin reconstruir la tabla principal.
* **Carga de Misión Crítica:** Incluye un motor de *Backfilling* por lotes (Batching) para sincronizar tablas de Terabytes en producción sin bloqueos.

### 🛠️ Core Stack

* **Lenguaje:** PL/pgSQL (Triggers dinámicos y funciones de búsqueda).
* **Estrategia:** Subconsultas de Inyección de Predicado + Consultas Preparadas.
* **Compatibilidad:** PostgreSQL 12 a 18+.

---

### ⚡ Quick Start (Ejemplo de uso)

```sql
-- 1. Registra tu tabla masiva
SELECT crear_particion_logica('public', 'ventas', 'id_venta', 'fecha_emision');

-- 2. Agrega una columna extra de búsqueda rápida
SELECT agregar_columna_mapeo('ventas', 'codigo_hash');

-- 3. Busca en Terabytes en < 1ms
SELECT * FROM buscar_maestro('ventas', 'codigo_hash', 'H77-TX99');

```
 
