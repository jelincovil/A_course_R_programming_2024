# Optimización numérica


## Optimizando funciones objetivos

Cuando tenemos una función analitica que necesita ser minimizada o maximizada local o globalmente,
estamos frente al caso de un caso de optimización de una función objetivo.

Esto ocurre en al caso de querer encontrar un estimador de maxima verosimilitud o los estimadores de minimos cuadrados.

### Máximos y minimos globales

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
  z(\alpha, \beta) = \sum_{i=1}^n= (y_i - \alpha - \beta x_i)^2 = \sum_{i=1}^{n} y_i^2 - 2\alpha\sum_{i=1}^{n} y_i - 2\beta\sum_{i=1}^{n} x_iy_i + n\alpha^2 + \beta^2\sum_{i=1}^{n} x_i^2 + 2\alpha\beta\sum_{i=1}^{n} x_i
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

```r

```

```r

```


```r

```


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

  
```r


```

```r


```

```r


```



### Maximización y otros tipos de restricciones.
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


### Alternativas a *lp ( )*
### Programación cuadrática
