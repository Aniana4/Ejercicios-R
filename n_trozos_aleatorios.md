Objetivo
--------

El ejercicio pretende trocear un data frame en n trozos iguales muestreados aleatoriamente. Primero solucionamos de forma particular y luego generalizamos. Se ha utilizado iris como ejemplo. Requiere el paquete data.table instalado

``` r
mi.iris <- data.table(iris)

#Agregamos una columna con un valor aleatorio de 1 al número de muestras de cada especie

mi.iris[, selector:= sample(1:length(Sepal.Width), length(Sepal.Width)), by = c("Species")]

# marcamos el grupo al que pertenece dentro de cada especie

mi.iris[, id.grupo:= ceiling(selector/(length(Sepal.Length)/2)), by = c("Species")]

# Clasificamos por especie y grupo

mi.iris$conjunto <- paste(as.character(mi.iris$Species), ".", as.character(mi.iris$id.grupo), sep = "")

# Comprobamos que se ha agrupado correctamente

dcast.data.table(mi.iris, Species ~ id.grupo,fun=length, value.var = "Species")
```

    ##       Species  1  2
    ## 1:     setosa 25 25
    ## 2: versicolor 25 25
    ## 3:  virginica 25 25

Generalizamos la solución
-------------------------

------------------------------------------------------------------------

``` r
mi.iris <- data.table(iris)
trozos <- 5 

#Agregamos una columna con un valor aleatorio de 1 al número de muestras de cada especie

mi.iris[, selector:= sample(1:length(Sepal.Width), length(Sepal.Width)), by = c("Species")]

# marcamos el grupo al que pertenece dentro de cada especie

mi.iris[, id.grupo:= ceiling(selector/(length(Sepal.Length)/trozos)), by = c("Species")]

# Clasificamos por especie y grupo

mi.iris$conjunto <- paste(as.character(mi.iris$Species), ".", as.character(mi.iris$id.grupo), sep = "")

# Comprobamos que se ha troceado correctamente

dcast.data.table(mi.iris, Species ~ id.grupo,fun=length, value.var = "Species")
```

    ##       Species  1  2  3  4  5
    ## 1:     setosa 10 10 10 10 10
    ## 2: versicolor 10 10 10 10 10
    ## 3:  virginica 10 10 10 10 10
