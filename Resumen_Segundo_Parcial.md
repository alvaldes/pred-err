# Resumen Completo Segundo Parcial - Predicción de Errores en ISW

## PREPROCESAMIENTO DE DATOS

### Introducción

**Calidad de Datos**: Los datos tienen calidad si satisfacen los requerimientos del uso intencionado.

**Factores de calidad:**

- Exactitud
- Completez
- Consistencia
- Oportunidad
- Credibilidad
- Interpretabilidad

**Problemas comunes**: Datos inexactos, incompletos e inconsistentes son propiedades comunes de bases de datos y data-warehouses grandes en el mundo real.

### Tareas Principales en Preprocesamiento

1. **Limpieza de datos**: Rellenar datos faltantes, suavizar ruido, identificar/remover outliers, resolver inconsistencias
2. **Integración de datos**: Integrar múltiples bases de datos, archivos o fuentes
3. **Reducción de datos**: Obtener representación reducida del dataset (reducción dimensional, reducción de muestras)
4. **Transformación de datos**: Normalización, discretización, generación de jerarquía de conceptos

### Formatos de Datos Estándar

- **CSV** (Comma-Separated Values)
- **XML** (Extensible Markup Language)
- **SQL** (Structured Query Language)
- **JSON** (JavaScript Object Notation)

**Biblioteca recomendada**: PANDAS para manejo de estos formatos

### Fuentes de Datos Públicos

- **UCI Repository**: Repositorio de Machine Learning
- **Kaggle Datasets**: Competencias y datasets
- **R-bloggers Datasets**: Recursos para R y análisis de datos

---

## LIMPIEZA DE DATOS

### 1. Datos Faltantes (Missing Values)

**Técnicas para rellenar datos faltantes:**

#### a) Ignorar la tupla

- Se hace cuando falta la etiqueta de clase
- No es efectivo a menos que la tupla tenga muchos valores faltantes

#### b) Rellenar manualmente

- Consume mucho tiempo
- No es factible para datasets grandes

#### c) Usar constante global

- Reemplazar por "Desconocido" o ∞
- Simple pero pierde información

#### d) Usar medida de tendencia central

```python
# Media para distribución normal
df['columna'].fillna(df['columna'].mean(), inplace=True)

# Mediana para distribuciones sesgadas
df['columna'].fillna(df['columna'].median(), inplace=True)
```

#### e) Media/mediana por clase

```python
# Rellenar por clase
for clase in df['target'].unique():
    mask = df['target'] == clase
    df.loc[mask, 'columna'] = df.loc[mask, 'columna'].fillna(
        df.loc[mask, 'columna'].mean()
    )
```

#### f) Valor más probable

- Usar inferencia bayesiana
- Usar regresión
- Usar árboles de decisión
- Predecir el valor usando otros atributos

### 2. Eliminación de Outliers

**Definición**: Punto muy lejos de la media de la variable aleatoria correspondiente.

**Medición**: Distancia con respecto a un umbral (generalmente múltiplos de desviación estándar)

**Reglas para distribución Gaussiana:**

- 2σ cubre 95% de los puntos
- 3σ cubre 99% de los puntos

**Fórmula estadística:**

```python
# Detección de outliers
outliers = abs(X - X.mean()) >= n * X.std()

# Ejemplo con n=3
outliers = abs(df['columna'] - df['columna'].mean()) >= 3 * df['columna'].std()

# Eliminar outliers
df_clean = df[~outliers]
```

**Efectos de los outliers:**

- Producen errores grandes durante entrenamiento
- Efectos desastrosos si son resultado de mediciones ruidosas

### 3. Desviación Absoluta de la Mediana (MAD)

**Más robusta que desviación estándar** para detectar outliers.

```python
import numpy as np

def mad_outliers(data, threshold=3.5):
    """
    Detecta outliers usando MAD
    threshold: típicamente 3.5 (equivalente a ~3 desv. estándar)
    """
    median = np.median(data)
    mad = np.median(np.abs(data - median))
    modified_z_scores = 0.6745 * (data - median) / mad
    return np.abs(modified_z_scores) > threshold
```

---

## TRANSFORMACIÓN DE DATOS

### Normalización de Características (Feature Normalization)

**Definición**: Proceso de escalar números en un dataset para mejorar la precisión de cálculos numéricos.

**Cuándo usar normalización:**

- Variables en diferentes unidades
- Varianza entre columnas es sustancial
- Métodos que tratan atributos numéricos (distancias, gradientes)

### Técnicas de Normalización

#### 1. División por máximo

```python
X_norm = X / X.max()
```

Rango: [0, 1]

#### 2. Min-Max Scaling

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
X_norm = scaler.fit_transform(X)

# Fórmula manual
X_norm = (X - X.min()) / (X.max() - X.min())
```

Rango: [0, 1]

#### 3. Z-Score Standardization

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_std = scaler.fit_transform(X)

# Fórmula manual
X_std = (X - X.mean()) / X.std()
```

Resultado: Media = 0, Desviación estándar = 1

#### 4. División por desviación estándar

```python
X_norm = X / X.std()
```

**Elección de método:**

- **Min-Max**: Cuando necesitas rango específico [0,1]
- **Z-Score**: Cuando asumes distribución normal o quieres comparar variables
- **Por máximo**: Cuando solo necesitas escalar a [0,1] de forma simple
- **Por std**: Cuando quieres preservar la forma de distribución

---

## BALANCEO DE CLASES

### Introducción

**Problema**: La distribución de clases en muchas aplicaciones no está balanceada.

**Ejemplo**: Transacciones con tarjeta de crédito

- Mayoría: transacciones normales (clase mayoritaria)
- Minoría: transacciones fraudulentas (clase minoritaria)

### Consecuencias del Desbalanceo

- Sesgo hacia la clase mayoritaria
- Baja sensibilidad en clases minoritarias
- Métricas engañosas (alta precisión general pero bajo desempeño en clases importantes)
- Pérdida de información valiosa

### Soluciones

1. **Reponderación de ejemplos**: Asignar pesos diferentes a las clases
2. **Remuestreo de ejemplos**: Modificar la cantidad de ejemplos (más utilizado)

---

## MÉTODOS DE BALANCEO

### 1. Under-sampling (Submuestreo)

**Concepto**: Reducir instancias de la clase mayoritaria.

#### Random Under-sampling

```python
from imblearn.under_sampling import RandomUnderSampler

rus = RandomUnderSampler(random_state=42)
X_resampled, y_resampled = rus.fit_resample(X, y)
```

#### Tomek Links

Elimina pares de instancias de clases diferentes muy cercanas (reduce ruido en frontera).

```python
from imblearn.under_sampling import TomekLinks

tomek = TomekLinks()
X_resampled, y_resampled = tomek.fit_resample(X, y)
```

#### Edited Nearest Neighbours (ENN)

Remueve instancias mal clasificadas por sus vecinos.

```python
from imblearn.under_sampling import EditedNearestNeighbours

enn = EditedNearestNeighbours()
X_resampled, y_resampled = enn.fit_resample(X, y)
```

#### Condensed Nearest Neighbour (CNN)

Mantiene solo instancias esenciales para la frontera de decisión.

```python
from imblearn.under_sampling import CondensedNearestNeighbour

cnn = CondensedNearestNeighbour(random_state=42)
X_resampled, y_resampled = cnn.fit_resample(X, y)
```

### 2. Over-sampling (Sobremuestreo)

**Concepto**: Incrementar instancias de la clase minoritaria.

#### Random Over-sampling (Sencillo)

Duplica aleatoriamente instancias minoritarias.

```python
from imblearn.over_sampling import RandomOverSampler

ros = RandomOverSampler(random_state=42)
X_resampled, y_resampled = ros.fit_resample(X, y)
```

**Desventaja**: Provoca sobre-ajuste (overfitting) al duplicar exactamente las mismas instancias.

#### SMOTE (Synthetic Minority Oversampling Technique)

**Ventaja principal**: Genera instancias **sintéticas** en lugar de duplicar existentes.

**Algoritmo:**

1. Selecciona una instancia minoritaria xi
2. Encuentra k vecinos más cercanos (típicamente k=5)
3. Selecciona aleatoriamente uno de estos vecinos xj
4. Genera nueva instancia en la línea entre xi y xj
5. Repite hasta alcanzar balance deseado

**Fórmula matemática:**

```
x_nuevo = xi + λ × (xj - xi)
```

donde λ ∈ [0, 1] es un número aleatorio

**Implementación:**

```python
from imblearn.over_sampling import SMOTE
from collections import Counter

# Crear y aplicar SMOTE
smote = SMOTE(random_state=42, k_neighbors=5)
X_balanced, y_balanced = smote.fit_resample(X, y)

print(f"Distribución original: {Counter(y)}")
print(f"Distribución después de SMOTE: {Counter(y_balanced)}")
```

**Ventajas:**

- ✅ Evita overfitting (no duplica instancias)
- ✅ Mejor generalización
- ✅ Preserva información de la clase minoritaria
- ✅ Ajustable con parámetro k

**Desventajas:**

- ❌ Puede generar ruido en regiones de solapamiento
- ❌ Menos efectivo en alta dimensionalidad
- ❌ Diseñado para datos continuos
- ❌ Asume distribución similar entre vecinos

**Variantes de SMOTE:**

1. **Borderline-SMOTE**: Se enfoca en la frontera de decisión
2. **ADASYN**: Genera más instancias donde hay menos densidad
3. **SMOTE-NC**: Para datos categóricos y continuos
4. **SMOTE-N**: Para datos categóricos solamente

```python
from imblearn.over_sampling import BorderlineSMOTE, ADASYN

# Borderline-SMOTE
bsmote = BorderlineSMOTE(random_state=42)
X_bs, y_bs = bsmote.fit_resample(X, y)

# ADASYN
adasyn = ADASYN(random_state=42)
X_ada, y_ada = adasyn.fit_resample(X, y)
```

### 3. Métodos Híbridos

Combinan over-sampling y under-sampling.

#### SMOTEENN

SMOTE + Edited Nearest Neighbours

```python
from imblearn.combine import SMOTEENN

smote_enn = SMOTEENN(random_state=42)
X_resampled, y_resampled = smote_enn.fit_resample(X, y)
```

#### SMOTETomek

SMOTE + Tomek Links

```python
from imblearn.combine import SMOTETomek

smote_tomek = SMOTETomek(random_state=42)
X_resampled, y_resampled = smote_tomek.fit_resample(X, y)
```

### Métricas de Evaluación para Datos Desbalanceados

⚠️ **IMPORTANTE**: La precisión (accuracy) **NO es confiable** en datasets desbalanceados.

**Ejemplo del problema:**

```
Dataset: 95% clase normal, 5% clase anormal
Modelo que predice siempre "normal" → 95% accuracy
Pero 0% detección de clase anormal!
```

**Métricas recomendadas:**

#### 1. Matriz de Confusión

```
                Predicho Positivo    Predicho Negativo
Real Positivo       TP                    FN
Real Negativo       FP                    TN
```

#### 2. Precision (Precisión)

```
Precision = TP / (TP + FP)
```

De todas las predicciones positivas, ¿cuántas fueron correctas?

#### 3. Recall (Sensibilidad/Exhaustividad)

```
Recall = TP / (TP + FN)
```

De todos los casos positivos reales, ¿cuántos fueron detectados?

#### 4. F1-Score

```
F1 = 2 × (Precision × Recall) / (Precision + Recall)
```

Media armónica entre precision y recall.

#### 5. AUC-ROC

Área bajo la curva ROC (Receiver Operating Characteristic).

#### 6. AUC-PR

Área bajo la curva Precision-Recall.

**Implementación:**

