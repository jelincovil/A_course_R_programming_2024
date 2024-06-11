# Álgebra Matricial
Podemos resumir en 5 puntos claves la aplicación del Algebra Matricial a Data Science:

- **Álgebra Matricial**: La página se centra en el álgebra matricial, una rama importante de las matemáticas.
- **Vectores y Matrices en R**: Se discuten los conceptos básicos de vectores y matrices dentro del lenguaje de programación R.
- **Operaciones Matriciales**: Se describen operaciones como la multiplicación de matrices, la inversión matricial y la resolución de sistemas lineales.
- **Descomposiciones Matriciales**: Se exploran descomposiciones matriciales específicas, como LU, Choleski y QR.
- **Funciones Avanzadas**: Se mencionan funciones avanzadas de R como *outer()* y *apply()* para operaciones matriciales.

<img src="https://raw.githubusercontent.com/jelincovil/A_course_R_programming_2024/main/images_for_r_programming/intro_matrices.png" width="800"> 
  
## Vectores y matrices en R


Matrices pueden ser construidas usando las funciones matrix(), cbind() or rbind().

*Syntax*
```r

matrix(data, nrow, ncol) # data is a vector of nrow*ncol values
cbind(d1, d2, ..., dm) # d1, ..., dm are vectors (columns)
rbind(d1, d2, ..., dn) # d1, ..., dn are vectors (rows)

```

Aquí construimos una matriz  H3  de Hilbert de 3 × 3, donde la entrada (i,j) es 1/(i + j − 1). 
Notemos que  ncol no es requerido en el comando que lo creó, ya que los datos al argumento se le ha asignado un vector que consta de nueve elementos; 
está claro que si hay tres filas también debe haber tres columnas.

```r
H3 <- matrix(c(1, 1/2, 1/3, 1/2, 1/3, 1/4, 1/3, 1/4, 1/5), nrow = 3)
H3
```


Nosotro podriamos construir la matriz uniendo columnas de esta manera.
```r
1/cbind(seq(1, 3), seq(2, 4), seq(3, 5))

```

En este ejemplo, rbind() podria dar el mismo resultado, debido a la simetria

Por otra parte, es claro que las matrices no necesaruiamente deben ser cuadradas.

```r
matrix(seq(1, 12), nrow = 3)
```

O alternativamente

```r
x <- seq(1, 3)
x2 <- x^2
X <- cbind(x, x2)
X
class(X)

```
Una matriz tambien puede ser construida por medio de un vector.


```r

X <- matrix(c(1, 2, 3, 1, 4, 9), ncol = 2)
X

```

```r

Y <- matrix(c(1, 2, 3, 1, 4, 9), nrow = 2)
Y

```

###  Acceder a elementos de la matriz;  nombres de filas y columnas

Construimos una matriz uniendo columnas:

```r

x <- seq(1, 3)
x2 <- x^2
X <- cbind(x, x2)

```

```r
X[3, 2]

```

```r
X[3,]

```


```r

X[, 2]

```

```r
X[3, , drop = FALSE]

```

```r

X[, 2, drop = FALSE]

```

Podemos acceder a los nombres de columnas y lineas de la matriz X
```r
colnames(X)

rownames(X)

```

Podemos inserir los nombres en cada linea de la matriz

```r

rownames(X) <- c("obs1", "obs2", "obs3")
X
rownames(X)
```

errores al llamar las columnas
```r
X$x

X[, "x"]

```


### Propiedades matriciales

Dimensión de la matriz

```r
dim(X)

```
Determinante de una matriz
```r
det(H3)

```
La diagonal de una matriz
```r
diag(X)

diag(H3)
```

El trazp de una matriz

```r
trace <- function(data) sum(diag(data))

trace(X)

trace(H3)

```

Diagonal y transpuesta de una matriz

```r
diag(diag(H3))

t(X)

```

### Matrices triangulares

```r

```

```r

```

```r

```

### Matriz aritmética

```r

```

```r

```

```r

```

## Multiplicación e inversión de matricial 

```r

```

```r

```

```r

```

### Inversión matricial
### La descomposición LU
### Inversión de una matriz en R
### Resolución de sistemas lineales

## Valores propios y vectores propios

## Otras descomposiciones matriciales

### La descomposición en valores singulares de una matriz
### La descomposición de Choleski de un positivo.  matriz definida

### La descomposición QR de una matriz.

## Otras operaciones matriciales

### Función *outer ( )*
### Función *apply ( )*





