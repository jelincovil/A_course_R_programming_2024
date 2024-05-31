
# Programación compleja en la libraría *tidyverse*


Webite del Tidy verse  (Tidyverse)[https://www.tidyverse.org/] .

```r
library(tidyverse)
```


1. Gran cantidad de funcionalidad: Existen más de 3000 objetos en los paquetes base de R y más de 15,000 paquetes disponibles en CRAN, la mayoría conteniendo decenas de objetos propios. Esto representa una enorme cantidad de funcionalidad.

2. Descubrimiento de funcionalidades: Se espera que las personas puedan descubrir estas funcionalidades utilizando el sistema de ayuda y otros recursos, así como haciendo suposiciones juiciosas sobre dónde buscar.

3. Importancia de la consistencia: La adivinanza se facilita cuando se siguen principios consistentes al escribir el código. Por ejemplo, los nombres deben reflejar el propósito de los paquetes y funciones.

4. Inconsistencias en R: Debido a la forma en que se escribió, R base es bastante inconsistente en sus convenciones de nomenclatura. Ha tenido docenas de contribuyentes desde entonces, y las modas en la informática han cambiado, por lo que incluye muchos estilos diferentes de nomenclatura.

5. El tidyverse: En este capítulo, se examinará el tidyverse, una colección de paquetes de R diseñados para seguir una filosofía de diseño consistente. Se comenzará con una presentación de la filosofía, luego se estudiarán varios paquetes de tidyverse en detalle para ver cómo funciona en la práctica.


## Los principios de *tidyverse*

1. **Reutilizar estructuras de datos existentes**: R admite muchas formas diferentes de almacenar datos. Se debe dar un alto valor a la consistencia: enfocarse en un pequeño número de estructuras que ya son familiares para los usuarios, en lugar de inventar nuevas. Se recomienda usar marcos de datos para conjuntos de datos rectangulares y vectores simples para los más pequeños.

2. **Componer funciones simples con el pipe**: El "pipe" es el operador de pipe %>% que se discutió en la Sección 4.2.5. En lugar de escribir funciones complejas que hacen mucho, Wickham recomienda descomponer las operaciones complejas en operaciones secuenciales simples. Escribe funciones para esas operaciones simples que siguen las convenciones que funcionan con el operador %>% y deja que el usuario junte esos pasos en cadenas de transformaciones fáciles de entender.

3. **Abrazar la programación funcional**: Se mencionó la programación funcional en la introducción al Capítulo 4.

4. **Diseñar para humanos**: La eficiencia computacional debería ser una preocupación secundaria para la usabilidad. La eficiencia computacional debería ser una preocupación secundaria para la usabilidad.
   
##  La librería *tibble*: la mejora de la función *data.frame()*

El primer principio de tidyverse recomienda utilizar marcos de datos para la mayoría de los conjuntos de datos rectangulares. Sin embargo, los marcos de datos en base R tienen algunos
valores predeterminados que pueden causar confusión:

```r

data.frame("Annual income" = 100000)

```

```r

df1 <- data.frame(x = 1:2, y = 2:1)
df1[, 1:2]

```


- En versiones de R anteriores a la 4.0.0, los vectores de caracteres se cambian automáticamente a factores.
- Los nombres se cambian a nombres legales de variables R, p. los espacios se reemplazan con puntos:
- La impresión de marcos de datos muy grandes ocupa mucho espacio en la consola.
- Tomar un subconjunto de un marco de datos a veces produce un marco de datos y otras veces produce un vector:

Además de esos cambios, la función tibble() tiene otras diferencias con respecto a marco de *data.frame()*:
- Todos los argumentos deben tener la misma longitud, o longitud 1; Se supone que el reciclaje de otras longitudes es un error.
- Los argumentos se evalúan en orden y los argumentos anteriores se pueden utilizar en la definición de los posteriores. Por ejemplo,
 
## El paquete *readr*: leyendo los datos de *tidyverse*

## La libraría *stringr* para manipular cadenas


## La libraria *dplyr* para manipular data-frames 

## Otros librarias/paquetes de *tidyverse* 

### Libraría *tidyr*: 
### Libraría *forcats:*
### Libraría *purrr*: 

## Referencias