```python
from sklearn.metrics import (precision_score, recall_score, f1_score,
                             roc_auc_score, confusion_matrix,
                             classification_report)

# Calcular métricas
precision = precision_score(y_true, y_pred)
recall = recall_score(y_true, y_pred)
f1 = f1_score(y_true, y_pred)
auc_roc = roc_auc_score(y_true, y_pred_proba)

# Matriz de confusión
cm = confusion_matrix(y_true, y_pred)

# Reporte completo
print(classification_report(y_true, y_pred))
```

### Buenas Prácticas en Balanceo

**Cuándo usar balanceo:**

- Ratio de desbalance > 10:1
- Datos suficientes en clase minoritaria
- Problemas de clasificación supervisada

**Recomendaciones CRÍTICAS:**

1. **Aplicar DESPUÉS de la división**: Balancear solo conjunto de entrenamiento

```python
# ✅ CORRECTO
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
X_train_bal, y_train_bal = smote.fit_resample(X_train, y_train)
modelo.fit(X_train_bal, y_train_bal)
modelo.predict(X_test)  # Test sin modificar

# ❌ INCORRECTO
X_bal, y_bal = smote.fit_resample(X, y)
X_train, X_test, y_train, y_test = train_test_split(X_bal, y_bal)
```

2. **Validación cruzada estratificada**: Mantener proporción de clases

```python
from sklearn.model_selection import StratifiedKFold

skf = StratifiedKFold(n_splits=5)
for train_idx, test_idx in skf.split(X, y):
    X_train, X_test = X[train_idx], X[test_idx]
    y_train, y_test = y[train_idx], y[test_idx]
    # Aplicar balanceo solo en train
```

3. **Experimentar**: Probar múltiples métodos

4. **Evaluar apropiadamente**: No confiar solo en accuracy

---

## AGRUPAMIENTO (CLUSTERING)

### Introducción

**Definición**: Aprendizaje no supervisado que revela la organización de patrones en clusters (grupos) sensibles.

**Nombres alternativos:**

- Unsupervised learning (Machine Learning)
- Numerical taxonomy (Biología, Ecología)
- Typology (Ciencias Sociales)
- Partition (Teoría de Grafos)

**Ejemplo motivador:**
Organizar animales: oveja, perro, gato (mamíferos), gorrión, gaviota (aves), víbora, lagarto (reptiles), pez dorado, salmonete rojo, tiburón azul (peces), rana (anfibios).

**Criterios de agrupamiento posibles:**
a) Forma de reproducción (vivíparos vs ovíparos)
b) Existencia de pulmones
c) Ambiente donde viven (terrestre, acuático, aéreo)
d) Combinación de criterios

**Conclusión**: El proceso de asignar objetos a clusters puede llevar a resultados muy diferentes dependiendo del criterio específico usado.

### Clasificación vs Agrupamiento

| Característica | Clasificación                  | Agrupamiento                 |
| -------------- | ------------------------------ | ---------------------------- |
| **Clases**     | Conocidas a priori             | Se descubren                 |
| **Tipo**       | Aprendizaje supervisado        | Aprendizaje no supervisado   |
| **Datos**      | Requiere etiquetas             | No requiere etiquetas        |
| **Objetivo**   | Predecir clase de nuevos datos | Organizar datos existentes   |
| **Evaluación** | Precisión, recall              | Inercia, silhouette          |
| **Etapas**     | Entrenamiento → Prueba         | Descubrimiento de estructura |

### Pasos para Desarrollar una Tarea de Clustering

1. **Selección de características (Feature Selection)**

   - Seleccionar características que codifiquen la máxima información
   - Puede requerir preprocesamiento

2. **Medida de proximidad (Proximity Measure)**

   - Cuantifica qué tan "similares" o "disimilares" son dos vectores
   - **Similaridad**: Número grande para elementos similares
   - **Distancia**: Número pequeño/cero para elementos similares

3. **Criterio de agrupamiento (Clustering Criterion)**

   - Depende de la interpretación del experto del término "sensible"
   - Puede expresarse como función de costo u otro tipo de reglas

4. **Algoritmo de agrupamiento (Clustering Algorithm)**

   - Esquema algorítmico específico que descubre la estructura de clustering

5. **Validación de resultados (Validation)**

   - Verificar correctitud usando tests apropiados

6. **Interpretación de resultados (Interpretation)**
   - Integrar resultados con evidencia experimental y análisis del experto

### Aplicaciones del Clustering

1. **Reducción de datos (Data Reduction)**

   - Cuando N (cantidad de datos) es muy grande
   - Agrupar en m clusters (m ≪ N)
   - Procesar cada cluster como una sola entidad

2. **Generación de hipótesis (Hypothesis Generation)**

   - Aplicar clustering para inferir hipótesis sobre la naturaleza de los datos
   - Luego verificar con otros datasets

3. **Prueba de hipótesis (Hypothesis Testing)**

   - Verificar validez de una hipótesis específica

4. **Predicción basada en grupos**
   - Aplicar clustering al dataset
   - Caracterizar clusters resultantes
   - Dado un patrón desconocido, determinar su cluster más probable
   - Caracterizarlo basándose en la caracterización del cluster

### Definición Formal de Clustering

Sea X nuestro dataset:

```
X = {x₁, x₂, ..., xₙ}
```

Un **m-clustering** de X es una partición de X en m conjuntos (clusters) C₁, ..., Cₘ tal que:

1. **No vacío**: Cᵢ ≠ ∅, i = 1, ..., m
2. **Cubren todo**: ⋃ᵢ₌₁ᵐ Cᵢ = X
3. **Disjuntos**: Cᵢ ∩ Cⱼ = ∅, i ≠ j

**Adicionalmente**: Los vectores contenidos en un cluster Cᵢ son "más similares" entre sí y "menos similares" a vectores de otros clusters.

### Tipos de Grupos (Clusters)

#### 1. Grupos Compactos (Globular/Compact Clusters)

- **Forma**: Esférica o globular
- **Distancia**: Puntos cercanos al centroide
- **Algoritmos**: K-means, K-medoids, K-medians

#### 2. Grupos Prolongados (Elongated Clusters)

- **Forma**: Alargada o elíptica
- **Distancia**: Puntos pueden estar lejos del centroide pero cerca entre sí
- **Algoritmos**: Clustering jerárquico, DBSCAN

#### 3. Grupos de Caparazón (Shell-based Clusters)

- **Forma**: Anular o de caparazón
- **Distancia**: Forman capas o anillos concéntricos
- **Algoritmos**: Clustering espectral, Mean Shift

#### 4. Grupos Irregulares (Irregular Clusters)

- **Forma**: Arbitrarias y complejas
- **Algoritmos**: DBSCAN, clustering basado en densidad

### Propiedades de un Cluster

1. Cada cluster tiene al menos una muestra
2. La unión de todos los clusters = dataset completo: ⋃Cᵢ = D
3. Los clusters son disjuntos: Cᵢ ∩ Cⱼ = ∅ para i ≠ j
4. Alta cohesión interna (puntos similares dentro del cluster)
5. Alta separación externa (clusters diferentes entre sí)

### Medidas de Calidad de Clustering

#### Medidas Internas (sin verdad de referencia)

**1. Inercia (WCSS - Within-Cluster Sum of Squares)**

```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=3)
kmeans.fit(X)
inercia = kmeans.inertia_  # Menor es mejor
```

**Fórmula:**

```
WCSS = Σᵢ Σₓ∈Cᵢ ||x - μᵢ||²
```

**Interpretación:**

- Mide compacidad de los clusters
- Valores menores indican clusters más compactos
- Siempre disminuye al aumentar k

**2. Coeficiente de Silhouette**

```python
from sklearn.metrics import silhouette_score

score = silhouette_score(X, labels)  # Rango [-1, 1]
```

**Fórmula para punto i:**

```
s(i) = (b(i) - a(i)) / max(a(i), b(i))
```

donde:

- a(i): distancia promedio intra-cluster
- b(i): distancia promedio al cluster más cercano

**Interpretación:**

- Valor cercano a +1: Bien asignado
- Valor cercano a 0: En la frontera entre clusters
- Valor cercano a -1: Mal asignado

**3. Índice de Davies-Bouldin**

```python
from sklearn.metrics import davies_bouldin_score

db_score = davies_bouldin_score(X, labels)  # Menor es mejor
```

**Interpretación:**

- Mide ratio entre dispersión intra-cluster y separación inter-cluster
- Valores menores indican mejor clustering

#### Medidas Externas (requieren verdad de referencia)

**1. Índice de Rand Ajustado (ARI)**

```python
from sklearn.metrics import adjusted_rand_score

ari = adjusted_rand_score(y_true, y_pred)  # Rango [-1, 1]
```

**2. Información Mutua Normalizada (NMI)**

```python
from sklearn.metrics import normalized_mutual_info_score

nmi = normalized_mutual_info_score(y_true, y_pred)  # Rango [0, 1]
```

**3. Homogeneidad, Completitud y V-measure**

```python
from sklearn.metrics import homogeneity_score, completeness_score, v_measure_score

homogeneity = homogeneity_score(y_true, y_pred)
completeness = completeness_score(y_true, y_pred)
v_measure = v_measure_score(y_true, y_pred)
```

### Tipos de Algoritmos de Clustering

#### 1. Algoritmos Particionales

**Características:**

- Dividen datos en k particiones
- Cada partición representa un cluster
- Generalmente basados en optimización iterativa

**Ventajas:**

- ✅ Eficientes computacionalmente
- ✅ Escalables a grandes datasets
- ✅ Fáciles de implementar

**Desventajas:**

- ❌ Requieren especificar k a priori
- ❌ Sensibles a inicialización
- ❌ Pueden converger a mínimos locales
- ❌ Asumen clusters de forma similar

**Ejemplos:**

- K-means
- K-medians
- K-medoids (PAM)

#### 2. Algoritmos Basados en Densidad

**Características:**

- Forman clusters por regiones de alta densidad
- Puntos en regiones de baja densidad = ruido
- No requieren especificar k

**Ventajas:**

- ✅ Descubren formas arbitrarias
- ✅ Manejan ruido y outliers
- ✅ No necesitan k a priori

**Desventajas:**

- ❌ Sensibles a parámetros (ε, minPts)
- ❌ Dificultad con densidades variables
- ❌ Complejidad computacional alta

**Ejemplos:**

- DBSCAN
- OPTICS
- Mean Shift

#### 3. Algoritmos Jerárquicos

**Características:**

- Crean jerarquía de clusters (dendrograma)
- Dos enfoques: aglomerativo y divisivo

**Ventajas:**

- ✅ No requieren k a priori
- ✅ Generan dendrograma (visualización)
- ✅ Útiles para datos jerárquicos

**Desventajas:**

- ❌ Complejidad O(n³)
- ❌ No escalables a grandes datasets
- ❌ Decisiones de fusión/división son irreversibles

**Aglomerativos (Bottom-up):**

- Comienzan con n clusters (cada punto es un cluster)
- Van uniendo clusters progresivamente

**Criterios de enlace:**

- **Simple (Single)**: Distancia mínima entre puntos
- **Completo (Complete)**: Distancia máxima entre puntos
- **Promedio (Average)**: Distancia promedio entre puntos
- **Ward**: Minimiza varianza dentro de clusters

**Divisivos (Top-down):**

- Comienzan con 1 cluster (todos los puntos)
- Van dividiendo clusters progresivamente
- **Algoritmo**: DIANA

```python
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import dendrogram, linkage
import matplotlib.pyplot as plt

# Clustering jerárquico aglomerativo
agg = AgglomerativeClustering(n_clusters=3, linkage='ward')
labels = agg.fit_predict(X)

# Crear dendrograma
Z = linkage(X, method='ward')
plt.figure(figsize=(10, 5))
dendrogram(Z)
plt.title('Dendrograma')
plt.xlabel('Índice de muestra')
plt.ylabel('Distancia')
plt.show()
```

