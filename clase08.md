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

1. **En clasificación**, las clases son conocidas a priori, en agrupamiento no (las tengo que encontrar).
2. **La clasificación** es aprendizaje supervisado, el **agrupamiento** es aprendizaje no supervisado.
3. **En clasificación** se entrena un modelo con datos etiquetados, en **agrupamiento** se descubren patrones ocultos.
4. **La clasificación** predice la clase de nuevos datos, el **agrupamiento** organiza los datos existentes.
5. **En clasificación** se evalúa con métricas como precisión y recall, en **agrupamiento** se evalúa con métricas como inercia y silhouette.

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

#### 1. Grupo Compacto (Globular)

- **Características**: Forma esférica o globular
- **Distancia**: Los puntos están cerca del centroide
- **Algoritmos adecuados**: K-means, K-medoids
- **Ejemplo**: Grupos de clientes con comportamientos de compra similares

#### 2. Grupo Prolongado (Elongated)

- **Características**: Forma alargada o elíptica
- **Distancia**: Los puntos pueden estar lejos del centroide pero cerca entre sí
- **Algoritmos adecuados**: Clustering jerárquico, DBSCAN
- **Ejemplo**: Datos geográficos a lo largo de una carretera

#### 3. Grupo de Caparazón (Shell-based)

- **Características**: Forma anular o de caparazón
- **Distancia**: Los puntos forman capas o anillos concéntricos
- **Algoritmos adecuados**: Clustering espectral, Mean Shift
- **Ejemplo**: Patrones de actividad en redes sociales

#### 4. Grupos Irregulares

- **Características**: Formas arbitrarias y complejas
- **Distancia**: No siguen patrones geométricos regulares
- **Algoritmos adecuados**: DBSCAN, clustering basado en densidad
- **Ejemplo**: Regiones geográficas con formas naturales irregulares

> **Red neuronal SOM o de Kohonen**: Utilizada para clustering y visualización de datos de alta dimensionalidad mediante mapas auto-organizados.

### Nuestra definición de grupo

Un **cluster** o grupo debe satisfacer las siguientes propiedades:

1. **Cada conjunto tiene muestras**: Todo cluster debe contener al menos un punto de datos
2. **La unión de las muestras es todo el conjunto de datos**: ∪ Ci = D (donde D es el dataset completo)
3. **Los grupos son disjuntos**: Ci ∩ Cj = ∅ para i ≠ j (no hay solapamiento entre clusters)
4. **Cohesión interna alta**: Los puntos dentro de un cluster son similares entre sí
5. **Separación externa alta**: Los puntos de diferentes clusters son diferentes entre sí

### Medidas de Calidad de Clustering

#### Medidas Internas (no requieren verdad de referencia)

1. **Inercia (Within-cluster Sum of Squares - WCSS)**:

   ```
   WCSS = Σi Σx∈Ci ||x - μi||²
   ```

   - Donde μi es el centroide del cluster i
   - Menor inercia indica mejor agrupamiento

2. **Coeficiente de Silhouette**:

   ```
   s(i) = (b(i) - a(i)) / max(a(i), b(i))
   ```

   - a(i): distancia promedio intra-cluster
   - b(i): distancia promedio al cluster más cercano
   - Rango: [-1, 1], valores cercanos a 1 son mejores

3. **Índice de Davies-Bouldin**:
   - Mide la relación entre dispersión intra-cluster y separación inter-cluster
   - Valores más bajos indican mejor clustering

#### Medidas Externas (requieren verdad de referencia)

1. **Índice de Rand Ajustado (ARI)**
2. **Información Mutua Normalizada (NMI)**
3. **Homogeneidad, Completitud y V-measure**

### Tipos de algoritmos de agrupamiento

#### 1. Algoritmos Particionales

- **Características**: Dividen los datos en k particiones
- **Ventajas**: Eficientes, escalables
- **Desventajas**: Requieren especificar k de antemano
- **Ejemplos**: K-means, K-medians, K-medoids

#### 2. Algoritmos Basados en Densidad

- **Características**: Forman clusters basados en regiones de alta densidad
- **Ventajas**: Pueden encontrar clusters de forma arbitraria, manejan ruido
- **Desventajas**: Sensibles a parámetros de densidad
- **Ejemplos**: DBSCAN, OPTICS, Mean Shift

#### 3. Algoritmos Jerárquicos

- **Características**: Crean una jerarquía de clusters
- **Ventajas**: No requieren especificar k, crean dendrograma
- **Desventajas**: Complejidad computacional alta O(n³)

##### 3.1 Aglomerativos (Bottom-up)

