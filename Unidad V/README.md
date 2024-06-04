
# Simulación

Cinco aplicaciones principales de la simulación estadística con R:

1. **Modelos de Regresión**: R permite simular datos que siguen un modelo o estructura dada². Esto es útil para comprender la naturaleza de los datos y para poner a prueba algunos procedimientos de modelos de regresión.

2. **Análisis de escenarios hipotéticos**: La simulación estadística en R implica crear modelos o experimentos computacionales para imitar procesos reales, generando datos aleatorios mediante funciones estadísticas y métodos de muestreo.

3. **Análisis de series de tiempo**: R proporciona funciones para analizar y predecir datos de series de tiempo.

4. **Pruebas estadísticas**: R permite realizar una amplia gama de pruebas estadísticas, que se pueden aplicar en diversas disciplinas.

5. **Estadísticas descriptivas**: R ofrece funciones para calcular estadísticas descriptivas básicas como la media, mediana, desviación típica, varianza, rango, cuartiles, percentiles y estadísticas de resumen para la exploración de datos.



## Generación de números pseudoaleatorios

La Generación de números pseudoaleatorios en computación se refiere al proceso de producir secuencias de números que parecen aleatorios, pero en realidad son generados por un algoritmo determinista. Estos son los puntos clave:

## Simulación de otras variables aleatorias.

### Generación de números Uniformes(a, b) 

```r
runif(5)

runif(10, min = -3, max = -1)
```

```r
u <- runif(10, min = -3, max = -1)
hist(u)
```

Notemos que una misma función con los mismo parametros genera secuencias de numeros diferentes.

```r

runif(5)

```

La función *set.seed(n)* fija la semilla de la generación de números aleatorios
proveyendo una misma secuencia de números:

```r

set.seed(32789) 
runif(5)

set.seed(666)
runif(5)

```

### Variables aleatorias de Bernoulli

```r

set.seed(23207) # use this to obtain our output
guesses <- runif(20)
correct.answers <- (guesses < 0.2)
correct.answers

```

```r

table(correct.answers)

```

```r
# Generamos números 0,1
rbinom(10,1,0.65)

Graficamos en un barplot
barplot(table(rbinom(10,1,0.65)))

```

### Variables aleatorias de Poisson

Función de probabilidad de Poisson

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/16/Poisson_pmf.svg/1280px-Poisson_pmf.svg.png" width="500"> 

Consideremos una variable $X \sim Poisson (3,7)$ con función de probabilidad
gráficada por

```r
s <- seq(0:8)
p <-  round( dpois(s,3.7), 3)
plot(s,p, type = "o")

```

```r
rpois(10, 3.7)

```




### Números aleatorios exponenciales

Función de densidad exponencial
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f5/Exponential_distribution_pdf_-_public_domain.svg/1280px-Exponential_distribution_pdf_-_public_domain.svg.png" width="500"> 

Un banco tiene un solo cajero que se enfrenta a una fila de 10 clientes. El
El tiempo para atender a cada cliente se distribuye exponencialmente con la tasa.
3 por minuto. Podemos simular los tiempos de servicio (en minutos) para los 10
clientes.


```r
t <- seq(0,1, by=0.1)

p <-  round( dexp(t, rate = 3), 3)

plot(t, p, type = "l", col = "blue", 
     lwd=2, ylab = "f_exp", xlab = "T")

```


```r

servicetimes <- rexp(30, rate = 3)
servicetimes
hist(servicetimes)

```

Simulando 25 puntos de un proceso de Poisson de parametro $\lambda = 1.5$ comenzando desde 0.



```r

X <- rexp(25, rate = 1.5)
cumsum(X)
plot(0:24, X, type = "o", 
     main = "Proceso de Poisson",
     xlab = "Tiempo")

```

### Variables aleatorias Normales/Gaussianas

Función de densidad Normal
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/74/Normal_Distribution_PDF.svg/1920px-Normal_Distribution_PDF.svg.png" width="500"> 


```r
x <- rnorm(10, -3, 0.5)
print(x)
hist(x, main = "Variables Normales)

```

```r

x <- rnorm(100000) # simulate from the standard normal
x <- x[(0 < x) & (x < 3)] # reject all x's outside (0,3)
hist(x, probability=TRUE, main = "Variables Normales)

```


## Generación de números aleatorios multivariados

```r


```


```r


```


```r


```

--------------------------------------------------------------------------
## Simulación de cadena de Markov

## Integración de Monte Carlo

## Métodos avanzados de simulación

### Muestreo de rechazo/aceptación
### Muestreo de rechazo para distribuciones bivariadas
### Importancia del muestreo
### El algoritmo de Metrópolis-Hastings



##  Sección 1

### ddd
### ddd
### ddd

## Referencias
