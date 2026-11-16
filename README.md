## Preguntas

1. ¿Por qué el frontend no necesita conectarse a PostgreSQL?
Porque el frontend solo enseña la página al usuario. Cuando necesita datos se los pide al backend, y es el backend quien habla con la base de datos. Por eso el frontend ni siquiera está en la red de la base de datos.

2. ¿Por qué el backend pertenece a dos redes?
Porque tiene que hablar con dos cosas distintas: con el frontend (red "frontend") y con PostgreSQL (red "datos"). Es el único que está en medio de los dos.

3. ¿Qué servicio necesita publicar un puerto en el anfitrión?
El frontend, porque es lo que se abre desde el navegador (puerto 8080). El backend y la base de datos no publican nada, solo se hablan por dentro.

4. ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?
Porque los contenedores ya se hablan por la red interna de Docker, así que abrir un puerto al exterior no arregla el fallo. Además deja la base de datos expuesta y es un riesgo de seguridad. Lo correcto es mirar las redes, el nombre del servicio y las variables.

5. ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?
Que todo queda en un único fichero y se levanta con un solo comando ("docker compose up -d"). No hay que escribir tres "docker run" largos ni acordarse de las redes, los volúmenes y las variables. También se arranca en el orden correcto (la base de datos primero) y se puede repetir igual en otro ordenador.

6. ¿Por qué hemos dividido la arquitectura en dos redes?
Para separar y tener más seguridad. La red "frontend" conecta el frontend con el backend, y la red "datos" conecta el backend con la base de datos. Así el frontend no puede llegar a PostgreSQL directamente.

7. ¿Por qué el backend utiliza "db" como dirección de PostgreSQL?
Porque "db" es el nombre del servicio en el compose y Docker lo convierte en la IP del contenedor con su DNS interno. Así no hace falta saber la IP, que puede cambiar al reiniciar.

8. ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?
Con "docker compose down" se paran y se quitan los contenedores, pero el volumen "postgres_data" sigue ahí y los datos no se pierden. Con "docker compose down -v" se borran también los volúmenes y, con ellos, todos los datos de la base de datos.

9. ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?
Se perderían todas las incidencias guardadas y el historial, y la aplicación quedaría vacía. Si no hay copia de seguridad no se podrían recuperar, y la empresa perdería información y tiempo de trabajo. Por eso hay que hacer copias de seguridad.
