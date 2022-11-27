---
layout: post
title:  "Pruebas de Software: Tipología"
date:   2022-08-26 15:01:35 +0300
tags: [ QA, Testing ]
image:  '/images/posts/2022/20220826-pruebas-software/2022-08-26-pruebas-software-post-image.jpg'
description: Este post es el inicio de una serie en la que trataré de profundizar en los distintos tipos de pruebas de software, en cuanto a objetivos que persiguen, planteamiento, ejecución y otras características de interés. Como punto de partida, se va a exponer una categorización de pruebas de software que enlazará directamente con sus post específicos en los casos en los que aplique. En el resto de casos, simplemente quedarán indicadas en este post como parte de la jerarquía.
hidden: true
comments: true
---

Este post es el inicio de una serie en la que trataré de profundizar en los distintos tipos de pruebas de software, en cuanto a objetivos que persiguen, planteamiento, ejecución y otras características de interés. Como punto de partida, se va a exponer una categorización de pruebas de software que enlazará directamente con sus post específicos en los casos en los que aplique. En el resto de casos, simplemente quedarán indicadas en este post como parte de la jerarquía.

## Criterios de clasificación

* Tipo de ejecución
* Aproximación al problema
* Objeto de verificación

## Tipo de ejecución

Atendiendo al tipo de ejecución las pruebas de software pueden clasificarse como: 

### Automáticas

Este tipo de pruebas son ejecutadas por máquinas, generalmente se ejecutan mediante scripts.

Algunas de sus ventajas respecto a las pruebas manuales son:
* su ejecución es más rápida
* son más fiables

Entre sus inconvenientes se encuentran:
* su calidad depende de la calidad de los scripts

Sin embargo, por su naturaleza, las pruebas automáticas son un componente clave tanto para la integración continua como para la entrega continua y, por tanto, para asegurar la calidad del software.

### Manuales

Se trata de pruebas llevadas a cabo por personas, que interactúan con el software verificando su correcto funcionamiento

El principal inconveniente de este tipo de pruebas es su coste, ya que se requiere personal dedicado a la ejecución de pruebas.
Otro inconveniente es que el resultado se puede afectado por el error humano.

Entonces, ¿por qué realizar pruebas manuales?. Pues porque los test manuales probar en profundidad nuevas características del producto, para verificar su funcionamiento, proporcionar feedback de usabilidad y experiencia de usuario. Las pruebas automáticas tienen sentido sobre todo en los casos de pruebas de regresión para asegurar que las nuevas características no han introducido bugs en las funcionalidades anteriores.  
## Referencias

* [Foto de Girl with Red Hat](https://unsplash.com/@girlwithredhat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText")
* [Pruebas de software - Wikipedia](https://es.wikipedia.org/wiki/Pruebas_de_software)
* [Software testing - Wikipedia](https://en.wikipedia.org/wiki/Software_testing)
* [Programación y más](https://programacionymas.com/blog/tipos-de-testing-en-desarrollo-de-software)
* [Arbusta](https://arbusta.net/testing-automation-manual-diferencias/)
* [El gran dilema del QA: ¿Automatización o testing manual?](https://www.bbvanexttechnologies.com/blogs/el-gran-dilema-del-qa-automatizacion-o-testing-manual/)