- **Proceso**: Comienzan con n clusters (uno por punto) y los van uniendo
- **Criterios de enlace**:
  - **Simple/Mínimo**: distancia entre puntos más cercanos
  - **Completo/Máximo**: distancia entre puntos más lejanos
  - **Promedio**: distancia promedio entre todos los pares
  - **Ward**: minimiza la varianza intra-cluster

##### 3.2 Divisivos (Top-down)

- **Proceso**: Comienzan con un cluster y lo van dividiendo
- **Algoritmos**: DIANA (DIvisive ANAlysis)

## K-Means

- Pertenece a los algoritmos de **particionamiento**
- Utiliza **vectores referencia**, **centroides** o **prototipos** (todos son términos equivalentes)
- Es un procedimiento **iterativo** que busca optimizar una función objetivo
- Genera **k grupos**, cada uno representado por un centroide
- Genera **k etiquetas**, una por cada grupo
- Con las etiquetas se asignan las muestras a los grupos
- De esta forma sabemos a qué grupo pertenece cada muestra

### Función Objetivo de K-Means

La función que K-means busca **minimizar** es:

```
J = Σᵢ₌₁ᵏ Σₓ∈Cᵢ ||x - μᵢ||²
```

Donde:

- **k**: número de clusters
- **Ci**: conjunto de puntos en el cluster i
- **μᵢ**: centroide del cluster i
- **||x - μᵢ||²**: distancia euclidiana al cuadrado

### Algoritmo K-Means (Pasos detallados)

1. **Inicializar k centroides** (aleatoriamente o con algún criterio)

   ```python
   μ₁, μ₂, ..., μₖ = inicializar_centroides(k, datos)
   ```

2. **Calcular etiquetas** (asignar cada punto al centroide más cercano)

   ```python
   Para cada punto x:
       etiqueta[x] = argmin ||x - μᵢ||²
   ```

3. **Actualizar centroides** (calcular el promedio de puntos en cada cluster)

   ```python
   Para cada cluster i:
       μᵢ = (1/|Cᵢ|) Σₓ∈Cᵢ x
   ```

4. **Repetir hasta convergencia**:
   - Criterio de parada: ΔError > Tolerancia
   - O cantidad máxima de iteraciones alcanzada
   - O centroides no cambian significativamente

### Implementación Práctica

```python
import numpy as np
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt

# Ejemplo básico
def kmeans_ejemplo():
    # Generar datos de ejemplo
    np.random.seed(42)
    datos = np.random.rand(100, 2)

    # Aplicar K-means
    kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
    etiquetas = kmeans.fit_predict(datos)
    centroides = kmeans.cluster_centers_

    # Visualizar resultados
    plt.scatter(datos[:, 0], datos[:, 1], c=etiquetas, cmap='viridis')
    plt.scatter(centroides[:, 0], centroides[:, 1],
               c='red', marker='x', s=200, linewidths=3)
    plt.title('K-Means Clustering')
    plt.show()

    return etiquetas, centroides
```

### Ventajas de K-Means

1. **Simplicidad**: Fácil de entender e implementar
2. **Eficiencia**: Complejidad temporal O(n·k·i·d)
   - n: número de puntos
   - k: número de clusters
   - i: número de iteraciones
   - d: dimensionalidad
3. **Escalabilidad**: Funciona bien con datasets grandes
4. **Convergencia garantizada**: Siempre converge a un mínimo local

### Desventajas de K-Means

1. **Sensible a inicialización**: Muy dependiente de los centroides iniciales
2. **Requiere especificar k**: Necesitas saber cuántos clusters quieres
3. **Asume clusters esféricos**: No funciona bien con formas irregulares
4. **Sensible a outliers**: Los valores atípicos afectan los centroides
5. **Escala de variables**: Requiere normalización si las variables tienen diferentes escalas

### Solución a la sensibilidad de inicialización

- **Ejecutar múltiples veces** el algoritmo (ej. 100 iteraciones con diferentes inicializaciones)
- **Quedarse con el resultado** que tenga menor función objetivo
- **K-means++**: Método inteligente de inicialización (explicado más adelante)

### Selección del número óptimo de clusters (k)

#### 1. Método del Codo (Elbow Method)

```python
def metodo_codo(datos, k_max=10):
    inercias = []
    k_valores = range(1, k_max + 1)

    for k in k_valores:
        kmeans = KMeans(n_clusters=k, random_state=42)
        kmeans.fit(datos)
        inercias.append(kmeans.inertia_)

    plt.plot(k_valores, inercias, 'bo-')
    plt.xlabel('Número de clusters (k)')
    plt.ylabel('Inercia (WCSS)')
    plt.title('Método del Codo')
    plt.show()
```

#### 2. Coeficiente de Silhouette

