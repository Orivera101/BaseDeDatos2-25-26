# Observabilidad y monitoreo básico

Un sistema productivo requiere visibilidad.

No basta con que funcione.
Debe poder medirse.

Herramientas básicas:

### db.serverStatus()

Nos da métricas como:

* Opcounters (lecturas/escrituras)
* Memoria
* Locks
* Conexiones activas

Explain como herramienta continua:
No solo para desarrollo.
También para diagnóstico en producción.

Indicadores críticos:

* totalDocsExamined muy alto.
* Uso frecuente de COLLSCAN.
* Tiempo creciente en consultas repetidas.

En MongoDB Atlas:
Se pueden observar:

* CPU
* IOPS
* Uso de disco
* Latencia
* Balanceo entre shards

Una buena práctica profesional:

Establecer métricas base.
Comparar después de cambios.
Nunca optimizar sin medir.

