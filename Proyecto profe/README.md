## Ejemplo de Proyecto

# Titulo: función escalable para la estimación de curvas


## Objetivo: 
   -
   -
   -
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
mweth <- function(signal = NULL, 
                  filtro = NULL, 
                  waveletn = NULL,
                  j0 = NULL){
  
  # Verificar si la longitud de la señal es una potencia de 2
  n <- length(signal)
  if (!log2(n) %% 1 == 0) {
    warning("La longitud de la señal no es una potencia de 2, la función puede no funcionar correctamente.")
  }
  
  signal <- as.numeric( unlist(signal) ) # the signal enter as a list
  decom <- wd(signal, filter.number= filtro, family= waveletn )
  
  # j=1, ..., J
  j1 <- 7  
  decom <- nullevels(decom, (j1-1):(nlevelsWT(decom)-1) ) # Shrinkage
  
  decom.th <- threshold(decom, levels = (j0-1):(j1-2), 
                        type = "soft", 
                        policy = "sure", 
                        by.level= TRUE) # thresholding
  
  hatf <- wr(decom.th, start.level = j1-1 ) # start.level com ruido
  
  coef <- list()
  for (j in (j0-1):(j1-2)) {
    coef[[paste0("level", j+1)]] <- accessD(decom.th, level=j)
  }
  
  result <- list(hatf = hatf, coef = coef, decom.th = decom.th)
  class(result) <- "mweth"
  
  return(result)
}

### Método de Impresión para la Clase S3
print.mweth <- function(x, ...) {
  cat("Wavelet Based Curve Estimation\n")
  cat("================================\n")
  cat("Hatf (Smoothed Signal):\n")
  print(summary(x$hatf))
  cat("\nDecomposed Threshold (decom.th):\n")
  print(summary(x$decom.th))
  cat("\nWavelet Coefficients:\n")
  print(lapply(x$coef, summary))
}

```