```python
from sklearn.metrics import silhouette_score

def silhouette_analysis(datos, k_max=10):
    silhouette_scores = []
    k_valores = range(2, k_max + 1)

    for k in k_valores:
        kmeans = KMeans(n_clusters=k, random_state=42)
        etiquetas = kmeans.fit_predict(datos)
        score = silhouette_score(datos, etiquetas)
        silhouette_scores.append(score)

    plt.plot(k_valores, silhouette_scores, 'ro-')
    plt.xlabel('Número de clusters (k)')
    plt.ylabel('Coeficiente de Silhouette')
    plt.title('Análisis de Silhouette')
    plt.show()
```

> [!IMPORTANT]
> **K-means++**: Método de inicialización inteligente que selecciona centroides iniciales
> que están lejos unos de otros, mejorando la convergencia y calidad del clustering.

### K-means++ (Inicialización Inteligente)

**Algoritmo K-means++**:

1. Elegir el primer centroide aleatoriamente
2. Para cada centroide adicional:
   - Calcular D²(x) para cada punto x (distancia al centroide más cercano)
   - Elegir el siguiente centroide con probabilidad proporcional a D²(x)
3. Continuar hasta tener k centroides
4. Proceder con el algoritmo K-means estándar

```python
# K-means++ está implementado por defecto en scikit-learn
kmeans_plus = KMeans(n_clusters=3, init='k-means++', random_state=42)
```

> [!NOTE]
> **Las semillas inicializadoras o seeds** son los centroides iniciales.
> **K-means++** se enfoca en mejorar la selección de estas semillas.

> [!NOTE]
> Se puede encontrar en **Scikit-Learn** como el parámetro `init='k-means++'`
> en la clase KMeans (es el valor por defecto).

## K-Medians

**K-Medians** es una variante de K-means que utiliza la **mediana** en lugar de la **media** para calcular los centroides.

### Diferencias clave con K-means

#### Función Objetivo

```
J = Σᵢ₌₁ᵏ Σₓ∈Cᵢ ||x - medianᵢ||₁
```

- Utiliza la **distancia Manhattan (L1)** en lugar de la distancia euclidiana
- El centroide es la **mediana** de cada dimensión, no la media

#### Algoritmo K-Medians

1. **Inicializar k centroides** (medianas iniciales)
2. **Asignar puntos** al centroide más cercano usando distancia Manhattan
3. **Actualizar centroides** calculando la mediana de cada cluster:

   ```python
   Para cada cluster i y dimensión j:
       medianᵢⱼ = mediana(todos los valores en dimensión j del cluster i)
   ```

4. **Repetir hasta convergencia**

#### Ventajas de K-Medians

1. **Más robusto a outliers**: La mediana es menos sensible a valores extremos
2. **Mejor para datos con ruido**: Maneja mejor la presencia de datos atípicos
3. **Distribuciones asimétricas**: Funciona mejor cuando los datos no siguen distribución normal

#### Desventajas de K-Medians

1. **Mayor costo computacional**: Calcular medianas es más costoso que medias
2. **Menos común**: Menos implementaciones disponibles en librerías estándar
3. **Convergencia más lenta**: Puede requerir más iteraciones

#### Implementación práctica

```python
import numpy as np
from scipy.spatial.distance import pdist, squareform

def k_medians(datos, k, max_iter=100, tol=1e-4):
    n, d = datos.shape

    # Inicialización aleatoria de centroides
    centroides = datos[np.random.choice(n, k, replace=False)]

    for iteracion in range(max_iter):
        # Asignar puntos a clusters (distancia Manhattan)
        distancias = np.zeros((n, k))
        for i, centroide in enumerate(centroides):
            distancias[:, i] = np.sum(np.abs(datos - centroide), axis=1)

        etiquetas = np.argmin(distancias, axis=1)

        # Actualizar centroides (calcular medianas)
        nuevos_centroides = np.zeros_like(centroides)
        for i in range(k):
            cluster_puntos = datos[etiquetas == i]
            if len(cluster_puntos) > 0:
                nuevos_centroides[i] = np.median(cluster_puntos, axis=0)
            else:
                nuevos_centroides[i] = centroides[i]

        # Verificar convergencia
        if np.allclose(centroides, nuevos_centroides, atol=tol):
            break

        centroides = nuevos_centroides

    return etiquetas, centroides
```

## K-Medoids

**K-Medoids** utiliza **puntos reales del dataset** como representantes de cada cluster, en lugar de calcular centroides artificiales.

### Características principales

#### Medoid vs Centroide

- **Medoid**: El punto **real** del dataset que está más al centro del cluster
- **Centroide**: Punto **calculado** (puede no existir en el dataset) que representa el centro

#### Función Objetivo

```
J = Σᵢ₌₁ᵏ Σₓ∈Cᵢ d(x, medoidᵢ)
```

