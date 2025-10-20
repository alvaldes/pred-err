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

> [!NOTE]
> PCA genera tantos componentes principales como variables originales, pero los primeros componentes retienen la mayor parte de la variación presente en todas las variables originales.

## Propiedades y limitaciones de PCA

- Suposición de linealidad: Asume que los datos observados son combinaicones lineales
- Suposición de la importancia stadistica de la mediana y covarianza
- supone que las varianzas tienen importancia dinamicas: los componentes con moayor varianza son los mas importantes o interesantes y los que tienen mas bajos son ruido y pueden ser descartados.

## Aplicaciones de PCA

- Compresión de datos
- Pre-procesamiento de datos: Estandarización, normalización de los datos tal que cada variable tenga zero media y varianza unitaria.
- Visualización de datos

## Example

```python
from sklearn import datasets
import mataplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
import pandas as pd
from sklearn.decomposition import PCA

wine_data = datasets.load_wine(as_frame=True)
df = wine_data.data
print(wine_data)
print(wine_data.target)

df.info()

df.describe() # para ver que cada variable no es zero

fig, ax = plt.subplots()
#ax.boxplot(df)
df.boxplot()
plt.show()

std_scaler = StandardScaler()
# Normalizamos los datos y se convierten en un arreglo numpy
std_data = std_scaler.fit_transform(df)
# visualizamos los datos normalizados
scaled2 = pdf.DataFrame(scaled_df) # convertimos a dataframe para visualizar con boxplot
fig, ax = plt.subplots()
scaled2.boxplot()
plt.show()

# Reducimos dimensionaldad o extraemos los componentes principales
pca = PCA()
pca.fit_transform(scaled_df) # fijarse que le paso un arreglo numpy

# direcciones de maxima varianza
print(pca.components_)

# El attr explained_variance_ratio_ indica la varianza explicada por cada componente principal
print('Varianza explicada por cada componente principal:', pca.explained_variance_ratio_)
print('Varianza total explicada:', sum(pca.explained_variance_ratio_))
# de aqui se puede busacar ese 85% de varianza acumulada para saber con cual quedarse

pca2 = PCA(n_components=3)
newData = pca2.fit_transform(scaled_df)

newData = pd.DataFrame(newData)
print(newData.describe())
newData.boxplot()
plt.show()
print(newData)

fif, aux = plt.subplots()
ax = plt.axes(projection='3d')
ax.scatter3D(newData[0], newData[1], newData[2])
plt.show()
```

> [!IMPORTANT]
> Tarea: Descargar el Iris dataset y experimentar con la técnica PCA
> Pasarlo a 2 o a 3 y explicar porque (varianza acc > 85%)
> Mostrarlo en plot
