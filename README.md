# HelloWorld

## 2.- ¿Cómo montar y desmontar un usb en el sistema por terminal?
Lo primero que se debe hacer es conectar el dispositivo y que la máquina virtual lo reconozca, para verificar esto se utiliza el comando:

```bash
lsblk
```

Este comando enlista todos los dispositivos de bloque conectados, ya sea que estén montados o no.
# Imagen 1

En la figura podemos ver el disco USB que se conectó el cual recibe el nombre de sdb, esto es por la nomeclatura explicada con anterioridad. Se puede observar el campo de "MOUNTPOINT", si en esta sección no aparece ningún parámetro significa que el dispositivo no está montado.

Un concepto importante que conocer es los diferentes tipos de usuario. Para conocer el usuario que está logeado se utiliza el siguiente comando:
```bash
whoami

```

Al momento de abrir una nueva terminal por default, en este caso, el usuario que está logeado es maria, dicho usuario no tiene todos los permisos y para poder realizar los siguientes ejercicios es necesario tenerlos, ya que vamos a trabajar directamente con el hardware. Para poder logearse en el super usuario, se utiliza el comando:
```bash
sudo -s
```

Al momento de ejecutarlo pide la constraseña y una vez dad, se ingresa a root, root tiene todos los permisos. En la siguiente imagen, se puede ver aplicado lo anterior, se cambió el prompt por # y dice que nos encontramos en root.
# Imagen 2

En los siguientes puntos no se va a logear en root, se va a utilizar el comando "sudo" para poder ejecutar los comandos.

Para poder montar un dispositivo es necesario conocer el tipo de sistema de archivos en el cual está formateado, se utilizará el siguiente comando que nos proporciona la información necesaria para llevar a cabo esta operación y el ID del dispositivo: 
```bash
sudo blkid
```

En la figura podemos ver la información desplegada por el comando, es necesario buscar nuestro dispositivo, el cual como mencionamos anteriormente está con la etiqueta de sdb, se puede ver que el tipo de sistema de archivos para dicho dispositivo es "vfat".

Lo siguiente, con el objetivo de una mayor organización, es crear un espacio para montar el USB. Para esto, se crearán dos carpetas utilizando el comando:
```bash
mkdir
```
# Imagen 3

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

En la siguiente figura se observa el comando aplicado para nuestro ejemplo, donde el sistema de archivos es vfat y el dispositivo sdb, el cual se coloca en la carpeta creada anteriormente. Después se realiza un lsblk y efectivamente en MOUNTPOINT podemos ver que ya se encuentra montado

# Imagen 4

Para desmontarlo se utiliza la siguiente instrucción y se obtiene el siguiente resultado:
```bash
umount
```

# Imagen 5

En la figura, se puede ver el uso del comando para desmontar, seguido de lsblk para verificar que se haya desmontado y, en efecto, ya no aparece ningún parámetro en la sección de MOUNTPOINT

## 3.- Enlistar la información de los dispositivos de bloque conectados, aunque no estén montados en terminal

Para poder enlistar la información de los dispositivos de bloque conectados se utiliza el siguiente comando:
```bash
lsblk
```

Dicho comando da como resultado todos los dispositivos de bloque conectados a la computadora, da su nombre, el tamaño y el MOUNTPOINT, el cual es el lugar donde se ha montado dicho dispositivo. Si no hay ningún parámetro en la sección significa que no está montado.

# Imagen 6

## 4.- Mostrar la tabla de particiones del disco donde está instalado el sistema operativo en terminal

Para mostrar la tabla de particiones se utiliza el siguiente comando:
```bash
sudo fdisk -l "dispositivo"
```

# Imagen 7

En este caso al querer mostrar la tabla de particiones del disco donde está instalado el sistema operativo, en este caso Linux, nos vamos al disco de la máquina virtual, el cual tiene como etiqueta "sda". Nos muestra información importante como el tamaño del disco, el modelo del disco, el tamaño de los sectores, el tipo de etiqueta del disco, su identificador y por último la tabla de particiones. En la primera partición tiene la bandera de booteable, la segunda es una partición extendida y la tercera partición se encuentra dentro de la partición extendida.

## 5.- Conectar una memoria USB ("USB") y mostrar su tabla de particiones en terminal (hacer respaldo antes porque se va a borar toda la informacioón dentro del disco usb en pasos posteriores).

Para poder visualizar la tabla de particiones de nuestro disco USB, se ejecuta el siguiente comando:
```bash
sudo fdisk -l /dev/sdb
```
# Imagen 8

Nos da el tamaño de la usb en GB, en bytes y en sectores. El modelo de la usb es DataTraveler 2.0. Los sectores son de 512 bytes. Y nos da la tabla de particiones, el usb tiene 4 particiones, nos indica el inicio y final de cada partición, el tamaño, y el tipo, hay desconocida y de gestor de arranque. 

# 6.- Borrar todas las particiones del "USB" en terminal

Para borrar las particiones de la usb se utiliza el siguiente comando:
```bash
sudo fdisk "dispositivo"
```

Una vez que se ingresa se utiliza la letra "d", nos pide el número de partición que queremos borrar y se elimina, y así sucesivamente. Estos cambios son realizados en memoria, para escribirlos se utiliza la letra "w" y así se modifica la tabla de particiones de manera definitiva

# Imagen 9

# Imagen 10

Para verificar que se hayan eliminado, se enlista la tabla de particiones de nuevo, en la figura 10 se puede observar que ya no hay ninguna.
