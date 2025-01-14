---
title: "Segmentación Censo 2022"
abstract: "Generación de Segmentos Geográficos en áreas urbanas y suburbanas"
author: 
  - "Hernán Alperin"
  - "Verónica Heredia"
  - "Manuel Retamozo"
date: "Junio, 2022"
fontsize: 12pt
geometry: margin=1in
output:
  powerpoint_presentation: 
    reference_doc: cac-custom-reference.pptx
    keep_md: true
    fig_width: 7
    fig_height: 4
    fig_caption: yes
  beamer_presentation: 
    toc: yes
    incremental: yes
  html_document:
    toc: yes
    toc_depth: 3
    dev: svg
    includes:
      in_header: header.html
      before_body: doc_prefix.html
      after_body: doc_suffix.html
  slidy_presentation: default
  revealjs::revealjs_presentation:
    theme: sky
    incremental: true
    fig_caption: true
    center: true
    transition: convex 
    background_transition: concave
    toc: yes
    toc_depth: 3
    dev: svg
    reveal_options:
      hash: true
      progress: true
    includes:
      in_header: header.html
      before_body: doc_prefix.html
      after_body: doc_suffix.html      
  ioslides_presentation: default
  pdf_document: 
    toc: yes
    fig_caption: yes
    number_sections: yes
    keep_tex: yes
encoding: unicode
---
\usepackage{fontspec}
\setmainfont{DejaVu Sans}

![Segmentación Censo 2022](imagenes/logocensox100.png)

# Introducción 😁 {data-background-image="imagenes/fondo.idera.png"}

<!-- por acá empieza Vero la resentación -->

"Generación de Segmentos Geográficos en áreas urbanas y suburbanas"

## ¿Qué es la segmentación? 🤔 {data-background-image="imagenes/fondo.idera.png"}

Son las tareas que permiten subdividir el radio censal en segmentos.

Para asegurar que todas las viviendas sean censadas, hay que determinar qué área y qué viviendas le corresponden a cada censista.

## Tareas: Generar mapas, recorridos, planillas. {data-background-image="imagenes/fondo.idera.png"}

-   16 millones de domicilios.
-   De 8 a 12 hs. de trabajo.
-   Carga promedio en zona urbana: entre 30 y 36 viviendas (según la provincia)
-   Carga promedio en asentamientos: 18 viviendas.
-   Carga promedio en zona suburbana: 12 a 15 viviendas.
-   Aprox. 650 mil censistas

## Descripción del problema. {data-background-image="imagenes/fondo.idera.png"}

-   Armar el recorrido de 650.000 censistas para que visiten todas las viviendas del territorio siguiendo las reglas definidas en el MANUAL del SEGMENTADOR.

## El problema {data-background-image="imagenes/fondo.idera.png"}

Un problema de optimización, matemáticamente se formula como: Minimizar $f(x)$ sujeto a: $x \in S$, donde $S$ es el conjunto de los valores entre los que podemos buscar la solución, lo que se llama conjunto de soluciones factibles.

En este caso, $S$ es el conjunto de todas las segmentaciones posibles, $x$ es una segmentación y $f(x)$ es el costo de dicha segmentación.

## Función objetivo con costo que incluye penalidad {data-background-image="imagenes/fondo.idera.png"}

-   Cantidad de viviendas por segmento
-   Cantidad de manzanas por segmento
-   Tipos de ejes que se cruzan

## Delimitación del segmento {data-background-image="imagenes/fondo.idera.png"}

-   Todo espacio (con o sin viviendas) debe estar asignado a un segmento.
-   Identificar claramente Inicio y Fin de cada segmento.

## Facilitar el recorrido del censista {data-background-image="imagenes/fondo.idera.png"}

Evitar

-   Recorrido discontinuo
-   Cruce en diagonal
-   "En lo posible" cruce de avenidas, rutas, vías de ferrocarril o cursos de agua.

