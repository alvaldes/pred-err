# Comandos Python - Práctica de Predicción de Errores

## Carga y Descarga de Datos

```python
# UCI Repository
dataset = fetch_ucirepo(id=73)  # Descargar dataset por ID desde UCI
X = dataset.data.features  # Extraer variables independientes
y = dataset.data.targets  # Extraer variable objetivo

# Archivos CSV
df = pd.read_csv('archivo.csv')  # Leer archivo CSV
df = pd.read_csv('archivo.csv', index=False)  # Leer CSV sin usar primera columna como índice

# Otros formatos
df_json = pd.read_json('archivo.json')  # Leer archivo JSON
df_xml = pd.read_xml('archivo.xml')  # Leer archivo XML
df_sql = pd.read_sql_query("SELECT * FROM tabla", conn)  # Leer desde base de datos SQL

# Guardar datos
df.to_csv('salida.csv', index=False)  # Guardar como CSV sin índices
df.to_json('salida.json', orient="records", force_ascii=False, indent=2)  # Guardar como JSON
df.to_xml('salida.xml', index=False, root_name="datos", row_name="registro")  # Guardar como XML
```

## Exploración Inicial de Datos

```python
df.shape  # Mostrar dimensiones del dataset (filas, columnas)
df.head()  # Mostrar primeras 5 filas
df.head(10)  # Mostrar primeras 10 filas
df.iloc[0]  # Mostrar primera fila
df.iloc[0:5, 0:3]  # Mostrar primeras 5 filas y primeras 3 columnas
df.info()  # Información general del dataset (tipos, memoria, nulos)
df.describe()  # Estadísticas descriptivas de columnas numéricas
df.columns  # Nombres de las columnas
df.dtypes  # Tipos de datos por columna
len(df)  # Número de filas en el dataset
```

## Análisis de Datos Faltantes

```python
df.isnull().sum()  # Contar valores nulos por columna
df.isnull().sum().sum()  # Contar total de valores nulos
(df.isnull().sum() / len(df)) * 100  # Porcentaje de valores nulos por columna
df.isna().any(axis=1)  # Detectar filas que contienen algún valor nulo
missing_per_row = df.isnull().sum(axis=1)  # Contar valores nulos por fila
```

## Tratamiento de Datos Faltantes

```python
# Eliminar datos faltantes
df.dropna()  # Eliminar filas con cualquier valor nulo
df.dropna(axis=1)  # Eliminar columnas con cualquier valor nulo
df[~df.isnull().any(axis=1)]  # Filtrar filas sin valores nulos

# Rellenar datos faltantes
df.fillna(0)  # Rellenar con valor constante
df.fillna(df.mean())  # Rellenar con la media
df.fillna(df.median())  # Rellenar con la mediana
df.fillna(df.mode().iloc[0])  # Rellenar con la moda
df.fillna('UNK')  # Rellenar variables categóricas con valor desconocido
df.groupby('clase').transform(lambda x: x.fillna(x.mode().iloc[0]))  # Rellenar por grupo/clase
```

## Detección de Outliers

```python
# Método de desviación estándar
outliers_std = df.apply(lambda x: abs(x - x.mean()) >= 3 * x.std())  # Detectar outliers (±3σ)

# Método MAD (Median Absolute Deviation)
def detectar_outliers_mad(serie, umbral=3.5):  # Función para detectar outliers usando MAD
    mediana = np.median(serie)
    mad = np.median(np.abs(serie - mediana))
    z_mod = 0.6745 * (serie - mediana) / mad
    return np.abs(z_mod) > umbral

outliers_mad = df.apply(detectar_outliers_mad)  # Aplicar detección MAD a todas las columnas
outliers_filas = df[outliers_mad.any(axis=1)]  # Obtener filas con al menos un outlier
```

## Estadísticas Descriptivas

```python
df.mean()  # Media aritmética por columna
df.median()  # Mediana por columna
df.std()  # Desviación estándar por columna
df.mode()  # Moda por columna
df.min()  # Valor mínimo por columna
df.max()  # Valor máximo por columna
df.quantile([0.25, 0.5, 0.75])  # Cuartiles específicos
df.value_counts()  # Frecuencia de valores únicos (Series)
df['columna'].value_counts()  # Frecuencia de valores en una columna específica
```

## Selección y Filtrado de Datos

```python
# Selección de columnas
df['columna']  # Seleccionar una columna
df[['col1', 'col2']]  # Seleccionar múltiples columnas
df.select_dtypes(include=['object'])  # Seleccionar columnas por tipo de dato
df.select_dtypes(include=['float64', 'int64'])  # Seleccionar columnas numéricas

# Filtrado de filas
df[df['columna'] > 5]  # Filtrar por condición
df[df['columna'].isin(['valor1', 'valor2'])]  # Filtrar por lista de valores
df[(df['col1'] > 5) & (df['col2'] < 10)]  # Filtro con múltiples condiciones (AND)
df[(df['col1'] > 5) | (df['col2'] < 10)]  # Filtro con múltiples condiciones (OR)
```

## Manipulación de DataFrames

```python
# Combinación de datos
pd.concat([df1, df2], axis=1)  # Concatenar DataFrames horizontalmente
pd.concat([df1, df2], axis=0)  # Concatenar DataFrames verticalmente

# Eliminación
df.drop(['columna1', 'columna2'], axis=1)  # Eliminar columnas
df.drop(categorical_variables, axis=1, inplace=True)  # Eliminar columnas y modificar original

# Agrupación
df.groupby('columna').mean()  # Agrupar y calcular media
df.groupby('columna').agg({'col1': 'mean', 'col2': 'sum'})  # Agregaciones múltiples
df.groupby('columna').size()  # Contar elementos por grupo
```

## Funciones de Transformación

```python
df.apply(lambda x: x.upper())  # Aplicar función a todas las columnas
df.apply(lambda x: x.mean())  # Aplicar función por columna
df.apply(detectar_outliers_mad)  # Aplicar función personalizada
df['columna'].map({'a': 1, 'b': 2})  # Mapear valores específicos
df.transform(lambda x: x.fillna(x.mean()))  # Transformar manteniendo forma original
```

## Operaciones Matemáticas con NumPy

```python
np.random.seed(42)  # Fijar semilla para reproducibilidad
np.random.normal(50, 5, 9)  # Generar números aleatorios con distribución normal
np.append(array, valor)  # Agregar valor al final del array
np.median(array)  # Calcular mediana
np.abs(array)  # Valor absoluto
np.isinf(array)  # Detectar valores infinitos
np.isnan(array)  # Detectar valores NaN
```

## Creación de Funciones Personalizadas

```python
def funcion_personalizada(parametro, umbral=3.5):  # Definir función con parámetros opcionales
    """Documentación de la función"""
    resultado = parametro * umbral
    return resultado

# Función lambda
lambda x: x > 5  # Función anónima para usar en apply/filter
```
