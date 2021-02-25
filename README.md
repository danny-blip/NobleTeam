# Manejo de discos
## ¿Cuáles son las diferencias entre hda, sda y vda? ¿Qué significa la letra y el número al final de los identificadores?
###### Los nombres de los discos y particiones de los diferentes sistemas operativos se les otorgan diferentes nombres, por ejemplo:
###### Para MS-DOS y Windows, los dispositivos suelen llamarse por letras (A, B, C, D...):
* Unidad A: primera unidad de disquetes
* Unidad B: segunda unidad de disquetes
* Unidad C: primera unidad de disco duro
* Unidad D: primera unidad de CD/DVD
###### En Linux se otorgan diferentes nombres a los discos, y las particiones llegan a ser diferentes a las de otros sistemas operativos. A cada dispositivo se le otorga su posición y particuones. Linux necesita conocer el nombre de los dispositivos para crear y montar sus particiones en disco.
###### En Linux el nombre de cualquier dispositivo empieza por `/dev/` (device).
* Para el disco maestro en el controlador IDE primario el comando es `/dev/hda`.
* Para el disco esclavo del controlador IDE primario el comando es `/dev/hdb`.
* Para el disco maestro y esclavo del controlador IDE secundario el comando `/dev/hdc` y `/dev/hdd`.
###### Por otro lado, existen varios tipos de particiones:
* Primarias: como máximo 4.
* Extendidas: como máximo 1 (en este caso, tres primarias como máximo).
* Lógicas: casi todas las que se deseen, existen dentro de las extendidas.
###### La numeración al final de los identificadores corresponde al tipo de partición, por ejemplo, si se tiene:
###### `/dev/hda4` -> El 4 indica que es la cuarta particion.
###### Se sabe que el máximo de particiones primarias que pueden tenerse son 4. Si se tiene una partición extendida, se le asignaría la partición 4. A partir de la partición 5, las particiones son lógicas.
###### `/dev/hda5` -> Primera partición lógica.
###### Cuando se habla de `/dev/sda`, debe entenderse que se refiere al primer disco detectado de tipo IDE, SATA, SCSI emulado y virtualizado por el hipervisor, en donde el sistema operativo invitado no sabe que está ejecutándose sobre un hipervisor, es decir, no sabe que está virtualizado y no requiere de cambios para funcionar en la configuración.
* La primera unidad de disco SCSI (identificación SCSI address-wise) se llama `/dev/sda`.
* La segunda unidad de disco SCSI (address-wise) se llama `/dev/sdb`, y así sucesivamente.
###### Las particiones en cada disco se enumeran según el número de particiones por unidad de disco, es decir, si se tiene un sistema de dos discos, uno tiene dirección SCSI 2 y se llama `/dev/sda` y el otro tiene dirección SCSI 4 y se llama `/dev/sdb`.
###### Si existen dos particiones de `/dev/sda`, se enumeran de la siguiente forma:
* `/dev/sda1` -> Primera partición en la primera unidad de disco SCSI del sistema.
* `/dev/sda2` -> Segunda partición en la primera unidad de disco SCSI del sistema.
###### Lo mismo aplica al disco `/dev/sdb` y sus particiones.
###### En la paravirtualización, el sistema operativo invitado es consciente de que está siendo ejecutado en un hipervisor y de que incluye código para que las transiciones de invitado a hipervisor sean eficientes.
###### Todos los archivos `/dev/vda` se localizan en el espacio asignado de la máquina virtual.
###### Ejemplo de Xen Virtual Block Device:
* `/dev/vda` -> Primera partición en la primera unidad de disco Xen VBD del sistema.
* `/dev/vdb` -> Segunda partición en la primera unidad de disco Xen VBD del sistema.
* `/dev/vdp` -> Decimosexta partición en la primera unidad de disco Xen VBD del sistema.
###### La enumeración, es decir, las particiones de `vda`, se manejan de la misma manera que los discos IDE, las particiones pueden llegar hasta 15 bits.
###### La diferencia entre `hda`, `sda` y `vda` es que hda es la partición de disco maestro IDE, mientras que sda son las particiones de disco externo detectado emulado sin que el s.o. invitado sepa que se está ejecutando, mientras que el `vda` el s.o. invitado está consciente de que se ejecuta y de que es un disco virtual en la nube que emula lo que hace un `sda`, pero sin la necesidad de hardware.
## ¿Cómo montar y desmontar una USB en el sistema por terminal?
###### Primero, se debe saber qué dispositivos están montados, para eso, se utiliza el siguiente comando:
* `df -h`
###### Para desmontar una USB en terminal se utiliza el siguiente comando:
* `sudo umount /dev/sdb1`

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/2A.png "2a")