## Segmentos {data-background-image="imagenes/fondo.idera.png"}

![Mapas de análisis](imagenes/mapa.png){width="90%"}

# Tipos de Radios {data-background-image="imagenes/fondo.idera.png"}

-   Urbano:

    población agrupada únicamente

-   Suburbano:

    población agrupada en pequeños poblados o en bordes amanzanados

::: notes
**Urbano:** población agrupada únicamente, y conformado por manzanas y/o sectores pertenecientes a una localidad.

**Suburbano:** agrupada en pequeños poblados, barrios residenciales grandes o bordes amanzanados del area periurbana de algúnas localidades.
:::

## Radios Urbanos o parte Urbana de Radios Mixtos {data-background-image="imagenes/fondo.idera.png"}

### Según distribución de viviendas dada una carga deseada. {data-background-image="imagenes/fondo.idera.png"}

-   Esparcidos: todos los lados con pocas viviendas.
-   Densos: todas las manzanas con muchas viviendas.
-   Combinados: situaciones intermedias, algunas manzanas con pocas viviendas o algunos lados con muchas viviendas.

# Optimización global {data-background-image="imagenes/fondo.idera.png"}

![Espacio de soluciones](imagenes/optimizacion_global.png){width="90%"}

## Espacio factible definido por vecindario de segmentación {data-background-image="imagenes/fondo.idera.png"}

![Optimización Discreta](imagenes/optimizacion_discreta.png){width="75%"}

## Vecinos se calculan con operaciones elementales {data-background-image="imagenes/fondo.idera.png"}

1.  Extraer componente (manzana o lado) de segmento

2.  Transferir componente de un segmento a otro

3.  Fusionar 2 segmentos

## Generación de segmentaciones vecinas {data-background-image="imagenes/fondo.idera.png"}

## ![Extraer componente](imagenes/extraer.png){width="100%"}

## ![Transferir componente](imagenes/transferir.png){width="100%"}

## ![Fusionar componente](imagenes/fusionar.png){width="100%"}

# Algoritmos {data-background-image="imagenes/fondo.idera.png"}

## Algoritmo para radios esparcidos (lados completos) {data-background-image="imagenes/fondo.idera.png"}

1.  Tomar una segmentación inicial y hacer que sea la actual
2.  Calcular todas las segmentaciones vecinas a la actual y elejir una de costo mínimo
3.  Si su costo es menor que el costo de la segmentación actual, hacer ésta la nueva segmentación actual e iterar
4.  Si no, la segmentación actual es un mínimo local, terminar.

## Algoritmo para radios densos (manzanas independientes)

1.  En cada manzana encontrar el número entero $d_m$ más cercano a cantidad de viviendas de la manzana dividido por la cantidad de viviendas deseada
2.  Hacer ese, el número de segmentos en la manzana $m$
3.  Calcular la cantidad de viviendas por segmento en la manzana $m$, $s_m = v_m / d_m$
4.  Segmentar el listado de la manzana $m$ cortando a pisos enteros, avanzando con segmentos de cantidad de viviendas lo más cercano a $s_m$ posible

## Algoritmo para radios combinados {data-background-image="imagenes/fondo.idera.png"}

1.  Aplicar el algoritmo para radios esparcidos (lados completos)
2.  Para todos los segmentos que superen un valor umbral de viviendas aplicar algoritmo de (listado) manzana independiente
3.  Juntar segmentos con cero o pocas viviendas

# Otras funciónes en el core

## Numeración de los segmentos {data-background-image="imagenes/fondo.idera.png"}

-   Cada segmento del radio urbano se identifica con un número de dos dígitos, de 01 en más, dentro del radio.
-   En el área urbana del radio mixto los segmentos se numeran del 60 en más excluyendo al 90.
-   El segmento 90 contiene las viviendas colectivas del radio.

## Descripción de domicilios {data-background-image="imagenes/fondo.idera.png"}

