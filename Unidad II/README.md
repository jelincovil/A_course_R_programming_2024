## Personalizar el aspecto de un gráfico
- ggtitle
- xlab y ylab.
- theme()
- scale()
- annotate()
- windWin80: un conjunto de datos de *R*.

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
- Usar xlab() and ylab(). para dejar más información en los ejes.
- Agregar lineas de contorno en base a los datos y no ecuaciones. Para hacer esto, podríamos agregar una función PDF estimada a
el gráfico usando geom_density2d():
- La función o theme() podría ayudar a destacar la cuadricula.
- 

```r
ggplot(MPV::windWin80, aes(x = h0, y = h12)) +
  geom_density2d(col = "red") +
  geom_contour(data = grid, aes(x = x1, y = x2, z = z) ) +
  geom_jitter() +
  xlab("Wind speed at midnight (km/h)") +
  ylab("Wind speed at noon (km/h)") +
  theme(panel.grid = element_blank()) +
  annotate("text", x = 46, y = 20, hjust = 0, label = "Too many points") +
  annotate("segment", x = 46, y = 20, xend = 35, yend = 34) +
  annotate("text", x = 5, y = 50, vjust = -1, label = "Too few points") +
  annotate("segment", x = 5, y = 50, xend = 6, yend = 11)

```
## Subdividir el gráfico por grupo

Usaremos la función facet_grid()

```r

ggplot(phones, aes(x = Region, y = Telephones, fill = Region)) +
  geom_col() +
  facet_wrap(vars(Year)) +
  theme(axis.text.x = element_blank(), axis.ticks.x = element_blank()) +
  xlab(element_blank())

```

```r
data(mtcars)
ggplot(mpg, aes(hwy, cty)) +
  geom_point() +
  facet_grid(cut_number(displ, 3) ~ cyl)

```

## Crear grupos con *ggplot()*

```r
For example, with the mpg data set,
ggplot(mpg, aes(hwy, fill = factor(cyl))) +
geom_histogram(binwidth = 2)
```

## Otros sistemas gráficos
## El paquete lattice

```r
library(lattice)
xyplot(Telephones ˜ Year | Region, data = phones)
```

## El paquete grid

```r
library(grid)
grid.rect() # draw black rectangle containing original viewport
vp <- viewport(h = 0.4, w = 0.6, angle = 60)
pushViewport(vp) # create viewport rotated 60 degrees (counter clockwise)
# with height 40% and width 60% of original box
grid.rect(gp = gpar(col = "red")) # draw red rectangle around new viewport
pushViewport(vp) # create new viewport nested in previous viewport,
# rotated 60 degrees and with height 40%
# and width 60% of previous viewport.
grid.rect(gp = gpar(col = "blue"))
```

## Referencias
- Introduction to Scientific Programming and Simulation Using R: https://nyu-cdsc.github.io/learningr/assets/simulation.pdf

- ggplot2: Elegant Graphics for Data Analysis: ggplot2: Elegant Graphics for Data Analysis (3e) (ggplot2-book.org) https://ggplot2-book.org/

  
