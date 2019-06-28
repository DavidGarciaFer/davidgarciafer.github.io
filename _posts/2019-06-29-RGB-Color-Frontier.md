---
layout: post
title:  "Frontera de un color RGB"
categories: [python, matplotlib, procesamiento de imagenes, matematicas]
tags: [color, colores, rgb, distancia, esfera]
---

# Frontera de un color RGB

Hace unos días, trabajando en un proyecto de procesamiento de imágenes en el que debía determinar si el color de una fuente de luz era rojo tropecé con el siguiente problema: Dado un color RGB ¿cómo determinar si dicho color es una tonalidad de rojo?

### Una nota sobre el modelo de color RGB

RGB (Red, green and blue) es un modo de codificar un color a través de la cantidad o intensidad de los colores primarios de la luz, estos son, rojo, verde y azul.

La cantidad de cada color se puede codificar de diferentes maneras, por ejemplo, la intensidad del componente rojo puede medirse entre 0 y 255, es usual
representar estas cantidades en base hexadecimal (0x00 y 0xFF respectivamente).

Ejemplos:

## Una aproximación al problema