- **d()** puede ser cualquier función de distancia
- **No está limitado a distancia euclidiana** como K-means

### Algoritmo PAM (Partitioning Around Medoids)

#### Fase 1: BUILD (Construcción)

1. Seleccionar k objetos iniciales como medoids
2. Para cada objeto no seleccionado, calcular el costo de intercambiarlo con cada medoid actual
3. Seleccionar el intercambio que más reduzca el costo total

#### Fase 2: SWAP (Intercambio)

1. Para cada par (medoid, no-medoid):
   - Calcular el costo de intercambiar el medoid por el no-medoid
   - Si el costo se reduce, realizar el intercambio
2. Repetir hasta que no haya mejoras

#### Implementación práctica

```python
from sklearn_extra.cluster import KMedoids
import numpy as np

def k_medoids_ejemplo():
    # Generar datos de ejemplo
    np.random.seed(42)
    datos = np.random.rand(100, 2)

    # Aplicar K-medoids
    kmedoids = KMedoids(n_clusters=3, random_state=42, method='pam')
    etiquetas = kmedoids.fit_predict(datos)
    medoids = kmedoids.cluster_centers_

    return etiquetas, medoids

# Implementación manual simplificada
def k_medoids_manual(datos, k, max_iter=100):
    n = len(datos)

    # Inicializar medoids aleatoriamente
    medoids_idx = np.random.choice(n, k, replace=False)

    for _ in range(max_iter):
        # Asignar puntos a medoids más cercanos
        distancias = np.zeros((n, k))
        for i, medoid_idx in enumerate(medoids_idx):
            for j in range(n):
                distancias[j, i] = np.linalg.norm(datos[j] - datos[medoid_idx])

        etiquetas = np.argmin(distancias, axis=1)

        # Intentar mejorar cada medoid
        nuevos_medoids = medoids_idx.copy()
        for i in range(k):
            cluster_puntos_idx = np.where(etiquetas == i)[0]
            if len(cluster_puntos_idx) == 0:
                continue

            mejor_costo = float('inf')
            mejor_medoid = medoids_idx[i]

            # Probar cada punto del cluster como nuevo medoid
            for candidato_idx in cluster_puntos_idx:
                costo = np.sum([np.linalg.norm(datos[candidato_idx] - datos[j])
                               for j in cluster_puntos_idx])
                if costo < mejor_costo:
                    mejor_costo = costo
                    mejor_medoid = candidato_idx

            nuevos_medoids[i] = mejor_medoid

        # Verificar convergencia
        if np.array_equal(medoids_idx, nuevos_medoids):
            break

        medoids_idx = nuevos_medoids

    return etiquetas, datos[medoids_idx]
```

### Ventajas de K-Medoids

1. **Más robusto a outliers**: Los medoids son puntos reales, menos afectados por ruido
2. **Funciona con cualquier métrica de distancia**: No limitado a distancia euclidiana
3. **Interpretabilidad**: Los centros de cluster son puntos reales observables
4. **Datos categóricos**: Puede manejar datos no numéricos con métricas apropiadas

### Desventajas de K-Medoids

1. **Mayor complejidad computacional**: O(n²) vs O(nk) de K-means
2. **Escalabilidad limitada**: No funciona bien con datasets muy grandes
3. **Puede quedar atrapado en mínimos locales**: Como K-means
4. **Requiere más memoria**: Necesita almacenar matriz de distancias

### Comparación K-means vs K-medians vs K-medoids

| Aspecto                 | K-means          | K-medians      | K-medoids                 |
| ----------------------- | ---------------- | -------------- | ------------------------- |
| **Centroide**           | Media aritmética | Mediana        | Punto real del dataset    |
| **Distancia**           | Euclidiana (L2)  | Manhattan (L1) | Cualquiera                |
| **Robustez a outliers** | Baja             | Media          | Alta                      |
| **Complejidad**         | O(nkid)          | O(nkid)        | O(n²ki)                   |
| **Escalabilidad**       | Excelente        | Buena          | Limitada                  |
| **Datos categóricos**   | No               | No             | Sí (con métrica adecuada) |
| **Interpretabilidad**   | Media            | Media          | Alta                      |

## Mean Shift

**Mean Shift** es un algoritmo de clustering **basado en densidad** que encuentra clusters desplazándose hacia las regiones de mayor densidad de puntos.

### Concepto fundamental

El algoritmo se basa en la idea de que los puntos en un cluster tienden a **converger hacia el modo (pico) de densidad** de su distribución local.

#### Función de Densidad (Kernel Density Estimation)

```
f(x) = (1/n) Σᵢ₌₁ⁿ K((x - xᵢ)/h)
```

Donde:

- **K()**: función kernel (típicamente Gaussiano)
- **h**: ancho de banda (bandwidth)
- **n**: número de puntos
- **xᵢ**: puntos de datos

