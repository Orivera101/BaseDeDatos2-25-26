# Optimización de pipelines de Aggregation

Ahora unimos lo aprendido en clase 9 con optimización.

Regla principal:

Reducir volumen lo antes posible.

### Caso incorrecto

```JS
db.cursos.aggregate(pipeline, { allowDiskUse: true })
```

Estamos agrupando todo para luego filtrar.

### Caso optimizado

```JS
db.cursos.aggregate(pipeline, { allowDiskUse: true })
```

Filtramos antes de agrupar.

### Proyección temprana

Si no necesitamos todos los campos:

```JS
db.cursos.aggregate(pipeline, { allowDiskUse: true })
```

Reduce memoria usada en `$group`.

### explain en aggregation

Siempre verificar:

* ¿Se usó índice en \$match?
* ¿Hay COLLSCAN?
* ¿Cuántos documentos pasaron a \$group?

La optimización profesional siempre se mide.

