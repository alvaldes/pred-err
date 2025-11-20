# Clase14 18-11-25

## Evaluación

### Particionamiento de conjunto de datos

Muchas veces ocurre el fenómeno data leakage y esto ocurre por no hacer bien el Particionamiento.

El proceso de seleccionar los mejores hiperparámetros se llama Validación.

Debemos separar en 3 conjuntos:

- Conjunto de entrenamiento: son usados para aprender la instancia del modelo a partir de una clase de modelo.

- Conjunto de Validación: son usados explícitamente para seleccionar los hiperparámetros con el mejor rendimiento de acuerdo a una estimación de la generalización de error.

- Conjunto de prueba: son usados exclusivamente para evaluar el rendimiento al final del proceso y nunca son usados en el proceso de aprendizaje.

Si queremos conocer acerca del rendimiento de un modelo procedemos de la siguiente forma:

1. Particionar el conjunto original en datos de entrenamiento y prueba. Pro ejemplo, usar 30% (20%) del conjunto original para propósitos de prueba. Usar el 30% cuando el conjunto es pequeño (1000 - 1500) o mediano.

2. Usar los datos de entrenamiento restantes para seleccionar los hiperparámetros mediante Validación cruzada.

3. Entrenar nuevamente el modelo con los parámetros seleccionados, utilizando el conjunto de entrenamiento completo.

4. Evaluar el rendimiento del modelo obtenido usando el conjunto de prueba.

### Validación cruzada (cross-validation)

Tecnica de validacion de modelos para evaluar como generalizaria los resultados de un analisis estadistico a un conjunto de datos independiente (test). Es utlizada para estimar que tan exacto seria un modelo predictivo en la practica.

Una ronda de validacion cruzada involucra particionar una muestra de datos en subconjutos complenetarios, se ejecuta el analisis en un subconjunto (llamado conjunto de entrenamiento), y se valida el analisis en el otro subconjunto (llamado conjunto de validacion).

Para reducir la variabilidad, se ejecutan multiples rondas de validacion cruzadad utilizando direrentes particiones y los resultados de validacion son promediados sobre las rondas. Esto es lo que se usaba lo viejo.

Lo que se usa ahora es:

### Validacion Cruzada con K iteraciones (K-fold crossvalidation)

En esta tecnica, el conjunto de datos X se divide aleatoriamente en K partes de tamaño igual, Xi, i=1...,K. Para generar cada par, mantenemos una de las K partes fuera como el conjunto de validacion y combinamos las restantes K -1 partes para formar el conjunto de entrenamient. Hacemos esto K veces, cada vez dejando afuera otra de las K partes, y obtenemos K pares:

V1 = X1, T1 = X2 U X3 U ... U Xk
V2 = X2, T2 = X1 U X3 U ... U Xk
.
.
.
Vk = Xk, Tk = X1 U X2 U ... U Xk-1

Los resultados de validacion son promediados sobre las K iteraciones. Usuaalmente el valor de K es 10 o 30 (30 casi no se usa)

accuracy = (ri+r2+...+rk)/k ± std

Ej: acc=92 ± 1.5

> Mejor 93.5%, Peor 90.5%

#### Uso de la validacion cruzada

- Mi clasificador generaliza y poner el promedio
- Busqueda de hiperparámetros

### Busqueda de hiperparámetros

```pseudo
CrossValidation(X, k)
X is the train set and k is the K-fold
Partition data into k equally sized blocks X = X1 U ... U Xk
For all γ ϵ {γmin, ..., γmax}
  For all i ϵ {1, ..., k}
    Train a model of complexity γ on X\Xi
    Compute the error E(γ, Xi) on the test set Xi
  Compute the mean error E(γ) = 1/k ∑i=1 hasta k (E(γ, Xi))
Choose the value γopt = argmin γ E(γ) with smallest mean error
Train the final model with compplexity γopt on the whole data set X
```

No se recomienda en conjuntos de datos grandes

> se recomentoda usar un particionamiento aleatorio stratified (representacion de las clases en los porcientos 60-20-20)

### Leave-one-out cross-validation (LOOCV) - Jacknife in UK

Variante del k-fold cross-validation donde k = N (numero de instancias) en el conjunto de datos. En cada iteración, se usa una sola instancia como conjunto de validación y el resto como conjunto de entrenamiento. Se repite este proceso para cada instancia en el conjunto de datos.

Esto es utilizado en aplicaciones donde los datos etiquetados son difíciles de obtener (ej: diagnóstico medico).

> Se recomienda para conjuntos pequeños de datos.

> [!IMPORTANT]
> Práctica: Utilizando una herramienta o biblioteca de clasificación para aprendizaje computacional, realice lo siguiente:
> • Implemente el método de validación cruzada con K iteraciones para encontrar el o los mejores parámetros de los algoritmos K-NN y máquina de soporte vectorial. Utilice un conjunto de datos de su proyecto para la experimentación.
> • Realice lo mismo que el punto anterior, pero utilizando validación cruzada "leave-one-out".
> Fecha de entrega: 24/noviembre/2025