#### Vector Mean Shift

```
m(x) = [Σᵢ₌₁ⁿ xᵢ · G((x - xᵢ)/h)] / [Σᵢ₌₁ⁿ G((x - xᵢ)/h)]
```

### Algoritmo Mean Shift

1. **Para cada punto de datos**:
   - Inicializar ventana centrada en el punto
   - Calcular la media de los puntos dentro de la ventana
   - Desplazar la ventana hacia la nueva media
   - Repetir hasta convergencia (cuando el desplazamiento es mínimo)

2. **Identificar modos**: Los puntos de convergencia son los centros de los clusters

3. **Asignar etiquetas**: Puntos que convergen al mismo modo pertenecen al mismo cluster

### Implementación práctica

```python
from sklearn.cluster import MeanShift, estimate_bandwidth
import numpy as np
import matplotlib.pyplot as plt

def mean_shift_ejemplo():
    # Generar datos de ejemplo
    np.random.seed(42)

    # Crear 3 grupos con diferentes densidades
    cluster1 = np.random.normal([2, 2], 0.5, (50, 2))
    cluster2 = np.random.normal([6, 6], 1.0, (30, 2))
    cluster3 = np.random.normal([2, 6], 0.8, (40, 2))

    datos = np.vstack([cluster1, cluster2, cluster3])

    # Estimar bandwidth automáticamente
    bandwidth = estimate_bandwidth(datos, quantile=0.2, n_samples=500)

    # Aplicar Mean Shift
    ms = MeanShift(bandwidth=bandwidth, bin_seeding=True)
    etiquetas = ms.fit_predict(datos)
    centros_cluster = ms.cluster_centers_

    # Visualizar resultados
    plt.figure(figsize=(12, 4))

    # Datos originales
    plt.subplot(1, 2, 1)
    plt.scatter(datos[:, 0], datos[:, 1], alpha=0.6)
    plt.title('Datos Originales')

    # Resultado del clustering
    plt.subplot(1, 2, 2)
    colors = ['red', 'blue', 'green', 'purple', 'orange']
    for i in range(len(centros_cluster)):
        cluster_data = datos[etiquetas == i]
        plt.scatter(cluster_data[:, 0], cluster_data[:, 1],
                   c=colors[i % len(colors)], alpha=0.6,
                   label=f'Cluster {i}')

    plt.scatter(centros_cluster[:, 0], centros_cluster[:, 1],
               c='black', marker='x', s=200, linewidths=3,
               label='Centros')
    plt.title('Mean Shift Clustering')
    plt.legend()
    plt.show()

    return etiquetas, centros_cluster, bandwidth

# Implementación manual simplificada
def mean_shift_manual(datos, bandwidth, max_iter=300, tol=1e-3):
    n, d = datos.shape

    # Inicializar con todos los puntos como semillas
    semillas = datos.copy()

    for iteracion in range(max_iter):
        nuevas_semillas = np.zeros_like(semillas)

        for i, semilla in enumerate(semillas):
            # Calcular distancias a todos los puntos
            distancias = np.linalg.norm(datos - semilla, axis=1)

            # Encontrar puntos dentro del bandwidth
            dentro_ventana = distancias <= bandwidth
            puntos_ventana = datos[dentro_ventana]

            if len(puntos_ventana) > 0:
                # Calcular nueva media
                nuevas_semillas[i] = np.mean(puntos_ventana, axis=0)
            else:
                nuevas_semillas[i] = semilla

        # Verificar convergencia
        desplazamientos = np.linalg.norm(nuevas_semillas - semillas, axis=1)
        if np.all(desplazamientos < tol):
            break

        semillas = nuevas_semillas

    # Encontrar modos únicos (centros de cluster)
    modos = []
    for semilla in semillas:
        es_nuevo = True
        for modo in modos:
            if np.linalg.norm(semilla - modo) < bandwidth/2:
                es_nuevo = False
                break
        if es_nuevo:
            modos.append(semilla)

    modos = np.array(modos)

    # Asignar etiquetas
    etiquetas = np.zeros(n)
    for i, punto in enumerate(datos):
        distancias_modos = [np.linalg.norm(punto - modo) for modo in modos]
        etiquetas[i] = np.argmin(distancias_modos)

    return etiquetas.astype(int), modos
```

### Parámetros importantes

#### 1. Bandwidth (Ancho de banda)

- **Controla el tamaño de la ventana** de búsqueda
- **Bandwidth pequeño**: Muchos clusters pequeños
- **Bandwidth grande**: Pocos clusters grandes
- **Estimación automática**: `estimate_bandwidth()` en scikit-learn

#### 2. Bin seeding

