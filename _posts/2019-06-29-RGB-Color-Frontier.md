---
layout: post
title:  "Frontera de un color RGB"
categories: [python, graficos, procesamiento de imagenes, matematicas]
tags: [color, colores, rgb, distancia, esfera, matplotlib, numpy, pyplot]
---

Hace unos días, trabajando en un proyecto de **procesamiento de imágenes** en el que debía determinar el color de una fuente de luz tropecé con el siguiente problema: Dado un color **RGB**... *¿Cómo determinar si dicho color es una tonalidad de rojo? (o de cualquier otro color básico).*

**Contenidos**
* TOC
{:toc}

-----
### Una nota sobre el modelo de color RGB

[RGB][1] *(Red, green and blue)* es un modo de codificar un color a través de la cantidad o intensidad de los colores primarios de la luz, estos son, rojo, verde y azul.

La intensidad de cada color se puede codificar de diferentes maneras, por ejemplo, la intensidad de cada una de las componentes puede medirse entre 0 y 255. Es usual
representar estas cantidades en [base hexadecimal][2] (0x00 y 0xFF respectivamente).

Ejemplos:

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-7zz7{background-color:#ffff00;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-hs0z{background-color:#ff0000;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-jqcb{background-color:#ff8000;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-tu2r{background-color:#cccccc;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-7bqb{background-color:#0000ff;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-7btt{font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-ksrh{background-color:#000000;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-ukt9{background-color:#00ff00;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-lrim{background-color:#994c00;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-517h{background-color:#990099;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-3xi5{background-color:#ffffff;border-color:inherit;text-align:center;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-7btt">Color</th>
    <th class="tg-7btt">Código entero</th>
    <th class="tg-7btt">Código hexadecimal</th>
    <th class="tg-7btt">Apariencia</th>
  </tr>
  <tr>
    <td class="tg-c3ow">Negro</td>
    <td class="tg-c3ow">(0, 0, 0)</td>
    <td class="tg-c3ow">#000000</td>
    <td class="tg-ksrh"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Rojo</td>
    <td class="tg-c3ow">(255, 0, 0)</td>
    <td class="tg-c3ow">#FF0000</td>
    <td class="tg-hs0z"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Verde</td>
    <td class="tg-c3ow">(0, 255, 0)</td>
    <td class="tg-c3ow">#00FF00</td>
    <td class="tg-ukt9"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Azul</td>
    <td class="tg-c3ow">(0, 0, 255)</td>
    <td class="tg-c3ow">#0000FF</td>
    <td class="tg-7bqb"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Naranja</td>
    <td class="tg-c3ow">(255, 128, 0)</td>
    <td class="tg-c3ow">#FF8000</td>
    <td class="tg-jqcb"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Amarillo</td>
    <td class="tg-c3ow">(255, 255, 0)</td>
    <td class="tg-c3ow">#FFFF00</td>
    <td class="tg-7zz7"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Marrón</td>
    <td class="tg-c3ow">(153, 76, 00)</td>
    <td class="tg-c3ow">#994C00</td>
    <td class="tg-lrim"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Morado</td>
    <td class="tg-c3ow">(153, 0, 153)</td>
    <td class="tg-c3ow">#990099</td>
    <td class="tg-517h"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Gris</td>
    <td class="tg-c3ow">(204, 204, 204)</td>
    <td class="tg-c3ow">#CCCCCC</td>
    <td class="tg-tu2r"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Blanco</td>
    <td class="tg-c3ow">(255, 255, 255)</td>
    <td class="tg-c3ow">#FFFFFF</td>
    <td class="tg-3xi5"></td>
  </tr>
</table>

----

## Una primera aproximación al problema

Es razonable afirmar que los colores que tengan una cantidad elevada de rojo pueden considerarse una tonalidad de rojo. En el siguiente ejemplo podemos ver todos los colores que tienen la componente *R = 255*, es decir, en su valor máximo.

![RGB Colors, R = 255](/assets/r255.png)

Como vemos, a medida que añadimos más cantidad de azul (eje vertical) y de verde (eje horizontal) obtenemos colores muy extremos, como el amarillo o incluso el blanco, los cuales es posible que no nos interese considerar como rojos. 

Por esta razón es necesario explorar otras soluciones.

## La "frontera de colores"

Como hemos explicado más arriba cada color RGB puede representarse a través de **tres componentes básicas**. Por esta razón el espacio de colores RGB es un **espacio tridimensional**. En consecuencia podemos definir la [distancia euclídea][3] entre dos colores como lo hacemos usualmente.

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

Es decir, es la longitud del segmento rectilíneo que los une.

-----

Una vez tenemos una distancia es razonable pensar que un color RGB será "parecido" al rojo en caso de que su **distancia** al rojo puro (255, 0, 0) sea **"pequeña"**.

Es aquí donde entra en juego la idea que me lleva realizar este post. Sería interesante **visualizar**, para un color dado, qué colores se encuentran a una distancia *r* fija. Estos colores son los que se encuentran en la superficie de la **esfera** de centro el color dado y como radio la distancia deseada *r*.

## Visualización

En este ejemplo concreto tomaremos como centro de la esfera el color **rojo puro** *(255, 0, 0)* e iremos variando el valor del radio para **observar las diferentes fronteras** de colores e intentar determinar visualmente a partir de qué distancia aparecen colores que a nuestro propio criterio ya no consideramos rojos. Para ello, podemos **representar** cada punto del espacio **como el propio color que codifica**.

![RGB Color Frontier](/assets/color_frontier_rgb.gif)

En el GIF solo puede verse un cuarto de la esfera de la que hablábamos. Esto es porque el espacio RGB en el que nos movemos es un cubo. De hecho, se puede definir como:
$$
\mathbb{Z^3} \cap [0, 255]^3
$$

Así pues, **no existen colores con coordenadas negativas o con valores mayores a 255**. Este hecho también se puede observar para valores del radio mayor a 255 cuando la esfera empieza a ser cortada por los otros tres planos del cubo. Para diferentes colores de origen obtendremos cortes diferentes de la esfera.

A mi criterio personal, los colores en la esfera de radios superiores a 150 ya presentan tonalidades demasiado azuladas y verdosas como para considerarlos rojo.

**Observación:** El plano de colores que visualizamos en la primera imagen del post se corresponde con el plano izquierdo que podemos ver en en la animación.

## Códificación

Terminemos el post comentando el código que utilicé para realizar la esfera de la animación.

----

### Fundamentos

Hay múltiples maneras de representar una esfera de radio r en el espacio tridimensional, pero en este caso utilizaremos una **parametrización** que nos hará las cosas muy sencillas. Podemos definir cada punto de la esfera como:

$$
x = x_0 + r\cos\theta\sin\varphi\\
y = y_0 + r\sin\theta\sin\varphi\\
z = z_0 + r\cos\varphi
$$

- El centro de la esfera es 
$$(x_0, y_0, z_0)$$

- Parámetros: 
$$
\theta \in [0, 2\pi]
$$
, y  
$$\varphi \in [0, \pi]$$

-----

### Desarrollo del código

Ahora, pasemos todo esto a código *Python*. Para ello utilizaremos las librerías *pyplot* y numpy, así como algunas herramientas extra de *matplotlib*. Nuestros **import** serán los siguientes:

```python
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np
```

Primero definimos los **parámetros estáticos**, toda esta información puede pasarse como argumento si queremos encapsular el código posterior en una función para realizar animaciones. Por un lado debemos **fijar el centro de la esfera**, que será nuestro color de origen. Además, **definimos** el **radio** de la esfera y la **resolución**, que está relacionada la cantidad de puntos de la esfera que mostraremos. Os recomiendo que juguéis con este último parámetro hasta que encontréis un valor que os satisfaga.

```python
# Initial parameters
x0, y0, z0 = 255, 0, 0
radius = 100
res = 200
```

A continuación haremos uso de *numpy* para definir el **espacio de parámetros**. Aquí entra en juego la resolución de la que hablábamos arriba. Para el parámetro 
$$\theta$$ 
vamos a escoger *res* valores, comprendidos entre 
$$0$$
y
$$2\pi$$
. Procederemos de manera análoga para 
$$
\varphi
$$
.

Este paso es un buen punto para optimizar el código, sobre todo en este caso, sabiendo que nuestro punto de origen se encuentra en una esquina del cubo, podríamos tomar menos valores de los parámetros, ya que muchos de los puntos que vamos a obtener mediante esta parametrización van a ser desechados. Aún así, para esta ocasión quiero hacer el código funcional **para cualquier punto del espacio RGB**, por lo que lo definiremos como sigue:

```python
# Defining parameter spaces
phi_values = np.linspace(0, np.pi, res)
theta_values = np.linspace(0, 2 * np.pi, res)
```

Ahora es el momento de generar los puntos de la esfera. Iteramos sobre cada valor de 
$$
\varphi
$$
 y para cada uno de ellos iteramos sobre cada valor de 
$$
\theta
$$
. Generamos cada coordenada con la parametrización que vimos más arriba y por último **comprobamos que el punto sea válido**. Cada coordenada o componente de un color se guardará en listas independientes, pero en la entrada de mismo índice.


```python
# Color coordinates will be stored here
r, g, b = [], [], []

for phi in phi_values:
    for theta in theta_values:
        # Calculating the coordinates of the point
        x = x0 + radius * np.cos(theta) * np.sin(phi)
        y = y0 + radius * np.sin(theta) * np.sin(phi)
        z = z0 + radius * np.cos(phi)

        # Checking if the point is inside the RGB space
        if x >= 0 and x < 255 \
                  and y >= 0 and y < 255 \
                  and z >= 0 and z < 255:
            r.append(x)
            g.append(y)
            b.append(z)
```
Por último, creamos una lista con todos los colores que hemos generado en el formato *[R, G, B]*. Esto nos será muy útil para **representar los colores** en la esfera. Es importante que pasemos a *int* los valores de cada color, ya que pyplot no interpretará correctamente los colores en formato decimal.

```python
# Preparing the array of colors
C = np.array([[int(r[i]), int(g[i]), int(b[i])] for i in range(len(r))])
```

Ya solo queda **representar los puntos** que hemos calculado en el espacio de tres dimensiones. Para ello utilizaremos la función *scatter* de *pyplot*, a la que le pasamos como parámetro también la lista de colores que hemos preparado, dividiándola entre 255, ya que pyplot necesita una codificación de RGB con valores entre 0 y 1. Aprovechamos para configurar la longitud de los ejes, para que se adapten al espacio RGB, sus etiquetas, el título del gráfico y su posición inicial (parámetros *elev* y *azim*). Con esto nuestro código está **ya listo para funcionar**.

```python
# Plotting the graph
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

# Axis limits
ax.set_xlim3d(0, 255)
ax.set_ylim3d(0, 255)
ax.set_zlim3d(0, 255)

# Axis labels and colors
ax.set_xlabel('R', color='#FF0000')
ax.set_ylabel('G', color='#00FF00')
ax.set_zlabel('B', color='#0000FF')

# Title
plt.title('RGB Color Frontier\nRadius = ' + str(int(radius)), y=1.05)

ax.scatter(r, g, b, c =C/255.0)

# Initial position
ax.view_init(elev=30., azim=135)
plt.show()

```

### GIF animado de la gráfica

Si os interesa **crear un GIF animado** como el de este post os recomiendo que visitéis [este tutorial creado por Eliot Andres][4] . Además os dejo el enlace al [código completo en mi GitHub][5].

¡Un saludo y nos vemos en el siguiente post!

[1]: https://en.wikipedia.org/wiki/RGB_color_model
[2]: https://en.wikipedia.org/wiki/Hexadecimal
[3]: https://en.wikipedia.org/wiki/Euclidean_distance
[4]: https://ndres.me/post/matplotlib-animated-gifs-easily/
[5]: https://github.com/DavidGarciaFer/blog-code/tree/master/Color-Frontier