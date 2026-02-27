# Patrones de modelado en MongoDB

MongoDB ha desarrollado patrones reconocidos para resolver problemas comunes.

### Subset Pattern

Consiste en almacenar solo una parte de la información dentro del documento principal.

Ejemplo:

Guardar en el documento del usuario solo los últimos 5 pedidos.

Ventaja:

* Documento liviano
* Lectura frecuente optimizada

### Bucket Pattern

Usado para datos de series temporales.

En lugar de guardar un documento por evento, agrupamos múltiples eventos en un mismo documento.

Ejemplo conceptual:

```JS
{
  sensorId: 1,
  fecha: "2026-02-26",
  mediciones: [ 20.1, 20.5, 20.7 ]
}
```

Ventajas:

* Reduce cantidad de documentos
* Mejora rendimiento
* Optimiza almacenamiento

### Extended Reference Pattern

Guardar parte de la información relacionada dentro del documento, pero mantener referencia al documento completo.

Ejemplo:

Pedido guarda:

* clienteId
* nombreCliente
* ciudadCliente

Aunque exista colección `clientes`.

Beneficio:

Evita \$lookup frecuente pero mantiene consistencia lógica.


