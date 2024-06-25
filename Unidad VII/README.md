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
  z(\alpha, \beta) = \sum_{i=1}^{n} y_i^2 - 2\alpha\sum_{i=1}^{n} y_i - 2\beta\sum_{i=1}^{n} x_iy_i + n\alpha^2 + \beta^2\sum_{i=1}^{n} x_i^2 + 2\alpha\beta\sum_{i=1}^{n} x_i
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

## Programación linear
### Resolver problemas de programación lineal en R
### Maximización y otros tipos de restricciones.
### Situaciones especiales
### Variables no restringidas
### Programación entera
### Alternativas a *lp ( )*
### Programación cuadrática
