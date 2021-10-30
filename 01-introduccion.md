# Introduccion

## Problemas del desarrollo de software

Los 3 grandes problemas del desarrollo de software son:

- **Construir:**
  <br>
  La contruccion de software es compleja y escribir el codigo es una tarea muy pequeña la cual no abarca la mayor parte del desarrollo. Los problemas que abarcan la mayor parte de la construccion son:

  - Entorno de desarrollo: en la maquina local donde desarrollas ¿qué versión del lenguaje tienes instalado? ¿la versión correcta con la que tu equipo trabaja? ¿codigo esta actualizado? ¿estas trabajando correctamente en el proyecto?

  - dependencias: por lo general tú no construyes un software desde cero, la mayor parte del software es codigo de terceros. ¿qué dependencias tienes instaladas? ¿estas trabajando correctamente con las dependencias? ¿tus dependencias son las mismas con las que trabajan tu equipo?

  - entorno de ejecucion: si trabajas en un proyecto de cual involucra **node js** (ejemplo) tal vez tu tienes una version de este entorno y tu compañero de trabajo no, ahi es cuando un problema surgue, puede que a ti de funcione el codigo y a tu compañero de trabajo no.

  - servicios externos: por lo general las aplicaciones que desarrolles van a estar conectadas con servicios externos, ejemplo una base de datos, aca surgue un problema, ¿estas usando la version de la base de datos es la correcta? ¿que pasa si la base de datos es muy pesada y tu maquina no la corre?

- Distribuir: Tu código tiene que transformarse en un artefacto, o varios, que pueden ser transportados a donde tengan que ser ejecutados. Por ejemplo Si se trabaja con java este genera un . .jar o si es un proyecto Android este genera un .apk, entonces la pregunta es: ¿En cuál repositorio debe ser colocado? Problemas al distribución de software:

  - Divergencia de repositorios, Ejemplo: Un proyecto Android genera un .apk, entonces esta se alojará en la playstore, y se tiene una bases de datos Mysql la cuál la aplicación hará uso, esta estará alojada en otro lugar

  - Divergencia de artefactos, Ejemplo: Un proyecto Android genera un .apk, y si esta aplicación accede a una bases de datos, esta tiene otra extensión y lo convierte en otro artefacto a parte.

  - versionado, ¿Cómo versionar cada artefacto que conforma nuestro software?

- Realidad de Ejecutar Software:
  La máquina donde se escribe el código de software siempre es distintiva ala máquina de donde se ejecuta de manera productiva.

  Problemas al distribución de software:

  - Compatibilidad con el entorno productivo
  - Dependencias
  - Disponibilidad de servicios externos
  - Recursos de hardware.

  ¿como nos aseguramos que nuestra aplicacion va a correr como tiene que correr?

  ¿como me aseguro que las dependencias nativas que necesito se van ejecutar correctamente en la app?

  ¿como se que los servicios externos que se van a instalar en el entorno productivo son los mismos que utilice en el desarrollo si no tengo acceso a la configuracion del entorno?

  Los recursos del hardware de tu maquina son diferentes a los del entorno productivo ¿como puedes estar seguro que en tu app correra bien si tu maquina es de 12gb de ram y el servidor es de 1gb de ram?

“Docker te permite construir, distribuir y ejecutar cualquier aplicación en cualquier lado”

Una razón por la que Docker es tan popular es que cumple la promesa de “desarrollar una vez, ejecutar en cualquier lugar”. Docker ofrece una forma sencilla de empaquetar una aplicación y sus dependencias de tiempo de ejecución en un solo contenedor; también proporciona una abstracción en tiempo de ejecución que permite que el contenedor se ejecute en diferentes versiones del kernel de Linux.

Usando Docker, un desarrollador puede crear una aplicación en contenedor en su estación de trabajo, y luego implementar fácilmente el contenedor en cualquier servidor habilitado para Docker. No es necesario volver a probar o volver a sintonizar el contenedor para el entorno del servidor, ya sea en la nube o en las instalaciones

---

## Virtualizacion

La virutalizacion nos permite atacar los 3 grandes problemas del desarrollo de software. Para la virtualizacion existen dos soluciones las cuales son:

- **Maquinas virtual:** estos nos permite poder ejecutar un sistema operativo dentro de nuestra maquina local y con esto poder arrancar nuestra app, sin embargo esto nos trae un gran problema es cual es:

  - si tenemos mas de una aplicacion hara que tengamos mas de un sistema operativo replicando todo el codigo del OS en las demas aplicaciones

  - Tambien hace se ha la hora de ejecutar nuestra app se lenta debido que no solo tiene que ejecutar la app tambien tiene que primero iniciarse el OS, y el arranque de estos por lo general es lento

  - El manejo de estas VM tambien son un problema, debido a que estas maquinas funcionan como una maquina real, entonces hay que estar pendiente de sus actualizaciones mantenimiento.

  - A la hora de hacer el despliegue de las VM a la nube tambien surgen problemas, esto debido a sus formatos, las VM tienen muchos formatos y cada nube manejan un formato

- **Docker:** La solucion a los problemas de las VM es docker (no es la unica, pero actualmente es casi un estandar) el cual trae una solucion la cual es **contenedores**

  Un contendor en docker tiene la premisa de funcionar como un estandar es cual soluciona los problemas a la hora de contruir software y el cual nos permite construir y desplejar facilmente los contenedores

  Sus premisas son:

  - flexibles: cualquier app que quieras poner se podra poner

  - Livianos: reutiliza el kernel (no instalan un OS por app)

  - Portables: estan diseñados para correr de la misma manera en cualquier lugar

  - Bajo acoplamiento: un contenedor es auto contenido, en un contendor esta todo lo que necesita para correr

  - Escalables: si se nesecita aumentar la capacidad de la app podemos replicar un contenedor o crear muchos contenedores con la premisa de que ese conedores funcionan con el mismo codigo y correran igual

  - Seguros: un contendor solo puede acceder a al codigo que necesita para correr, este no puede acceder a toda la informacion de la maquina anfitriona o de otros contenedores

**BONUS:** los contenedores de docker se pueden explicar con la misma analogia que los contenedores del comercio maritimo y estos contendores de docker vienen a solucionar el desarrollo de software como lo hizo los contendores del comercio maritimo a comienzos del siglo XX :).

<img src="https://www.redeszone.net/app/uploads-redeszone.net/2016/02/docker-vs-virtual-machines.png"/>

---
