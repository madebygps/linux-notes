# Introducción a Docker

[Introducción a los contenedores Docker - Learn](https://aka.ms/dockcontainers5)
- Obtenga más información sobre imágenes, Dockerfiles, comandos y más.

[Descripción general de Docker](https://docs.docker.com/get-started/overview/)

[Build a containerized web application with Docker.](https://aka.ms/introcontainers5)
- Ejercicio que estamos usando en esta sesión.

[Administer containers in Azure](https://aka.ms/admincontainers5)
- Ruta de aprendizaje de 5 horas que cubre más temas relacionados con contenedores, incluida una introducción a Azure Kubernetes Service.

# El problema que resuelven los contenedores

Por lo general, hay más de un equipo trabajando en el éxito de su aplicación. Está el equipo de desarrollo que crea la aplicación y el equipo de operaciones que se encarga de su implementación y administración. Por lo general, cada equipo tendrá un entorno en el que trabajar:

- Entorno de desarrollo.
- Entorno de control de calidad.
- Entorno de preproducción.
- Entorno de producción.

etc

Hay algunos desafíos que ocurren debido a esta configuración:

- Diferentes entornos requieren una gestión diferente de software y hardware.
- Cada implementación de nuestra aplicación en nuestros entornos debe ocurrir de manera consistente.
- Cada implementación debe ejecutarse de tal manera que esté aislada de otras aplicaciones que se ejecutan en el mismo hardware para aprovechar al máximo los recursos sin comprometerse entre sí.
- Nuestras aplicaciones deben ser portátiles.

Aquí es donde entran los contenedores para salvar el día.

# ¿Qué es un contenedor?

Un contenedor es un entorno vagamente aislado que nos permite crear y ejecutar paquetes de software. Estos paquetes (llamados imágenes de contenedor) incluyen el código y todas las dependencias para ejecutar aplicaciones de manera rápida y confiable en cualquier entorno informático.

La imagen del contenedor se convierte en la unidad que usamos para distribuir nuestras aplicaciones.

El proceso de implementación y ejecución de nuestras aplicaciones con contenedores se denomina contenedorización.

Uno de los puntos fuertes de la creación de contenedores es que no tiene que configurar el hardware ni perder tiempo instalando sistemas operativos y software para alojar una implementación.

Dado que los contenedores están aislados entre sí, nos ayudan a mejorar la seguridad de nuestra aplicación. Se pueden ejecutar varios contenedores en el mismo hardware, lo que mejora la eficiencia del uso del hardware.

# ¿Qué es Docker?

Docker es la plataforma de contenedores más popular. Lo usamos para desarrollar, enviar y operar contenedores.

# Arquitectura Docker

![https://docs.microsoft.com/en-us/learn/modules/intro-to-docker-containers/media/2-docker-architecture.svg](https://docs.microsoft.com/en-us/learn/modules/intro-to-docker-containers/media/2-docker-architecture.svg)

## Docker host

### Docker Engine

Docker Engine está configurado como una implementación cliente-servidor donde el cliente y el servidor pueden ejecutarse en el mismo host o en uno remoto y comunicarse a través de la API REST de Docker. Los componentes que componen el motor son:

- **El cliente Docker**
    - Es una línea de comandos llamada `docker` que se usa para interactuar con un servidor Docker local o remoto y funciona como la interfaz principal para administrar contenedores.
- **El servidor Docker**
    - Un demonio llamado `dockerd` (programa de computadora que se ejecuta como un proceso en segundo plano). Su trabajo es responder a las solicitudes del cliente de Docker a través de la API de Docker Rest, interactuar con otros demonios y realizar un seguimiento del ciclo de vida de nuestros contenedores.
- **Objetos acoplables**
    - Al trabajar con Docker, creará y trabajará con imágenes, contenedores, redes, complementos y otros objetos.

## Docker hub

Docker Hub es un registro de contenedores de Docker y es el registro público predeterminado que utiliza Docker para la administración de imágenes. Un registro de contenedores son repositorios que podemos usar para almacenar y distribuir imágenes de contenedores que creamos.

# Cómo funcionan las imágenes de Docker

## ¿Qué es una imagen de contenedor?

Una imagen de contenedor se compone de:

- código de aplicación
- paquetes de sistema
- binarios
- bibliotecas
- Archivos de configuración
- sistema operativo

Esta imagen, cuando se ejecuta, se convierte en un contenedor. Una imagen es inmutable, para aplicarle cambios tendrías que crear una nueva imagen.

**Una imagen de contenedor es un paquete inmutable que contiene todo el código de la aplicación, los paquetes del sistema, los archivos binarios, las bibliotecas, los archivos de configuración y el sistema operativo que se ejecuta en el contenedor. Los contenedores Docker que se ejecutan en Linux comparten el kernel del sistema operativo host y no requieren un sistema operativo de contenedor, siempre que el binario pueda acceder al kernel del sistema operativo directamente.**

## ¿Cuál es el sistema operativo anfitrión?

El sistema operativo en el que se ejecuta el motor Docker es el sistema operativo host.

- Los contenedores que se ejecutan en Linux comparten el mismo kernel del sistema operativo host y no requieren un sistema operativo de contenedor, siempre que el sistema binario pueda acceder al kernel del sistema operativo directamente.
- Los contenedores que se ejecutan en Windows necesitan un sistema operativo contenedor. El contenedor depende del kernel del sistema operativo para administrar servicios como el sistema de archivos, la administración de la red, la programación de procesos y la administración de la memoria.

## ¿Qué es el sistema operativo contenedor?

El sistema operativo del contenedor es el sistema operativo que forma parte de la imagen del contenedor. Podemos incluir diferentes versiones de Linux o Windows en un contenedor y esto nos permite acceder a funciones específicas del sistema operativo.

![https://docs.microsoft.com/en-us/learn/modules/intro-to-docker-containers/media/3-container-ubuntu-host-os.svg](https://docs.microsoft.com/en-us/learn/modules/intro-to-docker-containers/media/3-container-ubuntu-host-os.svg)

Está aislado del sistema operativo host y es el entorno en el que implementamos y ejecutamos nuestra aplicación.

Este aislamiento significa que el entorno para nuestra aplicación que se ejecuta en desarrollo es el mismo que en producción.

## ¿Qué es el sistema de archivos de unificación apilable (`Unionfs`)

[Unionfs: un sistema de archivos de unificación apilable](https://unionfs.filesystems.org/)

`Unionfs` es un sistema de archivos que le permite apilar varios directorios, llamados ramas, de tal manera que parece como si el contenido estuviera fusionado. Funciona sobre otros sistemas de archivos y surgió porque los contenedores necesitan una forma más eficiente de compartir segmentos de memoria física que los sistemas de archivos convencionales.

[UnionFS: un sistema de archivos de un contenedor](https://medium.com/@knoldus/unionfs-a-file-system-of-a-container-2136cd11a779)

![https://miro.medium.com/max/462/0*BhhgkPFuHnQhOcy0.jpg](https://miro.medium.com/max/462/0*BhhgkPFuHnQhOcy0.jpg)

Aunque el contenido parece fusionado, se mantiene físicamente separado. `Unionfs` le permite agregar y eliminar ramas a medida que construye su sistema de archivos.

## Imagen base e imagen principal

[Crear una imagen base](https://docs.docker.com/develop/develop-images/baseimages/)

- **Imagen base:** una imagen que usa la imagen `scratch` de Docker (contenedor vacío que no crea una capa de sistema de archivos). Este tipo de imagen supone que la aplicación puede usar directamente el kernel del sistema operativo host.
    - Permitirnos un mayor control sobre el contenido de la imagen final.
- **Imagen principal:** una imagen en la que se basa tu imagen.
    - La mayoría de los archivos acoplables parten de una imagen principal, ya que ya se basan en un sistema operativo y otros componentes instalados que necesitaríamos.

## ¿Qué es un Dockerfile?

Un Dockerfile es un archivo de texto que contiene las instrucciones que usamos para crear y ejecutar una imagen de Docker. Se define:

- La imagen base o padre que usamos para crear la nueva imagen.
- Comandos para actualizar el sistema operativo base e instalar software adicional.
- Construir artefactos para incluir (aplicación desarrollada, etc.).
- Servicios a exponer (almacenamiento, configuración de red, etc.).
- Comando a ejecutar cuando se lanza el contenedor.

```bash
    # Paso 1: Especifique la imagen principal para la nueva imagen
    DESDE ubuntu: 18.04

    # Paso 2: actualice los paquetes del sistema operativo e instale software adicional
    EJECUTAR apt -y actualizar && apt install -y wget nginx software-properties-common apt-transport-https \
        && wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O paquetes-microsoft-prod.deb \
        && dpkg -i paquetes-microsoft-prod.deb \
        && universo add-apt-repository \
        && apt -y actualizar \
        && apt install -y dotnet-sdk-3.0

    # Paso 3: Configurar el entorno Nginx
    Inicio de nginx del servicio CMD

    # Paso 4: Configurar el entorno Nginx
    COPIAR ./predeterminado /etc/nginx/sitios-disponibles/predeterminado

    # PASO 5: Configurar el directorio de trabajo
    WORKDIR /aplicación

    # PASO 6: Copie el código del sitio web al contenedor
    COPIAR ./sitio web/. .

    # PASO 7: Configurar requisitos de red
    EXPONER 80:8080

    # PASO 8: Definir el punto de entrada del proceso que se ejecuta en el contenedor
    PUNTO DE ENTRADA ["dotnet", "website.dll"]
```

Cada uno de estos pasos crea una imagen de contenedor en caché a medida que construimos la imagen de contenedor final. Estas imágenes de contenedores almacenadas en caché se superponen a las anteriores y se presentan como una sola imagen una vez que se completan todos los pasos (gracias a `unionfs`)

El comando `ENTRYPOINT` indica qué proceso se ejecutará una vez que ejecutemos un contenedor desde una imagen.

## Cómo administrar las imágenes de Docker

La CLI de Docker nos permite administrar imágenes al compilarlas, enumerarlas, eliminarlas y ejecutarlas. La CLI envía todas las consultas al demonio `docerkd`.

- Usamos `docker build` para construir imágenes de Docker.
- Una etiqueta de imagen es una cadena de texto que se utiliza para versionar una imagen. Podemos etiquetar una imagen usando el indicador de comando `-t` al construir. Si no se especifica, la imagen se etiqueta con la etiqueta `más reciente`.
- `imágenes acoplables` se utiliza para ver imágenes en un registro acoplable local.
- `docker rmi` se usa para eliminar una imagen del registro local de docker.
    - No puede eliminar una imagen si todavía está en uso.

# Cómo funcionan los contenedores Docker

## Cómo gestionar contenedores Docker

Un contenedor docker tiene un ciclo de vida que puede administrar y rastrear el estado del contenedor.

![https://docs.microsoft.com/en-us/learn/modules/intro-to-docker-containers/media/4-docker-container-lifecycle.svg](https://docs.microsoft.com/en-us/learn/modules/intro-to-docker-containers/media/4-docker-container-lifecycle.svg)

- Utilice el comando `ejecutar` para colocar un contenedor en estado de ejecución
    - Se considera que un contenedor está en ejecución hasta que se pausa, se detiene o se elimina.
    - Un contenedor puede salir automáticamente cuando finaliza el proceso en ejecución o si el proceso entra en un estado de error.
- Utilice el comando `reiniciar` para reiniciar un contenedor
    - Al reiniciar un contenedor, recibe una señal de finalización para permitir que cualquier proceso en ejecución se cierre correctamente antes de que finalice el kernel del contenedor.
- Use el comando `pause` para pausar un contenedor en ejecución.
    - Este proceso suspende todos los procesos en el contenedor.
- Use el comando `stop` para detener un contenedor en ejecución.
## Cómo ver los contenedores disponibles

- use el comando `docker ps` para enumerar los contenedores en ejecución.
    - Pase el argumento `-a` para ver todos los contenedores en todos los estados.

## ¿Por qué se les da un nombre a los contenedores?

Utilice la bandera `--name` para dar a un contenedor un nombre explícito. Los nombres son únicos y nos permiten ejecutar varias instancias de contenedor de la misma imagen.

# Configuración de almacenamiento de contenedores Docker

Considere siempre los contenedores como temporales cuando piense en almacenar datos.

- El almacenamiento de contenedores es temporal
- El almacenamiento de contenedores está acoplado a la máquina host subyacente
- Los buzos de almacenamiento de contenedores son menos eficientes

Los contenedores pueden hacer uso de volúmenes y enlazar montajes para conservar los datos.

## ¿Qué es un volumen?

- Un volumen se almacena en el sistema de archivos del host en una ubicación de carpeta específica y se considera la estrategia de almacenamiento de datos preferida para usar con contenedores.
    - Elija una carpeta que no vaya a ser modificada por procesos que no sean de Docker.
        - Cree y administre el volumen con el comando `docker volume create`.
        - Puede crear volúmenes como parte del proceso de creación de contenedores (dockerfile).
        - Docker creará el volumen si no existe cuando intente montar el volumen en un contenedor por primera vez.
        - Una vez montados, estos volúmenes se aíslan de la máquina host.
        - Múltiples contenedores pueden usar simultáneamente los mismos volúmenes.
        - Los volúmenes no se eliminan cuando un contenedor deja de usarlo.

## ¿Qué es un montículo de unión?

- Conceptualmente lo mismo que un volumen, excepto que puede montar cualquier archivo o carpeta en el host. También espera que el anfitrión pueda cambiar el contenido de estos montajes.
- Tienen una funcionalidad limitada en comparación con los volúmenes.
- Más rendimiento.
- Depende de que el host tenga una estructura de carpetas específica.

# Configuración de la red del contenedor Docker

La configuración de red nos permite crear y configurar aplicaciones que pueden comunicarse de forma segura entre sí.

## ¿Qué es la red puente?

La red puente es la configuración predeterminada que se aplica a los contenedores cuando se lanzan sin especificar ninguna configuración de red adicional.

- Es una red de contenedores interna, privada y aislada de la red de host de Docker.
- A cada contenedor en la red puente se le asigna una dirección IP y una máscara de subred con el nombre de host predeterminado al nombre del contenedor.
- Los contenedores conectados a la red de puente predeterminada pueden acceder a otros contenedores conectados por puente por dirección IP y no por nombres de host.
- Para habilitar la asignación de puertos entre los puertos del contenedor y los puertos del host de la ventana acoplable, use el indicador `--publish` del puerto de Docker. Esto configura efectivamente una regla de firewall que mapea los puertos.
- la interfaz de red *Docker0* no está disponible en macOS cuando se usa la red puente

## Red anfitriona

La red de host le permite ejecutar el contenedor directamente en la red de host. Esto elimina efectivamente el aislamiento entre el host y el contenedor a nivel de red.

El contenedor solo puede usar puertos que el host aún no usa.

la configuración de la red del host no es compatible con los escritorios de Windows y macOS.

## Ninguna red

Para deshabilitar la red para contenedores, use la opción de ninguna red.

# Cuándo usar contenedores Docker

- Uso eficiente del hardware.
    - Al eliminar la máquina virtual y el requisito adicional del sistema operativo, podemos liberar recursos en el host y usarlos para ejecutar otros contenedores.
- Aislamiento de contenedores.
    - Los contenedores Docker brindan características de seguridad para ejecutar múltiples contenedores simultáneamente en el mismo host sin afectarse entre sí
- Portabilidad de aplicaciones
    - Con Docker, el contenedor se convierte en la unidad que usamos para distribuir aplicaciones. Este concepto asegura que tengamos un formato de envase estandarizado.
- Gestión de entornos de alojamiento.
    - Configuramos el entorno de nuestra aplicación internamente al contenedor. Esta contención brinda flexibilidad para que nuestro equipo de operaciones administre el entorno de la aplicación mucho más de cerca.
- Implementaciones en la nube
    - Los contenedores Docker son la arquitectura de contenedores predeterminada que se usa en los servicios de contenedorización de Azure y son compatibles con muchas otras plataformas en la nube.

# ¿Cuándo no usar contenedores Docker?

- Seguridad y virtualización
    - Los contenedores comparten un solo kernel de sistema operativo host, que puede ser un único punto de ataque.
- Seguimiento del servicio
    - Administrar las aplicaciones y los contenedores es más complicado que las implementaciones de máquinas virtuales tradicionales. Existen funciones de registro que nos informan sobre el estado de los contenedores en ejecución. Sin embargo, la información más detallada sobre los servicios dentro del contenedor es más difícil de monitorear.

# Demo

- [Siga aquí](https://docs.microsoft.com/en-us/learn/modules/run-docker-with-azure-container-instances/)