###### Para montar una USB en terminal se utilizan los siguientes comandos (en este caso, se montará en una carpeta en el escritorio llamada USB):
* `cd Escritorio/`
* `mkdir USB`
* `sudo mount /dev/sdb1 USB`

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/2b.png "2b")

## ¿Cómo enlistar la información de los dispositivos de bloque conectados aunque no estén montados en terminal?
*	Ejecutar el comando `lsblk` (este comando proviene de la abreviatura «List Block Devices»).

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/3a.png "3a")

* En la primera columna del enlistado, se tiene un campo llamado «Name», el cual indica el nombre del dispositivo o de la partición.
*	En la segunda columna de la tabla, se tiene un campo llamado «Maj:Min», que se explica a continuación:
1. El término «Maj» hace referencia al término «major».
2. El término «Min» hace referencia al término «minor».
3. Estos términos mencionados anteriormente, es la forma en la cual el Kernel, mediante números enteros, se refiere a los dispositivos de manera interna.
* `lsblk -b` despliega el tamaño de cada dispositivo en bytes, como se puede ver en la imagen, a comparación de la imagen anterior, en el campo «»size» ya no existe la presencia de letras M, ya que los despliega en bytes.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/3b.png "3b")

* `lsblk -d` Imprime los dispositivos de bloque titulares y no las particiones, como puede observarse en la imagen, no se muestran las particiones de `sda`.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/3c.png "3c")

* `lsblk -m` muestra la tabla en donde se observan los diversos permisos que tiene los dispositivos de bloque en el enlistado.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/3d.png "3d")

* Abajo de los del campo anteriormente mencionado, se tiene un formato de números de la siguiente manera: 7:1. La cual se explica a continuación:
1. El primer número del formato hace alusión al tipo de dispositivo que es, si se tiene un disco SCSI, se le asigna el número 8 y, en caso de que sean discos IDE, se les asigna el número 3; a los discos ópticos se les asigna el número 11, para los disquetes es el número 2, `/dev`, `/null` y `/zero` se les asocia el número 1, a consolas virtuales y terminales se les asocia el número 4 y, a los dispositivos `vcs1` y `vcsa1`, se les asocia el número 7.
2. El segundo número especifica al dispositivo qué hay dentro del primer número.
*	En el tercer campo de la tabla que tiene como nombre «RM» hace alusión a si es extraíble. En caso de que sea extraíble, se despliega un 1, en caso contrario, se despliega un 0.
*	En el quinto campo de la tabla se hace referencia a si es solo lectura, en caso de que sea solo lectura, se despliega un 1, en caso contrario, se despliega un 0.
*	En el sexto campo de la tabla se hace referencia a qué tipo de dispositivo es. Dentro de estos puede haber muchos, como por ejemplo:
1. disk: unidad de almacenamiento.
2. part: partición del disco.
3. loop: un pseudodispositivo que permite que un dispositivo sea accesible como un dispositivo de bloque.
* En el último campo de la tabla se hace referencia al punto de montaje de cada partición o del dispositivo de bloques.
* Dentro de este comando, hay ciertas variaciones, las cuales se explican a continuación:
1. `lsblk -a` despliega los dispositivos vacíos, como se puede observar en la imagen, se puede ver que, en la parte de «NAME», hay un campo llamado «loop 7», el cual no aparece en la imagen anterior.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/3e.png "3e")

2. `lsblk -o` muestra la tabla de los dispositivos de bloque de manera personalizada. En este ejemplo, se desplegará el nombre y el tamaño. Como puede verse en el ejemplo a continuación, se tiene que escribir el comando que está escrito al principio de la viñeta. Después, se deja un espacio y se escriben los nombre de las columnas que se desean que aparezcan; si es más de un elemento, van separados por comas.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/3f.png "3f")

## ¿Cómo mostrar la tabla de particiones del disco donde está instalado el sistema operativo en terminal?
* Primero, se deben tener privilegios de súper usuario:
1. `sudo su`
2. Ingresar la contraseña, la cual es la que se utiliza para acceder al equipo.
3. Ingresar el comando `fdisk -l`, el cual mostrará la siguiente información:

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/4a.png "4a")

