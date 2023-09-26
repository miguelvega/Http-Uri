# Respuestas de la Actividad HTTP - URI
Del sitio web generador de palabras aleatorias, podemos ver una imagen que se mantiene estática, mientras que la palabra ubicada debajo de la imagen se renueva tras cargar la página.

<p align="center">
  <img src="https://github.com/miguelvega/CC3S2/assets/124398378/a355cc08-a88b-4480-b420-5256ae1b6da5" alt="Imagen 1" width="50%" />
</p>

<p align="center">
  <img src="https://github.com/miguelvega/CC3S2/assets/124398378/dfaba003-c277-4b32-8065-12204c9361e7" alt="Imagen 2" width="50%" />
</p>

## Curl
Instalamos curl con el comando `sudo apt install curl` 

![Captura de pantalla de 2023-09-24 19-23-09](https://github.com/miguelvega/CC3S2/assets/124398378/6a89fca4-5bd1-41ec-afb5-42a05c976432)

Usamos el comando curl  `curl 'http://randomword.saasbook.info'`  en la terminal y nos muestra el 
siguiente codigo html.

![Captura de pantalla de 2023-09-24 19-25-04](https://github.com/miguelvega/CC3S2/assets/124398378/814ecf89-91f3-4753-8cf4-b16f6499451a)

Luego, guardamos la salida del comando anterior en  `first_test_curl.html` 


![Captura de pantalla de 2023-09-24 19-31-19](https://github.com/miguelvega/CC3S2/assets/124398378/13828e81-608e-4e0a-9dd1-4dbfa26e9244)

![Captura de pantalla de 2023-09-24 19-31-33](https://github.com/miguelvega/CC3S2/assets/124398378/2d22fd0c-c4d4-42d4-af63-7063ae9f0b6d)


### Pregunta:¿Cuáles son las dos diferencias principales que has visto anteriormente y lo que ves en un navegador web 'normal'? ¿Qué explica estas diferencias?
La primera diferencia  es que al abrir el archivo html no se puede ver la imagen, esto es porque la solicitud curl solo devuelve el contenido html como respuesta mas no devuelve otros elementos como los css o imagenes, por eso no se reconce la imagen.<br>
Otra diferencia es que al cargarlo nuevamente, la palabra no experimenta cambios.

![Captura de pantalla de 2023-09-24 20-01-17](https://github.com/miguelvega/CC3S2/assets/124398378/4fd67f30-6837-4407-bc00-59c90bf09ddc)!

## Curl y Netcat

Usamos el comando `nc -l 8081` para escuchar por el puerto 8081 desde nuestro servidor falso.

![Captura de pantalla de 2023-09-24 20-50-01](https://github.com/miguelvega/CC3S2/assets/124398378/29404048-8ae3-46ad-8ada-5abafde49d24)

### Pregunta: Suponiendo que estás ejecutando curl desde otro shell ¿qué URL tendrás que pasarle a curl para intentar acceder a tu servidor falso y por qué?

Debemos usar el siguiente URL: `curl 'http://localhost:8081/'`. <br>
Porque dado que queremos hacer una solicitud http debemos poner http, ponemos localhost, ya que el servidor es nuestra computadora local, y 8081 que hace referencia al puerto en donde se encuentra el servidor falso.

![Captura de pantalla de 2023-09-24 20-51-45](https://github.com/miguelvega/CC3S2/assets/124398378/70ce795a-fdf9-467c-86b1-a894e4202ff6)

Una vez enviada la solicitud a nuestro servidor, podemos apreciar lo diguiente.
![Captura de pantalla de 2023-09-24 20-51-57](https://github.com/miguelvega/CC3S2/assets/124398378/949e3fb3-1cff-4317-a06e-9a20a506147c)

Así es como un servidor web real percibe una conexión desde curl.<br>
En la primera linea podemos ver el método de solicitud HTTP utilizado, en este caso, GET, a continuacion el recurso que se está solicitando, en este caso, la raíz del servidor web. "HTTP/1.1" que indica la versión del protocolo HTTP utilizada. En la segunda linea se aprecia al host que es localhost al que se esta enviando la solicitud en el puerto 8081. En la tercera linea, User-Agent que es curl que es el agente de usuario que está realizando la solicitud. Finalmente, tenemos Accept como un */* que significa que el cliente está dispuesto a aceptar cualquier 
tipo de contenido en respuesta del servidor.

### Pregunta: La primera línea de la solicitud identifica qué URL desea recuperar el cliente. ¿Por qué no ves http://localhost:8081 en ninguna parte de esa línea?
Debido a que servidor falso está aceptando la solicitud desde un puerto LOCAL, por lo tanto no es necesario incluir localhost.

Entonces, hemos visto una solicitud HTTP desde el punto de vista del servidor, ahora veamos cómo se ve la respuesta desde el punto de vista del cliente  luego de presionar enter en la shell del servidor.

![Captura de pantalla de 2023-09-24 21-15-13](https://github.com/miguelvega/CC3S2/assets/124398378/6a7b7127-49d0-4e46-aacb-3ca0199d3d43)


Luego, al colocar `curl -i 'http://randomword.saasbook.info/'`se obtiene lo siguiente :
![Captura de pantalla de 2023-09-24 23-41-22](https://github.com/miguelvega/CC3S2/assets/124398378/7c11f03a-543d-4ea2-badf-25e1effd5b95)

### Pregunta: Según los encabezados del servidor, ¿cuál es el código de respuesta HTTP del servidor que indica el estado de la solicitud del cliente y qué versión del protocolo HTTP utilizó el servidor para responder al cliente?
El código de respuesta es de 200 y el "OK" indica que la solicitud HTTP se completó correctamente y que el servidor ha entregado el recurso solicitado con éxito. La version de HTTP es la 1.1.

### Pregunta: Cualquier solicitud web determinada puede devolver una página HTML, una imagen u otros tipos de entidades. ¿Hay algo en los encabezados que crea que le dice al cliente cómo interpretar el resultado?.

Si, en la imagen anterior nos muestra la informacion que proporciona el encabezado para que el cliente interprete el resultado, por ejemplo, `Content-Type: text/html;charset=utf-8` indica el tipo de contenido que se está enviando como respuesta. En este caso, el contenido es de tipo "text/html", lo que significa que el servidor está enviando una página web HTML. También se especifica la codificación de caracteres UTF-8 utilizada para interpretar el contenido. 

## ¿Qué sucede cuando falla un HTTP request?

### Pregunta: ¿Cuál sería el código de respuesta del servidor si intentaras buscar una URL inexistente en el sitio generador de palabras aleatorias? Pruéba esto utilizando el procedimiento anterior.

Como se observa en la imagen de abajo, pusimos un link inexistente y no retorna un cuerpo html en la respuesta y en la primera linea del encabezado, devuelde el numero 404, este número es el código de estado HTTP que se devuelve en la respuesta. El código de estado "404" se conoce comúnmente como "Error 404" o "Not Found" (No encontrado). Indica que el recurso solicitado por el cliente no fue encontrado en el servidor.

![Captura de pantalla de 2023-09-25 01-14-03](https://github.com/miguelvega/CC3S2/assets/124398378/3a5dcda6-d2c1-4f4e-b02a-aaedb37404c4)


### ¿Qué otros códigos de error HTTP existen? Utiliza Wikipedia u otro recurso para conocer los significados de algunos de los más comunes: 200, 301, 302, 400, 404, 500. Ten en cuenta que estas son familias de estados: todos los estados 2xx significan funcionó, todos los 3xx son redireccionar etc.

200 (OK): Indica que la solicitud fue realizada con exito y si se encontró la información solicitada​

301 (Moved Permanently): indica que el host si ha sido capaz de comunicarse con el servidor pero que el recurso solicitado ha sido movido a otra dirección permanentemente.

302 (Found): se produce cuando el recurso solicitado ha sido trasladado temporalmente a una nueva ubicación.

400 (Bad Request): El error 400 solicitud incorrecta ocurre cuando el servidor no procesará la solicitud, porque no puede, o no debe, debido a algo que es percibido como un error del cliente (ej: solicitud malformada, sintaxis errónea, etc). La solicitud contiene sintaxis errónea y no debería repetirse.

404 (Not Found): Este error indica que el recurso solicitado no se encuentra en el servidor y por lo tanto no ha sido encontrada.​

500 (Internal Server Error): El error HTTP 500, indica un error interno en el servidor que impide que la solicitud se complete correctamente.

### Tanto el encabezado 4xx como el 5xx indican condiciones de error. ¿Cuál es la principal diferencia entre 4xx y 5xx?.
Los errores del tipo 4xx indican que el error se origina en el cliente, es decir, debido a un problema en la solicitud misma como por ejemplo, poner una url inexistente en la solicitud (votará error 404) en cambio los errores del tipo 5xx son errores del servidor, no del cliente, al momento de procesar la solicitud del cliente. Ocurren a menudo debido a problemas internos del servidor, como un fallo en una base de datos, una sobrecarga del servidor o una configuración incorrecta.

## ¿Qué es un cuerpo de Request?

Se creo formulario.html

![Captura de pantalla de 2023-09-25 01-59-25](https://github.com/miguelvega/CC3S2/assets/124398378/e0d3dcf8-9e30-48b6-988f-ed7e1c6a417b)

Lo abrimos en el navegador
![Captura de pantalla de 2023-09-25 02-00-10](https://github.com/miguelvega/CC3S2/assets/124398378/36d58143-97c4-4259-af42-762c2c548b2a)


### Pregunta: Cuando se envía un formulario HTML, se genera una solicitud HTTP POST desde el navegador. Para llegar a tu servidor falso, ¿con qué URL deberías reemplazar Url-servidor-falso en el archivo anterior?

Para llegar al servidor falso de debe cambiar dentro del archivo formulario.html el valor del campo action a `http://localhost:8081/` 
, luego abrimos el archivos en el navegador, colocamos nuestras credenciales y lo guardamos

![Captura de pantalla de 2023-09-25 03-09-14](https://github.com/miguelvega/CC3S2/assets/124398378/80764851-f64e-4389-a3b9-774aa07b7dbe)

Con lo cual tendriamos lo siguiente en la terminal al iniciar el falso servidor
![Captura de pantalla de 2023-09-25 03-09-24](https://github.com/miguelvega/CC3S2/assets/124398378/86937fd0-2bf2-4015-8c7e-128ffcb3c207)

Notamos que el servidor falso està escuchando y al cerrar la ventana en el browser cortamos la conexión.

### Pregunta:¿Cómo se presenta al servidor la información que ingresó en el formulario? ¿Qué tareas necesitaría realizar un framework SaaS como Sinatra o Rails para presentar esta información en un formato conveniente a una aplicación SaaS escrita, por ejemplo, en Ruby?

Se envía el formulario al servidor utilizando el método "post" y la acción `http://localhost:8081/`, con lo cual la información ingresada se envía al servidor en el cuerpo de la solicitud HTTP como datos codificados. Los datos del formulario se codifican en el cuerpo de la solicitud de acuerdo con el tipo de codificación que se especifica en el encabezado de la solicitud.<br>
Ahora bien, las tareas generales que un framework SaaS como Sinatra o Rails tendría que realizar para presentar esta información en una aplicación SaaS podrian
ser las siguientes.<br>
- Recibir la Solicitud: El servidor debe recibir la solicitud HTTP POST en la dirección especificada, que en este caso es `http://localhost:8081/`.
- Analizar la Solicitud: El framework debe examinar la solicitud entrante y extraer la información del formulario codificada en su cuerpo, que abarca los valores correspondientes a "email", "password" y "secret_info".
- Validación y Procesamiento: El framework debe realizar la validación de los datos según las reglas de la aplicación, como verificar si se proporcionó una dirección de correo electrónico válida, verificar la contraseña, etc.
- Acceso a la Base de Datos: Si la aplicación requiere acceso a una base de datos para autenticar al usuario, el framework puede interactuar con la base de datos para verificar las credenciales del usuario.
- Ejecución de Lógica de Negocio: Dependiendo de la lógica de negocio de la aplicación, el framework puede realizar diversas acciones, como autenticar al usuario, redirigirlo a páginas específicas, mostrar mensajes de error, etc.
- Generar una Respuesta: En última instancia, el framework debe generar una respuesta HTTP, que generalmente incluirá una página HTML o una redirección a otra página, dependiendo del resultado de la autenticación.

Ruby on Rails ofrece estructuras y pautas particulares diseñadas para la gestión de formularios y el procesamiento de datos provenientes de formularios HTM.<br>
Rails utiliza controladores y modelos para gestionar la lógica de negocio y acceder a la base de datos de manera eficiente. Sin embargo, la implementación exacta puede variar según la aplicación y los requisitos específicos de la misma. 

<br>

### Segùn el experimento anterior, repita varias veces para responder las siguientes preguntas observando las diferencias en el resultado impreso por nc:

#### ¿Cuál es el efecto de agregar parámetros URI adicionales como parte de la ruta POST?
Al agregar parámetros URI adicionales como parte de la ruta POST no tendran ningún efecto, pues simplementes se considerarán parte de la URL, pero no tendrán impacto en los datos del formulario que se envían al servidor, ya que cuando se envía un formulario HTML utilizando POST, los datos del formulario se envían en el cuerpo de la solicitud HTTP.Por tal motivo, los parámetros en la URI generalmente se utilizan en solicitudes GET para transmitir información en la URL.

#### ¿Cuál es el efecto de cambiar las propiedades de nombre de los campos del formulario?

Como se puede apreciar en el formulario visto anetorio, se ha cambiado los nombres de los campos de la siguiente manera:
- name="correo" en lugar de name="email"
- name="contrasenia" en lugar de name="password"

![Captura de pantalla de 2023-09-26 02-15-13](https://github.com/miguelvega/Http-Uri/assets/124398378/5f06d77b-fca8-4118-81c2-653cf9425e59)

Con lo cual el servidor falso recibe los datos de la siguiente manera.

![Captura de pantalla de 2023-09-26 02-17-03](https://github.com/miguelvega/Http-Uri/assets/124398378/9b61ee8f-ce68-4381-a764-af55157e8152)

El efecto de cambiar el nombre de las propiedades del formulario alterará cómo se etiquetan los datos cuando se envían al servidor. Si cambias los nombres de los campos, el servidor recibirá los datos con los nuevos nombres. Esto puede afectar la capacidad del servidor para procesar los datos si se espera que los nombres de los campos coincidan con ciertos nombres en el lado del servidor.

#### ¿Puedes tener más de un botón Submit? Si es así, ¿cómo sabe el servidor en cuál se hizo clic? (Sugerencia: experimenta con los atributos de la etiqueta <submit>).

Agregamos un nuevo botón Submit en el archivo `formulario.html`
![Captura de pantalla de 2023-09-26 02-34-53](https://github.com/miguelvega/Http-Uri/assets/124398378/42555ab6-ff2b-4da5-b3ec-778f8117186b)

Notamos lo siguiente en el lado del servidor
![Captura de pantalla de 2023-09-26 02-35-56](https://github.com/miguelvega/Http-Uri/assets/124398378/7d1e16bb-b479-4a25-acb4-f2322df7879e)

Podemos tener mas de un botón Submit en un formulario HTML. Para distinguir cuál se hizo clic, puedes usar el atributo name en los botones. En nuestro caso, tenemos dos botones con name="login" y name="register" y verificar en el lado del servidor cuál de ellos se incluyó en la solicitud para determinar la acción a realizar, como se puede apreciar en la imagen anterior `correo=miguelon%40gmail.com&contrasenia=1928&secret_info=secret_value&register=Register` se hizo clic en el boton register.

#### ¿Se puede enviar el formulario mediante GET en lugar de POST? En caso afirmativo, ¿cuál es la diferencia en cómo el servidor ve esas solicitudes?


#### ¿Qué otros verbos HTTP son posibles en la ruta de envío del formulario? ¿Puedes hacer que el navegador web genere una ruta que utilice PUT, PATCH o DELETE?.
Normalmente, los navegadores web solo admiten los verbos HTTP GET y POST al enviar formularios. Estos verbos corresponden a las operaciones de recuperación y envío de datos, respectivamente. Para utilizar los verbos HTTP PUT, PATCH o DELETE, generalmente necesitarías utilizar JavaScript o algún otro enfoque más avanzado, ya que los navegadores no generan solicitudes directas con estos verbos al enviar formularios HTML de manera estándar


## HTTP sin estados y cookies




### Pregunta: Prueba las dos primeras operaciones GET anteriores. El cuerpo de la respuesta para la primera debe ser "Logged in: false" y para la segunda "Login cookie set". ¿Cuáles son las diferencias en los encabezados de respuesta que indican que la segunda operación está configurando una cookie? (Sugerencia: usa curl -v, que mostrará tanto los encabezados de solicitud como los encabezados y el cuerpo de la respuesta, junto con otra información de depuración. curl --help imprimirá una ayuda voluminosa para usar cURL y man curl mostrará la página del manual de Unix para cURL en la mayoría de los sistemas.)

Primero, entramos a la siguiente pagina web `http://esaas-cookie-demo.herokuapp.com` en el navegador y vemos lo siguiente.
![Captura de pantalla de 2023-09-25 12-20-21](https://github.com/miguelvega/Http-Uri/assets/124398378/8a80b29a-f097-4b4c-89b9-5a0a003f9c80)

La diferencia se debe a que al aplicar el comando `curl -v 'http://esaas-cookie-demo.herokuapp.com'` no aparece el campo Set-Cookie en la salida, en cambio al aplicar el comando  `curl -v 'http://esaas-cookie-demo.herokuapp.com/login'`si aparece, como se puede ver en las dos siguientes imagenes.
![Captura de pantalla de 2023-09-25 12-19-51](https://github.com/miguelvega/Http-Uri/assets/124398378/e5c53b4d-cb2d-42a8-ab67-cc2a48227b10)

![Captura de pantalla de 2023-09-25 12-20-07](https://github.com/miguelvega/Http-Uri/assets/124398378/c5c268c0-4606-4017-89d3-d54a5f1973cc)