---

## ALGORITMOS DE AGRUPAMIENTO PARTICIONALES

### K-Means

**Tipo**: Algoritmo de particionamiento iterativo que utiliza centroides.

**Función Objetivo (minimizar):**

```
J = Σᵢ₌₁ᵏ Σₓ∈Cᵢ ||x - μᵢ||²
```

Donde:

- k: número de clusters
- Cᵢ: conjunto de puntos en cluster i
- μᵢ: centroide del cluster i
- ||x - μᵢ||²: distancia euclidiana al cuadrado

**Algoritmo Iterativo:**

1. **Inicialización**: Seleccionar k centroides iniciales m₁, m₂, ..., mₖ (aleatoriamente)

2. **Asignación**: Para cada punto xₜ, calcular:

```
bₜᵢ = { 1  si ||xₜ - mᵢ|| = minⱼ ||xₜ - mⱼ||
      { 0  en otro caso
```

3. **Actualización**: Minimizar error para obtener nuevos centroides:

```
E({mᵢ}ᵢ₌₁ᵏ|X) = Σₜ Σᵢ bₜᵢ ||xₜ - mᵢ||²
```

Derivando e igualando a 0:

```
mᵢ = (Σₜ bₜᵢ xₜ) / (Σₜ bₜᵢ)
```

Es decir, el centroide se actualiza a la **media** de todas las instancias asignadas.

4. **Iteración**: Repetir pasos 2-3 hasta convergencia o máximo de iteraciones

**Convergencia**: Cuando los centroides se estabilizan (no cambian significativamente).

**Implementación completa:**

```python
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import silhouette_score
import matplotlib.pyplot as plt
import numpy as np

# 1. Normalizar datos (IMPORTANTE)
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# 2. Método del codo para determinar k óptimo
inercias = []
silhouettes = []
K_range = range(2, 11)

for k in K_range:
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    kmeans.fit(X_scaled)
    inercias.append(kmeans.inertia_)
    silhouettes.append(silhouette_score(X_scaled, kmeans.labels_))

# 3. Graficar método del codo
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))

ax1.plot(K_range, inercias, marker='o')
ax1.set_xlabel('Número de clusters (k)')
ax1.set_ylabel('Inercia (WCSS)')
ax1.set_title('Método del Codo')
ax1.grid(True)

ax2.plot(K_range, silhouettes, marker='o', color='green')
ax2.set_xlabel('Número de clusters (k)')
ax2.set_ylabel('Silhouette Score')
ax2.set_title('Análisis de Silhouette')
ax2.grid(True)

plt.tight_layout()
plt.show()

# 4. Aplicar K-Means con k óptimo
k_optimo = 3  # Determinar del gráfico
kmeans_final = KMeans(n_clusters=k_optimo, random_state=42, n_init=10)
labels = kmeans_final.fit_predict(X_scaled)
centroides = kmeans_final.cluster_centers_

# 5. Resultados
print(f"K óptimo: {k_optimo}")
print(f"Inercia final: {kmeans_final.inertia_:.2f}")
print(f"Silhouette Score: {silhouette_score(X_scaled, labels):.3f}")
print(f"Iteraciones hasta convergencia: {kmeans_final.n_iter_}")

# 6. Visualizar resultados (2D)
if X_scaled.shape[1] >= 2:
    plt.figure(figsize=(10, 8))
    scatter = plt.scatter(X_scaled[:, 0], X_scaled[:, 1],
                         c=labels, cmap='viridis', alpha=0.6)
    plt.scatter(centroides[:, 0], centroides[:, 1],
               s=300, c='red', marker='X',
               edgecolors='black', linewidths=2,
               label='Centroides')
    plt.xlabel('Característica 1')
    plt.ylabel('Característica 2')
    plt.title(f'K-Means Clustering (k={k_optimo})')
    plt.colorbar(scatter, label='Cluster')
    plt.legend()
    plt.grid(True, alpha=0.3)
    plt.show()

# 7. Análisis de tamaño de clusters
unique, counts = np.unique(labels, return_counts=True)
for cluster, count in zip(unique, counts):
    print(f"Cluster {cluster}: {count} puntos ({count/len(labels)*100:.1f}%)")
```

**Ventajas de K-Means:**

- ✅ Simple y fácil de implementar
- ✅ Escalable a grandes datasets
- ✅ Eficiente: O(nkdi) donde n=muestras, k=clusters, d=dimensiones, i=iteraciones
- ✅ Garantiza convergencia

**Desventajas de K-Means:**

- ❌ Requiere especificar k a priori
- ❌ Sensible a inicialización (búsqueda local)
- ❌ Sensible a outliers
- ❌ Asume clusters esféricos de tamaño similar
- ❌ Usa solo distancia euclidiana

**Técnicas de Inicialización:**

1. **Random**: Selección aleatoria de k puntos
2. **K-Means++**: Inicialización inteligente (por defecto en sklearn)

```python
kmeans = KMeans(n_clusters=3, init='k-means++', random_state=42)
```

### K-Medians

**Diferencia con K-Means**: Utiliza la **mediana** en lugar de la media y la **distancia Manhattan (L₁)** en lugar de euclidiana.

**Función Objetivo:**

```
J = Σᵢ₌₁ᵏ Σₓ∈Cᵢ ||x - mᵢ||₁
```

donde ||·||₁ es la norma L₁ (Manhattan): ||x - y||₁ = Σⱼ|xⱼ - yⱼ|

**Actualización de centroides:**

```
mᵢ = median({xₜ : bₜᵢ = 1})
```

El centroide se actualiza al vector cuyas componentes son las **medianas** de cada dimensión.

**Ventaja principal**: Más robusto a outliers que K-Means.

**Implementación:**

```python
from sklearn_extra.cluster import KMedians

# K-Medians
kmedians = KMedians(n_clusters=3, random_state=42)
labels = kmedians.fit_predict(X_scaled)
centroides = kmedians.cluster_centers_
```

**Cuándo usar K-Medians:**

- Datos con outliers
- Distribuciones asimétricas
- Cuando la media no es representativa

### K-Medoids (PAM - Partitioning Around Medoids)

**Diferencia clave**: Escoge **elementos del conjunto de datos** como centros (medoids) en lugar de calcular promedios.

**Medoid**: Objeto de un grupo cuya disimilaridad media a todos los objetos en el grupo es mínima. Es el punto ubicado más hacia el centro en todo el grupo.

**Función Objetivo:**

```
E({xᵢ}ᵢ₌₁ᵏ|X) = Σₜ_noMed Σᵢ_med bₜᵢ ||xₜ - xᵢ||²
```

**Algoritmo PAM:**

1. **Inicialización**: Seleccionar k puntos del dataset como medoids iniciales

2. **Asignación**: Asociar cada punto al medoid más cercano

3. **Actualización** (mientras el costo disminuya):

   - Para cada medoid m:
     - Para cada no-medoid o:
       - Intercambiar m y o
       - Recalcular el costo (suma de distancias de puntos a sus medoids)
       - Si el costo total aumentó, deshacer el intercambio

4. **Convergencia**: Cuando no hay intercambios que reduzcan el costo

**Implementación:**

```python
from sklearn_extra.cluster import KMedoids

# K-Medoids
kmedoids = KMedoids(n_clusters=3, random_state=42, method='pam')
labels = kmedoids.fit_predict(X_scaled)
medoids = kmedoids.cluster_centers_

print(f"Índices de medoids: {kmedoids.medoid_indices_}")
print(f"Inercia: {kmedoids.inertia_:.2f}")
```

**Ventajas de K-Medoids:**

- ✅ Más robusto a outliers y ruido
- ✅ Trabaja con métricas de distancia arbitrarias
- ✅ Los centroides son puntos reales del dataset (interpretables)
- ✅ No requiere calcular promedios (útil para datos categóricos)

**Desventajas de K-Medoids:**

- ❌ Más costoso computacionalmente: O(k(n-k)²)
- ❌ No escala bien a grandes datasets
- ❌ Requiere especificar k

**Comparación K-Means vs K-Medians vs K-Medoids:**

| Aspecto               | K-Means         | K-Medians      | K-Medoids              |
| --------------------- | --------------- | -------------- | ---------------------- |
| **Centro**            | Media           | Mediana        | Punto real del dataset |
| **Métrica**           | Euclidiana (L₂) | Manhattan (L₁) | Cualquier métrica      |
| **Outliers**          | Sensible        | Robusto        | Muy robusto            |
| **Complejidad**       | O(nkdi)         | O(nkdi)        | O(k(n-k)²)             |
| **Interpretabilidad** | Media           | Media          | Alta (punto real)      |
| **Escalabilidad**     | Excelente       | Excelente      | Limitada               |

---

## PCA (ANÁLISIS DE COMPONENTES PRINCIPALES)

### Introducción

**Principal Component Analysis (PCA)** es una técnica para:

- Reducción de dimensionalidad
- Extracción de características
- Visualización de datos

También conocido como **Transformación Karhunen-Loève**.

### Dos Perspectivas de PCA

**1. Maximización de Varianza:**

- Proyección ortogonal de los datos en un espacio de menor dimensión (subespacio principal)
- Que **maximiza la varianza** de los datos proyectados

**2. Minimización de Error de Proyección:**

- Proyección lineal que **minimiza el costo promedio de proyección**
- Definido como la distancia cuadrada media entre puntos y sus proyecciones

### Objetivo Central

Reducir la dimensionalidad de un conjunto de datos (formado por variables interrelacionadas) mientras se retiene tanto como sea posible la variación presente.

**Logro**: Transformación a un nuevo conjunto de variables llamadas **Componentes Principales (PC)** que son:

- No correlacionados
- Ordenados: los primeros retienen la mayoría de la variación

### Conceptos Fundamentales

**Dataset original:**

- N observaciones: {xₙ} donde n = 1, ..., N
- D dimensiones (variables)
- Representación matricial: X = [x₁, x₂, ..., xₙ]ᵀ (matriz N × D)

**Objetivo:**

- Proyectar en espacio M-dimensional donde M < D
- Maximizar varianza preservada

### Formulación Matemática

#### 1. Proyección a 1 Dimensión

**Vector de dirección**: u₁ (D-dimensional, unitario: u₁ᵀu₁ = 1)

**Proyección de xₙ**: u₁ᵀxₙ (escalar)

**Media de datos proyectados**:

```
u₁ᵀx̄  donde  x̄ = (1/N) Σₙ₌₁ᴺ xₙ
```

**Varianza de datos proyectados**:

```
(1/N) Σₙ₌₁ᴺ {(u₁ᵀxₙ - u₁ᵀx̄)}² = u₁ᵀSu₁
```

donde **S es la matriz de covarianza**:

```
S = (1/N) Σₙ₌₁ᴺ (xₙ - x̄)(xₙ - x̄)ᵀ
```

#### 2. Maximización con Restricción

**Problema de optimización:**

```
Maximizar: u₁ᵀSu₁
Restricción: u₁ᵀu₁ = 1
```

**Usando Multiplicadores de Lagrange**:

```
L = u₁ᵀSu₁ + λ₁(1 - u₁ᵀu₁)
```

**Derivando e igualando a 0**:

```
∂L/∂u₁ = 2Su₁ - 2λ₁u₁ = 0
```

**Resultado**:

```
Su₁ = λ₁u₁
```

**Conclusión**: u₁ debe ser un **eigenvector de S**

**Varianza capturada**:

```
u₁ᵀSu₁ = u₁ᵀ(λ₁u₁) = λ₁(u₁ᵀu₁) = λ₁
```

**Primer Componente Principal**: Eigenvector con el **eigenvalue más grande** λ₁

#### 3. Componentes Adicionales

Los componentes adicionales se definen de forma incremental:

- Cada nueva dirección maximiza la varianza proyectada
- Entre todas las direcciones **ortogonales** a las ya consideradas

