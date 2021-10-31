# Docker avanzado

## Administrando ambiente de Docker

Es muy importante tener limpio nuestro ambiente de trabajo en docker, esto porque a medida de que vamos desarrollando podemos acumular demasiadas imagenes, volumenes, contenedores, redes, etc... esto hace que nuestra maquina se pueda quedar sin espacios y recursos para funcionar

```bash
# Eliminar todos los contenedores que no estan corriendo o apagados
docker container prune
```

```bash
# Eliminar todos los contenedores incluyendo los que estan corriendo (-q: muestra ID, -a: todos, -f: fuerza bruta)
docker rm -f $(docker ps -aq)
```

```bash
# Listar redes
docker network ls
```

```bash
# Eliminar las redes que no estamos usando
docker network prune
```

```bash
# Listar volumenes
docker volume ls
```

```bash
# Eliminar los volumenes que no estamos usando
docker volume prune
```

```bash
# Eliminar todos los contenedores, imagenes, redes y cache
docker system prune
```

```bash
# eliminar todas las imagenes
docker rmi -f $(docker images -a -q)
```

```bash
# Limito el uso de memoria
docker run -d --name app --memory 1g platziapp
```

```bash
# Ver cuantos recursos consume docker en mi sistema
docker stats
```

```bash
# Ver si el proceso muere por falta de recursos
docker inspect app
```

```bash
#Muestra el último proceso ejecutado
docker ps -l
```

```bash
#Envía la señal SIGKILL al contenedor
docker kill <nombre contenedor>
```

```bash
#Muestra los procesos del contenedor
docker exec <nombre contenedor> ps -ef
```

---

## Deteniendo contenedores correctamente: SHELL vs. EXEC

Al estar activo o recién crear un contendor, este debería estar ejecutando un proceso principal o main process para mantenerse en funcionamiento. En caso de que el main process se detenga el contendor debería dejar de funcionar.

Docker tiene una manera de manejar los procesos de los contenedores de manera estándar, cuando se manda detener el contenedor con la instrucción
docker stop <name-container>; con esta instrucción Docker manda una señal estándar de linux llamada SIGTERM al proceso; después de un tiempo el proceso deberá detenerse; en caso de no detenerse el proceso enviará otra señal llamada SIGKILL; con esta señal se garantiza que el proceso se ha terminado de manera forzada.

Existen problemas en la terminación de procesos ejecutados en los contenedores, una de las causas está asociado a ¿Cómo se debería declarar en Dockerfile el proceso a ejecutar?

Shell: Ejecuta el proceso como hijo del shell

```bash
FROM ubuntu:trusty
COPY ["loop.sh", "/"]
CMD /loop.sh
```

El primero es el dockerfile; sirve para construir nuestra imagen y el segundo es el archivo bash que se ejecutará como un main process o proceso principal del contenedor. Es importante destacar que la línea de CMD ejecuta nuestro archivo bash, a través de un shell, esta forma de expresar esta línea se conoce como shell form y esto genera que al momento de detener un contenedor que esté ejecutando un proceso por shell form necesariamente tendrá que usar un SIGKILL. Para evitar esto, el CMD debe ejecutar nuestro main process de form exec form , es decir nuestro archivo debe estar configurado de esta manera.

Exec: Ejecuta el comando como principal

```bash
FROM ubuntu:trusty
COPY ["loop.sh", "/"]
CMD ["/loop.sh"]
```

La idea de que el archivo esté configurado de esta manera es que de tiempo a que el contenedor procese las solicitudes restantes y pueda hacer un Great full shutdown. Otra nota importante es que los códigos de salida mayor a 128 es el resultado de una salida por un código de excepción.

---

## Contenedores ejecutables: ENTRYPOINT vs CMD

El ENTRYPOINT es el comando que se va a ejecutar por defecto, y en CMD va el parametro del comando.
Este parametro se puede modificar desde el run del contenedor.

```docker
FROM ubuntu:trusty
# CMD ["/bin/ping", "-c", "3", "localhost"]

ENTRYPOINT [ "/bin/ping", "-c", "3"]
CMD ["localhost"]
```

NOTA: [Best practices for writing Dockerfiles.](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

---

## El contexto de build

Al igual que **.gitignore** en git, en Docker existe el **.dockerignore** que lista archivos o carpetas que se deben ignorar al momento de tomar en cuenta el contexto de build para construir una imagen.

Tambien el contexto del build se puede sobreescribir para un contenedor en el docker-compose en caso tal que quieran hacer solamente docker-compose run, y el mismo tambien tomara en cuenta lo especificado en el dockerignore.

Esto es muy importante tenerlo en cuenta, debido a que hay ciertas cosas que no queremos colocar en nuestras imagenes, si copiamos todo el contexto de build corremos el riesgo de:

- nuestra imagen sea muy pesada
- pueden resultar fallas del contenedor porque se pisan archivos

## Multi-stage build

El Dockerfile de producción contiene 2 “fases de build” que se pueden pensar como hacer 2 build seguidos, en donde al final la imagen construida contendrá lo especificado en el ultimo de los build.

El primer build corre 1 test que verifica que todo funcione bien
El segundo build construye la imagen final aprovechando el caché de las capas del primer build.

Al final el 2do build es solo una extracción de lo que nos interesa del primer build.

Lo importante en este caso especifico es que si el test falla, entonces el build 2 no se corre, lo que significa que la imagen no se construye

```docker
# Define una "stage" o fase llamada builder accesible para la siguiente fase
FROM node:12 as builder
# copiamos solo los archivos necesarios
COPY ["package.json", "package-lock.json", "/usr/src/"]

WORKDIR /usr/src
# Instalamos solo las dependencias para Pro definidas en package.json
RUN npm install --only=production

COPY [".", "/usr/src/"]
# instalamos dependencias de desarrollo
RUN npm install --only=development

# Pasamos los tests
RUN npm run test
## Esta imagen esta creada solo para pasar los tests.

# Productive image
FROM node:12

COPY ["package.json", "package-lock.json", "/usr/src/"]

WORKDIR /usr/src
# instar las dependencias de PRO
RUN npm install --only=production

# Copiar  el fichero de la imagen anterior.
# De cada stage se reutilizan las capas que son iguales.
COPY --from=builder ["/usr/src/index.js", "/usr/src/"]
# Pone accesible el puerto
EXPOSE 3000

CMD ["node", "index.js"]
### En tiempo de build en caso de que algún paso falle, el build se detendrá por completo. por ejemplo si un test falla no se crea la imagen
```

Con este production.Dockerfile podemos tener una especie de integracion continua debido a que los test tienen que pasar para que la imagen se cree

---

## Docker in Docker

Existe la Posibilidad de usar Docker desde otros contenedores, se logra usando el Docker socket con bind mount se accede a el archivo docker sock a la maquina anfitriona y accediendo a el desde el otro Docker el cliente puede accederlo puede hablarle directamente.

Si queremos tener docker dentro de un contenedor, mas llamado docker-in-docker. Compartiendo el socket de nuestro local a nuestro contenedor que tendra docker.

```bash
docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock docker:19.03.12

docker ps

docker run -d --name app prodapp

#Comprobamos desde otra terminal

docker ps

# Correr dive siendo un contenedor que explora el estado de docker.

docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/bin/docker wagoodman/dive:latest prodapp
```

<img src="https://static.platzi.com/media/user_upload/image%20%282%29-6d647f06-ea90-4953-a425-0cfd7dc55933.jpg" alt="">
