---
title: "DDD: Identificando Bounded Contexts"
date: 2017-10-08T23:03:17+02:00
tags: ["DDD"]
---

Durante los últimos meses he estado reflexionando bastante sobre **DDD** (_Domain Driven Design_)
con algunos compañeros de trabajo. No soy ni mucho menos un experto en este tema, pero me
gustaría hacer algunos posts sobre cómo he ido asimilando varios de los conceptos más
importantes, ya que con el tiempo ha ido cambiando mi manera de ver algunos de ellos. En este
primer post me gustaría empezar por los **Bounded Contexts**.

Cuando empiezas a programar cualquier plataforma tienes que resolver un problema de negocio, por
lo que el primer paso es modelarlo.
Si este problema de negocio es bastante grande y complejo, puede ser interesante utilizar DDD para este
proceso de modelado. DDD aborda este problema de modelado dividiendo el problema principal
(**Dominio**) en diferentes problemas más pequeñas (**Subdominios**). En el ámbito de la solución,
el equivalente a estos subdominios es a lo que se llama **Bounded Contexts**, mientras que el
equivalente al Dominio sería el **Modelo de Dominio**.

### Y... ¿Cómo identificar los diferentes Bounded Contexts en mi Modelo de Dominio?

La primera vez que me puse a estudiar este concepto me pareció entender bien su definición, y me
hacía una idea de lo que significaba un Bounded Context dentro de un Modelo de Dominio. Pero
me hacía siempre la misma pregunta: si estoy construyendo una aplicación y hago uso de
DDD para su modelado...¿Qué estrategia puedo seguir para ayudarme a identificar mis diferentes
Bounded Contexts? ¿Debo confiar en tener momentos de inspiración para ayudarme a dividir mi
problema en problemas más pequeños? :) Estaba claro que no, y fue aquí cuando entró en juego
para mí otro concepto importante de DDD: el **Lenguaje Ubicuo**.

En DDD, la motivación del Lenguaje Ubicuo es encontrar un lenguaje donde conceptos relacionados con
el negocio signifiquen lo mismo y se llamen igual para todas las partes implicadas: gente de
business, diferentes equipos de desarrollo, e incluso dentro del propio código. A partir de ahí, sale
otra definición de Bounded Context:

**"_Un Bounded Context es un espacio delimitado donde un elemento del negocio tiene un significado perfectamente definido_"**

Es decir, es un espacio donde
hemos encontrado un lenguaje ubicuo. Veámos el significado de
esta afirmación con un ejemplo:


Existe una plataforma donde un usuario puede registrar actividades
deportivas o unirse a algunas previamente existentes. A su vez, puede abonar los costes de estas
actividades en la misma plataforma, por lo que harán falta una serie de datos:

<img class="special-img-class" src="/images/bounded-context/1st.png" />

Fijándonos en el concepto de Usuario, en algunas partes de
la plataforma un usuario va a estar perfectamente definido a partir de su nombre o su email,
como en la parte donde se crea una actividad o te puedes unir a una existente. Sin embargo, la
definición de usuario incluye una tarjeta de crédito si estamos hablando de abonar el coste de una actividad.


Hacen falta estos tres datos en nuestro modelo, pero en la parte de la aplicación donde se gestionan
las actividades, ¿entra la tarjeta de crédito en la definición de usuario? No, por lo que si tenemos
estas dos operaciones juntas, no estamos logrando tener un lenguaje ubicuo. Para lograrlo,
se va a dividir nuestro modelo en dos Bounded Contexts, donde cada uno tendrá una
implementación y una definición diferente de Usuario.

* **Contexto de Gestión de Actividades**
* **Contexto de Billing**

Así, se logra que todas las partes implicadas en el proyecto hablen lo mismo dentro de cada contexto.

<img class="special-img-class" src="/images/bounded-context/2nd.png" />

En este punto, a través del lenguaje ubicuo hemos encontrado una estrategia para identificar
diferentes Bounded Contexts dentro de nuestra aplicación, y lo hemos hecho teniendo en cuenta
los atributos que tiene un determinado concepto en las diferentes partes de la misma
(el concepto Usuario en nuestro ejemplo).

Algunos días más tarde, después de una pequeña charla con [Ronny](https://twitter.com/ronnyancorini) me
cambió un poco el _chip_ en cuanto a las propiedades de un concepto que se deben tener en cuenta para
filtrar. Como acabo de mencionar, en el ejemplo de arriba se ha conseguido tener un lenguaje
ubicuo teniendo en cuenta los atributos relevantes de un Usuario en cada parte de la aplicación, y se han identificado
dos Bounded Contexts a partir de ahí. Pero...¿queremos modelar nuestra clase Usuario teniendo en cuenta solamente atributos? Este enfoque nos puede llevar a un
anti-patrón de DDD: [un modelo anémico](https://www.martinfowler.com/bliki/AnemicDomainModel.html).

Para evitar esto, en lugar de tener en cuenta los atributos que tiene un concepto dentro de una
parte de nuestro negocio, es mejor tener en cuenta qué operaciones de negocio se llevan a cabo
en cada parte. Por tanto, en nuestro ejemplo, habrá un Bounded Context donde se hagan
operaciones como crear una actividad o unirse a ella (y se necesitarán un nombre o un email como
atributo) y otro contexto donde se harán operaciones como validación de datos de facturación (y
se necesitará una tarjeta de crédito como atributo). Teniendo en cuenta estas operaciones, creo que es
mejor manera de construir nuestro lenguaje y hacer esta separación.

En resumen, como podéis ver, a lo largo de este tiempo he ido evolucionando la visión que tenía de este concepto.
Seguramente esto seguirá pasando y me iré dando cuenta de otros detalles que ahora mismo se me
escapan o he interpretado mal. Por tanto, cualquier feedback es muy bienvenido!! :)