**Resultado**: Los M componentes principales son los eigenvectors de S correspondientes a los M eigenvalues más grandes.

### Algoritmo PCA Completo

**Pasos:**

1. **Centrar los datos**: Restar la media

```python
X_centered = X - X.mean(axis=0)
```

2. **Calcular matriz de covarianza**:

```python
S = (1/N) * X_centered.T @ X_centered
# O usar numpy
S = np.cov(X.T)
```

3. **Calcular eigenvalues y eigenvectors**:

```python
eigenvalues, eigenvectors = np.linalg.eig(S)
```

4. **Ordenar por eigenvalue descendente**:

```python
idx = eigenvalues.argsort()[::-1]
eigenvalues = eigenvalues[idx]
eigenvectors = eigenvectors[:, idx]
```

5. **Seleccionar M componentes principales**:

```python
W = eigenvectors[:, :M]  # Matriz de proyección (D × M)
```

6. **Proyectar datos**:

```python
X_pca = X_centered @ W  # Datos en nuevo espacio (N × M)
```

### Implementación Completa en Python

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import load_wine
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

# 1. Cargar datos
wine_data = load_wine(as_frame=True)
df = wine_data.data
target = wine_data.target

print(f"Dimensiones originales: {df.shape}")
print(f"\nPrimeras filas:")
print(df.head())
print(f"\nEstadísticas descriptivas:")
print(df.describe())

# 2. Visualizar distribución original
fig, axes = plt.subplots(2, 1, figsize=(12, 8))

# Datos sin normalizar
df.boxplot(ax=axes[0])
axes[0].set_title('Datos Originales (Sin Normalizar)')
axes[0].set_xticklabels(axes[0].get_xticklabels(), rotation=45, ha='right')

# 3. NORMALIZAR (CRÍTICO para PCA)
scaler = StandardScaler()
scaled_data = scaler.fit_transform(df)
scaled_df = pd.DataFrame(scaled_data, columns=df.columns)

# Datos normalizados
scaled_df.boxplot(ax=axes[1])
axes[1].set_title('Datos Normalizados (Media=0, Std=1)')
axes[1].set_xticklabels(axes[1].get_xticklabels(), rotation=45, ha='right')

plt.tight_layout()
plt.show()

# Verificar normalización
print("\n--- Verificación de Normalización ---")
print(f"Medias (deben ser ~0): {scaled_df.mean().values}")
print(f"Desviaciones estándar (deben ser ~1): {scaled_df.std().values}")

# 4. Aplicar PCA (todos los componentes)
pca = PCA()
pca_data = pca.fit_transform(scaled_data)

print(f"\n--- Resultados PCA ---")
print(f"Número de componentes: {pca.n_components_}")
print(f"Dimensiones de datos transformados: {pca_data.shape}")

# 5. Analizar varianza explicada
print(f"\nVarianza explicada por componente:")
for i, var in enumerate(pca.explained_variance_ratio_, 1):
    print(f"  PC{i}: {var:.4f} ({var*100:.2f}%)")

varianza_acumulada = pca.explained_variance_ratio_.cumsum()
print(f"\nVarianza acumulada:")
for i, var_acum in enumerate(varianza_acumulada, 1):
    print(f"  Hasta PC{i}: {var_acum:.4f} ({var_acum*100:.2f}%)")

# 6. Visualizar varianza explicada
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))

# Gráfico de barras (varianza individual)
ax1.bar(range(1, len(pca.explained_variance_ratio_) + 1),
        pca.explained_variance_ratio_,
        alpha=0.7, color='steelblue')
ax1.set_xlabel('Componente Principal')
ax1.set_ylabel('Proporción de Varianza Explicada')
ax1.set_title('Varianza Explicada por Componente')
ax1.grid(True, alpha=0.3)

# Gráfico de línea (varianza acumulada)
ax2.plot(range(1, len(varianza_acumulada) + 1),
         varianza_acumulada, marker='o', linestyle='-', linewidth=2)
ax2.axhline(y=0.85, color='r', linestyle='--',
           label='85% varianza', linewidth=2)
ax2.axhline(y=0.95, color='g', linestyle='--',
           label='95% varianza', linewidth=2)
ax2.set_xlabel('Número de Componentes')
ax2.set_ylabel('Varianza Acumulada')
ax2.set_title('Varianza Acumulada')
ax2.legend()
ax2.grid(True, alpha=0.3)
ax2.set_ylim([0, 1.05])

plt.tight_layout()
plt.show()

# 7. Determinar número óptimo de componentes
umbral = 0.85
n_componentes_85 = (varianza_acumulada >= umbral).argmax() + 1
print(f"\n--- Selección de Componentes ---")
print(f"Componentes para ≥{umbral*100}% varianza: {n_componentes_85}")
print(f"Varianza explicada: {varianza_acumulada[n_componentes_85-1]:.4f}")

# 8. Aplicar PCA con número óptimo
pca_final = PCA(n_components=n_componentes_85)
datos_reducidos = pca_final.fit_transform(scaled_data)

print(f"\n--- Reducción de Dimensionalidad ---")
print(f"Dimensiones originales: {df.shape}")
print(f"Dimensiones reducidas: {datos_reducidos.shape}")
print(f"Reducción: {(1 - datos_reducidos.shape[1]/df.shape[1])*100:.1f}%")

# 9. Crear DataFrame con componentes principales
df_pca = pd.DataFrame(
    datos_reducidos,
    columns=[f'PC{i+1}' for i in range(n_componentes_85)]
)
df_pca['target'] = target

print(f"\nPrimeras filas del dataset reducido:")
print(df_pca.head())

# 10. Visualización 2D
if n_componentes_85 >= 2:
    plt.figure(figsize=(10, 8))
    scatter = plt.scatter(df_pca['PC1'], df_pca['PC2'],
                         c=target, cmap='viridis',
                         s=50, alpha=0.7, edgecolors='black')
    plt.xlabel(f'PC1 ({pca_final.explained_variance_ratio_[0]*100:.1f}%)')
    plt.ylabel(f'PC2 ({pca_final.explained_variance_ratio_[1]*100:.1f}%)')
    plt.title('PCA - Primeros 2 Componentes Principales')
    plt.colorbar(scatter, label='Clase de Vino')
    plt.grid(True, alpha=0.3)

    # Agregar etiquetas de clase
    for i, name in enumerate(wine_data.target_names):
        mask = target == i
        plt.scatter([], [], c=plt.cm.viridis(i/2), label=name, s=100)
    plt.legend()

    plt.tight_layout()
    plt.show()

# 11. Visualización 3D
if n_componentes_85 >= 3:
    from mpl_toolkits.mplot3d import Axes3D

    fig = plt.figure(figsize=(12, 9))
    ax = fig.add_subplot(111, projection='3d')

    scatter = ax.scatter(df_pca['PC1'], df_pca['PC2'], df_pca['PC3'],
                        c=target, cmap='viridis',
                        s=50, alpha=0.7, edgecolors='black')

    ax.set_xlabel(f'PC1 ({pca_final.explained_variance_ratio_[0]*100:.1f}%)')
    ax.set_ylabel(f'PC2 ({pca_final.explained_variance_ratio_[1]*100:.1f}%)')
    ax.set_zlabel(f'PC3 ({pca_final.explained_variance_ratio_[2]*100:.1f}%)')
    ax.set_title('PCA - Primeros 3 Componentes Principales')

    plt.colorbar(scatter, label='Clase de Vino', shrink=0.5)
    plt.tight_layout()
    plt.show()

# 12. Análisis de Loadings (Contribuciones)
loadings = pd.DataFrame(
    pca_final.components_.T,
    columns=[f'PC{i+1}' for i in range(n_componentes_85)],
    index=df.columns
)

print(f"\n--- Loadings (Contribuciones de Variables) ---")
print(loadings)

# Visualizar loadings del PC1
plt.figure(figsize=(10, 6))
loadings_pc1 = loadings['PC1'].sort_values(key=abs, ascending=False)
colors = ['red' if x < 0 else 'green' for x in loadings_pc1]
plt.barh(range(len(loadings_pc1)), loadings_pc1, color=colors, alpha=0.7)
plt.yticks(range(len(loadings_pc1)), loadings_pc1.index)
plt.xlabel('Loading')
plt.title('Contribuciones de Variables al PC1')
plt.axvline(x=0, color='black', linestyle='--', linewidth=0.8)
plt.grid(True, alpha=0.3, axis='x')
plt.tight_layout()
plt.show()

# 13. Biplot (opcional - para 2 componentes)
if n_componentes_85 >= 2:
    def biplot(score, coeff, labels, target):
        """
        Biplot: muestra observaciones y variables en mismo gráfico
        """
        plt.figure(figsize=(12, 9))

        # Puntos (observaciones)
        scatter = plt.scatter(score[:, 0], score[:, 1],
                            c=target, cmap='viridis',
                            s=50, alpha=0.6, edgecolors='black')

        # Vectores (variables)
        n = coeff.shape[0]
        for i in range(n):
            plt.arrow(0, 0, coeff[i, 0]*3, coeff[i, 1]*3,
                     color='r', alpha=0.5, head_width=0.05)
            plt.text(coeff[i, 0]*3.2, coeff[i, 1]*3.2,
                    labels[i], color='r', fontsize=10)

        plt.xlabel(f'PC1 ({pca_final.explained_variance_ratio_[0]*100:.1f}%)')
        plt.ylabel(f'PC2 ({pca_final.explained_variance_ratio_[1]*100:.1f}%)')
        plt.title('Biplot PCA')
        plt.colorbar(scatter, label='Clase')
        plt.grid(True, alpha=0.3)
        plt.tight_layout()
        plt.show()

    biplot(datos_reducidos[:, :2],
           pca_final.components_.T[:, :2],
           df.columns,
           target)

# 14. Reconstrucción de datos (opcional)
# PCA inverso: proyectar de vuelta al espacio original
datos_reconstruidos = pca_final.inverse_transform(datos_reducidos)
datos_reconstruidos = scaler.inverse_transform(datos_reconstruidos)

