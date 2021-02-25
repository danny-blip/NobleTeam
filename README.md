# Manejo de discos
## Identificar y describir las diferencias entre hda, sda y vda. Además, explicar qué significa la letra y el número al final de los identificadores
###### Los nombres de los discos y particiones de los diferentes sistemas operativos se les otorgan diferentes nombres, por ejemplo:
###### Para MS-DOS y Windows, los dispositivos suelen llamarse por letras (A, B, C, D...):
* Unidad A: primera unidad de disquetes
* Unidad B: segunda unidad de disquetes
* Unidad C: primera unidad de disco duro
* Unidad D: primera unidad de CD/DVD
###### En Linux se otorgan diferentes nombres en los discos, y las particiones llegan a ser diferentes a las de otros sistemas operativos. A cada dispositivo se le otorga su posición y partucuones. Linux necesita conocer el nombre de los dispositivos para crear y montal sus particiones en disco.
###### En Linux el nombre de cualquier dispositivo empieza por /dev/ (device).
* Para el disco maestro en el controlador IDE primario se le llama /dev/hda
* Para el disco esclavo del controlador IDE primario se le llama /dev/hdb
* Para el disco maestro y esclavo del controlador IDE secundario se le denomina /dev/hdc y /dev/hdd
###### Por otro lado, existen varios tipos de particiones:
* Primarias: como máximo 4.
* Extendidas: como máximo 1 (en este caso, tres primarias como máximo).
* Lógicas: casi todas las que se deseen, estas están dentro de una extendida.
###### La numeración al final de los identificadores corresponde al tipo de partición, por ejemplo, si se tiene:
###### /dev/hda4 -> El 4 indica que es la cuarta particion.
###### Se sabe que el máximo de particiones primarias que pueden tenerse son 4. Si se tiene una partición extendida, se le asignaría la partición 4. A partir de la partición 5, las particiones son lógicas.
###### /dev/hda5 -> Primera partición lógica.
###### Cuando se habla de /dev/sda, debe entenderse que se refiere al primer disco detectado de tipo IDE, SATA, SCSI emulado y virtualizado por el hipervisor, en donde el sistema operativo invitado no sabe que está ejecutándose sobre un hipervisor, es decir, no sabe que está virtualizado y no requiere de cambios para funcionar en la configuración.
* La primera unidad de disco SCSI (identificación SCSI address-wise) se llama /dev/sda.
* La segunda unidad de disco SCSI (address-wise) se llama /dev/sdb, y así sucesivamente.
###### Las particiones en cada disco se enumeran según el número de particiones por unidad de disco, es decir, si se tiene un sistema de dos discos con dirección SCSI 2 que se llama /dev/sda y SCSI 4 se llama /dev/sdb.
###### Si existen dos particiones de /dev/sda, se enumeran de la siguiente forma:
* /dev/sda1 -> 1° partición en la primera unidad de disco SCSI del sistema.
* /dev/sda2 -> 2° partición en la primera unidad de disco SCSI del sistema.
###### Lo mismo aplica al disco /dev/sdb y sus particiones.
###### En la paravirtualización, el sistema operativo invitado es consciente de que está siendo ejecutado en un hipervisor y de que incluye código para que las transiciones de invitado a hipervisor sean eficientes.
###### Todos los archivos /dev/vda se localizan en el espacio asignado de la máquina virtual.
###### Ejemplo de Xen Virtual Block Device:
* 0 = /dev/vda -> primera partición en la primera unidad de disco Xen VBD del sistema.
* 16 = /dev/vdb -> segunda partición en la primera unidad de disco Xen VBD del sistema.
* 240 = /dev/vdp -> decimosexta partición en la primera unidad de disco Xen VBD del sistema.
###### La enumeración, es decir, las particiones de vda, se manejan de la misma manera que los discos IDE, las particiones pueden llegar hasta 15 bits.
###### La diferencia entre hda, sda y vda es que hda es la partición de disco maestro IDE, mientras que sda son las particiones de disco externo detectado emulado sin que el s.o. invitado sepa que se está ejecutando, mientras que el vda el s.o. invitado está consciente de que se ejecuta y de que es un disco virtual en la nube que emula lo que hace un sda, pero sin la necesidad de hardware.
## ¿Cómo montar y desmontar una USB en el sistema por terminal?
###### Primero, se debe saber qué dispositivos están montados, para eso, se utiliza el siguiente comando:
* df -h
###### Para desmontar una USB en terminal se utiliza el siguiente comando:
* umount /dev/sdb1
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/2c.png "2c")
###### Para montar una USB en terminal se utilizan los siguientes comandos (en este caso, se montará en una carpeta en el escritorio llamada USB):
* cd Escritorio/
* mkdir USB
* sudo mount /dev/sdb1 USB
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/2a.png "2a")
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/2b.png "2b")
## Enlistar la información de los dispositivos de bloque conectados aunque no estén montados en terminal
* Abrir la línea de comandos.
*	Dentro de la línea de comandos, colocar el comando lsblk.
*	El comando anterior, proviene de la abreviatura (List Block Devices).
*	Este comando obtiene un enlistado de todos los dispositivos de bloque que estén o no estén montados.
*	Al colocar el comando y presionar la tecla «intro», despliega la siguiente información:
* xDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDddddd
* En la primera columna del enlistado, se tiene un campo llamado «Name», la cual nos menciona el nombre del dispositivo o de la partición.
*	En la segunda columna de la tabla, se pude apreciar que hay un campo llamado «Maj:Min» los cuales se explican a continuación:
  * El término «Maj» hace referencia al término «major».
