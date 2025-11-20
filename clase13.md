# Clase13 10/11/2025

## Máquinas de Soporte Vectorial (SVM - Support Vector Machines)

Son definidas naturalmente para clasificacion binario,
Aunque se hacen cosas para hacerlos multiclase, naturalmente son binarios.

La etiqueta target debe ser {-1, +1}

Como todos los problemas lineanes, las SVMs usan hiperplanos
de separacion como limites de descicion entre dos clases.

El margen es aquel que espacio que separa el plano de la clase.

Un hiperplano de margen maximo es aquel que separa limpiamente
a las 2 clases, y para la cual una region grande (o margen) existe
entre el hiperplano y los ejemplos de entrenamiento mas cercanos.

> [!NOTE]
> Esto de margen maximo es lo que hace que un clasificador se bueno
> generalizando a datos no vistos.

### SVM: Caso linealmente separable

funcion linear: w^T \* x +w0 = 0

w^T \* x + w0 >= +1 ∀x e w1

w^T \* x + w0 <= -1 ∀x e w2

> [!NOTE]
> w se calcula con lo de λ o la constante de Lagrange

### SVM: Clases no linealmente separables

Tres categorias:

- fuera de la banda yi(w^T \* xi + w0) >= 1
- dentro de la banda 0 <= yi(w^T \* xi + w0) < 1
- clasificados erroneamente - pasado la banda yi(w^T \* xi + w0) <= 0

Los tres casos pueden ser tratados con una sola formula:

yi(w^T \* xi + w0) >= 1 - ξi

ξi < 0 => correctamente clasificado y fuera de la banda
0 < ξi < 1 => correctamente clasificado pero dentro de la banda
ξi > 1 => incorrectamente clasificado

> [!NOTE]
> ξi (<ji>) tambien es conocido como variable de holgura o de ajuste

> [!IMPORTANT]
> Tarea: Investigar el caso multiclase y el truco de kernel para introducir
> funciones kernel que ayudan a resolver el caso para clases no separables.
> XOR - espacio no lineal - introduccion de un Kernel - se agrega otra dimension

## Investigación: Clasificación Multiclase y Kernel Trick para Clases No Separables Linealmente

### 1. Clasificación Multiclase en SVM

Las Máquinas de Vectores de Soporte (SVM) son inherentemente clasificadores binarios, pero se han desarrollado estrategias meta-algorítmicas para manejar problemas multiclase:

#### Estrategias Principales:

**One-vs-Rest (OvR):**
- Se entrena un clasificador por cada clase
- Cada modelo trata una clase como positiva y todas las demás como negativas
- Para N clases, se crean N modelos
- La predicción final se basa en el modelo con mayor puntuación

**One-vs-One (OvO):**
- Se entrenan K(K-1)/2 clasificadores binarios para K clases
- Cada clasificador distingue entre un par de clases
- La predicción final usa un esquema de votación
- Preferible para pocas clases con recursos computacionales suficientes

### 2. El Kernel Trick: Fundamento Matemático

#### Concepto Central:
El kernel trick permite calcular productos internos entre puntos de datos en un espacio de características de alta dimensión sin transformar explícitamente los datos.

#### Formulación Matemática:
Para datos `x_i ∈ ℝⁿ` y un kernel `k : ℝⁿ × ℝⁿ → ℝ`, existe una transformación no lineal `φ : ℝⁿ → ℝᵐ` tal que:

```
k(x_i, x_j) = ⟨φ(x_i), φ(x_j)⟩
```

#### Ventajas:
- **Eficiencia computacional**: Evita el cómputo explícito en espacios de alta dimensión
- **Espacios infinito-dimensionales**: Permite trabajar implícitamente en espacios de dimensión infinita
- **Flexibilidad**: Transforma algoritmos lineales para resolver problemas no lineales

### 3. El Problema XOR: Caso No Separable Linealmente

#### Definición del Problema:
El XOR es un ejemplo clásico de datos no separables linealmente:
- Puntos: (0,0)→0, (0,1)→1, (1,0)→1, (1,1)→0
- Imposible separar con una línea recta en 2D

#### Solución con Kernel Trick:
**Transformación de características:**
- Agregar característica `x₁ × x₂` hace el problema linealmente separable
- El kernel RBF mapea automáticamente a un espacio de dimensión infinita
- La separación se vuelve posible con un hiperplano lineal en el espacio transformado

### 4. Tipos de Kernels y Mapeo Dimensional

#### Kernel Lineal:
```
K(x_i, x_j) = x_i · x_j
```
- Para datos linealmente separables
- Computación rápida y simple

#### Kernel Polinomial:
```
K(x_i, x_j) = (x_i · x_j + c)ᵈ
```
- Captura interacciones entre características
- Popular en procesamiento de lenguaje natural
- Grado d=2 (cuadrático) más común

#### Kernel RBF (Gaussiano):
```
K(x, y) = exp(-γ||x - y||²)
```
- Mapea a dimensiones infinitas
- Maneja relaciones complejas y no lineales
- Más utilizado en práctica
- Ideal para patrones circulares o espirales

#### Kernel Sigmoide:
```
K(x, y) = tanh(γx^T y + r)
```
- Simula función de activación neuronal
- Para problemas con comportamiento de umbralización suave

### 5. Selección de Kernels

#### Criterios de Selección:
- **Datos lineales**: Kernel lineal
- **Tendencias curvadas**: Kernel polinomial
- **Formas complejas**: Kernel RBF
- **Patrones desconocidos**: Comenzar con RBF
- **Similitud neuronal**: Kernel sigmoide

#### Aplicaciones Específicas:
- **RBF**: Detección médica, patrones biológicos
- **Polinomial**: NLP, análisis de texto
- **Lineal**: Clasificación de documentos, datos sparse

La transformación kernel permite a los SVM resolver eficientemente problemas no lineales como XOR, convirtiendo espacios no separables en espacios donde la separación lineal es posible, todo esto sin el costo computacional de transformaciones explícitas de alta dimensión.
