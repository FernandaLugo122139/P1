# Práctica 1: Manejo de discos

##  1.- Identificar y describir las diferencias entre hda, sda y vda, además explicar qué significa la letra y el número al final de los identificadores (no requiere captura de pantalla).

* sda: Se usa para identificar algún dispositivo SCSI (Small Computer System Interface), esto también incluye los dispositivos SATA o discos USB removibles.

* hda: Se utiliza para denominar al disco maestro en el controlador IDE primario.

* vda: Son las siglas en inglés de Acceso al Escritorio Virtual (Virtual Desktop Access), este dispositivo permite que la máquina virtual se registre con el controlador, lo que permite que la máquina virtual y los recursos alojados para la misma estén disponibles.

* La letra que aparece luego de sda y hda es la letra utilizada para establecer el orden que llevan los discos. Siendo el primero en ser denominado por la letra 'a', el siguiente por la letra b y así consecutivamente.

* El numero que puede aparecer posteriormente señala la partición del disco.

## 2.- ¿Cómo montar y desmontar un usb en el sistema por terminal?
Lo primero que se debe hacer es conectar el dispositivo y que la máquina virtual lo reconozca, para verificar esto se utiliza el comando:

```bash
lsblk
```

Este comando enlista todos los dispositivos de bloque conectados, ya sea que estén montados o no.

![Imagen 01](/ImagesREADME/1.png)

En la figura podemos ver el disco USB que se conectó el cual recibe el nombre de sdb, esto es por la nomeclatura explicada con anterioridad. Se puede observar el campo de "MOUNTPOINT", si en esta sección no aparece ningún parámetro significa que el dispositivo no está montado.

Un concepto importante que conocer es los diferentes tipos de usuario. Para conocer el usuario que está logeado se utiliza el siguiente comando:
```bash
whoami

```

Al momento de abrir una nueva terminal por default, en este caso el usuario que está logeado es María, dicho usuario no tiene todos los permisos y para poder realizar los siguientes ejercicios es necesario tenerlos, ya que vamos a trabajar directamente con el hardware. Para poder logearse en el super usuario, se utiliza el comando:
```bash
sudo -s
```

Al momento de ejecutarlo pide la constraseña y una vez dada, se ingresa a root, donde se tienen todos los permisos. En la siguiente imagen, se puede ver aplicado lo anterior, se cambió el prompt por # y dice que nos encontramos en root.

![Imagen 02](/ImagesREADME/2.png)

En los siguientes puntos no se va a logear en root, se va a utilizar el comando "sudo" para poder ejecutar los comandos.

Para poder montar un dispositivo es necesario conocer el tipo de sistema de archivos en el cual está formateado, se utilizará el siguiente comando que nos proporciona la información necesaria para llevar a cabo esta operación y el ID del dispositivo: 
```bash
sudo blkid
```

![Imagen 03](/ImagesREADME/3.png)

En la figura podemos ver la información desplegada por el comando, es necesario buscar nuestro dispositivo, el cual como mencionamos anteriormente está con la etiqueta de sdb, se puede ver que el tipo de sistema de archivos para dicho dispositivo es "vfat".

Lo siguiente, con el objetivo de una mayor organización, es crear un espacio para montar el USB. Para esto, se crearán dos carpetas utilizando el comando:
```bash
mkdir
```

![Imagen 04](/ImagesREADME/4.png)

Para poder montarlo se utiliza el comando:
```bash
sudo mount -t "type" -o rw, umask=0 "origen" "destino"
```
- Sudo: Permisos de super usuario.
- Mount: Comando para montar el dispositivo.
- Type: Se refiere al tipo de sistema de archivos.
- rw: Permisos de lectura y escritura.
- Umask = 0: Quiere decir que se de permiso de lectura y escritura para todos los usuarios.
- Origen: El lugar donde se encuentra el dispositivo que se quiere montar.
- Destino: El lugar donde queremos montarlo.

En la siguiente figura se observa el comando aplicado para nuestro ejemplo, donde el sistema de archivos es vfat y el dispositivo sdb, el cual se coloca en la carpeta creada anteriormente. Después se realiza un lsblk y efectivamente en MOUNTPOINT podemos ver que ya se encuentra montado.

![Imagen 05](/ImagesREADME/5.png)

Para desmontarlo se utiliza la siguiente instrucción y se obtiene el siguiente resultado:
```bash
umount
```

![Imagen 06](/ImagesREADME/6.png)

En la figura, se puede ver el uso del comando para desmontar, seguido de lsblk para verificar que se haya desmontado y, en efecto, ya no aparece ningún parámetro en la sección de MOUNTPOINT.

## 3.- Enlistar la información de los dispositivos de bloque conectados, aunque no estén montados en terminal

Para poder enlistar la información de los dispositivos de bloque conectados se utiliza el siguiente comando:
```bash
lsblk
```

Dicho comando da como resultado todos los dispositivos de bloque conectados a la computadora, da su nombre, el tamaño y el MOUNTPOINT, el cual es el lugar donde se ha montado dicho dispositivo. Si no hay ningún parámetro en la sección significa que no está montado.

![Imagen 07](/ImagesREADME/7.png)

