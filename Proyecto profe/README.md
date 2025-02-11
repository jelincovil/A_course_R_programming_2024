# **Ejemplo de Proyecto de función de complejidad media o mayor**

## Titulo: función vectorizada para la estimación de curvas

### Objetivo: 

   - Estimar y gráficar la curva media sin ruido.
   - Estimar y gráficar un conjunto de  curvas medias sin ruido.

## Motivación empirica y teorica

<img src="https://fr.mathworks.com/help/wavelet/ug/noisysig_ex.png" width="800"> 


La idea es estimar una función continua $f(t)$ observada con un cierta distorción dada por un ruido $W(t)$

$$
Y(t) = f(t) + W(t)
$$

Una observación de $Y(t)$ de largo $n$ es dado por:

$$
\mathbf{Y} = ( Y(t_1), \ldots, Y(t_n) )
$$

$$
\widehat{f}(t)= \sum_{k} c_{kj_0} \phi_{j_0}(t) + \sum_{j} \sum_{k} d_{jk}\psi_{jk}(t).
$$

## Implementación

Primero, comenzamos escribiendo las funciones elementales que componen la idea base con la cual construiremos la la función mejorada.

Funciones bases
```r

# Empirical wavelet transformations
# Niveles para datos diarios: 
# j1 <- 7 ; j0 <- 6

vecd <- function(signal=NULL, 
                 filtro= NULL, 
                 waveletn= NULL, 
                 j0= NULL){
  
  signal <- as.numeric( unlist(signal) ) # the signal enter as a list
  decom <- wd(signal, filter.number= filtro, family= waveletn )
  dj0 <- vector("list", j0 ) 
  for ( j in  0:(j0-1) ) { dj0[[j+1]] <-  accessD(decom, level=j) }
  vec <- unlist(dj0)
  return(vec)
  
}

### Wavelet based curve estimation with thresholding 
mweth <- function( signal = NULL, 
                   filtro = NULL, 
                   waveletn = NULL,
                   j0=NULL){
  
  signal <- as.numeric( unlist(signal) ) # the signal enter as a list
  decom <- wd(signal, filter.number= filtro, family= waveletn )
  # j=1, ..., J
  j1 <- 7  ; j0 <- 1  
  decom <- nullevels(decom, (j1-1):(nlevelsWT(decom)-1) ) # Shrinkage
  decom.th <- threshold(decom, levels = (j0-1):(j1-2), 
                        type = "soft", 
                        policy = "sure", 
                        by.level= TRUE) # thresholding
  hatf <- wr(decom.th, 
             start.level = j1-1 ) # start.level com ruido
  
  return(hatf)
}

```


## Función objetivo mejorada

```r

### Wavelet based curve estimation with thresholding 

hat_f <- function(signal = NULL, # es la curva observada con distorción 
                  filtro = NULL, #
                  waveletn = NULL, #
                  j0 = NULL, # 
                  j1= NULL){

## Nota: paquete necesario "wavethresh":ver https://cran.r-project.org/web/packages/wavethresh/index.html
# Parte 0:  Verificar si la longitud de la señal/curva es una potencia de 2
  n <- length(signal)
  if (!log2(n) %% 1 == 0) {
    warning("La longitud de la señal no es una potencia de 2,
             la función puede no funcionar correctamente.")
  }
  
  # Descompongo los datos mediante la funcion "wd"
  # Parte I: descomposicicón de la curva con ruido
  signal <- as.numeric( unlist(signal) ) # the signal enter as a list
  decom <- wd(signal, filter.number= filtro, family= waveletn )
  
  # Parte 2: "denoising" o "alisamiento de la curva" 
  decom <- nullevels(decom, (j1-1):(nlevelsWT(decom)-1) ) # Shrinkage
  
  decom.th <- threshold(decom, levels = (j0-1):(j1-2), 
                        type = "soft", 
                        policy = "sure", 
                        by.level= TRUE) # thresholding
  # Parte 3: reconstrucción de la curva \hat{f}(t)
  hatf <- wr(decom.th, start.level = j0-1 ) # j0 start.level com ruido
  
  # Parte 4: obtengo la lista de coeficientes.
  coef <- list()
  for (j in (j0-1):(j1-2)) {
    coef[[paste0("level", j+1)]] <- accessD(decom.th, level=j)
  }
  
   # Parte 5: guardo los outpouts como una lista
  result <- list(hatf = hatf, coef = coef, decom.th = decom.th)
  class(result) <- "hat_f"
  
  return(result)
}

### Método de Impresión para la Clase S3
print.hat_f <- function(x, ...) {
  cat("Wavelet Based Curve Estimation\n")
  cat("================================\n")
  cat("Hatf (Funcion alisada):\n")
  print(summary(x$hatf))
  cat("\nDecomposed Threshold (decom.th):\n")
  print(summary(x$decom.th))
  cat("\nWavelet Coefficients:\n")
  print(lapply(x$coef, summary))
}


### Función en R vectorizada para estimar funciones via alisamiento

v_hat_f <- function(signal = NULL, # es la curva observada con distorcion 
                  filtro = NULL, #
                  waveletn = NULL, #
                  j0 = NULL, # 
                  j1= NULL){
  
  # Parte 0:  Verificar si la longitud de la señal/curva es una potencia de 2
  n <- length(signal[,1])
  if (!log2(n) %% 1 == 0) {
    warning("La longitud de la señal no es una potencia de 2, 
             la función puede no funcionar correctamente.") 
                          }
  hats_v <- apply( signals0, 2, function(x) hat_f(signal = x, 
                                                  filtro = filtro, 
                                                  waveletn = waveletn, 
                                                  j0 = j0, 
                                                  j1= j1)$hatf)
  
  result <- list(hats_v = hats_v)
  class(result) <- "hats_v"
  
  return(result)
}


### Método de Impresión para la Clase S3
print.v_hat_f <- function(x, ...) {
  cat("Wavelet Based Curve Estimation\n")
  cat("================================\n")
  cat("Hatf (Funcion alisada):\n")
  print(summary(x$hatf))
  }


```

