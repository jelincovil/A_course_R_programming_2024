## Ejemplo de Proyecto

# Titulo: función escalable para la estimación de curvas


## Objetivo: 
   -
   -
   -
## Motivación empirica y teorica

https://fr.mathworks.com/help/wavelet/ug/noisysig_ex.png

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



