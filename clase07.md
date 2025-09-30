# Clase07 - Balanceo de Datos

## ¿Qué es el Balanceo de Datos?

El **balanceo de datos** es una técnica fundamental en machine learning que se utiliza para abordar el problema de **datasets desbalanceados**, donde algunas clases están significativamente sub-representadas o sobre-representadas en comparación con otras.

> [!IMPORTANT]
> Un dataset desbalanceado puede llevar a modelos sesgados que favorecen las clases mayoritarias, 
> resultando en un bajo rendimiento en la predicción de clases minoritarias.

### Problemas de los Datasets Desbalanceados

1. **Sesgo hacia la clase mayoritaria**: Los algoritmos tienden a predecir la clase más frecuente
2. **Baja sensibilidad**: Dificultad para detectar instancias de la clase minoritaria
3. **Métricas engañosas**: Alta precisión general pero pobre desempeño en clases importantes
4. **Pérdida de información valiosa**: Las clases minoritarias pueden contener información crítica

## Técnicas de Balanceo de Datos

### 1. Métodos de Sub-muestreo (Under-sampling)

Reducen el número de instancias de la clase mayoritaria:

- **Random Under-sampling**: Elimina aleatoriamente instancias de la clase mayoritaria
- **Tomek Links**: Elimina pares de instancias de clases diferentes que están muy cerca
- **Edited Nearest Neighbours (ENN)**: Remueve instancias mal clasificadas por sus vecinos
- **Condensed Nearest Neighbour (CNN)**: Mantiene solo las instancias esenciales para la frontera de decisión

### 2. Métodos de Sobre-muestreo (Over-sampling)

Incrementan el número de instancias de la clase minoritaria:

- **Random Over-sampling**: Duplica aleatoriamente instancias de la clase minoritaria
- **SMOTE**: Genera instancias sintéticas (explicado en detalle más adelante)
- **ADASYN**: Adaptive Synthetic Sampling
- **Borderline-SMOTE**: Versión mejorada de SMOTE

### 3. Métodos Híbridos

Combinan técnicas de sub-muestreo y sobre-muestreo:

- **SMOTEENN**: SMOTE + Edited Nearest Neighbours
- **SMOTETomek**: SMOTE + Tomek Links

### 4. Métodos Basados en Costos

- **Cost-sensitive learning**: Asigna diferentes costos a errores de clasificación
- **Threshold moving**: Ajusta el umbral de decisión del clasificador

## SMOTE (Synthetic Minority Oversampling Technique)

### ¿Qué es SMOTE?

**SMOTE** es una técnica de sobre-muestreo que genera **instancias sintéticas** de la clase minoritaria en lugar de simplemente duplicar las existentes. Fue propuesta por Chawla et al. en 2002 y se ha convertido en una de las técnicas más populares para el balanceo de datos.

### ¿Cómo funciona SMOTE?

El algoritmo SMOTE sigue estos pasos:

1. **Selección de instancia**: Elige una instancia de la clase minoritaria
2. **Identificación de vecinos**: Encuentra los k vecinos más cercanos de la misma clase (típicamente k=5)
3. **Selección aleatoria**: Selecciona aleatoriamente uno de estos k vecinos
4. **Generación sintética**: Crea una nueva instancia en algún punto de la línea que conecta la instancia original con el vecino seleccionado
5. **Repetición**: Repite el proceso hasta alcanzar el balance deseado

### Fórmula matemática de SMOTE:

```
x_nuevo = x_i + λ * (x_vecino - x_i)
```

Donde:
- `x_i` es la instancia original de la clase minoritaria
- `x_vecino` es el vecino seleccionado aleatoriamente
- `λ` es un número aleatorio entre 0 y 1
- `x_nuevo` es la nueva instancia sintética generada

### Ventajas de SMOTE:

1. **Evita el overfitting**: No duplica instancias existentes
2. **Generalización mejorada**: Las instancias sintéticas ayudan a generalizar mejor
3. **Preserva información**: Mantiene las características de la clase minoritaria
4. **Flexibilidad**: Puede ajustarse el número de vecinos (k)

### Desventajas de SMOTE:

1. **Ruido**: Puede generar instancias sintéticas en regiones de solapamiento entre clases
2. **Curse of dimensionality**: Menos efectivo en espacios de alta dimensionalidad
3. **Datos categóricos**: Diseñado originalmente para datos continuos
4. **Distribución**: Asume que las instancias sintéticas siguen la misma distribución

### Variantes de SMOTE:

1. **Borderline-SMOTE**: Se enfoca en generar instancias cerca de la frontera de decisión
2. **ADASYN**: Genera más instancias sintéticas en regiones donde hay menos densidad de la clase minoritaria
3. **SMOTE-NC**: Versión para datos que contienen características categóricas y continuas
4. **SMOTE-N**: Versión específica para datos categóricos

### Implementación práctica:

```python
from imblearn.over_sampling import SMOTE
from sklearn.datasets import make_classification
from collections import Counter

# Crear dataset desbalanceado
X, y = make_classification(n_samples=1000, n_classes=2, 
                          weights=[0.9, 0.1], random_state=42)

print(f"Distribución original: {Counter(y)}")

# Aplicar SMOTE
smote = SMOTE(random_state=42, k_neighbors=5)
X_balanced, y_balanced = smote.fit_resample(X, y)

print(f"Distribución después de SMOTE: {Counter(y_balanced)}")
```

## Métricas de Evaluación para Datos Desbalanceados

> [!IMPORTANT]
> En datasets desbalanceados, la precisión (accuracy) no es una métrica confiable.
> Es mejor usar métricas como F1-score, AUC-ROC, precisión, recall y especificidad.

### Métricas recomendadas:

1. **Precision**: TP / (TP + FP)
2. **Recall (Sensibilidad)**: TP / (TP + FN)
3. **F1-Score**: 2 * (Precision * Recall) / (Precision + Recall)
4. **AUC-ROC**: Área bajo la curva ROC
5. **AUC-PR**: Área bajo la curva Precision-Recall

## Consideraciones Importantes

### Cuándo usar balanceo de datos:

- **Ratio de desbalance > 10:1**: Cuando hay una diferencia significativa entre clases
- **Datos suficientes**: Cuando se tiene suficiente información en la clase minoritaria
- **Problema de clasificación**: Principalmente en tareas de clasificación supervisada

### Buenas prácticas:

1. **Aplicar después de la división**: Balancear solo el conjunto de entrenamiento
2. **Validación cruzada stratificada**: Mantener la proporción de clases en cada fold
3. **Experimentar con diferentes técnicas**: Probar múltiples métodos de balanceo
4. **Evaluar con métricas apropiadas**: No confiar solo en la precisión

> [!NOTE]
> El balanceo de datos no siempre mejora el rendimiento del modelo. 
> Es importante experimentar y evaluar cuidadosamente los resultados.