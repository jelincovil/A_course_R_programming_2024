
# Unidad III:   Programando con R

La programación implica escribir sistemas de instrucciones relativamente complejos.
Hay dos estilos amplios de programación: el estilo **imperativo** implica encadenar 
instrucciones que le dicen a la computadora
qué hacer. El estilo **declarativo** implica
escribir una descripción del resultado final, sin dar detalles sobre cómo
para llegar allí.

##  **Control del flujo "flow" DETERMINISTA**

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
```r
```

```r
```

```r
```

### ¿Qué son las funciones?
### Alcance de las variables
### Devolución de varios objetos
### Uso de clases de S3 para controlar la impresión
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