# Calcular error de reconstrucción
error = np.mean((df.values - datos_reconstruidos)**2)
print(f"\n--- Reconstrucción ---")
print(f"Error cuadrático medio de reconstrucción: {error:.6f}")
```

### Criterios de Selección de Componentes

**1. Umbral de Varianza Acumulada (más común)**

```python
# Retener componentes que expliquen ≥85% de varianza
n_componentes = (varianza_acumulada >= 0.85).argmax() + 1
```

**Umbrales típicos:**

- 85%: Reducción agresiva
- 90%: Balance reducción/información
- 95%: Conservador

**2. Scree Plot (Método del Codo)**
Buscar "codo" donde la pendiente cambia drásticamente.

**3. Kaiser Criterion**
Retener componentes con eigenvalue > 1

```python
n_componentes = (pca.explained_variance_ > 1).sum()
```

**4. Objetivo Específico**

- Visualización: Reducir a 2D o 3D
- Compresión: Objetivo de tamaño
- Machine Learning: Validación cruzada

### Propiedades y Limitaciones de PCA

**Suposiciones:**

1. **Linealidad**: Datos observados son combinaciones lineales de una base

2. **Importancia estadística de media y covarianza**:

   - PCA usa eigenvectors de la matriz de covarianza
   - Solo encuentra ejes independientes bajo suposición Gaussiana
   - ⚠️ No garantiza que direcciones de máxima varianza contengan características buenas para discriminación

3. **Varianza grande = dinámica importante**:
   - PCA asume alta relación señal-ruido
   - Componentes con mayor varianza = dinámica interesante
   - Componentes con menor varianza = ruido
   - ⚠️ Esto no siempre es cierto

**Limitaciones:**

- ❌ Solo captura relaciones lineales
- ❌ Sensible a escala (requiere normalización)
- ❌ Pierde interpretabilidad de variables originales
- ❌ No garantiza separabilidad de clases
- ❌ Asume que varianza = información

### Aplicaciones de PCA

**1. Compresión de datos**

- Aproximar cada vector xₙ con menos dimensiones
- Reducir espacio de almacenamiento

**2. Pre-procesamiento**

- Estandarizar datos (media=0, varianza=1)
- Decorrelacionar características
- Reducir dimensionalidad antes de clasificación/regresión

**3. Visualización de datos**

- Reducir a 2D o 3D para graficar
- Explorar estructura de datos de alta dimensión

**4. Detección de outliers**

- Puntos con alta distancia de Mahalanobis en espacio PCA

**5. Extracción de características**

- Generar nuevas características no correlacionadas
- Capturar patrones principales

### PCA vs Otros Métodos de Reducción

| Método          | Tipo         | Lineal | Supervizado | Preserva                  |
| --------------- | ------------ | ------ | ----------- | ------------------------- |
| **PCA**         | Proyección   | Sí     | No          | Varianza global           |
| **LDA**         | Proyección   | Sí     | Sí          | Separabilidad de clases   |
| **t-SNE**       | Manifold     | No     | No          | Vecindades locales        |
| **UMAP**        | Manifold     | No     | No          | Estructura global y local |
| **Autoencoder** | Red Neuronal | No     | No          | Reconstrucción            |

---

## TEORÍA DE DECISIÓN BAYESIANA

### Introducción

**Objetivo**: Diseñar clasificadores basados en teoría de decisión de Bayes usando argumentos probabilísticos.

**Enfoque**: Clasificar un patrón desconocido en la clase **más probable**.

**Premisa**: La variación estadística de patrones y el ruido en sensores requieren enfoque probabilístico.

### Conceptos Fundamentales

**Tarea de clasificación**:

- M clases: ω₁, ω₂, ..., ωₘ
- Patrón desconocido representado por vector x

**Probabilidades a posteriori**:

```
P(ωᵢ|x), i = 1, 2, ..., M
```

**Interpretación**: P(ωᵢ|x) = probabilidad de que el patrón pertenezca a clase ωᵢ dado que el vector de características toma el valor x.

**Decisión**: Asignar x a la clase correspondiente al máximo de P(ωᵢ|x).

### Caso de Dos Clases

**Clases**: ω₁, ω₂

**Probabilidades a priori**: P(ω₁), P(ω₂) (conocidas o estimadas)

**Funciones de densidad de probabilidad condicionales**:

```
p(x|ωᵢ), i = 1, 2
```

También llamadas **funciones de verosimilitud (likelihood)** de ωᵢ con respecto a x.

**Nota**: Si x es discreto, p(x|ωᵢ) se denota P(x|ωᵢ).

### Regla de Bayes

**Cálculo de probabilidades a posteriori**:

```
P(ωᵢ|x) = [p(x|ωᵢ) × P(ωᵢ)] / p(x)
```

**Componentes:**

- **p(x|ωᵢ)**: Verosimilitud (likelihood) - también llamada evidencia
- **P(ωᵢ)**: Probabilidad a priori (prior) - también llamada creencia
- **P(ωᵢ|x)**: Probabilidad a posteriori (posterior)
- **p(x)**: Probabilidad total (evidencia marginal)

**Probabilidad total**:

```
p(x) = Σᵢ₌₁ᴹ p(x|ωᵢ) × P(ωᵢ)
```

Para dos clases:

```
p(x) = p(x|ω₁)P(ω₁) + p(x|ω₂)P(ω₂)
```

### Regla de Clasificación Bayesiana

**Para dos clases:**

```
Si P(ω₁|x) > P(ω₂|x) → x se clasifica como ω₁
Si P(ω₁|x) < P(ω₂|x) → x se clasifica como ω₂
Si P(ω₁|x) = P(ω₂|x) → indeterminado (puede asignarse a cualquiera)
```

**Usando la regla de Bayes** (p(x) es igual para ambas clases):

```
p(x|ω₁)P(ω₁) ≷ p(x|ω₂)P(ω₂)
```

**Si las probabilidades a priori son iguales** (P(ω₁) = P(ω₂) = 1/2):

```
p(x|ω₁) ≷ p(x|ω₂)
```

La decisión se basa solo en las verosimilitudes evaluadas en x.

### Extensión a M Clases

**Regla de Bayes generalizada**:

```
Asignar x a ωₘ si:
P(ωₘ|x) > P(ωⱼ|x) para todo j ≠ m
```

**Equivalentemente**:

```
clase(x) = arg max P(ωᵢ|x)
             i

         = arg max p(x|ωᵢ) × P(ωᵢ)
             i
```

---

## CLASIFICADOR BAYESIANO GAUSSIANO

### Distribución Gaussiana (Normal)

**Caso univariado (1 dimensión)**:

```
p(x) = (1 / √(2πσ²)) × exp(-(x - μ)² / (2σ²))
```

Parámetros:

- μ: media
- σ²: varianza

**Caso multivariado (l dimensiones)**:

```
p(x) = 1 / ((2π)^(l/2) |Σ|^(1/2)) × exp(-1/2 (x - μ)ᵀ Σ⁻¹ (x - μ))
```

Parámetros:

- **μ**: vector de medias (l × 1)
  ```
  μ = E[x]
  ```
- **Σ**: matriz de covarianza (l × l)
  ```
  Σ = E[(x - μ)(x - μ)ᵀ]
  ```
- **|Σ|**: determinante de Σ
- **Σ⁻¹**: inversa de Σ

### Matriz de Covarianza

**Definición**:

```
Σ = [σ₁₁  σ₁₂  ...  σ₁ₗ]
    [σ₂₁  σ₂₂  ...  σ₂ₗ]
    [ ⋮    ⋮    ⋱   ⋮ ]
    [σₗ₁  σₗ₂  ...  σₗₗ]
```

Donde:

- **Diagonal**: σᵢᵢ = varianza de la i-ésima variable
- **Fuera de diagonal**: σᵢⱼ = covarianza entre variables i y j

**Propiedades**:

- Simétrica: Σ = Σᵀ
- Semidefinida positiva
- σᵢⱼ = σⱼᵢ

### Estimación de Parámetros

**Dado conjunto de entrenamiento** de clase ωᵢ: {x₁, x₂, ..., xₙ}

**Estimador de Máxima Verosimilitud (MLE)**:

**Media**:

```
μ̂ᵢ = (1/N) Σₙ₌₁ᴺ xₙ
```

**Covarianza**:

```
Σ̂ᵢ = (1/N) Σₙ₌₁ᴺ (xₙ - μ̂ᵢ)(xₙ - μ̂ᵢ)ᵀ
```

**Probabilidad a priori**:

```
P̂(ωᵢ) = Nᵢ / N_total
```

donde Nᵢ es el número de muestras de clase ωᵢ

### Implementación del Clasificador Bayesiano

```python
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_iris
from sklearn.metrics import (accuracy_score, confusion_matrix,
                             classification_report)

# 1. Cargar datos
iris = load_iris()
X = iris.data
y = iris.target
nombres_clases = iris.target_names
nombres_features = iris.feature_names

print(f"Dataset: {len(X)} muestras, {X.shape[1]} características")
print(f"Clases: {nombres_clases}")
print(f"Características: {nombres_features}")

# 2. Dividir en train/test (80/20) con estratificación
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

print(f"\nTrain: {len(X_train)} muestras")
print(f"Test: {len(X_test)} muestras")

# 3. ENTRENAMIENTO: Calcular parámetros por clase
clases = np.unique(y_train)
parametros = {}

print("\n" + "="*70)
print("FASE DE ENTRENAMIENTO")
print("="*70)

for clase in clases:
    # Filtrar datos de esta clase
    X_clase = X_train[y_train == clase]

    # Calcular media (vector l-dimensional)
    media = np.mean(X_clase, axis=0)

    # Calcular covarianza (matriz l×l)
    covarianza = np.cov(X_clase.T)

    # Calcular probabilidad a priori
    prior = len(X_clase) / len(X_train)

    # Guardar parámetros
    parametros[clase] = {
        'media': media,
        'covarianza': covarianza,
        'prior': prior,
        'n_muestras': len(X_clase)
    }

    print(f"\n--- Clase {clase}: {nombres_clases[clase]} ---")
    print(f"Muestras de entrenamiento: {len(X_clase)}")
    print(f"Probabilidad a priori P(ω{clase}): {prior:.4f}")
    print(f"Vector de medias μ{clase}:")
    for i, (feat, val) in enumerate(zip(nombres_features, media)):
        print(f"  {feat}: {val:.3f}")
    print(f"Determinante de Σ{clase}: {np.linalg.det(covarianza):.6f}")

# 4. Función PDF Gaussiana Multivariada
def pdf_gaussiana_multivariada(x, media, covarianza):
    """
    Calcula la densidad de probabilidad según distribución Gaussiana multivariada

    Parámetros:
    -----------
    x : array (l,)
        Vector de características
    media : array (l,)
        Vector de medias
    covarianza : array (l, l)
        Matriz de covarianza

    Retorna:
    --------
    pdf : float
        Valor de la densidad de probabilidad
    """
    l = len(media)  # Dimensionalidad

    # Calcular diferencia x - μ
    diff = x - media

    # Calcular determinante e inversa de covarianza
    try:
        det_cov = np.linalg.det(covarianza)
        inv_cov = np.linalg.inv(covarianza)
    except np.linalg.LinAlgError:
        # Si la matriz es singular, usar pseudoinversa
        det_cov = np.linalg.det(covarianza + 1e-6 * np.eye(l))
        inv_cov = np.linalg.pinv(covarianza)

    # Término exponencial: -1/2 (x-μ)ᵀ Σ⁻¹ (x-μ)
    exponente = -0.5 * diff.T @ inv_cov @ diff

    # Coeficiente de normalización
    coeficiente = 1 / ((2 * np.pi) ** (l/2) * np.sqrt(abs(det_cov)))

    # PDF completa
    pdf = coeficiente * np.exp(exponente)

    return pdf

# 5. Función de clasificación
def clasificar_bayesiano(x, parametros):
    """
    Clasifica una muestra usando regla de Bayes

    Parámetros:
    -----------
    x : array (l,)
        Muestra a clasificar
    parametros : dict
        Diccionario con parámetros por clase

    Retorna:
    --------
    clase_predicha : int
        Clase con mayor probabilidad a posteriori
    posteriors : dict
        Probabilidades a posteriori de cada clase
    """
    posteriors = {}

    for clase, params in parametros.items():
        # Calcular verosimilitud: p(x|ωᵢ)
        likelihood = pdf_gaussiana_multivariada(
            x,
            params['media'],
            params['covarianza']
        )

        # Calcular posterior: p(x|ωᵢ) × P(ωᵢ)
        # (No dividimos por p(x) porque es constante para todas las clases)
        posterior = likelihood * params['prior']
        posteriors[clase] = posterior

    # Retornar clase con mayor posterior
    clase_predicha = max(posteriors, key=posteriors.get)

    return clase_predicha, posteriors

# 6. PRUEBA: Predecir conjunto de test
print("\n" + "="*70)
print("FASE DE PRUEBA")
print("="*70)

y_pred = []
all_posteriors = []

for x in X_test:
    clase_pred, posteriors = clasificar_bayesiano(x, parametros)
    y_pred.append(clase_pred)
    all_posteriors.append(posteriors)

# Convertir a numpy array
y_pred = np.array(y_pred)

# 7. EVALUACIÓN
print("\n--- RESULTADOS ---")
accuracy = accuracy_score(y_test, y_pred)
print(f"\nAccuracy: {accuracy:.4f} ({accuracy*100:.2f}%)")

