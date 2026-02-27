# Antipatrones comunes en diseño documental

Entender qué NO hacer es tan importante como saber qué hacer.

### Antipatrón 1: Sobre-normalización

Diseñar MongoDB como si fuera relacional.

Demasiadas colecciones pequeñas.
Exceso de \$lookup.

Resultado:

* Alto costo de consulta
* Complejidad innecesaria

### Antipatrón 2: Documentos gigantes

Guardar arrays que crecen sin límite.

Ejemplo peligroso:

```JS
{
  usuario: "Carlos",
  logs: [ ... miles de registros ... ]
}
```

Problemas:

* Reubicación constante en disco
* Impacto en rendimiento
* Riesgo de alcanzar 16 MB

### Antipatrón 3: Abuso de índices

Crear índices en todos los campos.

Consecuencias:

* Escrituras más lentas
* Mayor uso de memoria
* Fragmentación

El índice es una inversión, no un adorno.


