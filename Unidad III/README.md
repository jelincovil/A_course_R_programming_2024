
# Unidad III:   Programando con R

# Clase del sábado 28-05-2024

La programación implica escribir sistemas de instrucciones relativamente complejos.
Hay dos estilos amplios de programación: el estilo **imperativo** implica encadenar 
instrucciones que le dicen a la computadora
qué hacer. El estilo **declarativo** implica
escribir una descripción del resultado final, sin dar detalles sobre cómo
para llegar allí.

##  **Control del flujo "flow" DETERMINISTA**

graph TD
A[Inicio] --> B{¿Bucle for?}
B --> C{Sí} --> D[Ejecutar bloque de código]
D --> E{¿Continuar?}
E --> C
B --> F{No} --> G[Ejecutar bloque de código while]
H{¿Condición?}
G --> H
H --> I{Sí} --> D
H --> F
I --> G
F --> J[Fin]

**Explicación del diagrama:**

* El diagrama comienza en el nodo **A (Inicio)**.
* Desde **A**, hay una pregunta que decide si se debe usar un bucle **for** o un bucle **while**.
* Si la respuesta a la pregunta es **sí** (nodo **B**), el flujo pasa al nodo **C (Ejecutar bloque de código)**.
* Dentro del bucle **for**, el bloque de código se ejecuta (nodo **D**).
* Luego, hay otra pregunta que decide si se debe continuar con el bucle (nodo **E**).
* Si la respuesta es **sí**, el flujo vuelve al nodo **C** para ejecutar el bloque de código nuevamente.
* Si la respuesta es **no**, el flujo sale del bucle **for** y pasa al nodo **F (No)**.
* Si la respuesta a la pregunta inicial (nodo **B**) es **no**, el flujo pasa directamente al nodo **G (Ejecutar bloque de código while)**.
* Dentro del bucle **while**, el bloque de código se ejecuta (nodo **D**).
* Luego, la condición del bucle se evalúa en el nodo **H (¿Condición?)**.
* Si la condición es **sí**, el flujo vuelve al nodo **G** para ejecutar el bloque de código nuevamente.
* Si la condición es **no**, el flujo sale del bucle **while** y pasa al nodo **F (No)**.
* Finalmente, el flujo termina en el nodo **J (Fin)**.
* 
### El loop *for ( )*

Calculo de un factorial:

```r
n <- 100
result <- 1
for (i in 1:n)
result <- result * i
result
```
Calculo de la sequencia de Fibonacci:

```r
Fibonacci <- numeric(12)
Fibonacci[1] <- Fibonacci[2] <- 1
for (i in 3:12)
Fibonacci[i] <- Fibonacci[i - 2] + Fibonacci[i - 1]
```




### El condicionador *if (  )*

Syntax

```r
if (condition) {commands when TRUE}
if (condition) {commands when TRUE} else {commands when FALSE}
```
Un ejemplo simple

```r
x <- 3
if (x > 2) y <- 2 * x else y <- 3 * x
```

```r
Eratosthenes <- function(n) {
# Return all prime numbers up to n (based on the sieve of Eratosthenes)
if (n >= 2) {
sieve <- seq(2, n)
primes <- c()
for (i in seq(2, n)) {
if (any(sieve == i)) {
primes <- c(primes, i)
sieve <- c(sieve[(sieve %% i) != 0], i)
}
}
return(primes)
} else {
stop("Input value of n should be at least 2.")
}
}

Eratosthenes(50)
```

### El loop *while ( )*

Syntax
```r
while (condition) {statements}

```

Calculo de números de la secuencia de Fibonacci:
```r
Fib1 <- 1
Fib2 <- 1
Fibonacci <- c(Fib1, Fib2)
while (Fib2 < 300) {
Fibonacci <- c(Fibonacci, Fib2)
oldFib2 <- Fib2
Fib2 <- Fib1 + Fib2
Fib1 <- oldFib2
}

Fibonacci

```

```r
```
### el loop *repeat* y el *break* y proximos declaraciones

Syntax
```r
repeat { statements } if (condition) break
```

```r
x <- x0
tolerance <- 0.000001
repeat {
f <- xˆ3 + 2 * xˆ2 - 7
if (abs(f) < tolerance) break
f.prime <- 3 * xˆ2 + 4 * x
x <- x - f / f.prime
}
x
```

```r
```



## *GESTION DE LA COMPLEJIDAD ATRAVEZ DE FUNCIONES*

La mayoría de los programas informáticos reales son mucho más largos que los ejemplos que damos en este libro. La mayoría de las personas no pueden tener todos los detalles en la cabeza a la vez, por lo que es extremadamente importante encontrar formas de reducir la complejidad. A lo largo de los años se han desarrollado diversas estrategias de diseño de programas. En esta sección damos un breve resumen de algunas de las estrategias que nos han funcionado.

Las funciones son unidades autónomas de código R con un propósito bien definido. En general, las funciones toman entradas, hacen cálculos (posiblemente imprimiendo resultados intermedios, dibujando gráficos, llamando a otras funciones, etc.) y producen resultados. Si las entradas y salidas están bien definidas, el programador puede estar razonablemente seguro de si la función funciona o no: y una vez que funciona, puede pasar al siguiente problema.



### ¿Qué son las funciones?
Sintaxis
```r
func_name <- function (parameters) {
statement
}
```

```r
annuityAmt <- function(n, R, i) {
R*((1 + i)ˆn - 1) / i
}
annuityAmt(10, 400, 0.05)
```

```r
# define a function to compute power
power <- function(a = 2, b) {
    print(paste("a raised to the power b is: ", a^b))
}

# call the power function with arguments
power(2, 3)

# call function with default arguments
power(b=3)
```

```r
# define a function to compute power
power <- function(a, b) {
    return (a^b)
}

# call the power function with arguments
print(paste("a raised to the power b is: ", power(2, 3)))
```



### Devolución de varios objetos

```r
annuityValues <- function(n, R, i) {
amount <- R*((1 + i)ˆn - 1) / i
PV <- amount * (1 + i)ˆ(-n)
list(amount = amount, PV = PV)
}
annuityValues(10, 400, 0.05)
```


### Uso de clases de S3 para controlar la impresión

# Clase del martes 28-05-2024

### El operador %>% del paquete *magrittr*
###  La función *replicate ( )*

## *Consejos de programación miscelánea*

### Edite siempre el código en el editor, no en la consola

### Documentación usando "#"

### ¡La limpieza cuenta!

### Algunas pautas generales de programación

### Diseño de arriba hacia abajo

## *Depuración y mantenimiento*

### Reconociendo errores

### Hacer que el error sea reproducible

### Identificar la causa del error

### Corrección de errores y pruebas.

### Busque errores similares en otros lugares

### Depuración en RStudio

### Funciones de *browser( )*, *debug( )*, and *debugonce( )* 

### Depuración de los operadores %>% magrittr

## *Programación eficiente*

### Aprende tus herramientas

### Algoritmos eficientes

### Mide el tiempo que tarda tu programa

### Estar dispuesto a utilizar diferentes herramientas

### Optimice con cuidado

## *Referencias*
1.  Introduction  to  Scientific  Programming and Simulation  Using R: [https://nyu-cdsc.github.io/learningr/assets/simulation.pdf](https://nyu-cdsc.github.io/learningr/assets/simulation.pdf)
2.  Advanced R: [https://adv-r.hadley.nz/](https://adv-r.hadley.nz/)