print("\n--- Matriz de Confusión ---")
cm = confusion_matrix(y_test, y_pred)
print(cm)

print("\n--- Reporte de Clasificación ---")
print(classification_report(y_test, y_pred,
                           target_names=nombres_clases))

# 8. Analizar algunos ejemplos específicos
print("\n" + "="*70)
print("EJEMPLOS DE CLASIFICACIÓN")
print("="*70)

for i in range(min(5, len(X_test))):
    x_ejemplo = X_test[i]
    y_real = y_test[i]
    y_predicho = y_pred[i]
    posteriors = all_posteriors[i]

    print(f"\n--- Ejemplo {i+1} ---")
    print(f"Características: {x_ejemplo}")
    print(f"Clase real: {nombres_clases[y_real]}")
    print(f"Clase predicha: {nombres_clases[y_predicho]}")
    print(f"{'✓ CORRECTO' if y_real == y_predicho else '✗ INCORRECTO'}")

    print("\nProbabilidades a posteriori (sin normalizar):")
    total_posterior = sum(posteriors.values())
    for clase in clases:
        posterior = posteriors[clase]
        posterior_norm = posterior / total_posterior
        print(f"  P(ω{clase}|x) = {posterior:.6e} "
              f"(normalizada: {posterior_norm:.4f})")

# 9. Comparar con implementación de sklearn
print("\n" + "="*70)
print("COMPARACIÓN CON SKLEARN")
print("="*70)

from sklearn.discriminant_analysis import QuadraticDiscriminantAnalysis

# QDA (Quadratic Discriminant Analysis) = Clasificador Bayesiano Gaussiano
qda = QuadraticDiscriminantAnalysis()
qda.fit(X_train, y_train)
y_pred_sklearn = qda.predict(X_test)

accuracy_sklearn = accuracy_score(y_test, y_pred_sklearn)
print(f"\nAccuracy sklearn (QDA): {accuracy_sklearn:.4f}")
print(f"Accuracy implementación manual: {accuracy:.4f}")
print(f"Diferencia: {abs(accuracy - accuracy_sklearn):.6f}")

# 10. Ejercicio de ejemplo manual
print("\n" + "="*70)
print("EJERCICIO MANUAL DE CLASIFICACIÓN")
print("="*70)

# Ejemplo: clasificar x = [5.0, 3.0, 1.5, 0.3]
x_manual = np.array([5.0, 3.0, 1.5, 0.3])
print(f"\nClasificar: x = {x_manual}")

clase_pred, posteriors = clasificar_bayesiano(x_manual, parametros)

print("\nCálculos por clase:")
for clase in clases:
    params = parametros[clase]
    likelihood = pdf_gaussiana_multivariada(
        x_manual, params['media'], params['covarianza']
    )
    posterior = likelihood * params['prior']

    print(f"\n  Clase {nombres_clases[clase]}:")
    print(f"    Prior P(ω{clase}): {params['prior']:.4f}")
    print(f"    Likelihood p(x|ω{clase}): {likelihood:.6e}")
    print(f"    Posterior p(x|ω{clase})P(ω{clase}): {posterior:.6e}")

print(f"\nClase predicha: {nombres_clases[clase_pred]}")
```

### Ejercicios Prácticos del PDF

**Ejercicio 1**: Calcular PDF Gaussiana en puntos específicos

```python
# Calcular N(m, S) en x₁ = [0.2, 1.3]ᵀ y x₂ = [2.2, -1.3]ᵀ
# donde m = [0, 1]ᵀ, S = [[1, 0], [0, 1]]

m = np.array([0, 1])
S = np.array([[1, 0], [0, 1]])
x1 = np.array([0.2, 1.3])
x2 = np.array([2.2, -1.3])

pdf_x1 = pdf_gaussiana_multivariada(x1, m, S)
pdf_x2 = pdf_gaussiana_multivariada(x2, m, S)

print(f"p(x₁) = {pdf_x1:.6f}")
print(f"p(x₂) = {pdf_x2:.6f}")
```

**Ejercicio 2**: Clasificación con dos clases

```python
# Dos clases con distribuciones Gaussianas
# m₁ = [1, 1]ᵀ, m₂ = [3, 3]ᵀ
# S₁ = S₂ = [[1, 0], [0, 1]]
# P(ω₁) = P(ω₂) = 1/2
# Clasificar x = [1.8, 1.8]ᵀ

parametros_ej2 = {
    0: {
        'media': np.array([1, 1]),
        'covarianza': np.array([[1, 0], [0, 1]]),
        'prior': 0.5
    },
    1: {
        'media': np.array([3, 3]),
        'covarianza': np.array([[1, 0], [0, 1]]),
        'prior': 0.5
    }
}

x_ej2 = np.array([1.8, 1.8])
clase_pred, posteriors = clasificar_bayesiano(x_ej2, parametros_ej2)

print(f"Clasificar x = {x_ej2}")
print(f"Clase predicha: ω{clase_pred}")
for clase, post in posteriors.items():
    print(f"  P(ω{clase}|x) ∝ {post:.6e}")
```

**Ejercicio 3**: Efecto de probabilidades a priori diferentes

```python
# Repetir ejercicio 2 con:
# a) P(ω₁) = 1/6, P(ω₂) = 5/6
# b) P(ω₁) = 5/6, P(ω₂) = 1/6

# Caso a)
parametros_ej3a = {
    0: {
        'media': np.array([1, 1]),
        'covarianza': np.array([[1, 0], [0, 1]]),
        'prior': 1/6
    },
    1: {
        'media': np.array([3, 3]),
        'covarianza': np.array([[1, 0], [0, 1]]),
        'prior': 5/6
    }
}

clase_pred_a, posteriors_a = clasificar_bayesiano(x_ej2, parametros_ej3a)
print(f"\nCaso a) P(ω₁)=1/6, P(ω₂)=5/6:")
print(f"  Clase predicha: ω{clase_pred_a}")

# Caso b)
parametros_ej3b = {
    0: {
        'media': np.array([1, 1]),
        'covarianza': np.array([[1, 0], [0, 1]]),
        'prior': 5/6
    },
    1: {
        'media': np.array([3, 3]),
        'covarianza': np.array([[1, 0], [0, 1]]),
        'prior': 1/6
    }
}

clase_pred_b, posteriors_b = clasificar_bayesiano(x_ej2, parametros_ej3b)
print(f"\nCaso b) P(ω₁)=5/6, P(ω₂)=1/6:")
print(f"  Clase predicha: ω{clase_pred_b}")

print("\n¿De qué depende el resultado de clasificación?")
print("Depende tanto de la verosimilitud p(x|ωᵢ) como de la probabilidad")
print("a priori P(ωᵢ). Los priors pueden inclinar la decisión hacia")
print("la clase más probable a priori, especialmente cuando las")
print("verosimilitudes son similares.")
```

### Características del Clasificador Bayesiano

**Ventajas:**

- ✅ Óptimo bajo suposición Gaussiana
- ✅ Fundamentación teórica sólida
- ✅ Maneja correlaciones entre características
- ✅ No requiere iteraciones (entrenamiento directo)
- ✅ Incorpora conocimiento a priori

**Desventajas:**

- ❌ Requiere muchos datos para estimar covarianza
- ❌ Complejidad O(d³) por inversa de matriz
- ❌ Sensible a datos no Gaussianos
- ❌ Requiere N ≫ d² para estimación confiable
- ❌ Puede ser numéricamente inestable con matrices mal condicionadas

**Requiere N muestras por clase donde**:

- Para estimar media: N ∼ d
- Para estimar covarianza: N ∼ d²

---

## CLASIFICADOR NAIVE BAYES

### Motivación: La Maldición de la Dimensionalidad

**Problema**: Número de muestras requeridas crece exponencialmente con dimensionalidad.

**Ejemplo**:

- 1 dimensión: N = 200 muestras suficientes
- 2 dimensiones: N² = 40,000 muestras necesarias
- l dimensiones: N^l muestras necesarias

Para d = 10 dimensiones con N = 200:

- Muestras requeridas: 200¹⁰ = 10²³ muestras (¡imposible!)

### Solución: Suposición de Independencia

**Suposición clave de Naive Bayes**: Las características son **estadísticamente independientes**.

**Implicación matemática**:

```
p(x|ωᵢ) = p(x₁, x₂, ..., xₗ|ωᵢ)
        = p(x₁|ωᵢ) × p(x₂|ωᵢ) × ... × p(xₗ|ωᵢ)
        = ∏ⱼ₌₁ˡ p(xⱼ|ωᵢ)
```

**Consecuencia**: Ahora necesitamos estimar **l PDFs de 1 dimensión** para cada clase, en lugar de 1 PDF de l dimensiones.

**Muestras requeridas**: l × N en lugar de N^l

### PDF Gaussiana Univariada (1D)

Para cada característica j y clase ωᵢ:

```
p(xⱼ|ωᵢ) = (1 / √(2πσᵢⱼ²)) × exp(-(xⱼ - μᵢⱼ)² / (2σᵢⱼ²))
```

Parámetros por característica y clase:

- **μᵢⱼ**: media de característica j en clase ωᵢ
- **σᵢⱼ²**: varianza de característica j en clase ωᵢ

### Regla de Clasificación Naive Bayes

**Asignar muestra x a la clase**:

```
ωₘ = arg max P(ωᵢ) × ∏ⱼ₌₁ˡ p(xⱼ|ωᵢ)
      ωᵢ
```

**En log-espacio (más estable numéricamente)**:

```
ωₘ = arg max [log P(ωᵢ) + Σⱼ₌₁ˡ log p(xⱼ|ωᵢ)]
      ωᵢ
```

### Complejidad de Parámetros

| Clasificador           | Parámetros por Clase            | Total Parámetros |
| ---------------------- | ------------------------------- | ---------------- |
| **Bayesiano Completo** | μ (vector d×1) + Σ (matriz d×d) | C × (d + d²)     |
| **Naive Bayes**        | d medias + d varianzas          | C × 2d           |

Para C = 3 clases, d = 10 características:

- **Bayesiano**: 3 × (10 + 100) = 330 parámetros
- **Naive Bayes**: 3 × 20 = 60 parámetros

### Implementación Completa de Naive Bayes

```python
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_iris
from sklearn.metrics import (accuracy_score, confusion_matrix,
                             classification_report)

# 1. Cargar datos
iris = load_iris()
X = iris.data
y = iris.target
nombres_clases = iris.target_names
nombres_features = iris.feature_names

print(f"Dataset Iris: {X.shape[0]} muestras, {X.shape[1]} características")
print(f"Clases: {nombres_clases}")

# 2. Dividir train/test (80/20)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

print(f"\nTrain: {len(X_train)} muestras")
print(f"Test: {len(X_test)} muestras")

# 3. ENTRENAMIENTO: Calcular parámetros (medias y varianzas por característica)
clases = np.unique(y_train)
n_caracteristicas = X_train.shape[1]
parametros_nb = {}

print("\n" + "="*70)
print("FASE DE ENTRENAMIENTO - NAIVE BAYES")
print("="*70)
print("\nSUPOSICIÓN: Las características son estadísticamente independientes")
print("Por lo tanto: p(x|ωᵢ) = ∏ⱼ p(xⱼ|ωᵢ)")