* Lo que se acaba de hacer con este comando, es acceder a los enlistados de los discos duros, la que se busca en este caso es la siguiente:

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/4B.png "4b")

* Lo primero que se muestra en la imagen es el disco en donde está instalado el sistema operativo, si se observa en la parte de abajo, hay una tabla, esa es la tabla de particiones del sistema operativo.
* En caso de que no hubiese tabla, quiere decir que no hay particiones en ese disco.
* Para poder salir, solo se coloca el comando exit para salir del modo super usuario.
## ¿Cómo conectar una memoria USB y mostrar su tabla de particiones en terminal?
###### Para mostrar las tablas de particiones, se utiliza el siguiente comando:
* `sudo fdisk -l`

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/5a.png "5a")

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/5b.png "5b")

## ¿Cómo borrar todas las particiones de la USB en terminal?
* Para llevar a cabo este paso, es necesario que la USB esté desmontada.
* Ejecutar `sudo fdisk /dev/sdb`.
* Ejecutar `d` (delete) para borrar una partición.
* Introducir el número de partición a borrar. Si solo se tiene una, se seleccionará y se borrará de manera automática.
* Reperir para las particiones que se deseen borrar.
* Ejecutar `p` para ver los cambios realizados.
* Ejecutar `w` para guardar los cambios y salir.
* Ejecutar `q` para salir sin guardar los cambios.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/6a.png "6a")

## ¿Cómo crear tres particiones físicas y una extendida en la USB?
* Para llevar a cabo este paso, es necesario que la USB esté desmontada.
* Ejecutar `sudo fdisk /dev/sdb`.
* Ejecutar `n` (new) para crear una nueva partición.
* Introducir el tipo de partición («p» para primaria y «e» para extendida).
* Introducir el número de partición (se recomienda el valor por defecto).
* Introducir el punto de partida (se recomienda el valor por defecto).
* Introducir el tamaño de la partición, por ejemplo, «+1024M» para 1 GB.
* Repetir para las otras dos particiones físicas y una vez más para la extendida (para esta, se introduce «e» en el tipo de partición).
* Ejecutar `p` para ver los cambios realizados.
* Ejecutar `w` para guardar los cambios y salir.
* Ejecutar `q` para salir sin guardar los cambios.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/7a.png "7a")

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/7b.png "7b")

## ¿Cómo crear una partición lógica dentro de la partición extendida de la USB en terminal?
* Para llevar a cabo este paso, es necesario que la USB esté desmontada.
* Se necesitan 3 particiones físicas y una partición extendida.
* Ejecutar `sudo fdisk /dev/sdb`.
* Ejecutar `n` (new) para crear una nueva partición.
* Automáticamente se creará una partición lógica dentro de la extendida seleccionada.
* Introducir el punto de partida (se recomienda el valor por defecto)
* Introducir el tamaño de la partición, por ejemplo, «+512M» (debe ser menor al tamaño de la partición que la contiene).

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/8a.png "8a")

## En la interfaz gráfica de la aplicación «Discos», ¿cómo borrar las particiones para que solo exista una partición que abarque toda la USB?
* Para llevar a cabo este paso, es necesario que la USB esté desmontada.
* Abrir la aplicación Discos en Ubuntu.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9a.png "9a")

* Seleccionar la partición a borrar y dar clic en el ícono de «-».

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9b.png "9b")

* Eliminar la partición.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9c.png "9c")

* Repetir con todas las demás particiones, excepto la uno. Una vez que se tenga solo la primera partición, se selecciona y se da clic en ícono del engranaje.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9d.png "9d")

* Seleccionar la barrita de «tamaño actual» y arrastratla toda hacia la derecha para utilizar todo el almacenamiento para esa partición.

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9e.png "9e")

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9f.png "9f")

* ¡Y listo!

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9g.png "9g")

## ¿Cómo copiar un archivo .iso de distribución live de linux a la USB por medio del comando «dd» en terminal?
* Para llevar a cabo este paso, es necesario que la USB esté desmontada.
* Descargar un archivo iso de la distribución Linux deseada. Para este caso, se utilizará Kali Linux. Una vez que el archivo esté descargado, se abre la terminal, se cambia al directorio en donde se guardó el archivo `.iso` y se ejecuta el siguiente comando:

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/10a.png "10a")

* Se tardará unos minutos dependiendo de la capacidad de la USB. Finalmente, se debe montar la USB y, en ella, se tendrá lo siguiente:

![alt text](https://github.com/danny-blip/NobleTeam/blob/main/10B.png "10b")
