---
layout: post
title:  "Frontera de un color RGB"
categories: [python, matplotlib, procesamiento de imagenes, matematicas]
tags: [color, colores, rgb, distancia, esfera]
---
Hace unos días, trabajando en un proyecto de **procesamiento de imágenes** en el que debía determinar el color de una fuente de luz tropecé con el siguiente problema: Dado un color **RGB**... *¿Cómo determinar si dicho color es una tonalidad de rojo? (o de cualquier otro color básico).*

### Una nota sobre el modelo de color RGB

RGB (Red, green and blue) es un modo de codificar un color a través de la cantidad o intensidad de los colores primarios de la luz, estos son, rojo, verde y azul.

La cantidad de cada color se puede codificar de diferentes maneras, por ejemplo, la intensidad de cada componente puede medirse entre 0 y 255. Es usual
representar estas cantidades en base hexadecimal (0x00 y 0xFF respectivamente).

Ejemplos:

## Una primera aproximación al problema

Es razonable afirmar que los colores que tengan una cantidad elevada de rojo pueden considerarse una tonalidad de rojo. En el siguiente ejemplo podemos ver todos los colores que tienen la componente *R = 255*, es decir, en su valor máximo.

![RGB Colors, R = 255](/assets/r255.png)

Podemos observar que, a medida que añadimos más cantidad de azul (eje vertical) y de verde (eje horizontal) obtenemos colores muy extremos, como el amarillo o incluso el blanco, los cuales es posible que no nos interese considerar como rojos. 

Por esta razón es necesario explorar otras soluciones.

## La "frontera de colores"

Como hemos explicado más arriba cada color RGB puede representarse a través de tres componentes básicas. Por esta razón el espacio de colores RGB es un espacio vectorial tridimensional, con base cada una de sus compoentes rojo, verde y azul. 

### Distancia entre dos colores

Podemos definir la distancia euclídea entre dos colores de la siguiente manera:

Sean *C_1 = (R_1, G_1, B_1)$ y $C_2 = (R_2, G_2, B_2)* dos colores. Se define la distancia euclídea entre $C_1$ y $C_2$ como:

$$
d(C_1, C_2) = \sqrt{(R_1 - R_2)^2 + (R_1 - R_2)^2 + (R_1 - R_2)^2}
$$

Es decir, la longitud del segmento rectilíneo que los une.