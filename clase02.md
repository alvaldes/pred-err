# Clase 02 - 28/08/25

Teoria de decision: establecer una regla de decision de forma tal que se minimice el costo asociado a nuestra decicion.

Limite de decicion: este particiona el espacion e caracteristicas (R1, R2, Rn) en dos regiones --R2-- (una region para una clase particular y otra para una clase diferente)

Genrealizacion: esta se relaciona con el rendimiento de un clasificador cuando se le presentan patrones nuevos (objetos no vistos durante el entrenamiento).

> [!TODO]
> Buscar el concepto o idea de "maldicion de la dimensionalidad"
> Buscar como surgio - Articulos y demas
> No entregar - para el lunes

Feature extraction: proceso de transformar los datos brutos en un conjunto de caracteristicas (features) que pueden ser usadas para el entrenamiento de un modelo de machine learning.
Se utlizan metodos matematicos y estadisticos para transformar los datos brutos en un conjunto de caracteristicas relevantes.

| patron | C1      | C2 ...  | CD      |
| ------ | ------- | ------- | ------- |
| X1     | X1      | Item3.1 | Item4.1 |
| X2     | Item2.2 | Item3.2 | Item4.2 |
| Xn     | Xn1     | Xn2     | XnD     |

T: R^D --> R^d, d<<D

Dimensionality Reduction: proceso de reducir el numero de caracteristicas en un conjunto de datos.

ROI: Region of Interest: Region de interes

Generacion de caracteristicas: proceso de crear nuevas caracteristicas a partir de las existentes.
Obtencion de caracteristicas a partir de calculos directos (Formula). Generar la matrix de caracteristicas a partir de los datos brutos.

Feature Generation vs Feature Extraction
El primero crea nuevas caracteristicas a partir de las existentes, mientras que el segundo transforma los datos brutos en un conjunto de caracteristicas.
Extraccion no es quitar, es transformar.
Seleccion es quitar. Seleccionar las caracteristicas mas relevantes.

Fearure Selection: proceso de seleccionar un subconjunto de caracteristicas relevantes para el modelo de machine learning.
Existen algorimos de seleccion de caracteristicas que pueden ser utilizados para este proposito. Como backward elimination, forward selection, recursive feature elimination, entre otros.

Overfitting: cuando un modelo de machine learning se ajusta demasiado a los datos de entrenamiento, perdiendo la capacidad de generalizar a nuevos datos.
La extraccion de caracteristicas puede ayudar a reducir el overfitting al eliminar caracteristicas irrelevantes o redundantes.
