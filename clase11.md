# Clase11 22/10/2025

## Clasificador de Bayes Ingenuo (naive Bayes)

Si N se considera como un buen numero de datos de entrenamiento para obtener una estimacion suficientemente
exacta de una pdf en un espacio de una dimension, entonces N^l puntos serian requeridos para un espacio
l-dimensional.

si:
dim1 -- N = 200
dim2 -- N^2 = 40,000

Para aliviar esto, un enfoque es asumir que las caracteristicas individuales xj, j=1,2,...,l son independientes estadisticamente. Entonces si, es decir,

p(X|wi) = pi desde j=1 hasta l de p(xj|wi) --> se usa la pdf gaussiana de 1 dimension

> [!NOTE]
> El bayesiano es **2C** parámetros ya que se necesitan la media y varianza para cada clase.
> Pero para el naive bayes necesita **2Cl** parámetros (media y varianza para cada característica y cada clase.)

> [!IMPORTANT]
> Tarea:
> Use el conjunto de datos Iris y suponga que para cada clase las características son independientes estadísticamente. Esto es, cada una de estas sigue una distribución Gaussiana de una dimensión.
> Además, suponga que las clases son igualmente probables
> • Divida el conjunto de datos original en una partición de 80% para entrenamiento y otra de 20% para prueba (Test).
> • Para cada una de las 4-dimensiones y para cada una de las tres clases, use el conjunto de entrenamiento para calcular el estimado de máxima verosimilitud de los valores media
> m1j; m2j, m3j, j = 1,..., 4 y las varianzas σ<sup>2</sup>ij; σ2j 3; = 1,..,4

> [!IMPORTANT]
> Hasta aqui el parcial 2
