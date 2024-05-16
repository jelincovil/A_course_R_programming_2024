## Personalizar el aspecto de un gráfico
-- ggtitle
-- xlab y ylab.
-- theme()
-- scale()
-- annotate()
-- windWin80: un conjunto de datos de *R*.

Data from the MPV package contains pairs of observations of wind speed at the Winnipeg International Airport. The columns
give the wind speed in km per hour at midnight (h0) and noon (h12).

### Mejorando el gráfico
```r
library(MPV)
library(ggplot2)

a1 <- 3/2; a2 <- 5/3; b0 <- 108; b1 <- 5.3; b2 <- 65

grid <- with(MPV::windWin80,
             expand.grid(x1 = seq(0, max(h0), len=101),
                         x2 = seq(0, max(h12), len=101)))
grid$z <- with(grid, a1*a2*exp(-x1^a1/b2 - x2^a2/
                                 (b0+b1*x1))*x1^(a1-1)*x2^(a2-1)/(b2*(b0+b1*x1)) )
# Plot the data on top of the density function
ggplot(MPV::windWin80, aes(x = h0, y = h12)) +
  geom_contour(data = grid, aes(x = x1, y = x2, z = z) ) +
  geom_point()
```

- Sacar o destacar datos que se super ponen: geom_jitter()
- 

```r
ggplot(data=mtcars)
```
## Facetas de
## Otros sistemas gráficos
## El paquete lattice
## El paquete grid
## Gráficos interactivos
## El paquete matplot
## Referencias
- Introduction to Scientific Programming and Simulation Using R: https://nyu-cdsc.github.io/learningr/assets/simulation.pdf

- ggplot2: Elegant Graphics for Data Analysis: ggplot2: Elegant Graphics for Data Analysis (3e) (ggplot2-book.org)