## 4.- Mostrar la tabla de particiones del disco donde está instalado el sistema operativo en terminal

Para mostrar la tabla de particiones se utiliza el siguiente comando:
```bash
sudo fdisk -l "dispositivo"
```

![Imagen 08](/ImagesREADME/8.png)

En este caso al querer mostrar la tabla de particiones del disco donde está instalado el sistema operativo, en este caso Linux, nos vamos al disco de la máquina virtual, el cual tiene como etiqueta "sda". Nos muestra información importante como el tamaño del disco, el modelo del disco, el tamaño de los sectores, el tipo de etiqueta del disco, su identificador y por último la tabla de particiones. En la primera partición tiene la bandera de booteable, la segunda es una partición extendida y la tercera partición se encuentra dentro de la partición extendida.

## 5.- Conectar una memoria USB ("USB") y mostrar su tabla de particiones en terminal (hacer respaldo antes porque se va a borar toda la informacioón dentro del disco usb en pasos posteriores).

Para poder visualizar la tabla de particiones de nuestro disco USB, se ejecuta el siguiente comando:
```bash
sudo fdisk -l /dev/sdb
```

![Imagen 09](/ImagesREADME/9.png)

Nos da el tamaño de la usb en GB, en bytes y en sectores. El modelo de la usb es DataTraveler 2.0. Los sectores son de 512 bytes. Y nos da la tabla de particiones, la usb tiene 4 particiones, nos indica el inicio y final de cada partición, el tamaño, y el tipo, hay desconocida y de gestor de arranque. 

## 6.- Borrar todas las particiones del "USB" en terminal

Para borrar las particiones de la usb, se utiliza el siguiente comando:
```bash
sudo fdisk "dispositivo"
```

Una vez que se ingresa se utiliza la letra "d", nos pide el número de partición que queremos borrar y se elimina, y así sucesivamente. Estos cambios son realizados en memoria, para escribirlos se utiliza la letra "w" y así se modifica la tabla de particiones de manera definitiva.

![Imagen 10](/ImagesREADME/10.png)

![Imagen 11](/ImagesREADME/11.png)

Para verificar que se hayan eliminado, se enlista la tabla de particiones de nuevo, en la figura 10 se puede observar que ya no hay ninguna.

## 7.- Crear en la "usb" tres particiones físicas y una extendida en terminal.

Para poder crear particiones, primero se debe ejecutar el siguiente comando:
```bash
sudo fdisk "Dispositivo"
```

Una vez dentro, se utiliza la letra "n" para crear una partición. Se indica que tipo de partición se requiere, si primaria ("p") o extendida ("e"), se asigna el número de partición y el tamaño que queremos para dicha partición. En la figura se muestra cómo se fueron creando 3 físicas y una extendida. Se utiliza la letra "p" para ver como quedó la tabla de particiones y por último para escribir los cambios se utiliza la letra "w".

![Imagen 12](/ImagesREADME/12.png)

![Imagen 13](/ImagesREADME/13.png)

## 8.- Crear una partición dentro de la partición extendida del "usb" en terminal

![Imagen 14](/ImagesREADME/14.png)

Se utiliza el comando:
```bash
sudo fdisk /dev/sdb
```

Se ingresa la opción de crear una nueva partición con "n" y nos indica que, al estarse usando todas las particiones primarias, esta nueva partición va a ser una lógica la cuál va a formar parte de la extendida. Se asigna el tamaño de la partición, ya sea el valor predeterminado o personalizado. Con "p" podemos observar la tabla de particiones y ver que se crea la lógica y es parte de la extendida. Finalmente presionamos "w" para guardar y salir.

## 9.- En la interfaz gráfica de la aplicación disks, borrar las particiones para que sólo exista una partición que abarque toda la "usb".

![Imagen 15](/ImagesREADME/15.png)

![Imagen 16](/ImagesREADME/16.png)

Entramos a la interfaz gráfica llamada disks, se selecciona la partición y en el icono “-“ se le da borrar partición, se confirma la acción y así sucesivamente, hasta tener la usb completamente libre como se muestra en la siguiente figura:

![Imagen 17](/ImagesREADME/17.png)

Una vez hecho esto, se crea una sola partición que abarque toda la usb, para esto, se presiona el icono “+”. Se define el nombre, el tipo de sistema de archivos y el tamaño, como se muestran en las siguientes capturas:

![Imagen 18](/ImagesREADME/18.png)

![Imagen 19](/ImagesREADME/19.png)

Y listo, se ha creado la partición.

![Imagen 20](/ImagesREADME/20.png)

## 10.- Copiar un archivo .iso de ditribución live de linux a la usb por medio del comando "dd"

Para poder copiar un archivo .iso a la usb es necesario utilizar el siguiente comando:
```bash
sudo dd if= “Direccion donde se encuentra el archivo .iso” of=”Direccion donde se encuentra la usb” bs=”Velocidad a la cual se lee y se escribe al mismo tiempo” status=progress
```

![Imagen 21](/ImagesREADME/21.png)

Podemos visualizar mediante el ambiente gráfico que se ha copiado correctamente.

![Imagen 22](/ImagesREADME/22.png)
