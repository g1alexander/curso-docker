# Docker compose

es una herramienta que me permite atraves de un archivo llamado **docker-compose.yml** hacer configuraciones para comunicar multiples contendores de una forma super facil y menos manual como la que veniamos mirando en la sessiones anteriores

Con Compose puedes crear diferentes contenedores y al mismo tiempo, en cada contenedor, diferentes servicios, unirlos a un volúmen común, iniciarlos y apagarlos, etc. Es un componente fundamental para poder construir aplicaciones y microservicios.

Docker Compose te permite mediante archivos YAML, poder instruir al Docker Engine a realizar tareas, programáticamente. Y esta es la clave, la facilidad para dar una serie de instrucciones, y luego repetirlas en diferentes ambientes.

Un archivo de docker-compose se estructura de la siguiente manera:

```docker
# Versión del compose file
version: "3.9"

# Servicios que componen nuestra aplicación.
## Un servicio puede estar compuesto por uno o más contenedores.
services:
  # nombre del servicio.
  app:
    # Imagen a utilizar.
    image: apinode
    # Declaración de variables de entorno.
    environment:
      MONGO_URL: "mongodb://db:27017/test"
    # Indica que este servicio depende de otro, en este caso DB.
    # El servicio app solo iniciara si el servicio se inicia correctamente.
    depends_on:
      - db
    # Puerto del contenedor expuesto.
    ports:
      - "3000:3000"
  #servicio principal, en este caso la base de datos mongo. en dado caso que no se incialice correctamente, el servicio app no iniciará.
  db:
    image: mongo

```

Comandos utilies de docker-compose

```bash
#construir servicios
docker-compose build
```

```bash
#levantar los servicios
docker-compose up
```

```bash
#levantar los servicios en dettach ("segundo plano")
docker-compose up -d
```

```bash
#bajar los servicios
docker-compose down
```

```bash
#ver los logs de los contenedores
docker-compose logs
```

```bash
#seguimiento de los logs
docker-compose logs -f
```

```bash
#ver los logs de un contenedor
docker-compose logs [service]
```

```bash
#correr comando de un contenedor
docker-compose exec [service] [command]
```

---

## docker compose override

**docker-compose.override.yml** es un archivo que se encarga de sobreescribir tu configuración de docker-compose.yml , se puede usar para tener segura tu configuración y para no guardar los cambios en el repositorio de git.
Un equivalente podría ser los archivos de declaración de variables de entorno, donde hay un archivo .env declarando su nombre y valor, y hay una copia .env.example con solo las variables sin valor. En .gitignore se declara que los cambios en .env no serán guardados, pero mandamos el archivo de ejemplo al repositorio.

Esto nos permite personalizar o hacer pequeños cambios propios para nuestro ambiente sobre el original sin alterar ese archivo.

Articulo es para ampliar mas esta informacion: [**link**](https://medium.com/it-dead-inside/making-sense-of-docker-compose-overrides-efb757460d64)

Acabe mencionar tambien que podemos tener varios ambientes para manejar el archivo docker.compose.yml (desarrollo, produccion, etc.. explicado en el articulo)
