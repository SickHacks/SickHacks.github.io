---
title: "Máquinas: El mito de la aleatoriedad" 
published: 2024-09-21
description: ''
image: ''
tags: [programación, computación]
category: 'Programación'
draft: false 
lang: 'es'
---

Cuando hablamos de **aleatoriedad**, solemos pensar en algo completamente impredecible. Sin embargo, cuando se trata de máquinas, como las computadoras, esta idea se convierte en una ilusión. Las computadoras son sistemas deterministas: siempre siguen reglas predefinidas. Entonces, ¿Cómo logran generar lo que parece ser "azar"?

## ¿Qué Tan Aleatorias Son Las Computadoras?

Por naturaleza, las computadoras siguen algoritmos exactos. Esto significa que, dadas las mismas condiciones y entradas, siempre producirán los mismos resultados. La verdadera aleatoriedad es completamente impredecible, pero las computadoras no pueden lograr esto por sí solas. Así que, en lugar de generar números aleatorios reales, usan **generadores de números pseudoaleatorios** (PRNG).

![](img/aleatoriedad.png)

Los PRNG simulan la aleatoriedad usando fórmulas matemáticas. Estos generadores necesitan un punto de partida, conocido como **semilla**, que determina la secuencia de números "aleatorios". La curiosidad aquí es que si se usa la misma semilla, el PRNG generará exactamente la misma secuencia cada vez. Por lo tanto, la aleatoriedad que vemos es solo aparente.

## El Rol de la Semilla: El Corazón del "Azar"

La **semilla** es esencial en los PRNG. Cambiar la semilla produce una secuencia completamente diferente, lo que permite simular la variabilidad en el comportamiento del programa. Si no se especifica una semilla, las computadoras suelen elegir una automáticamente, utilizando datos como la hora del sistema, garantizando que cada ejecución sea única.

Esto es útil en campos como las simulaciones, donde fijar la semilla permite repetir los experimentos exactamente. En la **criptografía**, sin embargo, la semilla debe ser impredecible para mantener la seguridad, ya que una semilla predecible podría comprometer todo el sistema.

![](https://www.labiotech.eu/wp-content/uploads/2017/03/electronic-plants-e-plants-bioelectronics-Link%C3%B6ping.jpg)

## ¿Es Suficiente la Pseudoaleatoriedad?

Para la mayoría de las aplicaciones, los PRNG son suficientemente aleatorios. Desde videojuegos hasta simulaciones científicas, esta pseudoaleatoriedad funciona bien. Pero en áreas más críticas, como la criptografía o ciertos tipos de simulaciones, a veces es necesario recurrir a **fuentes físicas de aleatoriedad**. Esto incluye fenómenos impredecibles del entorno, como el ruido térmico o el tiempo entre eventos del sistema.

## Las máquinas dependen de la realidad para generar verdadera entropía

Un ejemplo fascinante de cómo las máquinas pueden aprovechar la realidad para generar **verdadera entropía** es la **pared de lámparas de lava de Cloudflare**. Esta instalación única utiliza el movimiento **impredecible** de la lava dentro de las lámparas como una fuente de entropía natural. Cada burbuja que sube y baja, y cada cambio en el patrón del líquido, introduce variaciones que son completamente **impredecibles**. Cloudflare captura estos movimientos y los utiliza para alimentar sus generadores de números aleatorios, asegurando que los datos producidos sean **genuinamente aleatorios** y no simplemente el resultado de fórmulas matemáticas. Este enfoque destaca la importancia de las **fuentes físicas** en la creación de números aleatorios verdaderos, mostrando cómo las máquinas pueden depender de la realidad para superar sus **limitaciones inherentes**.

![](https://www.cjr.org/wp-content/uploads/2022/09/AP_22243655794994.jpg)



Aunque las máquinas no son capaces de generar verdadera aleatoriedad por sí solas, los **PRNG** ofrecen una solución efectiva para la mayoría de las necesidades. Sin embargo, en campos donde la impredecibilidad es clave, es crucial utilizar fuentes de aleatoriedad genuina. La próxima vez que uses un número **"aleatorio"** en tu código, recuerda que detrás de esa aparente casualidad hay un algoritmo perfectamente calculado.
