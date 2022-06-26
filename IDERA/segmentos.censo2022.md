---
title: Segmentación Censo 2022
author: 
  - "Hernán Alperin"
  - "Verónica Heredia"
  - "Manuel Retamozo"
date: "June, 2022"
output:
  powerpoint_presentation: 
    reference_doc: cac-custom-reference.pptx
    keep_md: true
    fig_width: 7
    fig_height: 4
    fig_caption: yes
  html_document:
    toc: yes
    toc_depth: 2
    dev: svg
    includes:
      in_header: header.html
      before_body: doc_prefix.html
      after_body: doc_suffix.html
  slidy_presentation: default
  pdf_document: default
  ioslides_presentation: default
  beamer_presentation: 
    toc: yes
    incremental: yes
subtitle: "Generación de Segmentos Geográficos para el Censo 2022 en áreas urbanas
  y suburbanas"
encoding: utf8
---

<!-- por acá empieza Vero la resentación -->
# Introducción al problema. {style="class: center, inverse; background-image: url('plantilla_idera_html/img0.png');"}

## ¿Que es la segmentación?

Son las tareas que permiten subdividir el radio censal en segmentos.

Para asegurar que todas las viviendas sean censadas, hay que determinar qué área y qué viviendas le corresponden a cada censista.

------------------------------------------------------------------------

# Tareas: Generar mapas, recorridos, planillas. {.unlisted .unnumbered}

-   16 millones de domicilios.
-   De 8 a 12 hs. de trabajo.
-   Carga promedio en zona urbana: entre 30 y 36 viviendas (según la provincia)
-   Carga promedio en asentamientos: 18 viviendas.
-   Carga promedio en zona suburbana: 12 a 15 viviendas.
-   Aprox. 650 mil censistas

------------------------------------------------------------------------

# Descripción del problema.

-   Armar el recorrido de 650.000 censistas para que visiten todas las viviendas del territorio siguiendo las reglas definidas en el MANUAL del SEGMENTADOR.

------------------------------------------------------------------------

# El problema

Un problema de optimización, matemáticamente se formula como: Minimizar $f(x)$ sujeto a: $x \in S$, donde $S$ es el conjunto de los valores entre los que podemos buscar la solución, lo que se llama conjunto de soluciones factibles.

En este caso, $S$ es el conjunto de todas las segmentaciones posibles, $x$ es una segmentación y $f(x)$ es el costo de dicha segmentación.

------------------------------------------------------------------------

# Función objetivo con costo que incluye penalidad

-   Cantidad de viviendas por segmento
-   Cantidad de manzanas por segmento
-   Tipos de ejes que se cruzan

------------------------------------------------------------------------

# Delimitación del segmento

-   Todo espacio (con o sin viviendas) debe estar asignado a un segmento.
-   Identificar claramente Inicio y Fin de cada segmento.

------------------------------------------------------------------------

# Facilitar el recorrido del censista

## Evitar

-   Recorrido discontinuo
-   Cruce en diagonal
-   "En lo posible" cruce de avenidas, rutas, vías de ferrocarril o cursos de agua.

------------------------------------------------------------------------

# Segmentos

![Mapas de análisis](imagenes/mapa.png){width=90%}

------------------------------------------------------------------------

# Tipos de Radios

-   Urbano:

    población agrupada únicamente

-   Suburbano:

    población agrupada en pequeños poblados o en bordes amanzanados

::: notes
**Urbano:** población agrupada únicamente, y conformado por manzanas y/o sectores pertenecientes a una localidad.

**Suburbano:** agrupada en pequeños poblados o en bordes amanzanados de localidades.
:::

------------------------------------------------------------------------

# Radios Urbanos o parte Urbana de Radios Mixtos

## Según distribución de viviendas dada una carga deseada.

-   Esparcidos: todos los lados con pocas viviendas.
-   Densos: todas las manzanas con muchas viviendas.
-   Combinados: situaciones intermedias, algunas manzanas con pocas viviendas o algunos lados con muchas viviendas.

------------------------------------------------------------------------

# Elementos disponibles, o agrupación elemental.

-   Conteos: lados o manzanas completas
-   Listados: direcciones, pisos (no puede haber más de 1 segmento por piso) recorridos o manzanas independientes
-   Varias combinaciones de ambos.

------------------------------------------------------------------------

# Generación de segmentaciones vecinas

![Extraer componente](imagenes/extraer.png){width=90%}

