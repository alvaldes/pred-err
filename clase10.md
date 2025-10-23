# Clase10 16/10/25

## Clasificacion para prediccion de errores

> Patron a posteriore P(wi|x): probabilidad de que el patron x pertenezca a la clase wi

### Teoria de decision Bayesiana

> funcion de densidad de probabilidad (pdf) condicional P(x|wi): probabilidad de observar el patron x dado que pertenece a la clase wi.
> Tambien conocido como funcion de verosimilitud

> Se usa P mayuscula para denotar cuando ya se conoce el valor

#### Regla Bayesiana

P(wi|x) = (p(x|wi) \* P(wi)) / P(x)

donde:
p(x|wi) = funcion de verosimilitud
P(wi) = probabilidad a priori de la clase wi
P(x) = probabilidad total del patron x

La regla de clasificacion Bayesiana se puede establecer como:

- si P(w1|x) > P(w2|x) entonces x pertenece a la clase w1
- si P(w2|x) > P(w1|x) entonces x pertenece a la clase w2

> [!NOTE]
> si P(w1) = P(w2) = 1/2 la regla de clasificacion Bayesiana se puede reducir a p(x|w1) > p(x|w2)

> [!IMPORTANT]
> esto es un metodo parametrico porque utiliza parametros del dataset (medias y varianzas) para modelar las distribuciones de probabilidad.
> a esto es a lo que se le conoce como entrenamiento pero no hay iteraciones todo se calcula de una sola vez.

La pdf gausiana para l dimensiones es:
p(x) = 1/((2π)^(l/2) \* |Σ|^(1/2)) \* exp(-1/2 (x - μ)^T Σ^(-1) (x - μ))

clase(x) = arg max {p(X|wi) \* P(wi)}
i

> [!IMPORTANT]
> A la funcion de verosimilitud se le llama tambien evidencia y a la probabilidad a priori se le llama creencia.

> Tarea:
> Implementar un clasificador Bayesiano para el dataset de Iris.
> separar el dataset en 80% train y 20% test
