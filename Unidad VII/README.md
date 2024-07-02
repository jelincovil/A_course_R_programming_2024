# Optimización numérica


## Optimizando funciones objetivos

Cuando tenemos una función analitica que necesita ser minimizada o maximizada local o globalmente,
estamos frente al caso de un caso de optimización de una función objetivo.

Esto ocurre en al caso de querer encontrar un estimador de maxima verosimilitud o los estimadores de minimos cuadrados.

### Máximos y minimos globales

### MAx/min de una función continua univariada

```r
# Syntax
# optim(par, fn, ...)
```

Consideremos la función

$$
g(x) =|x-3.5| + |x-2| + |x-1|
$$

la cual tiene un minimo global.

```r
# Syntax
# optim(par, fn, ...)

g <- function(x) abs(x-3.5) + abs(x-2) + abs(x-1)

# Graficamos g
curve(g(x), from=-2, to=6,
      main="Función |x-3.5| + |x-2| + |x-1|")

# Como punto inicial x=0, comienza la busqueda
# del minimo
optim(0, g)

```

### Estimación de minimos cuadrado de una regresión
Veremos el caso de los estimadores de minimos cuadrados para un modelos de regresión lineal simple

$$
Y_i= \alpha + \beta X_i.
$$

para $i=1,2,...,n$.


```r
# Introduzimos el set de datos de X e Y
x = c(1, 3, 2, 3, 4)
y = c(10, 14, 12, 15, 20)
plot(x,y)
```

```r
# Rengo de los parametros y función a minimizar
alpha <- seq(-15, 15, by = 1)
beta <- seq(-15, 15, by = 1)

z = function(alpha, beta){
  out <- sum(y^2) - 2*alpha*sum(y)  - 2*beta*sum(y*x) + length(x)*alpha^2 +
    (beta^2)*sum(x^2) + 2*alpha*beta*sum(x)
  return(out)
}

```
La funcion de la suma de las diferencias al cuadrado entre el valor predicto y los reales como
función de alpha y beta es dado por

$$
  z(\alpha, \beta) = \sum_{i=1}^n (y_i - \alpha - \beta x_i)^2 = \sum_{i=1}^{n} y_i^2 - 2\alpha\sum_{i=1}^{n} y_i - 2\beta\sum_{i=1}^{n} x_iy_i + n\alpha^2 + \beta^2\sum_{i=1}^{n} x_i^2 + 2\alpha\beta\sum_{i=1}^{n} x_i
$$


```r
# Visualizamos la función a optimizar
SRC <- outer(alpha, beta, z)
persp(alpha, beta, SRC, theta=70)
```

Una persepectiva en otro angulo

```r
persp(alpha, beta, SRC, theta=120)

```

Usamos la función *optim()* de las funciones bases de R

```r
help(optim)
```

La apliacamos sobre la función z

```r
f <- function(par) z(par[1], par[2])
f(c(0,0))
set.seed(123)
optim(c(0,0), fn = f)
```
Los parametros alfa y betas que minimizan la función Z estan en

```r
optim(c(0,0), fn = f)$par
```

Finalmente, el modelo es para 6.001591 3.153134, es decir

$$
Y_i= 6 +  3.15X_i.
$$

para $i=1,2,...,n$.

Finalmente, podemos ver los datos junto con la linea media esperada
```r
# Grafica de los valores estimados y el puntos
plot(x,y)
curve(6 + 3.15*x, 
      from=0, to=5, 
      col="blue", 
      add= TRUE,
      lwd=3)
```

## Programación lineal




### Resolver problemas de programación lineal en R

Emplearemos una función de programación lineal disponible en R: la función lp() en el paquete *lpSolve* 
puede ser la más via disponible más estable. Se basa en el método símplex revisado.

```r

library(lpSolve)

```

La función lp() tiene una serie de parámetros; 

- objective.in: el vector de coeficientes de la función objetivo.
- const.mat: una matriz que contiene los coeficientes de las variables
  de decisión en el lado izquierdo de las restricciones; cada fila corresponde a un restricción.
- const.rhs: un vector que contiene las constantes dadas en el lado derecho de las restricciones.
- const.dir: un vector de caracteres que indica la dirección de la restricción
  para las desigualdades; Algunas de las entradas posibles son >=, == y <=.

### Ejemplo de las cantidade de sulfuro y carbono emitidos
  
```r
eg.lp <- lp(objective.in = c(5, 8), 
            const.mat = matrix(c(1, 1, 1, 2), nrow = 2), 
            const.rhs = c(2, 3), 
            const.dir = c(">=", ">="))
eg.lp
## El valor minimo de C= 13

eg.lp$solution

```

### Maximización y otros tipos de restricciones.

```r
eg.lp <- lp(objective.in = c(5, 8),
            const.mat = matrix(c(1, 1, 1, 2), nrow = 2),
            const.rhs = c(2, 3),
            const.dir = c("<=", "="), direction = "max")

eg.lp

#

eg.lp$solution

```

### Situaciones especiales

**Degeneración**