## Aplicaciones de las funciones `hat_f` y `v_hat_f`

En la aplicación utilizaremos datos generados por la función *DJ.EX* de la libreria *wavethresh* para generar
observaciones de la función *doppler* con y sin ruido presentadas en las siguientes figuras:

<img src="https://raw.githubusercontent.com/jelincovil/A_course_R_programming_2024/main/Proyecto%20profe/doopler.png" width="800"> 

<img src="https://raw.githubusercontent.com/jelincovil/A_course_R_programming_2024/main/Proyecto%20profe/doppler0.png" width="800"> 

Las funciónes *hat_f* y *v_hat_f* tiene por objetivo tomar la función doopler **con ruido** como input y entregarme la estimación de esta
**sin ruido**.

```r

# Aplicación con datos somilados
library(wavethresh)

J <- 12
M <- 2^{J} 
tm <- (c(1:M))/ M
epsilon <- 1
replicas <- 1000
s <- 1
r <- 6
ni <- 1 # 50, 100, 150, 200, 250, 
n <- ni*r
group.label0 <- unlist( lapply(1:r ,  function(x) rep(x, times = ni) ) )



  
doppl <-  DJ.EX(n=M, signal=1, rsnr=6, 
                  noisy = FALSE,
                  plotfn= FALSE)$doppler
  
signals0 <- sapply(1:n, function(x)  10 + epsilon*rnorm(M, 0, tm) )
S <- as.data.frame(signals0) 
# Grafico de los datos completos
plot.ts(S, main = "Doopler function con ruido")  

# Funcion a ser estimada
plot.ts(doppl)
# Funciones con ruido
plot.ts(S[,1], main = "Doopler function") 


# Aplicacion a una observacion (No vectorial)
f <- hat_f(signal = S[,1], 
                  filtro = 6, 
                  waveletn = "DaubLeAsymm",
                  j0 = 6,
                  j1=10)

print(f); 

f$hatf

plot.ts(S[,1], main = "Doopler function")  
lines(f$hatf, 
     type = "l", 
     col="red",
     lwd= 2)

# Aplicación a los datos completos

g <- v_hat_f(signal = S, 
           filtro = 6, 
           waveletn = "DaubLeAsymm",
           j0 = 6,
           j1=10)


# Resumen
summary(g$hats_v)

# Grafico
plot.ts(g$hats_v)


```

## Test de las funciones

### 1. Prueba para Verificar la Longitud Correcta de la Salida

Es importante asegurar que la longitud de la curva estimada `hatf` es igual a la longitud de la señal de entrada, ya que esto garantiza que la transformación y reconstrucción se realizan correctamente.

```r
library(testthat)
library(assertthat)

# Test para verificar la longitud de la salida
test_that("Longitud de hatf coincide con la señal de entrada", {
  # Genera una señal de prueba
  signal_test <- rnorm(128)  # longitud es una potencia de 2
  # Llama a la función hat_f
  result <- hat_f(signal = signal_test, 
                  filtro = 5, 
                  waveletn = "DaubLeAsymm", 
                  j0 = 2, j1 = 5)
  # Verifica que la longitud de hatf es igual a la longitud de signal_test
  expect_equal(length(result$hatf), length(signal_test))
})

```

### 2. Prueba para Verificar el Tipo de Salida y Componentes

Es esencial que la salida de la función sea de la clase correcta (`hat_f`) y que contenga todos los componentes necesarios (`hatf`, `coef`, `decom.th`).

