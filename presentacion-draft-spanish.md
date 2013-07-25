Un protocolo Real Time.

HTTP no es un protocolo Real Time. Cuando querés recibir muchos mensajes rápidamente, por ejemplo lo que está pasando en Twitter o Facebook, no querés tener que pedir cada 5 segundos para actualizar: querés verlos cuando están ahí. Para que HTTP te permita mandar muchos mensajes seguidos a medida que van llegando, tenés que resignarte a toda clase de estrategias para las que HTTP no fue diseñado, como colgar las conexiones de respuesta e ir mandando mensajes de a poco. Los desarrolladores pasan más tiempo eludiendo las restricciones que la herramienta provee que creando nuevas experiencias. Eso tiene que cambiar.

HTTP no es en tiempo real, tampoco, porque cuando solicitás información de un servicio remoto, tenés que esperar a que el servicio remoto responda la totalidad de la información antes de seguir procesando. No podés "enviar y olvidarte". Cada vez que hacés algo, tenés que esperar.

HTTP no es real time tampoco porque cada vez que querés cargar algo, tenés que crear una conexión nueva. Los navegadores se volvieron muy eficientes en reutilizar las conexiones, pero como desarrollador no tenés control sobre si las conexiones se reutilizan o no, y definitivamente las implementaciones de HTTP para lado servidor varían salvajemente en la optimización de consumo de red.

A través de REST, HTTP provee una interfaz muy aceptable para hacer consultas a bases de datos y arquitecturas para acceso a recursos: pero HTTP es lento, tan lento que en la práctica resulta bastantes veces 

JSTP sí es Real Time.
JSTP es un protocolo que estamos haciendo desde hace unos meses que nos sirve para comunicar las diferentes partes de nuestro sistema.

Lo que necesitabamos era tener un protocolo asincrónico que se aguante bastante carga, que sea rápido y escalable.

Para eso utilizamos sockets y websockets.

JSTP es un protocolo asincrónico: eso quiere decir que por cada paquete que le mandás al servidor, el servidor te puede mandar cualquier cantidad de paquetes. O sea también que el servidor puede responder con mensajes en cualquier momento.


Los mensajes son JSON.

JSTP es rápido porque las conexiones no se cierran. Es rápido también porque JSON es muy fácil de procesar y los paquetes son chiquitos.

A lo largo del tiempo fuimos implementando JSTP en diferentes herramientas, como por ejemplo MongoDB y Twitter. Esto nos sirve para poder tener solo una instancia de una base de datos o de un feed corriendo en otro servidor y consultarla desde las diferentes aplicaciones que estemos desarrollando. 

Tenemos la idea de que si vemos que alguna instancia está saturada la podamos duplicar y el sistema siga andando perfectamente.

JSTP necesitaba una semántica, o sea, una forma de describir en los paquetes que querés que el servidor haga. Para eso usamos la semántica de REST: JSTP es casi 100% compatible con REST, y si sos un desarrollador Web estás super habituado a la forma de trabajo.

JSTP tiene algo curioso que es la posibilidad de bindearse a eventos tanto en el propio sistema como en un servidor remoto. Lo que te permite esto es enviar un paquete una sola vez, generalmente cuando querés iniciar el servicio, y recibir a través del tiempo información relevante, como por ejemplo el stream de tweets en vivo de un hashtag de Twitter.

----------

Ya que somos de Argentina a todas nuestras implementaciones de JSTP les ponemos nombres en español que tengan que ver con el nombre de la aplicación o con lo que representa: por ejemplo, el pajarito de Twitter es "pájaro" y "monje" es una contracción de Mon-goDB y J-STP. 

La implementación más completa que hicimos de JSTP está desarrollada en JavaScript tanto para Node.js como para el browser.

Una de las mejores cosas de JSTP es que el browser usa exactamente el mismo protocolo que todo el resto del sistema. Si necesitás que el servidor se conecte con una base de datos, puede hacerlo en JSTP: si necesitás que el browser se comunique asincrónicamente con el servidor, puede hacerlo en JSTP. Incluso el browser se puede comunicar directamente con la base de datos usando JSTP (aunque no es muy recomendable por temas de seguridad).


Nosotros usamos JSTP en todo lo que hacemos. Vamos desarrollando más o más herramientas a medida que pasa el tiempo. Más allá de la implementación para Node.js, JSTP es una especificación, porque queremos construirlo como un estándar abierto. De hecho, la especificación de JSTP la podés encontrar en github en un repositorio público. Todo lo relativo a JSTP está público en github; podés hacer verlos, mejor aún podés crear issues, mejor aún podés forkearlos y mandarlos un pull request con mejor, y mejor aún podés crear tu propia implementación de JSTP para la plataforma que quieras.

Con JSTP es muy fácil hacer que todo se actualice a la vez. Aplicaciones para Android o iOS, o para Web, todo puede sincronizarse en tiempo real, y desarrollarlo es una pavada, esa era nuestra meta cuando pensamos jstp.

JSTP es escalable porque es simétrico. Eso quiere decir que cualquier nodo de JSTP se puede comportar como cliente o como servidor. Casi todos los mensajes de JSTP son idénticos: no hay preguntas y respuestas. Eso hace que sea posible distribuir paquetes por una red muy grande de computadoras, y que sea posible hacerlo asincrónicamente, sin esperar respuesta.

La idea es que JSTP sirva para comunicar cualquier cosa. Bases de datos, APIs de redes sociales, robots en Marte y hasta la cafetera bluetooth. Lo que sea.

La especificación de JSTP (que va por la versión 0.5) ya está bastante avanzada. La verdad es que cumple bastante con nuestras espectativas, y después de usarlo mucho tiempo ya casi no encontramos ampliaciones o correcciones para hacerle. Ahora empezamos una nueva etapa: vamos a hacer JSTP sirva para que la red de computadoras que forman una aplicación pueda escalar sola orgánicamente. Para eso estamos trabajando en dos cosas:

1. Cada motor JSTP va a ser capaz de reportar el estado de saturación de la computadora en la que está corriendo. Por ejemplo, si un motor de JSTP funciona como base de datos, podría reportar a las aplicaciones que usan la base de datos que hay poco espacio en disco. Si la computadora llega a un punto crítico de falta de espacio, la idea es que se añada otra instancia de base de datos JSTP y las aplicaciones puedan balancearse automáticamente entre ambas. Las métricas que los motores van a poder reportar serían espacio en disco, espacio en memoria RAM, estado de saturación del procesador y consumo de red.

2. Una extensión de seguridad para JSTP con encriptación de clave pública. Queremos lograr que cada motor de JSTP pueda mandar mensajes seguros que solamente pueda desencriptar el verdadero destinatario, no importa por cuantos motores haya pasado antes. Además queremos que sea posible identificar al autor de los mensajes para evitar que se pueda falsificar mensajes en una infraestructura abierta.