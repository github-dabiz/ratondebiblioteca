---
layout: post
title:  "OAuth 2.0: Protocolo y contenido"
tags: [ Seguridad, Identidad Digital, Autorización, OAuth2, OAuth ]
featured_image_thumbnail: assets/images/posts/2022/20220417-oauth2-codigo-autorizacion/20220414-oauth2-codigo-autorizacion-thumbnail.jpg
featured_image: assets/images/posts/2022/20220826-pruebas-software/post-image.jpg
hidden: false
---

En el anterior post ***[Autorización a Recursos: OAuth 2.0](./oauth2)*** se realizó una primera aproximación conceptual
a OAuth2. En este segundo post, el objetivo es introducir algunos elementos más, que quedaron pendientes, así como
profundizar a nivel técnico en el flujo más habitual de los Grant Types, el Código de Autorización

## Protocolo de Comunicación

Además de los actores y el flujo conversacional, para entender bien todos los pasos que se dan desde la petición de
autorización inicial, hasta la obtención de la información por parte del cliente, hay que conocer tanto el protocolo que
se utiliza para transmitir la información como su intérprete.

OAuth2 es un framework pensado ser implementado mediante protocolo de comunicación HTTP, basado en el principio de
cliente-servidor, en el que las peticiones son enviadas por una entidad: el agente del usuario (o un proxy a petición de
uno). La
mayoría de las veces el agente del usuario es un navegador Web, pero podría ser cualquier otro programa.

El intérprete es el elemento que articula la comunicación entre todos los actores, Usuario - Aplicación Cliente, Usuario

- Servidor de autorización, y en menor medida Aplicación Cliente - Servidor de Recursos. En este último caso,
  intervienen en las redirecciones HTTP.

Aunque también importante, no se va a profundizar en la estructura de la petición y respuesta HTTP por considerar que
queda fuera del alcance de artículo, aunque se puede encontrar una descripción detallada
en ***[Generalidades del protocolo HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Overview)***

## Registro de la Aplicación Cliente

Un paso importante que no se suele mencionar al explicar el flujo abstracto o flujo lógico de OAuth2, es el registro de
la Aplicación Cliente en el Servidor de Autorización. Antes de que la Aplicación Cliente esté en condiciones de
solicitar la autorización al Usuario y comunicarse con el Servidor de Autorizaciones, es necesario que este último la
tenga registrada como una aplicación de confianza. Generalmente, esto se hace a través de un formulario de registro en
la sección de “desarrolladores” o “API” del sitio web del Servidor de Recursos.

Uno de los datos que será necesario proporcionar, es la URI a la que el Servidor de Autorización deber redireccionar una
vez que el usuario haya dado su autorización. En consecuencia, será un endpoint de la Aplicación Cliente en el que
manejarán códigos de
autorización o tokens de acceso.

Durante el proceso de registro, el Servidor de Autorizaciones, proporcionará al Cliente un identificador (CLIENT_ID), y
un secreto (SECRET). El primero, es una cadena pública que identifica a la Aplicación Cliente, y el segundo se utiliza
para verificar la identidad del Cliente, por lo que debe estar protegido de posibles ataques para evitar suplantaciones
de identidad.

## Token de acceso

Es el elemento que otorga el Servidor de Autorización a la Aplicación Cliente para realizar solicitudes en nombre del
usuario. Se utilizará en la comunicación entre el Servidor de Recursos y la Aplicación Cliente para verificar los
privilegios de acceso, la autorización.

Los tokens de acceso no tienen por qué tener un formato determinado. Pueden ser Bearer Tokens (al portador) o
Sender-constrained (restringidos al emisor). Los tokens restringidos al emisor requieren que el cliente de OAuth
demuestre la posesión de una clave privada para poder utilizar el token de acceso, de manera que el token de acceso por
sí mismo no sería utilizable.

Hay una serie de propiedades de los tokens de acceso que son fundamentales para el modelo de seguridad de OAuth:

* No deben ser leídos o interpretados por el cliente OAuth. El cliente OAuth no es el destinatario del token.
* No transmiten al cliente OAuth la identidad del usuario ni ninguna otra información sobre el mismo.
* Solo deben utilizarse para realizar solicitudes al servidor de recursos.

Existen distintos tipos de token que se aplican en diferentes escenarios:

* Access Token
* ID Token
* Refresh Token

Los tokens de acceso se definen en OAuth, los tokens de identificación se definen en OpenID Connect.

Los tokens de acceso son lo que el cliente de OAuth utiliza para hacer peticiones a una API. El token de acceso está
destinado a ser validado por la API. Un token de identificación contiene información sobre lo que sucedió cuando un
usuario se autenticó, y está destinado a ser leído por el cliente de OAuth. El token de identificación también puede
contener información sobre el usuario, como su nombre o dirección de correo electrónico, aunque esto no es un requisito
de un token de identificación.

