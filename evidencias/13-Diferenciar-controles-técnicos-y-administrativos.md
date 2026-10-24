Medida,Técnico,Administrativo,Justificación
Crear roles de PostgreSQL,X,,Es un control implementado directamente mediante comandos en el motor de base de datos (CREATE ROLE).
Política de retención,(X),X,"Es principalmente un control administrativo (regla/documento que define cuánto tiempo guardar cada dato), aunque luego se apoya en controles técnicos (scripts de borrado) para ejecutarse."
Cifrado de datos,X,,Es una medida tecnológica aplicada sobre los datos o conexiones para proteger su confidencialidad.
Procedimiento para solicitudes ARCO,(X),X,"Es un control administrativo que define los pasos, responsables y tiempos legales para atender al titular; involucra una parte técnica cuando el DBA ejecuta la consulta en la BD."
Vista con información limitada,X,,Es un control técnico (CREATE VIEW) configurado en PostgreSQL para restringir qué columnas o filas puede ver un rol.
Clasificación de información,,X,"Es un control administrativo de gobierno de datos donde la organización define qué datos son públicos, internos, personales o sensibles."
Registro de auditoría,X,,"Es un control técnico (logs del sistema, triggers o extensiones de auditoría) que registra qué acciones se realizaron en la base de datos."
Política de control de acceso,,X,Es un control administrativo que define las reglas organizacionales sobre quién debe tener acceso a qué información; posteriormente el DBA la traduce en controles técnicos (roles y GRANT).
