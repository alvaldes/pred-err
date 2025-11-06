# Clase12 03/11/2025

## Árboles de Decisión

Un árbol de decision booleano es equivalente lógicamente a la afirmación
de que el attr objetivo es verdadero iff los attrs de entrada satisfacen una de
las rutas que guían a una hoja con valor verdadero.

Objetivo <=> (Ruta1 V Ruta2 V ... V RutaN)

donde cada Ruta es una conjunción de pruebas de attr-valor requeridas para
seguir tal ruta.

> [!NOTE]
> Esto es lo llamada forma normal disyuntiva (DNF, disjunctive normal form) en
> lógica proposicional.
> Lo cual significa que cualquier función proposicional se puede expresar como
> un árbol de decision.

## Induciendo Arboles de Decision a partir de Ejemplos

Un Árbol de Decisión puede ser inducido a partir de un conjunto de ejemplos etiquetados.
(x,y) donde x es un vector de atributos y y es el valor de la clase objetivo (booleano).

### Condiciones de parada

1. Todos los ejemplos pertenecen a la misma clase
   → Se crea un nodo hoja con esa clase.
   (Ya no hay necesidad de seguir dividiendo.)

2. Los ejemplos están mezclados
   → Se selecciona el siguiente attr mas importante.
   (Esta es la parte recursiva del algoritmo.)

3. No hay más atributos disponibles para dividir
   → Se crea una hoja con la clase mayoritaria entre los ejemplos restantes.
   (No hay atributos que permitan dividir más.)

4. No hay ejemplos en el nodo (conjunto vacío)
   → Se asigna la clase más común del nodo padre.
   (Esto ocurre cuando una combinación de valores no aparece en los datos.)

### Selección del Atributo de Prueba

Buscar un árbol no tan profundo aunque no sea el mas pequeño posible.

> Un attr perfecto divide los ejemplos en conjuntos, cada uno de los cuales son
> todos positivos o todos negatibos y por lo tanto seran hojas del arbol.

Necesitamos una medida formal de "bastante bueno" e "inutil realmente".

Usaremos la nocion de ganancia de informacion, la cual se define en terminos de entropia.

La entropia es una medida de la incertidumbre asociada a una variable aleatoria.

B(q) es la entropia de una variable aleatoria booleana que es verdadera con probabilidad q:

> [!IMPORTANT]
> B(q) = -(qlog2 q + (1-q)log2(1-q))

H(Objetivo) = B(p/(p+n)) , donde p son los ejemplos positivos y n los negativos.

Resto(A) = sum desde v en Valores(A) de ((pv + nv)/(p+n)) \* B(pv/(pv + nv))

Ganancia(A) = B(p/(p+n)) - Resto(A)

> [!IMPORTANT]
> Tarea: Calcular la ganancia para Tipo y Patron

B(p/(p+n))=B(6/12)=1.000

La entropía global 𝐵 = 1.00

Atributo: Patrons

Valores → None, Some, Full

| Patrons | Yes | No  | Total |
| ------- | --- | --- | ----- |
| None    | 0   | 2   | 2     |
| Some    | 4   | 0   | 4     |
| Full    | 2   | 4   | 6     |

Entropías por valor:

- None → B(0/2) = 0

- Some → B(4/4) = 0

- Full → B(2/6) = -2/6 log2(2/6) - 4/6 log2(4/6) = 0.918

Resto( Patrons) = 12(0) + 12(0) + 12(0.918) = 0.459
Ganancia(Patrons) = 1.000 - 0.459 = 0.541

✅ Ganancia(Patrons) = 0.541 bits

Atributo: Type

Valores → French, Thai, Burger, Italian

| Type    | Yes | No  | Total |
| ------- | --- | --- | ----- |
| French  | 1   | 1   | 2     |
| Thai    | 2   | 2   | 4     |
| Burger  | 2   | 2   | 4     |
| Italian | 1   | 1   | 2     |

Entropías por valor:

- French → B(1/2) = 1

- Thai → B(2/4) = 1

- Burger → B(2/4) = 1

- Italian → B(1/2) = 1

Reto(Type)=2/12(1)+4/12(1)+4/12(1)+2/12(1)
Resto(Type) = 12/12 = 1
Ganancia(Type) = 1- 1 = 0

✅ Ganancia(Type) = 0

> [!NOTE]
> Los Arboles de decision se usan comumente en problemas de clasificación como selector de caracteristicas.
> De aqui se toman esos attr y se pasan aun mejor clsasificador (SVM, Redes Neuronales, etc.)

> [!IMPORTANT]
> Que es la discretizacion cuando hay valores reales? Y como lo hace el Arbol decision?

## Tarea

Descargar un conjunto de datos del UCI y separar en 80/20 para train/test.
Entrenar un árbol de decisión con el conjunto de entrenamiento y evaluar su desempeño en el conjunto de prueba.
