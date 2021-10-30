# Contenedores

**NOTA:** listado de comando para utilizar con docker: [link](https://collectednotes.com/barckcode/docker-cheat-sheet)

## ¿Qué es un contenedor?

Un contendor afines practicos es con si fuera una maquina virtual aunque en si no los es.

La maquina virtual es un OS operativo aislado en el cual tiene su propia memoria RAM y sus propios discos

Un **contenedor de docker** corre bajo la maquina anfitriona, o sea tu computadora como tal y esto nos permite que sea mucho mas liviano de correr. Puntos a considerar:

- el contenedor esta de manera aislada de la maquina anfitriona, esto hace que seguro, debido a que no tendra acceso a toda la informacion de la maquina

- Un contendor se puede hacer la analogia de una persona que vive en una burbuja, debido a que a traves de le podemos mandar instruciones al contendor para por ejemplo limitar el uso de la ram:

  - tienes 8 gb de ram, pero tu quieres que el contendor en el que trabajas solo alcance un limite de 2 gb, esto se puede hacer

  por medio de configuraciones como estas podemos limitar y de que manera se va a compartar un contendor y este contenedor **creera** que ese es el limite de la maquina anfitriona y asi de esa manera se va a comportar que cualquier otra maquina donde ejecutemos el contendor

---

## Comandos

```bash
docker run hello-world (corro el contenedor hello-world)
```

```bash
docker ps (muestra los contenedores activos)
```

```bash
docker ps -a (muestra todos los contenedores)
```

```bash
docker inspect <containe ID> (muestra el detalle completo de un contenedor)
```

```bash
docker inspect <name> (igual que el anterior pero invocado con el nombre)
```

```bash
docker run –-name hello-platzi hello-world (le asigno un nombre custom “hello-platzi”)
```

```bash
docker rename hello-platzi hola-platzy (cambio el nombre de hello-platzi a hola-platzi)
```

```bash
docker rm <ID o nombre> (borro un contenedor)
```

```bash
docker container prune (borro todos lo contenedores que esten parados)
```

¿Como correr Ubuntu con docker?

```bash
docker run ubuntu (corre un ubuntu pero lo deja apagado)
```

```bash
docker run -it ubuntu (lo corre y entro al shell de ubuntu)
-i: interactivo
-t: abre la consola

```

```bash
cat /etc/lsb-release (veo la versión de Linux)
```

---

## Ciclo de vida de un contenedor

Cada vez que un contendor se ejecuta, en realidad lo que ejecuta es un proceso del sistema operativo. Este proceso se le conoce como Main process.

Main process
Determina la vida del contenedor, un contendor corre siempre y cuando su proceso principal este corriendo.

Sub process
Un contenedor puede tener o lanzar procesos alternos al main process, si estos fallan el contenedor va a seguir encedido a menos que falle el main.

Ejemplos manejados en el video

Batch como Main process
Agujero negro (/dev/null) como Main process

```bash
docker run --name alwaysup -d ubuntu tail -f /dev/null
```

_el ouput que te regresa es el ID del contentedor _

Te puedes conectar al contenedor y hacer cosas dentro del él con el siguiente comando (sub proceso)

```bash
docker exec -it alwaysup bash
```

Se puede matar un Main process desde afuera del contenedor, esto se logra conociendo el id del proceso principal del contenedor que se tiene en la maquina. Para saberlo se ejecuta los siguientes comandos;

```bash
docker inspect --format '{{.State.Pid}}' alwaysup
```

_El output del comando es el process ID (2474) _

Para matar el proceso principal del contenedor desde afuera se ejecuta el siguiente comando (solo funciona en linux)

```bash
sudo kill -9 2474
```

Otras alternativas para matar el **main process** del contenedor es con el comando:

```bash
docker stop <container_id or container_name>
-
or
-
docker kill <container_id or container_name>
```

## exponiendo un contenedor a la maquina anfitriona

```bash
# corro un contenedor de nombre proxy
docker run -d --name <name_contenedor> nginx
```

```bash
# apagar el contendor
docker stop <name_contenedor>
```

```bash
# elimina un contenedor
docker rm <name_contenedor>
```

```bash
# para y elimina un contenedor
docker rm -f <name_contenedor>
```

```bash
# corre un servidor y lo expone el puerto 80 al 8080 de mi maquina
docker run -d --name <name_contenedor> -p 8080:80 nginx
```

```bash
# ver los logs (seguimiento de las peticiones al servidor)
docker logs <name_contenedor>
```

```bash
# ver los logs con seguimiento (seguimiento de las peticiones al servidor)
docker logs -f <name_contenedor>
```

```bash
# ver los ultimos 10 logs
docker logs --tail 10 -f <name_contenedor>
```
