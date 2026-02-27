# Ejercicios

## Ejercicio 1 — Sparse Index

1. Crear colección usuarios con algunos documentos que tengan campo telefonoAlternativo.
2. Crear índice normal sobre telefonoAlternativo.
3. Crear índice sparse.
4. Comparar getIndexes().
5. Ejecutar consultas con explain().

```JS
use academia

db.usuarios.drop()
db.usuarios.insertMany([
  { nombre: "Ana", telefonoAlternativo: "1111" },
  { nombre: "Luis" },
  { nombre: "Carlos", telefonoAlternativo: "2222" },
  { nombre: "Marta" },
  { nombre: "Elena", telefonoAlternativo: "3333" }
])

db.usuarios.createIndex({ telefonoAlternativo: 1 })

db.usuarios.getIndexes()

db.usuarios.dropIndex({ telefonoAlternativo: 1 })

db.usuarios.createIndex(
  { telefonoAlternativo: 1 },
  { sparse: true }
)

db.usuarios.getIndexes()

db.usuarios.find(
  { telefonoAlternativo: { $exists: true } }
).explain("executionStats")
```

Preguntas:

* ¿Cuántos documentos indexa cada uno?
* ¿Cuál es el tamaño del índice?

## Ejercicio 2 — Hashed Index

1. Crear colección ventas con campo clienteId.
2. Crear índice hashed.
3. Ejecutar consulta exacta.
4. Intentar consulta por rango.

```JS
use tienda

db.ventas.drop()
db.ventas.insertMany([
  { clienteId: 1001, total: 200 },
  { clienteId: 1002, total: 150 },
  { clienteId: 1003, total: 300 },
  { clienteId: 1004, total: 120 },
  { clienteId: 1005, total: 450 }
])

db.ventas.createIndex({ clienteId: "hashed" })

db.ventas.getIndexes()

db.ventas.find({ clienteId: 1003 }).explain("executionStats")

db.ventas.find({ clienteId: { $gt: 1002 } }).explain("executionStats")
```

Reflexión:
¿Por qué el índice no sirve para rango?

## Ejercicio 3 — Simulación conceptual de shard key

Dado un sistema de pedidos:

* 90% consultas por clienteId.
* 10% reportes por fecha.

```JS
use tienda

sh.enableSharding("tienda")

sh.shardCollection("tienda.pedidos", { clienteId: 1 })

db.pedidos.insertMany([
  { clienteId: 1001, total: 200 },
  { clienteId: 1500, total: 300 },
  { clienteId: 2200, total: 500 },
  { clienteId: 2700, total: 100 }
])

db.pedidos.find({ clienteId: 1500 })

db.pedidos.find({ total: { $gt: 100 } })
```

¿Cuál shard key elegirías y por qué?

## Ejercicio 4 — Transacciones

1. Ejecutar dos inserts sin transacción.
2. Simular error intermedio.
3. Repetir con transacción.
4. Observar diferencia.

```JS
use academia

session = db.getMongo().startSession()
dbSession = session.getDatabase("academia")

session.startTransaction()

try {
  dbSession.cursos.insertOne({ nombre: "Nuevo Curso" })
  dbSession.estudiantes.updateOne(
    { nombre: "Ana" },
    { $set: { activo: true } }
  )
  session.commitTransaction()
} catch (e) {
  session.abortTransaction()
}

session.endSession()
```

## Ejercicio 5 — Monitoreo

Ejecutar múltiples consultas complejas.
Usar explain().
Identificar:

* COLLSCAN
* totalDocsExamined
* executionTimeMillis

```JS
use academia

db.cursos.find({ nivel: "avanzado" }).explain("executionStats")

db.cursos.find({ creditos: { $gt: 3 } }).explain("executionStats")

db.serverStatus()

db.currentOp()
```

