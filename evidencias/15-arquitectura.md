# Evidencia 15 — Propuesta de arquitectura de datos protegidos

## Objetivo
Diseñar una propuesta conceptual de arquitectura de base de datos que separe la información de acuerdo con su finalidad (Operación, Analítica y Auditoría), integrando controles de privacidad y seguridad.

## Procedimiento
1. Se tomó como base el esquema conceptual de separación en tres dominios: Operación, Analítica y Auditoría (más un entorno separado para Desarrollo).
2. Se definieron los controles específicos para acceso, minimización, protección de información sensible, retención, trazabilidad, eliminación y atención de solicitudes ARCO.

## Resultado

### Diagrama de Arquitectura por Finalidad

```text
                               BASE DE DATOS (PostgreSQL 16)
                                             │
            ┌────────────────────────────────┼────────────────────────────────┐
            │                                │                                │
            ▼                                ▼                                ▼
     [1. OPERACIÓN]                   [2. ANALÍTICA]                   [3. AUDITORÍA]
   (Esquema: operacion)             (Esquema: analitica)             (Esquema: auditoria)
            │                                │                                │
            ▼                                ▼                                ▼
  Datos necesarios por área:       Datos agregados / anónimos:      Trazabilidad y control:
  ├── T: clientes_soporte          ├── V: v_clientes_ciudad         ├── T: bitacora_arco
  │   (id, nombre, correo, tel)    ├── V: v_rangos_edad             ├── T: log_accesos
  ├── T: clientes_facturacion      └── V: v_region_cp               └── T: control_eliminados
  │   (id, nombre, rfc, cp)            (Sin nombre, correo ni RFC)      (Evidencia de cambios)
  └── T: control_consentimiento
            │
            └──────► [Proceso de Enmascaramiento / Datos Ficticios] ──► [Entorno de Desarrollo/Pruebas]