**	El término "Min" hace referencia al término «minor».
**	Estos términos mencionados anteriormente, es la forma la cual el Kernel mediante números enteros se refiere a los dispositivos de manera interna.
** -lsblk -b: nos despliega el tamaño de cada dispositivo en bytes, como se puede ver en la imagen a comparación de la imagen anterior, podemos ver que en el campo “size” ya no existe la presencia de letras M, ya que nos los despliega en bytes.
** lsblk -d: Imprime los dispositivos de bloque titulares y no las particiones, como podemos observar en la imagen, no se muestran las particiones de sda.
** lsblk -m: Muestra la tabla, donde se observan los diversos permisos que tiene los dispositivos de bloque en el enlistado.
* Abajo de los del campo anteriormente mencionado, se tiene un formato de números de la siguiente manera: 7:1. La cuál se explica a continuación:
** o	- El primer número del formato, hace alusión al tipo de dispositivo que es, si se tiene un disco SCSI se le asigna el número 8, en 	caso de que sean discos IDE se les asigna el número 3, para los discos ópticos se les asigna el número 11 para los disquetes es el número 2, /dev, /null y /zero se les asocia el número 1, consolas virtuales y terminales se les asocia el número 4 y los dispositivos vcs1 y vcsa1 se les asocia el número 7.
** El segundo número, especifica al dispositivo que hay dentro del primer número.
*	En el tercer campo de la tabla que tiene como nombre "RM" hace alusión a si es extraíble, en caso de que sea extraíble, se despliega un 1 en caso contrario se despliega un 0.
*	En el quinto campo de la tabla hace referencia a si es solo lectura, en caso de que sea solo lectura, se despliega un 1 en caso contrario se despliega un 0.
*	En el sexto campo de la tabla, se refiere a qué tipo de dispositivo es. dentro de estos puede haber muchos, como, por ejemplo:
** disk: unidad de almacenamiento.
** part: partición del disco.
** loop: un pseudodispositivo el cual permite que un dispositivo sea accesible como un dispositivo de bloque.
* En el último campo de la tabla, hace referencia al punto de montaje de cada partición o del dispositivo de bloques.
* Dentro de este comando, hay ciertas variaciones, las cuales se explican a continuación.
** lsblk -a: Nos despliega los dispositivos vacíos, como se puede observar en la imagen, podemos ver que en la parte de NAME, hay un campo llamado «loop 7» el cual no aparece en la imagen anterior.
** lsblk -o: Muestra de manera personalizada, la tabla de los dispositivos de bloque, en este ejemplo, vamos a desplegar el nombre y el tamaño, como se puede ver en el ejemplo a continuación, se tiene que escribir el comando que está escrito al principio de la viñeta, después se deja un espacio y se escriben los nombre de las columnas que quieres que aparezcan; si es más de un elemento, va separado por coma.

## Mostrar la tabla de particiones del disco donde está instalado el sistema operativo en terminal
## Conectar una memoria USB y mostrar su tabla de particiones en terminal
###### Para mostrar las tablas de particiones, se utiliza el siguiente comando:
* sudo fdisk -l
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/5a.png "5a")
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/5b.png "5b")
## Borrar todas las particiones de la USB en terminal
* sudo fdisk /dev/sdb
* d
* Introducir el número de partición a borrar. Si solo se tiene una, se seleccionará de manera automática
* Reperir para las particiones que se deseen borrar.
* p
* w
* q
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/6a.png "6a")
## Crear tres particiones físicas y una extendida en la USB
* sudo fdisk /dev/sdb
* n
* p
* Introducir el número de partición (se recomienda el valor por defecto)
* Introducir el punto de partida (se recomienda el valor por defecto)
* Introducir el tamaño de la partición, por ejemplo, "+1024M" para 1 GB
* Introducir "p" para ver la tabla de particiones
* Repetir para el número de particiones deseadas
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/7a.png "7a")
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/7b.png "7b")
## Crear una partición lógica dentro de la partición extendida de la USB en terminal
* Se necesitan 3 particiones físicas y una partición extendida
* sudo fdisk /dev/sdb
* n
* Automáticamente se creará una partición lógica dentro de la extendida seleccionada
* Introducir el punto de partida (se recomienda el valor por defecto)
* Introducir el tamaño de la partición, por ejemplo, "+512M" (debe ser menor al tamaño de la partición que la contiene)
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/8a.png "8a")
## En la interfaz gráfica de la aplicación "Discos", borrar las particiones para que solo exista una partición que abarque toda la USB
###### Abrir la aplicación Discos en Ubuntu
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9a.png "9a")
###### Seleccionar la partición a borrar
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9b.png "9b")
###### Eliminar la partición
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9b.png "9d")
###### Repetir con todas las demás particiones, excepto la uno. Una vez que se tenga solo la primera partición, se selecciona y se da clic en el engranaje.
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9f.png "9f")
###### Seleccionar la barrita de tamaño actual y arrastratla toda a la derecha para utilizar todo el almacenamiento para esa partición.
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9g.png "9g")
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9h.png "9h")
###### ¡Y listo!
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/9i.png "9i")
## Copiar un archivo .iso de distribución live de linux a la USB por medio del comando "dd"
###### Descargar un archivo iso de la distribución Linux deseada. Para este caso, se utilizará Kali Linux. Una vez que el archivo esté descargado, se abre la terminal y se ejecutan los siguientes comandos.
![alt text](https://github.com/danny-blip/NobleTeam/blob/main/10a.png "10a")
###### Se tardará unos minutos dependiendo de la capacidad de la USB.
