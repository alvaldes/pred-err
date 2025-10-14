# Clase08 - 02/10/25

## Agrupamiento de datos

Se agrupan datos similares en grupos o clusters, por los criterios de agrupamiento.

Medida de proximidad: similaridad o disimilitud. La distancia es una medida de disimilitud y para elementos similares es pequeña o cero.
Para medidas de similaridad, para elementos similares es un numero muy grande.

### Pasos

1. Selección de características: se debe seleccionar las características que se van a utilizar para el agrupamiento.
2. Adoptar una medida de proximidad.
3. Seleccionar un criterio de agrupamiento.
4. Seleccionar un algoritmo de agrupamiento: elegir un esquema de agrupamiento especifico.
5. Validar los resultados: una vez obtenidos los resultados del agrupamiento, se debe verificar la correctitud de los mismos.
6. Interpretar los resultados: se hace con un experto comparando con evidencia y análisis de la literatura para llegar a las conclusiones correctas.

### Clasificacion vs Agrupamiento

1. En clasificacion, las clases son conocidas a priori, en agrupamiento no (las tengo que encontrar).
2.

### Aplicaciones del Analisis de Agrupamiento

- Reduccion de datos: se pueden agrupar datos similares para reducir la cantidad de datos a analizar.
- Generacion de hipotesis: se pueden agrupar datos para generar hipotesis sobre los mismos.
- Prueba de hipotesis: se pueden agrupar datos para probar hipotesis sobre los mismos.
- Prediccion baseda en grupos: se pueden agrupar datos para predecir el comportamiento de los mismos.
- Para balancear clases minoritarias: se pueden agrupar datos para balancear clases minoritarias en problemas de clasificacion.

> [!IMPORTANT]
> Centroide o prototipo: representa a un grupo o cluster.

> [!NOTE]
> Definicion de Clustering: (no hay 1 sola definicion)
> Definicion cercana ala percepcion humana en 2 y 3 dimensiones.
> Los vectores son vistos como puntos en n dimensiones y los grupos son descritos como regiones continuas en el espacio n-dimensional.

### Tipos de Grupos

> depende del tipo de medida de proximidad y del criterio de agrupamiento. Ademas del problema a resolver.

- Grupo compacto
- Grupo prolongado
- Grupo de caparazon

> Red neuronal SOM o de Kohonem

### Nuestra definicion de grupo

Cada conjunto tiene muestras
La union de las muestras es todo el conjunto de datos
Los grupos son disjuntos

### Tipos de algoritmos de agrupamiento

- Particional
- basados en densidad
- jerarquicos
  - aglomerativos
  - divisivos

## K-Means

- pertenece al los algoritmos de particionamiento

- vectores referencia o centroides o prototipos es lo mismo

Es un procedimiento iterativo que busca optimizar una funcion objetivo.
Se generan k grupos, cada uno representado por un centroide.
Se generan k etiquetas, una por cada grupo.
Con las etiquetas se asignan las muestras a los grupos.
De esta forma sabemos cada muestra a que grupo pertenece.

Pasos:

1. Inicializar k centroides (aleatoriamente o con algun criterio).
2. Calcular etiquetas
3. Actualizar centroides
4. Hasta que Mi converja ( comunmente con un delta error ΔError > Tolerancia, o con una cantidad maxima de iteraciones)

Desventaja:

- es muy sensible a la inicializacion de los centroides.
- una solucion es ejecutar varias veces el algoritmo (100 iteraciones) y quedarse con el promedio de cada uno como el inicio del nuevo.

> [!IMPORTANT]
> Investigar Kmeans++

> [!NOTE]
> Las semillas inicializadoras o seeds son los centroides iniciales. O sea de eso trata Kmeans++.

> !NOTE]
> Se puede encontrar en Sci-Kit Learn como KMeans++.

## K-Medians

## K-Medoids

Medoide: el punto que esta mas al centro de todo. No esta ligada a ninguna metrica de distancia (se puede usar cualquiera).

## Mean Shift

## DBScan