Aquí hay algunas otras diferencias entre los tokens de identificación y los tokens de acceso:

* Los tokens de identificación están pensados para ser leídos por el cliente OAuth. Los tokens de acceso están
  destinados a ser leídos por el servidor de recursos.
* Los tokens de identificación son JWTs. Los tokens de acceso pueden ser JWTs pero también pueden ser una cadena
  aleatoria.
* Los tokens de identificación nunca deben ser enviados a una API. Los tokens de acceso nunca deben ser leídos por el
  cliente.

Por otro lado, un Refresh Token en OAuth2 es una cadena que el cliente de OAuth puede utilizar para obtener un nuevo
token de acceso sin la interacción del usuario. No debe permitir al cliente obtener ningún acceso más allá del alcance
de la concesión original. Existe para permitir a los servidores de autorización utilizar tiempos de vida cortos para los
tokens de acceso sin necesidad de involucrar al usuario cuando el token expira.

## Scopes

Los ***scopes*** en OAuth2 se utilizan para identificar los recursos a los que puede tener acceso la Aplicación Cliente.
Cuando el cliente solicita la autorización al Servidor de Autorización, envía la lista de ámbitos para los que se
solicita. El Servidor de Autorizaciones utiliza esta lista de ámbitos para generar la pantalla de consentimiento que
presentará al usuario durante el proceso de Autorización. Si el usuario acepta, el Servidor de Autorización emite un
token con el código de autorización.

## Respuesta OAuth

La respuesta de todos los Grant Types producen un formato de salida semejante, una estructura JSON con los siguientes
valores:

* ***access_token*** (requerido) Cadena cifrada que se utilizará posteriormente para autorizar las peticiones.
* ***token_type*** (requerido) Identifica el tipo de token, se suele utilizar el tipo “Bearer”.
* ***expires_in*** (recomendado) Indica la duración en segundos del access_token. Una vez caduca puede ser renovado
  usando el Grant Type de Refresh Token.
* ***refresh_token*** (opcional) Permiten volver a generar el un token de acceso. Se solicitará su valor durante el
  proceso de Refresh Token.
* ***scope*** (opcional) Indica el ámbito de uso del access token. La AC solicitará información de un scope concreto
  durante la obtención del token.
* ***id_token*** (opcional) Si se utiliza un scope con valor “openid”, puede significar que se quiere utilizar OpenId
  Connect (OIDC) para solicitar una autorización a partir de una autenticación. En ese supuesto, puede aparecer un JWT
  extra donde encontraremos la información sobre el perfil del usuario.

La aplicación debe garantizar que el access token no sea accesible para otras aplicaciones en el mismo dispositivo. El
token de acceso solo se puede usar a través de una conexión HTTPS, ya que pasarlo a través de un canal no encriptado
haría que sea trivial para terceros interceptarlo.

Una vez visto el protocolo de comunicación y alguno de los contenidos específicos que define el framework, es posible
profundizar en todos los pasos que se dan durante
un ***[flujo de comunicación con Código de Autorización](./oauth2-codigo-autorizacion)***

## Referencias

* Photo by [Yannik Mika](https://unsplash.com/@yannikm) on [Unsplash](https://unsplash.com/)
* [OAuth - Wikipedia](https://es.wikipedia.org/wiki/OAuth)
* [IONOS Digital Guide](https://www.ionos.es/digitalguide/servidores/seguridad/oauth-y-su-version-oauth2/)
* [Paradigma Digital](https://www.paradigmadigital.com/dev/oauth-2-0-equilibrio-y-usabilidad-en-la-securizacion-de-apis/)
* [Una introducción a OAuth2](https://www.digitalocean.com/community/tutorials/una-introduccion-a-oauth-2-es)
* [Access Tokens](https://www.oauth.com/oauth2-servers/access-tokens/)
* [OAuth 2.0, OpenID Connect y JSON Web Tokens (JWT) ¿Qué es qué?](https://www.returngis.net/2019/04/oauth-2-0-openid-connect-y-json-web-tokens-jwt-que-es-que/)
* [Explicación del protocolo OAuth 2](https://programacionymas.com/blog/protocolo-oauth-2)
* [Principio de autenticación y autorización OAuth2.0](https://programmerclick.com/article/31691178110/)
* [API Gateway OAuth 2.0 Authentication Flows](https://docs.oracle.com/cd/E39820_01/doc.11121/gateway_docs/content/oauth_flows.html)
* [OAuth 2 Introduction](https://www.techgeeknext.com/spring-boot-security/oauth2-introduction)
* [The Complete Guide to OAuth 2.0 and OpenID Connect Protocols](https://betterprogramming.pub/the-complete-guide-to-oauth-2-0-and-openid-connect-protocols-35ebc1cbc11a)
* [OAuth 2.0 Grant Types](https://www.developerro.com/2019/03/19/oauth-authentication-grant-types/)
* [Generalidades del protocolo HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Overview)