for clase in clases:
    # Filtrar datos de esta clase
    X_clase = X_train[y_train == clase]

    # Calcular media y varianza POR CARACTERÍSTICA
    medias = np.mean(X_clase, axis=0)  # Vector (n_caracteristicas,)
    varianzas = np.var(X_clase, axis=0)  # Vector (n_caracteristicas,)

    # Evitar varianzas cero
    varianzas = np.where(varianzas == 0, 1e-9, varianzas)

    # Probabilidad a priori
    prior = len(X_clase) / len(X_train)

    # Guardar parámetros
    parametros_nb[clase] = {
        'medias': medias,
        'varianzas': varianzas,
        'prior': prior,
        'n_muestras': len(X_clase)
    }

    print(f"\n--- Clase {clase}: {nombres_clases[clase]} ---")
    print(f"Muestras: {len(X_clase)}")
    print(f"P(ω{clase}): {prior:.4f}")
    print(f"\nParámetros por característica:")
    for j in range(n_caracteristicas):
        print(f"  {nombres_features[j]}:")
        print(f"    μ{clase}{j} = {medias[j]:.3f}")
        print(f"    σ²{clase}{j} = {varianzas[j]:.3f}")

# 4. Función PDF Gaussiana 1D
def pdf_gaussiana_1d(x, media, varianza):
    """
    Calcula PDF Gaussiana para UNA característica

    p(x) = (1/√(2πσ²)) × exp(-(x-μ)²/(2σ²))

    Parámetros:
    -----------
    x : float
        Valor de la característica
    media : float
        Media μ
    varianza : float
        Varianza σ²

    Retorna:
    --------
    pdf : float
        Densidad de probabilidad
    """
    # Evitar división por cero
    if varianza == 0:
        varianza = 1e-9

    # Coeficiente
    coef = 1 / np.sqrt(2 * np.pi * varianza)

    # Exponente
    exponente = -((x - media) ** 2) / (2 * varianza)

    return coef * np.exp(exponente)

# 5. Clasificador Naive Bayes
def clasificar_naive_bayes(x, parametros, usar_log=True):
    """
    Clasifica una muestra usando Naive Bayes

    ASUME independencia entre características:
    p(x|ωᵢ) = p(x₁|ωᵢ) × p(x₂|ωᵢ) × ... × p(xₗ|ωᵢ)

    Parámetros:
    -----------
    x : array (l,)
        Muestra a clasificar
    parametros : dict
        Parámetros por clase
    usar_log : bool
        Si True, usa log-probabilidades (más estable)

    Retorna:
    --------
    clase_predicha : int
    posteriors : dict
        Probabilidades a posteriori
    """
    posteriors = {}
    log_posteriors = {}

    for clase, params in parametros.items():
        if usar_log:
            # Trabajar en log-espacio (más estable numéricamente)
            log_posterior = np.log(params['prior'])

            # Sumar log-probabilidades de cada característica
            for j in range(len(x)):
                pdf_j = pdf_gaussiana_1d(
                    x[j],
                    params['medias'][j],
                    params['varianzas'][j]
                )
                log_posterior += np.log(pdf_j + 1e-300)  # Evitar log(0)

            log_posteriors[clase] = log_posterior
        else:
            # Trabajar en espacio normal (puede tener underflow)
            posterior = params['prior']

            # Multiplicar probabilidades de cada característica
            for j in range(len(x)):
                pdf_j = pdf_gaussiana_1d(
                    x[j],
                    params['medias'][j],
                    params['varianzas'][j]
                )
                posterior *= pdf_j

            posteriors[clase] = posterior

    # Retornar clase con mayor posterior
    if usar_log:
        clase_predicha = max(log_posteriors, key=log_posteriors.get)
        # Convertir log-posteriors a posteriors normales
        max_log = max(log_posteriors.values())
        posteriors = {
            c: np.exp(log_p - max_log)
            for c, log_p in log_posteriors.items()
        }
    else:
        clase_predicha = max(posteriors, key=posteriors.get)

    return clase_predicha, posteriors

# 6. PRUEBA: Clasificar conjunto de test
print("\n" + "="*70)
print("FASE DE PRUEBA")
print("="*70)

y_pred = []
all_posteriors = []

for x in X_test:
    clase_pred, posteriors = clasificar_naive_bayes(x, parametros_nb)
    y_pred.append(clase_pred)
    all_posteriors.append(posteriors)

y_pred = np.array(y_pred)

# 7. EVALUACIÓN
print("\n--- RESULTADOS NAIVE BAYES ---")
accuracy_nb = accuracy_score(y_test, y_pred)
print(f"\nAccuracy: {accuracy_nb:.4f} ({accuracy_nb*100:.2f}%)")

print("\n--- Matriz de Confusión ---")
cm = confusion_matrix(y_test, y_pred)
print(cm)

print("\n--- Reporte de Clasificación ---")
print(classification_report(y_test, y_pred,
                           target_names=nombres_clases))

# 8. Ejemplo detallado de clasificación
print("\n" + "="*70)
print("EJEMPLO DETALLADO DE CLASIFICACIÓN")
print("="*70)

idx_ejemplo = 0
x_ejemplo = X_test[idx_ejemplo]
y_real = y_test[idx_ejemplo]

print(f"\nMuestra: {x_ejemplo}")
print(f"Clase real: {nombres_clases[y_real]}")

print("\nCálculo paso a paso:")
for clase in clases:
    params = parametros_nb[clase]
    print(f"\n  --- Clase {nombres_clases[clase]} ---")
    print(f"  Prior P(ω{clase}): {params['prior']:.4f}")

    print(f"  Cálculo de likelihood (producto de PDFs):")
    likelihood = 1.0
    for j in range(len(x_ejemplo)):
        pdf_j = pdf_gaussiana_1d(
            x_ejemplo[j],
            params['medias'][j],
            params['varianzas'][j]
        )
        print(f"    p(x{j}={x_ejemplo[j]:.2f}|ω{clase}): {pdf_j:.6f}")
        print(f"      μ{clase}{j} = {params['medias'][j]:.2f}, "
              f"σ²{clase}{j} = {params['varianzas'][j]:.2f}")
        likelihood *= pdf_j

    posterior = likelihood * params['prior']
    print(f"  Likelihood ∏p(xⱼ|ω{clase}): {likelihood:.6e}")
    print(f"  Posterior P(ω{clase}) × ∏p(xⱼ|ω{clase}): {posterior:.6e}")

clase_pred_ejemplo = clasificar_naive_bayes(x_ejemplo, parametros_nb)[0]
print(f"\n  → Clase predicha: {nombres_clases[clase_pred_ejemplo]}")
print(f"  → {'✓ CORRECTO' if clase_pred_ejemplo == y_real else '✗ INCORRECTO'}")

# 9. Comparar con sklearn
print("\n" + "="*70)
print("COMPARACIÓN CON SKLEARN")
print("="*70)

from sklearn.naive_bayes import GaussianNB

gnb = GaussianNB()
gnb.fit(X_train, y_train)
y_pred_sklearn = gnb.predict(X_test)

accuracy_sklearn = accuracy_score(y_test, y_pred_sklearn)
print(f"\nAccuracy sklearn GaussianNB: {accuracy_sklearn:.4f}")
print(f"Accuracy implementación manual: {accuracy_nb:.4f}")
print(f"Diferencia: {abs(accuracy_nb - accuracy_sklearn):.6f}")

# 10. Comparar con Bayesiano completo
print("\n" + "="*70)
print("COMPARACIÓN: NAIVE BAYES vs BAYESIANO COMPLETO")
print("="*70)

from sklearn.discriminant_analysis import QuadraticDiscriminantAnalysis

# Bayesiano completo (QDA)
qda = QuadraticDiscriminantAnalysis()
qda.fit(X_train, y_train)
y_pred_qda = qda.predict(X_test)

accuracy_qda = accuracy_score(y_test, y_pred_qda)

print(f"\nAccuracy Naive Bayes: {accuracy_nb:.4f}")
print(f"Accuracy Bayesiano Completo (QDA): {accuracy_qda:.4f}")
print(f"Diferencia: {abs(accuracy_nb - accuracy_qda):.6f}")

print("\n--- Análisis ---")
print("• Naive Bayes asume independencia → menos parámetros")
print("• Bayesiano completo captura correlaciones → más parámetros")
print("• En Iris, las características tienen cierta correlación")
print(f"• {'Bayesiano' if accuracy_qda > accuracy_nb else 'Naive Bayes'} "
      f"tiene mejor desempeño en este caso")

# 11. Análisis de correlaciones (verificar suposición)
print("\n--- Matriz de Correlación (verificar independencia) ---")
import pandas as pd

df_iris = pd.DataFrame(X_train, columns=nombres_features)
correlation_matrix = df_iris.corr()
print(correlation_matrix)

print("\n¿Son las características independientes?")
print("Si hubiera independencia perfecta, todas las correlaciones")
print("fuera de la diagonal serían 0. Aquí vemos que sí hay")
print("correlaciones significativas, por lo que la suposición")
print("de Naive Bayes es violada. Sin embargo, ¡aún funciona bien!")
```

### Ejercicio Práctico del PDF

```python
# Ejercicio: Clasificador Naive Bayes en dataset Iris
# Suposición: Características independientes, distribución Gaussiana 1D

print("="*70)
print("EJERCICIO: NAIVE BAYES EN IRIS")
print("="*70)

# Ya implementado arriba, pero aquí resumen del proceso:

print("\n1. División 80/20 (train/test) con estratificación")
print(f"   Train: {len(X_train)} muestras")
print(f"   Test: {len(X_test)} muestras")

print("\n2. Estimación de parámetros por característica y clase:")
print("   Para cada clase ωᵢ y cada característica j:")
print("   • μᵢⱼ = media de característica j en clase i")
print("   • σ²ᵢⱼ = varianza de característica j en clase i")

print("\n3. Clasificación usando:")
print("   p(x|ωᵢ) = ∏ⱼ₌₁⁴ (1/√(2πσ²ᵢⱼ)) exp(-(xⱼ-μᵢⱼ)²/(2σ²ᵢⱼ))")

print("\n4. Asignar a clase con mayor P(ωᵢ) × p(x|ωᵢ)")

print(f"\n5. Resultado: Accuracy = {accuracy_nb:.4f}")
```

### Ventajas y Desventajas de Naive Bayes

**Ventajas:**

- ✅ **Menos parámetros**: 2Cl vs C(l + l²)
- ✅ **Robusto con pocos datos**: Funciona incluso con N < l²
- ✅ **Computacionalmente eficiente**: O(Cl) vs O(Cl³)
- ✅ **Funciona bien en alta dimensionalidad**: No requiere invertir matrices
- ✅ **Interpretable**: Parámetros por característica
- ✅ **No hay inversión de matriz**: No hay problemas de singularidad
- ✅ **Robusto a violaciones**: Funciona bien incluso si suposición es falsa

**Desventajas:**

- ❌ **Suposición de independencia**: Raramente es verdadera
- ❌ **Pérdida de información**: No captura correlaciones
- ❌ **Puede tener menor accuracy**: Si hay correlaciones fuertes
- ❌ **Sensible a características redundantes**: Las cuenta múltiples veces
- ❌ **Probabilidades mal calibradas**: Aunque clasificación sea correcta

### Comparación: Bayesiano vs Naive Bayes

| Aspecto                    | Bayesiano Completo                | Naive Bayes                    |
| -------------------------- | --------------------------------- | ------------------------------ |
| **Suposición**             | Características correlacionadas   | Características independientes |
| **Parámetros**             | Matriz covarianza completa (l×l)  | Solo varianzas (l valores)     |
| **Total parámetros**       | C × (l + l²)                      | C × 2l                         |
| **Datos necesarios**       | N ≫ l²                            | N ∼ l                          |
| **Complejidad entrenar**   | O(Cl³)                            | O(Cl)                          |
| **Complejidad clasificar** | O(l³)                             | O(l)                           |
| **Accuracy**               | Generalmente mayor (si hay datos) | Puede ser menor                |
| **Alta dimensionalidad**   | Falla sin suficientes datos       | Funciona bien                  |
| **Estabilidad numérica**   | Problemas con matrices singulares | Muy estable                    |
| **Interpretabilidad**      | Media                             | Alta                           |

### Cuándo Usar Cada Clasificador

**Usar Bayesiano Completo cuando:**

- Tienes muchos datos (N ≫ l²)
- Las características están correlacionadas
- Quieres máxima accuracy
- Dimensionalidad es baja (l < 20)

**Usar Naive Bayes cuando:**

- Datos limitados (N < l²)
- Alta dimensionalidad (l > 100)
- Necesitas eficiencia computacional
- Interpretabilidad es importante
- Como baseline rápido

### Ejemplo Numérico Completo

```python
# Ejemplo simple 2D para visualizar diferencias

