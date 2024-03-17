---
layout: post
title:  "Ingeniería del Caos"
date:   2024-03-16 22:55:35 +0300
tags: [ Docker, Virtualización ]
image:  '/images/posts/2024/2024-03-17-ingenieria-del-caos/chaos-627218.jpg'
description: El origen de la ingeniería del caos se podría situar hacia la década del 2010, fue popularizada por 
   Netflix, al tratar de asegurar la disponibilidad y el rendimiento de su servicio de streaming a gran escala
comments: true
---

El origen de la ingeniería del caos se podría situar hacia la década del 2010, fue popularizada por Netflix, al 
tratar de asegurar la disponibilidad y el rendimiento de su servicio de streaming a gran escala. Su éxito inspiró a 
otras empresas a adoptar prácticas similares, lo que ha llevado a la creación de un conjunto de principios, 
herramientas, y mejoras prácticas que se han convertido en la ingeniería del caos.

Podría decirse, que la ingeniería del caos es una disciplina que se centra en evaluar y mejorar la 
resistencia de los sistemas a condiciones inesperadas, a través de experimentos controlados. La ídea básica es 
identificar y corregir puntos débiles antes de que puedan convertirse en problemas reales en entornos productivos. 
Implica introducir fallos en el sistema de forma controlada para ver cómo responde, y asegurarse de que puede 
recuperarse de ellos sin afectar significativamente a las operaciones. 

## Principios

1. **Construir una hipótesis de comportamiento estable:** Antes de realizar cualquier experimento, se debe tener 
   claro cuál debe ser el comportamiento normal del sistema. Entender las métricas de rendimiento, la tolerancia a 
   fallos y los mecanismos de recuperación.
2. **Variedad en los experimentos:** Los experimentos deben cubrir una amplia gama de fallos posibles, desde 
   problemas de hardware (errores de servidor, discos, red, etc.), hasta problemas de software (bugs, sobrecarga, 
   fallos en las dependencias externas, etc.).
3. **Ejecutar experimentos en producción:** Siempre que sea posible y para obtener resultados más precisos y útiles, 
   los experimentos deben llevarse a cabo en el entorno de producción real, siempre que sea posible, ya que es el 
   entorno en el que se espera que el sistema maneje los fallos de manera efectiva.
4. **Automatización de experimentos**: La automatización de experimentos permite la ejecución regular y sistemática, 
   asegurando que los sistemas sean probados de manera continua y consistente para identificar y corregir fallos proactivamente.
5. **Minimizar el impacto**: Es crucial diseñar y ejecutar experimentos de manera que se minimice el impacto en los 
   usuarios finales, como limitar la escala o ejecutarlos durante periodos de baja actividad.
6. **Verificación de los puntos de integración y las rutas críticas:** Los experimentos deben enfocarse 
   especialmente en los puntos de integración y las rutas críticas del sistema, por ser los puntos más susceptibles 
   a fallos y tener mayor relevancia en la experiencia del usuario.
7. **Aprender de los experimentos**: Cada experimento debe ser seguido por un proceso de revisión en el que se analicen 
   los resultados, se identifiquen las lecciones aprendidas y se tomen medidas para mejorar la resiliencia del 
   sistema.
8. **Fomentar una cultura de confianza:** La implementación exitosa de la ingeniería del caos requiere una cultura organizacional que valore la transparencia, la responsabilidad y el aprendizaje continuo.
9. **Fomentar una cultura de resiliencia:** Es esencial fomentar una cultura organizacional que valore la 
   resiliencia como oportunidades para aprender y mejorar.


## Mejoras prácticas

En lo que se refiere a las mejoras prácticas, estas se centran en el marco experimental y cómo implementar los 
experimentos de manera efectiva y segura. Algunas de las mejoras prácticas incluyen: 

1. **Comenzar pequeño:** Los experimentos deben iniciarse aplicando los de menor escala y complejidad antes de 
   avanzar a experimentos más grandes y potencialmente más disruptivos. Esta secuencia minimiza los riesgos en el 
   entorno de producción y en la experiencia del usuario. 
2. **Definir claramente los objetivos del experimento:** Es crucial tener objetivos claros y medibles. Entender qué 
   aspectos del sistema se van a probar y qué resultados se esperan. 
3. **Monitoreo y alertas:** Se debe disponer de sistemas de monitoreo y alerta robustos. No solo porque ayudan a 
   detectar inmediatamente cuando algo va mal durante un experimento, sino porque proporciona datos críticos para el análisis posterior. 
4. **Documentar todo:** Es esencial mantener una documentación completa de cada experimento, incluyendo: diseño, 
   ejecución, resultados, y lecciones aprendidas. 
5. **Involucrar a equipos multidisciplinares:** En los experimentos se deben involucrar a equipos de desarrollo, 
   operaciones, seguridad, y cualquier otra área relevante. Así se asegura que todos los aspectos del sistema sean 
   considerados. 
