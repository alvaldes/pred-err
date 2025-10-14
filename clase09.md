# Clase09 - 13/10/25

Extraccion de características con PCA (Análisis de Componentes Principales)

- se usa para reducción de dimensiones, extracción de características y visualización de datos.
- Es conocido como el transformador Karhunen-Loeve

## Definición

> 1. Como una proyección ortogonal (vectores en 90 grados) de los datos en un espacio de dimension menor, conocido como el sub espacio principal,
>    de esta forma la varianza de los datos proyectados es maximizada.

2. Como una proyección linear que minimiza el costo promedio de la proyección, definida como la media cuadrada de los datos.

La idea central de PCA es reducir la dimensionalidad de un conjunto de datos (formado por un numero grande de variables interrelacionadas),
mientras retiren tanto como sea posible la variación presente en le conjunto de datos.

Lo anterior se logra mediante una transformación a un nuevo conjunto de variables...

> [!IMPORTANT]
> Tarea: Descargar el Iris dataset y experimentar con la técnica PCA
> Pasarlo a 2 o a 3 y explicar porque (varianza acc > 85%)
> Mostrarlo en plot
