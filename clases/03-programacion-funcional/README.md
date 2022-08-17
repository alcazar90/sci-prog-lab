# Notas sobre Programación  Funcional


## Funciones

¿Qué pasa si consultamos por `c` luego de definir y llamar la siguiente
función?

```python
def sumar(a, b):
    c = a + b
    return c
```

Arroja un `NameError` debido a que la función crea un _scope_ donde ocurren
todos los computos, retorna el valor de `c` dentro de este _scope_ o contexto,
y luego elimina la traza de calculos realizados para obtener el valor
retornado. Por lo tanto, la variable `c` no queda asignada en el ambiente global.

¿Puede una función tener un número de parámetros variable?

Sí, es posible pasar un número arbitrario de parámetros sin que estos
se encuentren previamente definidos en la función utilizando los _keywords_
`*args` y `**kwargs`.

```python
def funcion_n_parametros(*args):
    """args: parámetros sin nombre"""
    print('Los parámetros entregados son:', args)
```

Importante que las operaciones que se ejecuten dentro de la función resistan
un número de parámetros arbitrarios.

Es posible utilizar un número de parámetros arbitrarios identificados
por nombres con el _keyword_ `**kwargs` donde la función extrae el 
contenido de un diccionario, y donde las llaves de este diccionario
representan los nombres de los argumentos.

```python
from sklearn.linear_model import LogisticRegression
config = {'random_state': 42, 'penalty': 'l1', 'solver': 'lbfgs'}
clf = LogisticRegression(**config)
```

En el ejemplo anterior extraemos los parámetros definidos en nuestra
configuración con el fin de emparejarlos con los atributos para instanciar
la clase `LogisticRegression`. Es posible que la configuración solo tenga
1 parámetros o los `n` parámetros que acepta el modelo. 

Una función en `python` puede retornar multiples valores en una tupla, la 
lógica de retornar una tupla es porque es una estructura de datos
inmutables, y responde a la lógica de no corromper los valores retornados.

```python
def operaciones(a, b):
    return a, b
```

## _Scopes_

Cada función define un entorno de variables o _namespace_. Son espacios
reservados para cierto conjunto de operaciones.


Tres tipos de _scope_ en `python`:

1. **Global:** variables (u objetos si se desea) definidas en el cuerpo del código.
1. **Local:** variables definidas dentro de una función.
1. **Built-in:** variables predefinidas por el modulo built-in's (e.g. `print`).

El _scope_ local tiene prioridad sobre el _scope_ global en case de existir
redundancia en el nombre de variables.

Es posible modificar un _scope_ local una variable que se encuentre en _global_
utilizando el _keyword_ `global`.


## Programación funcional: `lambda`, `map`, `filter`, y `reduce`


Las funciones `lambda` generalmente se asocian a `map`, `filter`, y `reduce`
usando el modulo `functools` de `python`. Estas son funciones que reciben
otras funciones de argumento.

### Map

Permite aplicar la función objetivo sobre una colección de elementos (e.g.
lista, array). _Upside_? Evitamos iterar explicitamente usando un `for`.

```python
def al_cuadrado(x):
    return x** 2

lista = [1, 2, 3, 4, 5, 6]
map(al_cuadrado, lista)
```

`map` retorna un objeto iterable que puede ser iterado pero aún no ha sido
generado. Es posible evaluarlo utilizando la función `list`:

```python
list(map(al_cuadrado, lista))
```

### Funciones `lambda`

El _keyword_ `lambda` nos permite crear funciones anónimas en `python`. Estas
son funciones que no se les asigna un nombre y la idea es que desempeñen 
operaciones bien simples. Si el computa no vale la pena nombrarlo es un
buen candidato para que sea una función anónima. Reescribamos la operación
anterior pero sin definir la función `al_cuadrado`:

```python
list(map(lambda x: x**2, lista))
```

¿Cuál es la diferencia entre `map` y list comprehension? Sí...

```python
[x ** 2 for x in lista]
```

### Filter

La función `filter` permite mantener elementos de un arreglo según el valor
de verdad asociado a cada uno por la función objetivo.


```python
lista = [1, 2, 3, 4, 5, 6]
list(filter(lambda x: x % 2 == 0, lista))
```

Combinando operaciones:

```python
lista = [1, 2, 3, 4, 5, 6]
list(filter(lambda x: x % 2 == 0, map(lambda x: x ** 2, lista)))
```

Operación equivalente con list comprehension:

```python
lista = [1, 2, 3, 4, 5, 6]
[x ** 2 for x in lista if x ** 2 % 2 == 0]
```


### Reduce

Permite acumular valores de izquierda a derecha según una función sobre algún
iterable. La idea es reducir la lista a un solo valor según la función
estipulada. El primer argumento de los 2 que necesita `reduce` es el valor
acumulado y el segundo el valor de la siguiente secuencia.

```python
fun = lambda x, y : x + y
functools.reduce(fun, [1, 2, 3, 4, 5])
```

Al ejecutar lo anterior obtenemos la siguiente cadena de operaciones:

1. `fun(0, 1)`
1. `fun(1, 2)`
1. `fun(fun(1, 2))`


### Ejercicios propuestos para practicar programación funcional


**TODO:** Ver los ejercicios en el _notebook_.


## Referencias

Cuando asignamos una lista a una variable lo que guardamos en realidad es
la referencia a la lista y no los elementos de la lista en si. Ejemplo:

```python
lista_1 = [1, 2, 3, 4, 5]
lista_2 = lista_1
```

Cualquier modificación en `lista_1` también influenciará a `lista_2`. Una
forma de evitar que los cambios sean referenciados es crear una copia
literal y no una asignación de puntero:

```python
from copy import deepcopy
lista_1 = [1, 2, 3, 4, 5]
lista_2 = deepcopy(lista_1)
```