------------------------------------------------------------------------

![Transferir componente](imagenes/transferir.png){width=90%}

------------------------------------------------------------------------

![Fusionar componente](imagenes/fusionar.png){width=90%}

------------------------------------------------------------------------

# Optimización global

![Espacio de soluciones](imagenes/optimizacion_global.png){width=90%}

------------------------------------------------------------------------

# Espacio factible definido por vecindario de segmentación

::: columns
::: {.column width="50%"}
## Optimización Discreta

![](imagenes/optimizacion_discreta.png){width=45%}
:::

::: {.column width="50%"}
##   Vecinos se calculan con operaciones elementales

1.  Extraer componente (manzana o lado) de segmento
2.  Transferir componente de un segmento a otro
3.  Fusionar 2 segmentos
:::
:::

------------------------------------------------------------------------

# Algoritmo para radios esparcidos (lados completos)

1.  Tomar una segmentación inicial y hacer que sea la actual
2.  Calcular todas las segmentaciones vecinas a la actual y elejir una de costo mínimo
3.  Si su costo es menor que el costo de la segmentación actual, hacer ésta la nueva segmentación actual e iterar
4.  Si no, la segmentación actual es un mínimo local, terminar.

------------------------------------------------------------------------

# Algoritmo para radios densos (manzanas independientes)

1.  En cada manzana encontrar el número entero $d_m$ más cercano a cantidad de viviendas de la manzana dividido por la cantidad de viviendas deseada
2.  Hacer ese, el número de segmentos en la manzana $m$
3.  Calcular la cantidad de viviendas por segmento en la manzana $m$, $s_m = v_m / d_m$
4.  Segmentar el listado de la manzana $m$ cortando a pisos enteros, avanzando con segmentos de cantidad de viviendas lo más cercano a $s_m$ posible

------------------------------------------------------------------------

# Algoritmo para radios combinados

1.  Aplicar el algoritmo para radios esparcidos (lados completos)
2.  Para todos los segmentos que superen un valor umbral de viviendas aplicar algoritmo de (listado) manzana independiente
3.  Juntar segmentos con cero o pocas viviendas

------------------------------------------------------------------------

# Numeración de los segmentos

-   Cada segmento del radio urbano se identifica con un número de dos dígitos, de 01 en más, dentro del radio.
-   En el área urbana del radio mixto los segmentos se numeran del 60 en más excluyendo al 90.
-   El segmento 90 contiene las viviendas colectivas del radio.

------------------------------------------------------------------------

\newpage
# Carga de datos y procesamiento de datos.

![DTL](imagenes/DTL.png){width=90%}

|                           |
|:--------------------------|
| \# Software utilizado     |
| ![L](imagenes/amigos.png){width=90%} |

# Segmentación.

![Parámetros](imagenes/SP.png){width=90%}

------------------------------------------------------------------------

# Generación de resumenes.

![Resumen R3](imagenes/R3.png){width=90%}

| \# Radios Urbanos     |
|:----------------------|
| \# Radios Suburbanos  |
| ![](imagenes/RSU.png){width=90%} |

# Estructura de Base de Datos

![](imagenes/BD.png){width=90%}

------------------------------------------------------------------------

# Salidas Gráficas.

![](imagenes/plugin.png){width=90%}

------------------------------------------------------------------------

| \# Mapas de Fracción |
|:---------------------|
| \# Mapas de Radio    |
| ![](imagenes/MR.png){width=90%} |

# Mapas de Segmento

![](imagenes/MS.png){width=90%}

------------------------------------------------------------------------

# Optmización de recursos

-   Se redujo el tiempo de armado e impresión de mapas
-   Se utilizó el complemento atlas para automatizar el armado de los mapas para censistas, jefes de radio, fracción, departamento y la planilla R3 de recorrido del censista
-   Se informatizó y masificó la distribución de planos utilizando formato digital (.pdf .png)
-   Se fomentó el uso de herramientas de software libre, QGIS

------------------------------------------------------------------------

# Espacio de trabajo QGIS

![Mapa Fracción](imagenes/SG_fraccion.png){width=90%}

------------------------------------------------------------------------

# Conclusiones.

-   Pudimos resolver un problema complicado con algoritmos sencillos
-   Creamos una interface para la carga de datos y procesamiento de la segmentación
-   Creamos un plugin para la automatización de salidas gráficas y recorridos
-   Optimizamos tiempo y recursos para el armado de material de campo