- **Acelera el algoritmo** usando una grilla regular como semillas iniciales
- **Reduce complejidad** de O(n²) a aproximadamente O(n)

### Ventajas de Mean Shift

1. **No requiere especificar número de clusters**: Encuentra automáticamente k
2. **Forma arbitraria**: Puede encontrar clusters no esféricos
3. **Robusto a outliers**: Los outliers no forman clusters si están aislados
4. **Determinístico**: Siempre produce el mismo resultado (no aleatorio)
5. **Basado en densidad**: Funciona bien con clusters de diferentes densidades

### Desventajas de Mean Shift

1. **Sensible al bandwidth**: Difícil seleccionar el valor óptimo
2. **Complejidad computacional**: O(n²) sin optimizaciones
3. **No escalable**: Lento con datasets grandes
4. **Dimensionalidad alta**: Sufre de la maldición de la dimensionalidad
5. **Clusters de densidad similar**: Funciona mejor cuando los clusters tienen densidades comparables

### Selección del Bandwidth

#### Método 1: Estimación automática

```python
from sklearn.cluster import estimate_bandwidth

# Estima bandwidth basado en k-ésimo vecino más cercano
bandwidth = estimate_bandwidth(datos, quantile=0.2, n_samples=500)
```

#### Método 2: Validación cruzada

```python
def evaluar_bandwidth(datos, bandwidths):
    silhouette_scores = []

    for bw in bandwidths:
        ms = MeanShift(bandwidth=bw, bin_seeding=True)
        etiquetas = ms.fit_predict(datos)

        if len(np.unique(etiquetas)) > 1:
            score = silhouette_score(datos, etiquetas)
            silhouette_scores.append(score)
        else:
            silhouette_scores.append(-1)

    return silhouette_scores

# Probar diferentes valores
bandwidths = np.linspace(0.1, 2.0, 20)
scores = evaluar_bandwidth(datos, bandwidths)
mejor_bandwidth = bandwidths[np.argmax(scores)]
```

### Aplicaciones típicas

1. **Segmentación de imágenes**: Agrupar píxeles por color/intensidad
2. **Seguimiento de objetos**: Tracking en videos
3. **Análisis de datos biológicos**: Clustering de células por características
4. **Procesamiento de señales**: Encontrar picos en señales

## DBSCAN (Density-Based Spatial Clustering of Applications with Noise)

**DBSCAN** es un algoritmo de clustering **basado en densidad** que puede encontrar clusters de forma arbitraria y es robusto ante la presencia de ruido.

### Conceptos clave

#### Parámetros principales

- **ε (epsilon)**: Radio de vecindad
- **MinPts**: Número mínimo de puntos para formar un cluster

#### Tipos de puntos

1. **Punto Core (núcleo)**:
   - Tiene al menos MinPts puntos dentro de su ε-vecindad
   - Puede formar un cluster

2. **Punto Border (frontera)**:
   - Tiene menos de MinPts vecinos
   - Pero está en la ε-vecindad de un punto core

3. **Punto Noise (ruido)**:
   - No es core ni border
   - Se considera outlier

#### Definiciones formales

**ε-vecindad de un punto p**:

```
Nε(p) = {q ∈ D | dist(p,q) ≤ ε}
```

**Densidad directa alcanzable**:
Un punto q es densidad directa alcanzable desde p si:

- q ∈ Nε(p)
- |Nε(p)| ≥ MinPts (p es punto core)

### Algoritmo DBSCAN

```python
def dbscan_algoritmo(datos, epsilon, min_pts):
    n = len(datos)
    etiquetas = [-1] * n  # -1 indica no visitado
    cluster_id = 0

    for i in range(n):
        if etiquetas[i] != -1:  # Ya visitado
            continue

        # Encontrar vecinos
        vecinos = encontrar_vecinos(datos, i, epsilon)

        if len(vecinos) < min_pts:
            etiquetas[i] = 0  # Marcar como ruido
        else:
            cluster_id += 1
            expandir_cluster(datos, i, vecinos, cluster_id,
                           epsilon, min_pts, etiquetas)

    return etiquetas

def encontrar_vecinos(datos, punto_idx, epsilon):
    vecinos = []
    for i, punto in enumerate(datos):
        if np.linalg.norm(datos[punto_idx] - punto) <= epsilon:
            vecinos.append(i)
    return vecinos

def expandir_cluster(datos, punto_idx, vecinos, cluster_id,
                    epsilon, min_pts, etiquetas):
    etiquetas[punto_idx] = cluster_id

    i = 0
    while i < len(vecinos):
        vecino_idx = vecinos[i]

        if etiquetas[vecino_idx] == -1:  # No visitado
            etiquetas[vecino_idx] = cluster_id
            nuevos_vecinos = encontrar_vecinos(datos, vecino_idx, epsilon)

            if len(nuevos_vecinos) >= min_pts:
                vecinos.extend(nuevos_vecinos)

        elif etiquetas[vecino_idx] == 0:  # Era ruido
            etiquetas[vecino_idx] = cluster_id

        i += 1
```

