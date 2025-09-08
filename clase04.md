<!-- markdownlint-disable MD013 -->

# Clase04 03/09/25

## Calidad de Datos: Por que debemos preprocesar los datos?

> [!IMPORTANT]
> Existen muchos factores que involucran la calidad de datos, incluyendo exactitud,
> completez, consistencia, oportunidad, credibilidad e interpretabilidad.

### Tareas principales en le pre-procesamiento de datos

Las rutinas de **limpieza de datos** trabajan para "limpiar" los datos mediante
el rellenado/completado de datos faltantes, suavizado de datos con ruido, identificando
o removiendo datos atípicos, y resolviendo inconsistencias.

La **integración de datos** ayuda a integrar multiples bases de datos, archivos, etc.
Esto es, incluir datos de multiples fuentes.

La **reducción de datos** obtiene una representación reducida del conjunto de datos
que es mucho mas pequeña en volumen en volumen y produce el mismo (o casi el mismo)
resultado analítico. Las estrategias de reducción de datos incluyen a la reducción de
dimensionalidad y reducción de muestras.

> [!IMPORTANT]
> Reducción de muestras utiliza clustering y reducción de dimensionalidad, transformaciones.

Las técnicas de transformación de datos incluyen normalización, discretización
de datos, y generación de jerarquía de conceptos.

La normalización es convertir los datos a un rango de conveniencia (a,b)

La discretización de datos es la conversion de atributos continuos a atributos discretos.
O lo que es lo mismo, agrupar altura en (1,bajo)(2,mediano)(3,alto) pero no significa
que 1 sea mejor que 3, no hay orden ni prioridad.

La generación de jerarquía de conceptos es ontologías ...
