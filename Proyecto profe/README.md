## Ejemplo de Proyecto

# Titulo: función escalable para la estimación de curvas


## Objetivo: 
   - Estimar la curva media sin ruido y la grafique.
   - Estimar las curvas medias sin ruido y las grafique.
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


## Funcion mejoradas

# Empirical wavelet transformations
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

```


Función objetivo mejorada
```r

### Wavelet based curve estimation with thresholding 

hat_f <- function(signal = NULL, # es la curva observada con distorcion 
                  filtro = NULL, #
                  waveletn = NULL, #
                  j0 = NULL, # 
                  j1= NULL){
  ## Nota: paquete necesario "wavethresh"
  
  # Parte 0:  Verificar si la longitud de la señal/curva es una potencia de 2
  n <- length(signal)
  if (!log2(n) %% 1 == 0) {
    warning("La longitud de la señal no es una potencia de 2, la función puede no funcionar correctamente.")
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


### Vectorized Wavelet based curve estimation with thresholding 

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

Aplicaciones de las funciones

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


## Test de las funciones para una y varias curvas

