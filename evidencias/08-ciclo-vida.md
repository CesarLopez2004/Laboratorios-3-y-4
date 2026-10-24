# Evidencia 08 — Derecho de cancelación y ciclo de vida

## Objetivo
Analizar qué ocurre con los datos de un titular a lo largo de toda la infraestructura cuando se autoriza una solicitud de Cancelación, explicando por qué un `DELETE` en la tabla principal no elimina automáticamente toda su información en la organización.

## Procedimiento
1. Se revisaron todos los puntos donde se replica, respalda o exporta la información de la tabla `clientes`.
2. Se construyó el mapa de dispersión del dato y se analizaron las implicaciones técnicas de cada repositorio.

## Resultado

### Mapa de ubicación de datos derivados de `clientes`

```text
clientes (Tabla principal)
   │
   ├── tablas relacionadas (clientes_soporte, facturas, pedidos)
   ├── respaldos (archivos pg_dump, snapshots e históricos)
   ├── réplicas (servidores secundarios de lectura / standby)
   ├── registros de auditoría (logs de base de datos y bitácoras)
   ├── reportes y copias de analítica (vistas materializadas, BI)
   ├── exportaciones (archivos CSV o Excel descargados por usuarios)
   └── sistemas externos (CRM, sistemas de facturación PAC, correo)
Respuesta a la pregunta:
¿Por qué eliminar un registro de una tabla no necesariamente significa que toda la información relacionada con esa persona haya desaparecido de la organización?

Porque la base de datos productiva no es un sistema aislado; los datos se copian y propagan continuamente:

Tablas relacionadas: Si se copió información a clientes_soporte o tablas de facturación sin borrado en cascada (o si la ley fiscal impide borrar facturas por 5 años), los datos siguen existiendo en esas tablas.

Respaldos (Backups): Los respaldos anteriores contienen el registro eliminado. Los respaldos no se editan manualmente porque se corrompería su integridad; el dato permanece ahí hasta que el respaldo expire según su ciclo de rotación.

Réplicas y cachés: Puede existir un desfase temporal o copias en servidores de lectura/reporteo.

Registros de auditoría (Logs): Los archivos de log del servidor pueden conservar el rastro de inserciones o actualizaciones previas con los datos del cliente.

Archivos exportados y sistemas externos: Si un empleado exportó previamente la tabla a un archivo Excel/CSV en su computadora, o si el dato se envió a un proveedor externo, ejecutar DELETE FROM clientes en PostgreSQL no borra esos archivos externos.

Análisis
Atender el derecho de Cancelación implica gestionar todo el ciclo de vida del dato: primero verificar si existe obligación legal de conservarlo en bloqueo (por ejemplo, por temas fiscales), eliminar o anonimizar en las tablas operativas y secundarias (clientes_soporte), evitar las exportaciones descontroladas a CSV, y llevar un registro de IDs cancelados para que no se reactiven si algún día se restaura un respaldo antiguo.

Conclusión
Un comando DELETE es solo una parte técnica del proceso de cancelación. El verdadero control requiere conocer dónde vive cada copia del dato y limitar la creación de copias innecesarias dentro de la organización.