### Implementación práctica

```python
from sklearn.cluster import DBSCAN
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import make_blobs

def dbscan_ejemplo():
    # Generar datos con ruido
    centros = [[1, 1], [-1, -1], [1, -1]]
    X, _ = make_blobs(n_samples=750, centers=centros, n_features=2,
                      random_state=42, cluster_std=0.4)

    # Añadir puntos de ruido
    ruido = np.random.uniform(-3, 3, (50, 2))
    datos = np.vstack([X, ruido])

    # Aplicar DBSCAN
    dbscan = DBSCAN(eps=0.3, min_samples=10)
    etiquetas = dbscan.fit_predict(datos)

    # Visualizar resultados
    plt.figure(figsize=(10, 8))

    # Identificar clusters y ruido
    clusters_unicos = set(etiquetas)
    colors = ['red', 'blue', 'green', 'purple', 'orange', 'brown']

    for cluster_id in clusters_unicos:
        if cluster_id == -1:  # Ruido
            cluster_data = datos[etiquetas == cluster_id]
            plt.scatter(cluster_data[:, 0], cluster_data[:, 1],
                       c='black', marker='x', s=50, alpha=0.6,
                       label='Ruido')
        else:  # Cluster válido
            cluster_data = datos[etiquetas == cluster_id]
            plt.scatter(cluster_data[:, 0], cluster_data[:, 1],
                       c=colors[cluster_id % len(colors)],
                       alpha=0.6, s=50,
                       label=f'Cluster {cluster_id}')

    plt.title('DBSCAN Clustering')
    plt.legend()
    plt.show()

    # Estadísticas
    n_clusters = len(set(etiquetas)) - (1 if -1 in etiquetas else 0)
    n_noise = list(etiquetas).count(-1)

    print(f'Número de clusters: {n_clusters}')
    print(f'Puntos de ruido: {n_noise}')

    return etiquetas, n_clusters, n_noise
```

### Selección de parámetros

#### 1. Método k-distance

```python
def k_distance_plot(datos, k):
    from sklearn.neighbors import NearestNeighbors

    # Calcular k-ésimas distancias
    nbrs = NearestNeighbors(n_neighbors=k).fit(datos)
    distancias, indices = nbrs.kneighbors(datos)

    # Ordenar k-ésimas distancias
    k_distancias = np.sort(distancias[:, k-1], axis=0)[::-1]

    # Plotear
    plt.plot(range(len(k_distancias)), k_distancias)
    plt.xlabel('Puntos ordenados por distancia')
    plt.ylabel(f'{k}-ésima distancia más cercana')
    plt.title('Gráfico k-distance para selección de ε')
    plt.show()

    return k_distancias

# Buscar el "codo" en la curva para seleccionar ε
k_dist = k_distance_plot(datos, k=4)  # k típicamente = MinPts
```

#### 2. Regla empírica para MinPts

- **MinPts ≥ dimensionalidad + 1**
- **Para 2D**: MinPts = 4 es común
- **Para alta dimensionalidad**: MinPts = 2 \* dimensionalidad

### Ventajas de DBSCAN

1. **No requiere especificar k**: Encuentra automáticamente el número de clusters
2. **Forma arbitraria**: Puede encontrar clusters de cualquier forma
3. **Robusto a outliers**: Identifica y maneja puntos de ruido
4. **Clusters de diferente tamaño**: No asume clusters de tamaño similar
5. **Determinístico**: Produce resultados consistentes

### Desventajas de DBSCAN

1. **Sensible a parámetros**: Difícil seleccionar ε y MinPts óptimos
2. **Densidades variables**: Problemas con clusters de densidades muy diferentes
3. **Alta dimensionalidad**: Sufre de la maldición de la dimensionalidad
4. **Complejidad**: O(n log n) con índices espaciales, O(n²) sin ellos

### Variantes y mejoras

#### 1. HDBSCAN (Hierarchical DBSCAN)

- **Maneja densidades variables**
- **Construcción jerárquica** de clusters
- **Más robusto** en la selección de parámetros

```python
import hdbscan

clusterer = hdbscan.HDBSCAN(min_cluster_size=10, min_samples=5)
etiquetas = clusterer.fit_predict(datos)
```

#### 2. OPTICS (Ordering Points To Identify Clustering Structure)

- **Extensión de DBSCAN**
- **Produce un ordenamiento** que revela la estructura de clustering
- **Mejor para densidades variables**