6. **Realizar un análisis post-mortem:** Después de cada experimento, se deben analizar los resultados: qué sucedió, 
   por qué sucedió, y cómo se puede mejorar. 
7. **Implementar una estrategia de rollback:** Antes de comenzar cualquier experimento, se debe disponer de una 
   estrategia de rollback. Esto minimiza el riesgo y asegura la restauración del servicio. 
8. **Integrar la ingeniería del caos en el ciclo de vida del desarrollo de software:** En la medida de lo 
   posible se deben incorporar prácticas de ingeniería del caos desde las primeras etapas de desarrollo hasta la 
   producción, asegurando que la resiliencia sea una consideración central a lo largo del ciclo de vida del producto.

## Tipos de herramientas

Atendiendo al propósito que siguen y el tipo de fallos o condiciones que simulan se pueden clasificar en:

1. **Inyectores de Fallos de Infraestructura:** Estas herramientas simulan fallos en componentes de hardware o 
   infraestructura, como servidores, redes y sistemas de almacenamiento. Pueden apagar máquinas, simular fallas en 
   discos duros, o introducir latencia en la red, por ejemplo. 
   * Ejemplo: [Chaos Monkey (Netflix)](https://keepcoding.io/blog/que-es-chaos-monkey/#:~:text=de%20cualquier%20falla.-,%C2%BFQu%C3%A9%20es%20Chaos%20Monkey%3F,la%20estabilidad%20de%20la%20plataforma), que aleatoriamente termina instancias de servicios para probar la resiliencia 
   de los sistemas. 
2. **Generadores de Carga y Estrés:** Tratan de probar cómo se comportan los sistemas bajo condiciones de carga 
   extrema o inusual, como picos de tráfico elevado o demanda de procesamiento.
   * Ejemplo: [Stress-ng](https://github.com/ColinIanKing/stress-ng), que impone cargas de trabajo diversas para estresar varios componentes del sistema. 
3. **Simuladores de Latencia de Red y Particiones:** Permiten a los ingenieros probar cómo los sistemas se comportan 
   en condiciones de red adversas, incluyendo alta latencia, pérdida de paquetes, o desconexiones de red completas.
   * Ejemplo: [Toxiproxy](https://github.com/Shopify/toxiproxy) (Shopify), que simula condiciones de red adversas. 
4. **Herramientas de Estado de Aplicación Corrupto:** Simulan errores en el estado de la aplicación, como corrupción de 
   datos o fallos de caché, para ver cómo el sistema se recupera o maneja datos inconsistentes. 
   * Ejemplo: [SimianArmy (Netflix)](https://github.com/Netflix/SimianArmy), incluye varias herramientas además de Chaos Monkey, que pueden introducir 
     cambios en 
      el estado de la configuración del sistema. 
5. **Simuladores de Fallos de Dependencias Externas:** Orientadas a probar la resiliencia del sistema a fallos o 
   comportamientos inesperados de servicios externos o APIs de los que depende. 
   * Ejemplo: [Gremlin](https://www.gremlin.com/), que ofrece ataques dirigidos a simular fallos en servicios externos. 
6. **Plataformas de Orquestación de Experimentos de Caos:** Proporcionan un marco integral para planificar, ejecutar,
   y analizar experimentos de ingeniería del caos a escala, a menudo integrándose con sistemas de monitoreo y alerta 
   existentes.
   * Ejemplo: [Chaos Toolkit](https://chaostoolkit.org/), que permite definir y ejecutar experimentos de caos contra una variedad de sistemas. 
7. Herramientas de Análisis y Visualización: Aunque no realizan experimentos de caos por sí mismas, estas herramientas son cruciales para analizar los resultados de los experimentos y visualizar el impacto del caos en los sistemas. 
   * Ejemplo: [Kibana](https://www.elastic.co/es/elastic-stack) o [Grafana](https://grafana.com/), utilizadas para visualizar métricas y 
     logs.


En definitiva la Ingeniería del Caos es una disciplina esencial en el desarrollo y operación de sistemas 
resilientes y confiables, enfocada en la introducción deliberada de condiciones adversas en los sistemas para mejorar su 
capacidad de recuperación y tolerancia a fallos. Originada y popularizada por empresas líderes en tecnología, esta 
práctica se sustenta en principios bien definidos que guían la ejecución de experimentos controlados en entornos de 
producción.

   
## Referencias

* [Principles of Chaos Engineering](https://principlesofchaos.org/)
* [Netflix Tech Blog](https://netflixtechblog.com/)
* Imagen de <a href="https://pixabay.com/es/users/levelord-757667/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=627218">levelord</a> en <a href="https://pixabay.com/es//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=627218">Pixabay</a>
