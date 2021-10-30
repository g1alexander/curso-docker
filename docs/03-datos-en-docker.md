# Manejo de Datos con Docker

## Bind mounts

Es un mecanismo por el cual podemos darle acceso a nuestro disco a un contendor y consiste en que le damos una ruta en la cual le damos acceso de lectura y escritura.

Con esto conseguimos que todo lo que hagamos en el contendor tambien se vea reflejado en nuestra maquina y viceversa. Esto tiene sus riegos, debido a que el contendor tiene acceso a nuestra informacion y si son datos sensible podemos estar en riego de robo o perdida de la informacion

En un ejemplo, crearemos datos con la imagen de mongo db :)

```bash
docker run -d --name db mongo
```

```bash
docker ps (veo los contenedores activos)
```

```bash
docker exec -it db bash (entro al bash del contenedor)
```

```bash
mongosh (me conecto a la BBDD)

  use platzi ( creo la BBDD platzi)
  db.users.insert({“nombre”:“guido”}) (inserto un nuevo dato)
  db.users.find() (veo el dato que cargué)
```

```bash
docker run -d --name db -v <path de mi maquina>:<path dentro del contenedor(/data/db mongo)> (corro un contenedor de mongo y creo un bind mount)
```

---

## Volumenes

Una manera de evitar correr los riegos mencionandos anteriormente es por medio de los **volumenes**, esos espacios para almacenar informacion son propios y administrados por docker, esto nos da la tranquilidad de que la informacion esta segura y no se podra acceder o modificar los datos (a menos de que tengas acceso especial a ellos)

```bash
docker volume ls (listo los volumes)
```

```bash
docker volume create dbdata (creo un volume)
```

```bash
docker run -d --name db --mount src=dbdata,dst=/data/db mongo (corro la BBDD y monto el volume)
```

```bash
docker inspect db (veo la información detallada del contenedor)
```

```bash
docker exec -it db bash

mongo (me conecto a la BBDD)

  shows dbs (listo las BBDD)
  use platzi ( creo la BBDD platzi)
  db.users.insert({“nombre”:“guido”}) (inserto un nuevo dato)
  db.users.find() (veo el dato que cargué)
```

---

## Insertar y extraer archivos de un contenedor

independientemente de que usemos Bind mounts o Volumenes docker nos permite poner acceder, modificar, mover, copiar archivos y extraer archvios atraves del comando **cp**

```bash
touch prueba.txt (creo un archivo en mi máquina)
```

```bash
docker run -d --name copytest ubuntu tail -f /dev/null (corro un ubuntu y le agrego el tail para que quede activo)
```

```bash
docker exec -it copytest bash (entro al contenedor)

mkdir testing (creo un directorio en el contenedor)
```

```bash
docker cp prueba.txt copytest:/testing/test.txt (copio el archivo dentro del contenedor)
```

```bash
# con “docker cp” no hace falta que el contenedor esté corriendo
docker cp copytest:/testing localtesting (copio el directorio de un contenedor a mi máquina)
```

---

## Resumen

<img src="https://i1.wp.com/cdn-images-1.medium.com/max/800/1*bo6IOrBjaHbtkPgTKT08NA.png?w=1170&ssl=1">

- Host: Donde Docker esta instalado.

- Bind Mount: Guarda los archivos en la maquina local persistiendo y visualizando estos datos (No seguro).

- Volume: Guarda los archivos en el area de Docker donde Docker los administra (Seguro).

- TMPFS Mount: Guarda los archivos temporalmente y persiste los datos en la memoria del contenedor, cuando muera sus datos mueren con el contenedor.
