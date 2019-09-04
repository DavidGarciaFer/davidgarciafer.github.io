---
layout: post
published: true
title:  "Funcionamiento de @BotGuada"
categories: [twitter, bot, python, juegos]
tags: [twitter, bot, python, juego, guadalajara, provincia, españa, worldwarbot,
worldwar, spanish, spain]
---

[@BotGuada][1] es un bot de Twitter, que simula una guerra entre los diferentes municipios de la provincia de Guadalajara (España). Se trata de una réplica del bot [@worldwarbot][2], tratando de aplicar la misma mecánica de juego al mapa de Guadalajara.

**Contenidos**
* TOC
{:toc}

## Municipios participantes

El juego toma como participantes todos los términos municipales de la provincia de Guadalajara. Estos se pueden consultar en el siguiente [enlace][3]. Son un total de 288 localidades, a las que se ha añadido el término de Aldovera, perteneciente a Illana y declarado como despoblado, por necesidades técnicas.

## Funcionamiento

### Consideraciones previas

Para explicar el funcionamiento definiremos tres conceptos:

* **Pueblo**: Zona del mapa delimitada por una frontera.
* **Municipio**: Cada una de las localidades participantes. Al comienzo del juego cada municipio domina un único pueblo (el suyo propio).
* **Territorio**: Conjunto de pueblos que posee un municipio.

### Algoritmo

Cada hora en punto el bot se activa y realiza los siguientes pasos:

* Escoge un pueblo al azar.
* Si el pueblo escogido ha sido conquistado previamente por otro municipio puede independizarse con una probabilidad de $$\frac{1}{24}$$.
* En caso de que el pueblo no se independice se busca entre los pueblos dominados por un municipio diferente el pueblo más cercano al escogido y todos los pueblos cuyo pueblo más cercano es el escogido.
* De estos pueblos se escoge uno al azar, que será atacado y conquistado por el municipio que controla el pueblo escogido.
* Se generan las imágenes de los mapas y el mensaje del tweet.
* El tweet es publicado.

Este proceso se repite hasta que un solo municipio domina todos los pueblos de la provincia.

## Mapas

### Esquema del ataque

![Esquema del ataque](/assets/GuadaBot-Funcionamiento/esquema.png)

En este mapa se muestran los pueblos involucrados en un ataque de tres colores diferentes:

* Verde: En verde aparece el pueblo que ha sido conquistado en este turno (sólo uno por turno).
* Azul: En azul se muestran los pueblos que domina el municipio que ha conquistado en este turno.
* Rojo: El color rojo identifica qué pueblos le quedan al municipio que antes dominaba el pueblo conquistado en este turno.

### Mapa general

El mapa general muestra un color único por cada municipio, a medida que un municipio va conquistando pueblos éstos apareceran con el color del municipio que los domina.

En el primer turno el mapa era:

![Mapa inicial](/assets/GuadaBot-Funcionamiento/inicial.png)

Mientras que en el turno 162 el mapa es:

![Mapa mitad](/assets/GuadaBot-Funcionamiento/general.png)

## FAQ

### ¿Puedo hacer algo para que mi pueblo ataque / se independice?

No, el funcionamiento es totalmente aleatorio.

### ¿Cuánto dura cada partida?

Ya que las conquistas con aleatorias es imposible saber con exactitud la duración del juego. Antes de lanzar el bot se realizaron pruebas ejecutando 10.000 partidas para comparar sus duraciones. Los resultados fueron los siguientes:

![Duración de las partidas](/assets/GuadaBot-Funcionamiento/stats.jpeg)

A la vista de estos resultados las partidas suelen durar unos **dos meses y medio**. Hay que tener en cuenta que Guadalajara es la quinta provincia con más municipios de España. De esta manera la duración se dispara respecto a otros bots como el WorldWarBot, para el cual hay 194 países soberanos.

### ¿Por qué no aparece mi pueblo?

Si tu pueblo no aparece como participante es porque es una pedanía de algún otro municipio. Las pedanías no se han incluido por falta de recursos técnicos y porque la partida sería excesivamente larga.

### ¿Por qué mi pueblo ha vuelto a atacar si fue conquistado?

Puede darse la situación en la que un pueblo haya sido atacado pero que su municipio no haya sido derrotado. Por ejemplo, si Lupiana ha conquistado Horche aunque Chiloeches conquiste Lupiana (como pueblo) Lupiana sigue existiendo como municipio porque aún mantiene el dominio de Horche. En caso de que un municipio pierda todos los pueblos que poseía se especificará en el tweet que "ha sido derrotado" y ya no podrá volver a atacar a no ser que se independice.

### ¿Cómo recibir notificaciones?

Puedes activar la campana situada al lado del botón de *Seguir* para recibir notificaciones cada vez que se publique un tweet desde el bot.

## Contacto

Para cualquier comunicación sobre el bot puedes escribir un *Mensaje directo* en Twitter a la propia [cuenta del bot][1].

[1]: https://twitter.com/BotGuada
[2]: https://worldwarbot.com/bots/
[3]: https://es.wikipedia.org/wiki/Anexo:Municipios_de_la_provincia_de_Guadalajara
