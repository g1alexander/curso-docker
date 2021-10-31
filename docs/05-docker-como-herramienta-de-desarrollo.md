# Docker como herramienta de trabajo

Esta es practica estaremos utilizando una api de node para practicar con docker

```bash
# la api la sacamos del repositorio de platzi
git clone https://github.com/platzi/docker
```

```bash
#ingresamos al directorio y ejecutamos
docker build -t platziapp . (creo la imagen local)
```

```bash
docker image ls (listo las imagenes locales)
```

```bash
docker run --rm -p 3000:3000 platziapp (creo el contenedor y cuando se detenga se borra, lo publica el puerto 3000)
```

```bash
docker ps (veo los contenedores activos)
```

---

# Separar capas

siempre que nuestra aplicaion tenga dependencias independientemente de la tecnologia (node: npm, python: pip, php:composer) podemos serar en dos capas la intalacion de las dependencias del codigo de la aplicaion per se

Esto es muy util porque evita que tengamos que construir la imagen cada vez que tengamos cambios nuevos

```docker
# imagen base (node en version 12)
FROM node:12

# copia todo lo que hay en la carpeta actual
# a la carpeta /usr/src/
COPY [".", "/usr/src/"]

# establece el directorio de trabajo
WORKDIR /usr/src

# instala la dependencias de la aplicacion
RUN npm install

# expone el puerto del contenedor
EXPOSE 3000

# cuando el contendor se ejecuta lo hace bajo el comando node index.js
CMD ["node", "index.js"]
```

Si analizamos este codigo nos daremos cuenta que cada vez que hagamos un cambio por minimo que sea en los archivos este afectara a la capa 2 (**COPY [".", "/usr/src/"]**) y esto implica que ejecutara todo de ahi para abajo.

Para evitar esto podemos estructurar mejor nuestro Dockerfile de la siguiente manera:

```docker
FROM node:14

COPY ["package*.json", "/usr/src/"]

WORKDIR /usr/src

RUN npm install

COPY [".", "/usr/src/"]

EXPOSE 3000

CMD ["npm", "start"]
```

Los archivos **package** es el corazon de una aplicacion de javascript, en este orden de ideas es lo que necesitamos que si o si este en nuestro contendor, debido a que con este sabemos que dependencias necesitamos y que no

si una vez que ya esta montada la imagen y no hay modificaciones en los package es evita que por cada build se ejecute el **npm install**, porque en el build se saltara de una a la capa **COPY [".", "/usr/src/"]** porque es donde vamos a estar modificando constantemente el codigo

---

```bash
#--rm cuando el contenedor se apaga este se elimina
#-p el puerto en el que quiero escuchar en mi maquina apuntando al puerto del contenedor
#-v intercambiar datos atraves de bind-mount
docker run --rm -p 3000:3000 -v $(pwd)/index.js:/usr/src/index.js [imagen]
```

---

# Networks para comunicacion entre contenedores

Docker nos permite tener redes y con ellas asignarle estas a nuestros contenedores, esto es muy practicado porque nos permite tener 2 o mas contenedores conectados a la misma red y asi por ejemplo en un contenedor tener nuestra api y en otro nuestra base de datos y todo estaria funcionando sin ningun incoveniente

esto da paso que si tenemos esto configurado es como si nuestra aplicacion estuviera subida en un servidor en la nube, pero estando todo similado en contenedores de docker

```bash
#listo las redes
docker network ls
```

```bash
#creo la red
docker network create --attachable plazinet
```

```bash
#veo toda la definición de la red creada
docker inspect plazinet
```

```bash
#creo el contenedor de la BBDD
docker run -d --name db mongo
```

```bash
#conecto el contenedor “db” a la red “platzinet”
docker network connect plazinet db
```

```bash
#corro el contenedor “app” y le paso una variable
docker run -d --name app -p 3000:3000 --env MONGO_URL=mondodb://db:27017/test platzi
```

```bash
#conecto el contenedor “app” a la red “plazinet”
docker network connect plazinet app
```