```r

# Test para verificar la estructura de la salida
test_that("La salida es de clase hat_f y contiene los componentes adecuados", {
  signal_test <- rnorm(128)  # longitud es una potencia de 2
  result <- hat_f(signal = signal_test, 
                  filtro = 5, 
                  waveletn = "DaubLeAsymm", 
                  j0 = 2, j1 = 5)
  # Verifica la clase de la salida
  expect_s3_class(result, "hat_f")
  # Verifica que los componentes necesarios están presentes
  expect_true("hatf" %in% names(result))
  expect_true("coef" %in% names(result))
  expect_true("decom.th" %in% names(result))
})


 3 NAs

```

### Uso de `assertthat` para Verificaciones Adicionales

Puedes usar `assertthat` para hacer afirmaciones que verifiquen el comportamiento esperado dentro de la función, por ejemplo, verificar que las señales no sean nulas o que los parámetros estén dentro de un rango esperado:

```r
# Asegurarse que la señal de entrada no es nula
assert_that(!is.null(signal), msg = "La señal de entrada no debe ser nula.")

# Asegurarse que los parámetros j0 y j1 son enteros y que j0 < j1
assert_that(is.numeric(j0), j0 == as.integer(j0), j0 > 0,
            is.numeric(j1), j1 == as.integer(j1), j1 > j0,
            msg = "Los niveles j0 y j1 deben ser enteros positivos con j0 < j1.")
```

### Cómo Implementar las Pruebas

Asegúrate de colocar tus pruebas en un archivo de pruebas dentro de tu proyecto R, típicamente en una carpeta `tests/` si estás creando un paquete, o simplemente ejecutarlas directamente en tu ambiente de desarrollo para comprobar la integridad de tu función `hat_f`.

Estas pruebas ayudarán a garantizar que tu función se comporta como se espera en condiciones normales y alertará si se introducen cambios que alteren su comportamiento esperado.

### Test de Calidad del Código para la Función `hat_f`

#### 1. **Verificación de Integridad y Exactitud de la Transformación**

**Objetivo**: Confirmar que `hat_f` puede procesar una señal con ruido y recuperar una señal más suave manteniendo la integridad de la información original.

```r
library(testthat)
library(wavethresh)  # Asegúrate de que wavethresh está instalado y cargado

# Generar datos funcionales simulados con ruido
set.seed(123)
t <- seq(0, 1, length.out = 2^8)
y_true <- sin(2 * pi * t)  # señal sin ruido
y_noisy <- y_true + rnorm(length(t), mean = 0, sd = 0.2)  # señal con ruido

plot.ts(y_noisy)

# Test para verificar la integridad de la transformación aplicada por hat_f
test_that("hat_f mantiene la integridad y reduce el ruido", {
  # Aplicar hat_f a la señal ruidosa
  result <- hat_f(signal = y_noisy, 
                  filtro = 5, 
                  waveletn = "DaubLeAsymm", 
                  j0 = 2, j1 = 6)
  
  # Calcular el error cuadrático medio entre la señal suavizada y la original
  mse <- mean((result$hatf - y_true)^2)
  
  expect_true(mse < 0.05, info = "El MSE debería ser bajo, 
              indicando efectividad en la reducción de ruido")
})

```

#### 2. **Test de Sensibilidad a Parámetros**

**Objetivo**: Evaluar cómo los cambios en los parámetros `filtro`, `waveletn`, `j0` y `j1` afectan los resultados de `hat_f`.

```r
library(testthat)

# Parámetros para probar
filters <- c(4, 5, 6)  # Ejemplo de diferentes números de filtro
wavelet_types <- c("DaubExPhase", "DaubLeAsymm", "Coiflets ")  # Tipos de wavelets
levels_j0 <- c(2, 4)  # Diferentes niveles de j0
levels_j1 <- c(6, 7)  # Diferentes niveles de j1

# Test para verificar la sensibilidad de parámetros en hat_f
test_that("hat_f es robusta a variaciones de parámetros", {
  results <- list()
  
  for (filtro in filters) {
    for (wavelet in wavelet_types) {
      for (j0 in levels_j0) {
        for (j1 in levels_j1) {
          result <- hat_f(signal = y_noisy, 
                          filtro = filtro, 
                          waveletn = wavelet, 
                          j0 = j0, j1 = j1)
          results[[paste(filtro, wavelet, j0, j1, sep = "-")]] <- result$hatf
        }
      }
    }
  }
  
  # Suponemos que evaluamos la variabilidad en los resultados
  # Aquí podrías incluir una comparación más específica dependiendo de lo esperado
  unique_results <- length(unique(unlist(results)))
  expect_true(unique_results > 1, info = "Los resultados deben variar con diferentes parámetros")
})
```