-   Calle Cod - Nombre
-   Nro Catastral
-   Piso
-   Sin Nombre
-   Sin Número

## Descripción de colectivas {data-background-image="imagenes/fondo.idera.png"}

-   Domicilio +
-   Tipo de Colectiva

## Descripción de segmentos {data-background-image="imagenes/fondo.idera.png"}

-   Manzna completa
-   Lado
-   Domicilio
-   Excluyendo

# Carga de datos {data-background-image="imagenes/fondo.idera.png"}

![Carga de Datos](imagenes/DTL.png){height="70%"}

## Procesamiento de Datos {data-background-image="imagenes/fondo.idera.png"}

![ETL](imagenes/ETL.png){height="90%"}

# Elementos disponibles, o agrupación elemental. {data-background-image="imagenes/fondo.idera.png"}

-   Conteos: lados o manzanas completas
-   Listados: direcciones, pisos (no puede haber más de 1 segmento por piso) recorridos o manzanas independientes
-   Varias combinaciones de ambos.

# Software utilizado {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/amigos.png){width="90%"}

## Segmentación. {data-background-image="imagenes/fondo.idera.png"}

![Parámetros](imagenes/SP.png){width="90%"}

# Generación de resumenes. {data-background-image="imagenes/fondo.idera.png"}

![Resumen R3](imagenes/R3.png){width="90%"}

## Estructura de Base de Datos {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/BD.png){width="90%"}

# Tipos de radios {data-background-image="imagenes/fondo.idera.png"}

## Radios Urbanos {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/RU.png){width="90%"}

## Radios SubUrbanos {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/RSU.png){width="90%"}

## Segmentación de 1 Radio que compartido con 5 localidades {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/R5L.png){width="90%"}

# Resumen de la segmentación {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/resumen.png){width="90%"}

## Previsualización de la segmentación y de grafo {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/SegG.png){width="90%"}

# Proceso Completo. {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/proceso_completo.png){width="100%"}

# Salidas Gráficas. {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/plugin.png){width="100%"}

## Espacio de trabajo QGIS {data-background-image="imagenes/fondo.idera.png"}

![Mapa Fracción](imagenes/SG_fraccion.png){width="100%"}

## Mapas de Fracción {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/MF.png){width="100%"}

## Mapas de Radio {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/MR.png){width="100%"}

## Mapas de Segmento {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/MS.png){width="100%"}

# Conclusiones. {data-background-image="imagenes/fondo.idera.png"}

-   Pudimos resolver un problema complicado con algoritmos sencillos
-   Creamos una interface para la carga de datos y procesamiento de la segmentación
-   Creamos un plugin para la automatización de salidas gráficas y recorridos
-   Optimizamos tiempo y recursos para el armado de material de campo

## Optmización de recursos {data-background-image="imagenes/fondo.idera.png"}

-   Se redujo el tiempo de armado e impresión de mapas
-   Se utilizó el complemento atlas para automatizar el armado de los mapas para censistas, jefes de radio, fracción, departamento y la planilla R3 de recorrido del censista
-   Se informatizó y masificó la distribución de planos utilizando formato digital (.pdf .png)
-   Se fomentó el uso de herramientas de software libre, QGIS

# Gracias. {data-background-image="imagenes/fondo.idera.png"}

![](imagenes/colaboradores.png){width="100%"}

## Preguntas {data-background-image="imagenes/fondo.idera.png"}

### ?

## Contacto {data-background-image="imagenes/fondo.idera.png"}

Hernán [halperin\@indec.gob.ar](mailto:halperin@indec.gob.ar){.email}

Verónica [vheredia\@indec.gob.ar](mailto:vheredia@indec.gob.ar){.email}

Manuel @cyberoliverio

Cuándo y donde:


```bash
date
pwd
```

```
## lun 04 jul 2022 16:46:40 -03
## /home/manuel/laburo/indec/Charlas/IDERA
```