Para un problema con m variables de decisión, la degeneración surge cuando más de m límites de restricción se cruzan en un solo punto. Esta situación es bastante rara, pero tiene el potencial de causar dificultades para el símplex
método, por lo que es importante ser consciente de esta condición. En circunstancias muy raras, la degeneración puede impedir que el método converja a la solución óptima; La mayoría de las veces, sin embargo, hay poco de qué preocuparse
acerca de.

$$
x_1 + x_2 \leq 2; \quad x_1 + 4x_2 \leq 5.
$$


```r
degen.lp <- lp(objective.in = c(3, 1),
               const.mat = matrix(c(1, 1, 1, 4, 1, 2, 3, 1), nrow = 4),
               const.rhs = c(2, 3, 4, 4), const.dir = rep(">=", 4))
degen.lp
## El optimo de C es 3.333333
degen.lp$solution
## [1] 0.6666667 1.3333333

```


### Ejemplo aplicado al transporte

Vamos a revisar una aplicación que emplee la función `lpSolve` para resolver un sistema lineal a un problema de optimización de en el contexto de la producción.

### Situación a Resolver

**Problema:** Una fábrica produce dos productos, A y B. Cada producto requiere un tiempo específico en dos máquinas, Máquina 1 y Máquina 2. La fábrica quiere maximizar las ganancias produciendo estos productos, pero está limitada por la capacidad de tiempo disponible en cada máquina.

### Datos del Problema

1. **Tiempo requerido por producto (en horas):**
   - Producto A:
     - Máquina 1: 2 horas
     - Máquina 2: 1 hora
   - Producto B:
     - Máquina 1: 1 hora
     - Máquina 2: 3 horas

2. **Capacidad de tiempo disponible (en horas):**
   - Máquina 1: 100 horas
   - Máquina 2: 120 horas

3. **Ganancia por unidad producida:**
   - Producto A: $40
   - Producto B: $50

### Formulación del Sistema Lineal

**Función objetivo:** Maximizar las ganancias:

$$ 
\text{Maximizar } Z = 40A + 50B 
$$

**Restricciones:**
1. Capacidad de la Máquina 1:

$$
2A + 1B \leq 100
$$

3. Capacidad de la Máquina 2:

$$
1A + 3B \leq 120
$$

5. No negatividad:

$$
A \geq 0 
$$

$$ 
B \geq 0 
$$

### Código R para Resolver el Problema

A seguir presentamos el código en R para definir y resolver este problema utilizando `lpSolve`:

```r
# Instalar y cargar el paquete lpSolve
# Definir la función objetivo
f.obj <- c(40, 50)

# Definir las restricciones
f.con <- matrix(c(2, 1,  # Coeficientes de A y B en la restricción de la Máquina 1
                  1, 3), # Coeficientes de A y B en la restricción de la Máquina 2
                nrow = 2, byrow = TRUE)

# Definir la dirección de las restricciones
f.dir <- c("<=", "<=")

# Definir los lados derechos de las restricciones
f.rhs <- c(100, 120)

# Resolver el problema de programación lineal
resultado <- lp("max", f.obj, f.con, f.dir, f.rhs)

# Obtener y mostrar los resultados
valor_optimo <- resultado$objval
valores_variables <- resultado$solution

cat("Ganancia máxima:", valor_optimo, "\n")
cat("Unidades de A a producir:", valores_variables[1], "\n")
cat("Unidades de B a producir:", valores_variables[2], "\n")
```

### Explicación del Código

1. **Definir la función objetivo:** 
   ```r
   f.obj <- c(40, 50)
   ```
   Esto representa las ganancias por cada unidad de los productos A y B.

2. **Definir las restricciones:**
   ```r
   f.con <- matrix(c(2, 1,
                     1, 3), 
                   nrow = 2, byrow = TRUE)
   ```
   La matriz `f.con` contiene los coeficientes de las restricciones.

3. **Definir la dirección de las restricciones:**
   ```r
   f.dir <- c("<=", "<=")
   ```
   Esto indica que ambas restricciones son de tipo "menor o igual que".

4. **Definir los lados derechos de las restricciones:**
   ```r
   f.rhs <- c(100, 120)
   ```
   Estos son los valores máximos de horas disponibles para las máquinas 1 y 2.

5. **Resolver el problema:**
   ```r
   resultado <- lp("max", f.obj, f.con, f.dir, f.rhs)
   ```

6. **Obtener y mostrar los resultados:**
   ```r
   valor_optimo <- resultado$objval
   valores_variables <- resultado$solution

   cat("Ganancia máxima:", valor_optimo, "\n")
   cat("Unidades de A a producir:", valores_variables[1], "\n")
   cat("Unidades de B a producir:", valores_variables[2], "\n")
   ```

### Resultados Esperados

El resultado del código proporcionará la cantidad óptima de productos A y B que deben producirse para maximizar las ganancias, dado el tiempo disponible en las máquinas. También mostrará la ganancia máxima que se puede obtener bajo estas condiciones.

# Next (Sábado)
### Programación cuadrática