```python
from sklearn.cluster import OPTICS

optics = OPTICS(min_samples=5, xi=0.05, min_cluster_size=0.1)
etiquetas = optics.fit_predict(datos)
```

### Comparación de algoritmos de clustering

| Algoritmo      | Requiere k | Forma clusters | Maneja ruido | Complejidad |
| -------------- | ---------- | -------------- | ------------ | ----------- |
| **K-means**    | Sí         | Esféricos      | No           | O(nkid)     |
| **Mean Shift** | No         | Arbitraria     | Sí           | O(n²)       |
| **DBSCAN**     | No         | Arbitraria     | Sí           | O(n log n)  |
| **Jerárquico** | No         | Arbitraria     | Parcial      | O(n³)       |

---

## Ejercicios Prácticos

### Ejercicio 1: Implementación de K-means desde cero

```python
import numpy as np
import matplotlib.pyplot as plt

# TODO: Implementar K-means sin usar librerías
def mi_kmeans(datos, k, max_iter=100, tol=1e-4):
    """
    Implementar K-means desde cero

    Parámetros:
    - datos: array numpy de forma (n_samples, n_features)
    - k: número de clusters
    - max_iter: máximo número de iteraciones
    - tol: tolerancia para convergencia

    Retorna:
    - etiquetas: array de etiquetas de cluster para cada punto
    - centroides: array de centroides finales
    - costos: lista de valores de función objetivo por iteración
    """
    # Tu implementación aquí
    pass

# Probar con datos sintéticos
datos_test = np.random.rand(100, 2)
etiquetas, centroides, costos = mi_kmeans(datos_test, k=3)
```

### Ejercicio 2: Comparación de algoritmos

```python
# TODO: Comparar K-means, DBSCAN y Mean Shift en el mismo dataset
def comparar_algoritmos(datos):
    """
    Aplicar diferentes algoritmos de clustering al mismo dataset
    y comparar resultados
    """
    # Implementar comparación
    pass

# Generar dataset con diferentes tipos de clusters
# - Clusters esféricos
# - Clusters alargados
# - Clusters con ruido
```

### Ejercicio 3: Selección óptima de parámetros

```python
# TODO: Implementar métodos para encontrar k óptimo en K-means
def encontrar_k_optimo(datos, k_max=10):
    """
    Usar método del codo y silhouette para encontrar k óptimo
    """
    # Implementar método del codo
    # Implementar análisis de silhouette
    # Retornar k recomendado
    pass
```

### Ejercicio 4: Aplicación en predicción de errores

```python
# TODO: Aplicar clustering a métricas de software para identificar
# módulos propensos a errores
def clustering_metricas_software(metricas_dataset):
    """
    Aplicar clustering a métricas de software como:
    - Complejidad ciclomática
    - Líneas de código
    - Número de métodos
    - Acoplamiento

    Identificar grupos de módulos con patrones similares
    """
    pass
```

---

## Resumen y Conclusiones

### Puntos clave de la clase

1. **Clustering vs Clasificación**:
   - Clustering es **aprendizaje no supervisado**
   - No requiere etiquetas previas
   - Descubre patrones ocultos en los datos

2. **Tipos principales de algoritmos**:
   - **Particionales** (K-means, K-medoids): Dividen en k particiones
   - **Jerárquicos**: Crean jerarquías de clusters
   - **Basados en densidad** (DBSCAN, Mean Shift): Encuentran regiones densas

3. **Selección de algoritmo depende de**:
   - Forma esperada de los clusters
   - Presencia de ruido
   - Conocimiento previo sobre número de clusters
   - Tamaño del dataset

4. **Evaluación de clustering**:
   - **Métricas internas**: Silhouette, inercia, Davies-Bouldin
   - **Métricas externas**: ARI, NMI (cuando hay verdad de referencia)
   - **Inspección visual**: Fundamental en 2D/3D

### Para la siguiente clase

> [!IMPORTANT]  
> **Tarea para casa**:
>
> 1. Implementar K-means desde cero en Python
> 2. Probar con el dataset Iris usando diferentes valores de k
> 3. Comparar resultados con la implementación de scikit-learn
> 4. Generar visualizaciones de los clusters encontrados
> 5. Calcular métricas de evaluación (silhouette score)

> [!NOTE]
> **Próxima clase**: Continuaremos con **PCA (Principal Component Analysis)**
> para reducción de dimensionalidad y su relación con clustering.

### Recursos adicionales

- **Documentación scikit-learn**: <https://scikit-learn.org/stable/modules/clustering.html>
- **Artículo original K-means**: MacQueen, J. (1967)
- **DBSCAN paper**: Ester, M. et al. (1996)
- **Mean Shift**: Fukunaga, K. & Hostetler, L. (1975)
