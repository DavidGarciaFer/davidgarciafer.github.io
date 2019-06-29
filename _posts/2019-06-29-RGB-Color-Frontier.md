---
layout: post
title:  "Frontera de un color RGB"
categories: [python, matplotlib, procesamiento de imagenes, matematicas]
tags: [color, colores, rgb, distancia, esfera]
---
Hace unos días, trabajando en un proyecto de **procesamiento de imágenes** en el que debía determinar el color de una fuente de luz tropecé con el siguiente problema: Dado un color **RGB**... *¿Cómo determinar si dicho color es una tonalidad de rojo? (o de cualquier otro color básico).*

------
### Una nota sobre el modelo de color RGB

RGB *(Red, green and blue)* es un modo de codificar un color a través de la cantidad o intensidad de los colores primarios de la luz, estos son, rojo, verde y azul.

La intensidad de cada color se puede codificar de diferentes maneras, por ejemplo, la intensidad de cada una de las componentes puede medirse entre 0 y 255. Es usual
representar estas cantidades en base hexadecimal (0x00 y 0xFF respectivamente).

Ejemplos:
- <span style="color:#FF0000"> Rojo </span> = (255, 0, 0) = #FF0000
- <span style="color:#00FF00"> Verde </span> = (0, 255, 255) = #00FF00
- <span style="color:#0000FF"> Azul </span> = (0, 0, 255) = #0000FF
- <span style="color:#000000"> Negro </span> = (0, 0, 0) = #000000
------

## Una primera aproximación al problema

Es razonable afirmar que los colores que tengan una cantidad elevada de rojo pueden considerarse una tonalidad de rojo. En el siguiente ejemplo podemos ver todos los colores que tienen la componente *R = 255*, es decir, en su valor máximo.

![RGB Colors, R = 255](/assets/r255.png)

Como vemos, a medida que añadimos más cantidad de azul (eje vertical) y de verde (eje horizontal) obtenemos colores muy extremos, como el amarillo o incluso el blanco, los cuales es posible que no nos interese considerar como rojos. 

Por esta razón es necesario explorar otras soluciones.

## La "frontera de colores"

Como hemos explicado más arriba cada color RGB puede representarse a través de tres componentes básicas. Por esta razón el espacio de colores RGB es un espacio vectorial tridimensional, con base cada una de sus compoentes rojo, verde y azul. En consecuencia podemos definir la distancia euclídea entre dos colores como lo hacemos usualmente.

----
### Distancia entre dos colores

Podemos definir la distancia euclídea entre dos colores de la siguiente manera:

$$
d(C_1, C_2) = \sqrt{(R_1 - R_2)^2 + (R_1 - R_2)^2 + (R_1 - R_2)^2}
$$

Donde cada color se representa como:
$$
C_i = (R_i, G_i, B_i)
$$

Es decir, la longitud del segmento rectilíneo que los une.

-----

Una vez tenemos una distancia es razonable pensar que un color RGB será "parecido" al rojo en caso de que su distancia al rojo puro (255, 0, 0) sea pequeña.

Es aquí donde entra en juego la idea que me lleva realizar este post. Sería interesante visualizar, para un color dado, qué colores se encuentran a una distancia *r* fija. Estos colores son los que se encuentran en la superficie de la esfera de centro el color dado y como radio la distancia deseada *r*.

## Visualización

En este ejemplo concreto tomaremos como centro de la esfera el color rojo puro (255, 0, 0) e iremos variando el valor del radio para observar las diferentes fronteras de colores para intentar determinar visualmente a partir de qué distancia aparecen colores que a nuestro propio criterio ya no consideramos rojos. Para ello, podemos representar cada punto del espacio como el propio color que codifica.

![RGB Color Frontier](/assets/color_frontier_rgb.gif)

En el GIF solo puede verse un cuarto de la esfera de la que hablábamos. Esto es porque el espacio RGB en el que nos movemos es un cubo. De hecho, se puede definir como:
$$
\mathbb{Z^3} \cap [0, 255]^3
$$

Así pues, no existen colores con coordenadas negativas o con valores mayores a 255. Este hecho también se puede observar para valores del radio mayor a 255 cuando la esfera empieza a ser cortada por los planos del cubo. Para diferentes colores de origen obtendremos cortes diferentes de la esfera.

A mi criterio personal, los colores en la esfera de radios superiores a 150 ya presentan tonalidades demasiado azuladas y verdosas como para considerarlos rojo.

**Observación:** El plano de colores que visualizamos en la primera imagen del post se corresponde con el plano izquierdo que podemos ver en en la animación.

## Código

Terminemos el post comentando el código que utilicé para realizar la esfera de la animación.

Hay múltiples maneras de representar una esfera de radio r en el espacio tridimensional, pero en este caso utilizaremos una parametrización que nos hará las cosas muy sencillas. Podemos definir cada punto de la esfera como:

$$
x = x_0 + r\cos\theta\sin\varphi\\
y = y_0 + r\sin\theta\sin\varphi\\
z = z_0 + r\cos\varphi
$$

- El centro de la esfera es 
$$(x_0, y_0, z_0)$$

- Además, el parámetro
$$
\theta \in [0, 2\pi]
$$
, y el parámetro 
$$\varphi \in [0, \pi]$$

Intentemos pasar esto a código Python: