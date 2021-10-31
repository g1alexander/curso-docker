# Imagenes

Una imagen es una pieza de software liviana que contiene todo lo necesario para que un contenedor pueda ejecutarse exitosamente.

Una analogia para que podamos entender mejor el concepto de las imagenes es la POO, en programacion orientada a objeto tenemos lo que son Objectos o instanacias que son creados apartir de una clase o plantilla

En ese orden de ideas una **imagen** vendria siendo una clase de la cual saldran **contenedores** (nuestro objectos en este ejemplo)

## Profundizando en el concepto de imágen

Es bueno profundizar un poco más en el concepto de una imágen en Docker para entender su función, para posteriormente poder realizar una por nuestra cuenta desde 0, cuando no haya una imágen que cumpla con nuestras necesidades.

Una imágen contiene distintas capas de datos (distribución, diferente software, librerías y personalización).

Podemos llegar a la conslusión, que una imágen se conforma de distintas capas de personalización, en base a una capa inicial (base image), la dicha capa, es el más puro estado del SO.

La siguiente ilustración nos mostraría la representación gráfica, del concepto de una imágen en Docker.

<img src="https://static.packt-cdn.com/products/9781788992329/graphics/0ee3d4cf-2133-4143-a7c4-690274483841.png" >

Si observamos, partimos desde la base del SO, y vamos agregando capas de personalización hasta obtener la imágen que necesitamos:

distribución debian

- se agrega el editor emacs
- se agrega el servidor Apache
- se agregan los permisos de escritura para la carpeta /var/www de Apache

  Hay que tener en cuenta, que todo parte del Kernel de Linux, en caso de utilizar alguna distrubución de Linux
  .

Historico de una imágen
Podemos observar la historia de nuestra imágen, con el siguiente comando

```bash
docker history [imagen]
```

De esta manera podemos ver las capas de personalización que fuerón agregadas, para la construcción de la imágen que conocemos.

**comandos adicionales**

```bash
# mirar que imagenes tenemos
docker images

# bajar una imagen
docker pull [imagen]

#tambien podemos bajar una imagen con version especifica
docker pull [imagen]:[tag_version]

#eliminar todas las imagenes
docker image prune -a

#eliminar una imagen
docker image rm -f {id_image}
```

---

## Creando una imagen

```bash
#Se crea un directorio:
mkdir images
```

```bash
#se accede al directorio:
cd imagenes
```

```bash
#se crea un archivo llamado Dockerfile
touch Dockerfile
```

```docker
#Contenido del Dockerfile:
FROM ubuntu:latest
RUN touch /ust/src/hola-platzi.txt # este comando se ejecutará en tiempo de build
```

```bash
#Se crea una imágen, se le pasa el contexto de build
docker build -t ubuntu:platzi
```

```bash
#Iniciamos una conexión por terminal al contenedor de ubuntu
docker run -it ubuntu:platzi
```

```bash
#para loguearse en docker hub, se ejecuta lo siguiente y se ponen las claves correspondientes
docker login
```

```bash
#Para poder publicar nuestro contenedor, es necesario cambiar el tag ubuntu, dado que el mismo ya existe como un contenedor oficial y no tenemos permisos para modificarlo.
docker tag ubuntu:platzi miusuario/ubuntu:platzy
```

```bash
#Una vez hecho el cambio ya podremos subirlo a una cuenta de docker hub

#comando para hacer la publicación de nuestra imágen en docker hub

docker push miusuario/ubuntu:platzi
```

Capas de una imagen

<img src="https://static.platzi.com/media/user_upload/Screenshot%20from%202020-11-06%2019-53-30-a305c998-0991-44ad-9319-80cacb1a4bc7.jpg" alt="">

Retaggeo de una imagen

<img src="https://i.ibb.co/JBL946b/Screenshot-at-Feb-05-15-26-18.png" alt="">

---

## Practicas de documenetacion oficial de docker

- [Link](https://docs.docker.com/get-started/)

---

## Sistema de capas en una imagen

La importancia de entender el sistema de capas consiste en la optimización de la construcción del contenedor para reducir espacio ya que cada comando en el dockerfile crea una capa extra de código en la imagen.

Estas capas son inmutables y con esto docker se asegura de que cuando se construya una nueva imagen si ya esta basada en una capa que ya existe la reutiliza, optimizando espacio

Los contras de esto es que cada comando que se coloca en el **Dockerfile** es una capa nueva de una imagen, asi que debemos tener un control a la ahora de correr los comandos en Dockerfile

Una herramienta super util para visualizar las capas que hay en una imagen es [**Dive**](https://github.com/wagoodman/dive)