print("\n" + "="*70)
print("EJEMPLO NUMÉRICO: COMPARACIÓN VISUAL")
print("="*70)

# Generar datos sintéticos 2D con correlación
from sklearn.datasets import make_classification

X_synthetic, y_synthetic = make_classification(
    n_samples=200,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    class_sep=2.0,
    random_state=42
)

# Dividir
X_train_syn, X_test_syn, y_train_syn, y_test_syn = train_test_split(
    X_synthetic, y_synthetic, test_size=0.3, random_state=42
)

# Entrenar ambos clasificadores
# Naive Bayes
gnb_syn = GaussianNB()
gnb_syn.fit(X_train_syn, y_train_syn)
y_pred_nb_syn = gnb_syn.predict(X_test_syn)
acc_nb_syn = accuracy_score(y_test_syn, y_pred_nb_syn)

# Bayesiano completo
qda_syn = QuadraticDiscriminantAnalysis()
qda_syn.fit(X_train_syn, y_train_syn)
y_pred_qda_syn = qda_syn.predict(X_test_syn)
acc_qda_syn = accuracy_score(y_test_syn, y_pred_qda_syn)

print(f"\nDataset sintético 2D:")
print(f"  Accuracy Naive Bayes: {acc_nb_syn:.4f}")
print(f"  Accuracy Bayesiano: {acc_qda_syn:.4f}")

# Visualizar fronteras de decisión
import matplotlib.pyplot as plt

def plot_decision_boundary(clf, X, y, title):
    h = 0.02
    x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
    y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                         np.arange(y_min, y_max, h))

    Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)

    plt.contourf(xx, yy, Z, alpha=0.3, cmap='RdYlBu')
    plt.scatter(X[:, 0], X[:, 1], c=y, cmap='RdYlBu',
                edgecolors='black', s=50)
    plt.title(title)
    plt.xlabel('Característica 1')
    plt.ylabel('Característica 2')

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

plt.sca(ax1)
plot_decision_boundary(gnb_syn, X_train_syn, y_train_syn,
                      f'Naive Bayes (Acc: {acc_nb_syn:.3f})')

plt.sca(ax2)
plot_decision_boundary(qda_syn, X_train_syn, y_train_syn,
                      f'Bayesiano Completo (Acc: {acc_qda_syn:.3f})')

plt.tight_layout()
plt.show()

print("\nObservación:")
print("• Naive Bayes: fronteras lineales o cuadráticas simples")
print("• Bayesiano: fronteras cuadráticas más complejas")
print("• Bayesiano captura mejor la estructura cuando hay correlaciones")
```

---

## RESUMEN DE CLASIFICADORES

### Tabla Comparativa Completa

| Característica           | K-Means            | PCA                    | Bayesiano      | Naive Bayes       |
| ------------------------ | ------------------ | ---------------------- | -------------- | ----------------- |
| **Tipo**                 | No supervisado     | Transformación         | Supervisado    | Supervisado       |
| **Objetivo**             | Agrupar            | Reducir dimensiones    | Clasificar     | Clasificar        |
| **Suposición principal** | Clusters esféricos | Linealidad             | Gaussianidad   | Independencia     |
| **Parámetros**           | k centroides       | M componentes          | μ, Σ por clase | μⱼ, σ²ⱼ por clase |
| **Entrenamiento**        | Iterativo          | Directo (eigenvectors) | Directo (MLE)  | Directo (MLE)     |
| **Complejidad entrenar** | O(nkdi)            | O(nd² + d³)            | O(Cnd²)        | O(Cnd)            |
| **Complejidad predecir** | O(kd)              | O(Md)                  | O(Cd³)         | O(Cd)             |
| **Datos necesarios**     | N ∼ k              | N ≥ d                  | N ≫ d²         | N ∼ d             |
| **Alta dimensión**       | Regular            | Excelente              | Falla          | Excelente         |
| **Interpretabilidad**    | Alta               | Media                  | Media          | Alta              |

### Flujo de Trabajo Típico

```python
# Pipeline completo de preprocesamiento y clasificación

from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.naive_bayes import GaussianNB
from sklearn.model_selection import cross_val_score

# 1. Crear pipeline
pipeline = Pipeline([
    ('scaler', StandardScaler()),        # Normalizar
    ('pca', PCA(n_components=0.95)),     # Reducir dimensiones (95% varianza)
    ('classifier', GaussianNB())         # Clasificar
])

# 2. Entrenar
pipeline.fit(X_train, y_train)

# 3. Evaluar
y_pred = pipeline.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)

# 4. Validación cruzada
scores = cross_val_score(pipeline, X, y, cv=5)
print(f"Accuracy: {scores.mean():.3f} (+/- {scores.std():.3f})")
```

---

## COMANDOS PYTHON ESENCIALES

### Preprocesamiento

```python
# Limpieza de datos
df.fillna(df.mean())                    # Rellenar con media
df.fillna(df.median())                  # Rellenar con mediana
df.fillna(method='ffill')               # Forward fill
df.dropna()                             # Eliminar filas con NaN

# Detección de outliers
outliers = abs(X - X.mean()) >= 3 * X.std()
X_clean = X[~outliers]

# Normalización
from sklearn.preprocessing import StandardScaler, MinMaxScaler

scaler = StandardScaler()               # Z-score
X_std = scaler.fit_transform(X)

scaler = MinMaxScaler()                 # Min-max [0,1]
X_norm = scaler.fit_transform(X)
```

### Balanceo

```python
from imblearn.over_sampling import SMOTE
from imblearn.under_sampling import RandomUnderSampler

# SMOTE
smote = SMOTE(random_state=42, k_neighbors=5)
X_bal, y_bal = smote.fit_resample(X_train, y_train)

# Under-sampling
rus = RandomUnderSampler(random_state=42)
X_bal, y_bal = rus.fit_resample(X_train, y_train)
```

### Clustering

```python
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# K-Means
kmeans = KMeans(n_clusters=3, random_state=42)
labels = kmeans.fit_predict(X)

# Evaluar
silhouette = silhouette_score(X, labels)
inertia = kmeans.inertia_
```

### PCA

```python
from sklearn.decomposition import PCA

# PCA completo
pca = PCA()
pca.fit(X_scaled)

# PCA con umbral
pca = PCA(n_components=0.95)  # 95% varianza
X_pca = pca.fit_transform(X_scaled)

# Analizar
varianza = pca.explained_variance_ratio_
varianza_acum = varianza.cumsum()
```

### Clasificación Bayesiana

```python
from sklearn.naive_bayes import GaussianNB
from sklearn.discriminant_analysis import QuadraticDiscriminantAnalysis

# Naive Bayes
gnb = GaussianNB()
gnb.fit(X_train, y_train)
y_pred = gnb.predict(X_test)

# Bayesiano completo (QDA)
qda = QuadraticDiscriminantAnalysis()
qda.fit(X_train, y_train)
y_pred = qda.predict(X_test)
```

### Evaluación

```python
from sklearn.metrics import (accuracy_score, precision_score,
                             recall_score, f1_score,
                             confusion_matrix, classification_report)

# Métricas básicas
accuracy = accuracy_score(y_true, y_pred)
precision = precision_score(y_true, y_pred, average='weighted')
recall = recall_score(y_true, y_pred, average='weighted')
f1 = f1_score(y_true, y_pred, average='weighted')

# Matriz de confusión
cm = confusion_matrix(y_true, y_pred)

# Reporte completo
print(classification_report(y_true, y_pred))
```

---

## NOTAS FINALES DEL SEGUNDO PARCIAL

### ✅ Alcance del Examen

**Temas incluidos (hasta Clase 11):**

1. ✅ Preprocesamiento (limpieza, transformación)
2. ✅ Balanceo de clases (SMOTE y variantes)
3. ✅ Clustering (K-Means, K-Medians, K-Medoids)
4. ✅ PCA (reducción dimensional)
5. ✅ Clasificación Bayesiana (completa)
6. ✅ Naive Bayes (con suposición de independencia)

**Temas NO incluidos:**

- ❌ Árboles de Decisión
- ❌ Máquinas de Soporte Vectorial (SVM)

### 📚 Conceptos Clave para Estudiar

**Preprocesamiento:**

- Técnicas para datos faltantes
- Detección y eliminación de outliers
- Métodos de normalización (cuándo usar cada uno)

**Balanceo:**

- Diferencia entre over-sampling y under-sampling
- SMOTE: algoritmo, ventajas, desventajas
- Cuándo aplicar balanceo (antes/después de split)
- Métricas apropiadas para datos desbalanceados

**Clustering:**

- Diferencia entre supervisado y no supervisado
- K-Means: algoritmo, función objetivo, convergencia
- Variantes: K-Medians (mediana, L₁), K-Medoids (puntos reales)
- Métricas: inercia, silhouette, Davies-Bouldin
- Método del codo para determinar k

**PCA:**

- Objetivo: reducción dimensional preservando varianza
- Componentes principales = eigenvectors de matriz covarianza
- Proceso: centrar → covarianza → eigenvalues → proyectar
- Criterios de selección: umbral 85%, scree plot, Kaiser
- Importancia de normalizar ANTES de PCA

**Clasificación Bayesiana:**

- Regla de Bayes: P(ωᵢ|x) ∝ p(x|ωᵢ) × P(ωᵢ)
- PDF Gaussiana multivariada: parámetros μ y Σ
- Estimación de parámetros con MLE
- Requiere N ≫ d² datos

**Naive Bayes:**

- Suposición de independencia: p(x|ωᵢ) = ∏ⱼ p(xⱼ|ωᵢ)
- Ventaja: menos parámetros (2Cl vs C(l+l²))
- PDF Gaussiana 1D por característica
- Robusto incluso si suposición es violada
- Funciona bien en alta dimensionalidad

### 💡 Tips para el Examen

1. **Entender cuándo usar cada método**: Supervisado vs no supervisado, con/sin etiquetas

2. **Recordar fórmulas clave**:

   - K-Means: mᵢ = Σbₜᵢxₜ / Σbₜᵢ
   - PCA: Su = λu
   - Bayes: P(ωᵢ|x) ∝ p(x|ωᵢ)P(ωᵢ)
   - Naive Bayes: p(x|ωᵢ) = ∏ⱼ p(xⱼ|ωᵢ)

3. **Diferencias importantes**:

   - K-Means vs K-Medians vs K-Medoids
   - Bayesiano vs Naive Bayes
   - Inercia vs Silhouette
   - Over-sampling vs Under-sampling

4. **Proceso completo**:

   - Cargar datos → Limpiar → Normalizar → Balancear (si aplica)
   - Dividir train/test → Entrenar → Evaluar
   - Usar métricas apropiadas (no solo accuracy)

5. **Implementación práctica**:
   - Saber usar bibliotecas: sklearn, imblearn
   - Entender parámetros importantes
   - Interpretar resultados

---

## 📊 DATASETS RECOMENDADOS PARA PRÁCTICA

- **Iris**: Clasificación (Bayesiano, Naive Bayes), PCA
- **Wine**: PCA, clustering, clasificación
- **Breast Cancer**: Clasificación, balanceo
- **MNIST (pequeño)**: PCA, clasificación
- **Credit Card Fraud**: Balanceo con SMOTE
- **NASA/JIRA**: Predicción de errores (tema del curso)

¡Buena suerte en el examen! 🎯
