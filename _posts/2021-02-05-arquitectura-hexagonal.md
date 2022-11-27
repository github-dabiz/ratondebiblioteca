---
layout: post
title:  "Arquitectura hexagonal"
date:   2021-02-05 15:01:35 +0300
tags: [ Diseño, Arquitectura ]
image:  '/images/posts/2021/20210205P.jpg'
description: Propuesta en 2005 por Alistair Cockburn, también conocida como arquitectura de puertos y adaptadores, su finalidad es desacoplar las capas de la aplicación, y permitir así que evolucionen de forma aislada. La forma de conseguirlo, considerar el dominio como el núcleo de la arquitectura, y proteger su estructura de interacciones con elementos externos.
comments: true
---

Propuesta en 2005 por [Alistair Cockburn](http://web.archive.org/web/20180121161736/http://alistair.cockburn.us/Hexagonal+Architecture), también conocida como arquitectura de puertos y adaptadores, su finalidad es desacoplar las capas de la aplicación, y permitir así que evolucionen de forma aislada. La forma de conseguirlo, considerar el dominio como el núcleo de la arquitectura, y proteger su estructura de interacciones con elementos externos.

El motivo de representar este tipo de arquitecturas como un hexágono, es arbitrario, lo que pretende reflejar es el hecho de que las comunicaciones con el exterior tienen la misma naturaleza y deben asumir las mismas reglas.

<!--more-->
La consecuencia que se deriva de aplicar esta arquitectura, es desarrollar aplicaciones que son:

* Independientes de agentes externos
* Independientes de framework
* Independientes de la UI
* Independientes de BBDD
* Más adaptables
* Más reutilizables
* Más mantenibles
* Más testeables

## Motivación

En su origen, Alistair Cockburn hacía mucho énfasis en evitar la infiltración de la lógica de negocio en la interfaz de usuario. Ya que si esto se produce, da lugar a una arquitectura fuertemente acoplada entre el frontend y el backend.

Su propuesta pretende mejorar las siguientes áreas durante el ciclo de vida del producto:

* automatización de pruebas
* desacoplamiento de los detalles de infraestructura
* conexión con nuevos agentes
* abstraer el dominio de la comunicación con agentes externos, usuarios, comandos, lotes, etc.

Según él, la raíz del problema está en la interacción entre el interior y el exterior, la regla es que los detalles del código interior no deben filtrarse al exterior.

 Para conseguirlo, propone utilizar el patrón de puertos y adaptadores:

* Los **puertos** son las interfaces que definen la interacción con el exterior y exponen únicamente el contrato con nuestro dominio, dejando que toda la lógica de transformación esté de puertas afuera y no se contamine el interior. Pueden ser de varios tipos:
  * **de entrada**. Sirven para hacer peticiones a la aplicación
  * **de salida**. Sirven para solicitar información desde la aplicación
  * **de entrada/salida**. En general son adaptadores bidireccionales a un recurso. No representan comunicación bidireccional, sino que la aplicación es capaz de escuchar peticiones del recurso y hacerle peticiones.

* Los **adaptadores** son precisamente la forma de conectarse a través de dicho contrato. Establecen la comunicación y la conversión de datos entre el dominio y el exterior. Lo relevante en esta interacción, es que los adaptadores no pertenecen al core, así que pueden implementarse por separado. Para cada dispositivo externo habrá un adaptador, y será de entrada o salida dependiendo del sentido de la comunicación. Por lo general habrá varios adaptadores para cada tipo de puerto, uno por tipo de tecnología o agente que puede conectarse a ese puerto.

Además, diferencia entre dos tipos de puertos y adaptadores. Los **primarios o driving**, son los que habilitan conexiones hacia nuestra aplicación, y los **secundarios** que permiten conexiones desde nuestra aplicación. También tienen otra particularidad, en los que conectan la UI con la aplicación, el adaptador pertence a la capa de aplicación (API) del caso de uso, mientras que en el caso de backend, es éste el que inyecta el adaptador específico para permitir la comunicación con la utilidad. El núcleo en ambos casos solo debe conocer la interfaz.

![Workflow]({{site.baseurl}}/images/posts/2021/20210205_111.jpg)
*Puertos y adaptadores*

## Arquitectura hexagonal y DDD

Al ser el dominio el elemento en torno al cual se define este enfoque, encaja muy bien con la idea de **Domian Driven Design**, ya que se puede implementar su lógica sin atender a detalles específicos.

Sin embargo, un punto de vista crítico con todo este planteamiento, que considero muy interesante es el de [Javier Vélez Reyes - Ni Nueva Ni Arquitectura Ni Hexagonal](https://javiervelezreyes.com/ni-nueva-ni-arquitectura-ni-hexagonal/), que argumenta razonadamente en contra de esta propuesta, como un enfoque geniunamente arquitectónico y rebajándolo a un tipo de patrón o recomendación de buenas prácticas, que símplemnte aglutina conceptos preexistentes.

## Referencias

* [Alistair Cockburn](http://web.archive.org/web/20180121161736/http://alistair.cockburn.us/Hexagonal+Architecture)
* [Edu Slaguero: Arquitectura hexagonal](https://medium.com/@edusalguero/arquitectura-hexagonal-59834bb44b7f)
* [Allegro Tech Blog](https://blog.allegro.tech/2020/05/hexagonal-architecture-by-example.html)
* [hgraca](https://herbertograca.com/2017/09/14/ports-adapters-architecture/)
* [Capas, cebollas y colmenas: arquitecturas en el backend](https://www.adictosaltrabajo.com/2019/07/02/capas-cebollas-y-colmenas-arquitecturas-en-el-backend/)
* [Ni Nueva Ni Arquitectura Ni Hexagonal](https://javiervelezreyes.com/ni-nueva-ni-arquitectura-ni-hexagonal/)