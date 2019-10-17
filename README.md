# Manual de Instalación | Gentoo | Virtual Box
## Datos de Estudiante
* Ricardo Antonio Cutz Hernández
* 201503476
* Práctica 2

## Iniciando la Instalación
* Debemos iniciar con el default kernel:
```bash
$ gentoo
```
* Nos mostrara la siguiente pantalla:

![Pantalla de inicio](imgs/i1.PNG)


* Configuracion de la conexion de la red
```bash
$ ifconfig 

$ ping google.com
```
![Comprobacion de Red](imgs/i2.PNG)

## Configurando Usuarios

* Instalaremos Sudo en Gentoo
```bash
$ PENDIENTE
```
* Haremos el cambio de password del usuario root:
```bash
$ passwd
```
![Cambiando password](imgs/i3.PNG)

* Agregaremos al usuario 201503476
```bash
$ useradd -m -G users,wheel,audio,video u201503476
$ passwd u201503476
```

## Particiones para el Disco
Para crear las particiones utilizaremos parted
* Ingresaremos al programa:
```bash
$ parted -a optimal /dev/sda
```

### Definiendo el etiquetado y Configuraciones iniciales
* usaremos el etiquetado gpt
```bash
(parted)$ mklabel gpt
```

* definieremos la unidad de medida
```bash
(parted)$ unit mib
```

### Creando las particiones del disco
#### GRUB
* Para crear la particion grub ejecutaremos lo siguiente:

![Particion](imgs/i4.PNG)

* La particion anterior tiene un espacio de 2MB, usamos desde el Mb 1 al 3

#### Arranque
* Crearemos una particion de Arranque (128Mb):
```
(parted)$ mkpart primary 1 131
(parted)$ name 2 boot
```
#### Swap
* Crearemos una particion SWAP del doble de la ram (4096Mb)
```
(parted)$ mkpart primary 131 4227
(parted)$ name 3 swap
```
#### Root File System
* Crearemos la particion para el resto del disco
```
(parted)$ mkpart primary 4227 -1
(parted)$ name 3 rootfs
```

#### Resultado de las particiones:
![tabla](imgs/i5.PNG)

### Sistema de Archivos
* Listamos las particiones con el sisguiente comando
```
$ fdisk -l
```
![tba](imgs/i6.PNG)

* Crearemos un sistema de archivos ext4 en la particion /dev/sda4
```
$ mkfs.ext4 /dev/sda4
```
![tba](imgs/i7.PNG)
* Crearemos swap:
```
$ mkswap /dev/sda3
```
![tba](imgs/i8.PNG)

### Montando particion al Root de Gentoo
```bash
$ mount /dev/sda4 /mnt/gentoo
```

## Instalando el Stage 3
* Para realizar este paso puede dirigirse a la documentacion:
```
https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage/es
```

## Kernel de Linux

1. Instalando las fuentes del Kernel
```
$ emerge --ask sys-kernel/gentoo-sources
```
![tba](imgs/i9.PNG)

2. Instalando un modulo para informacion de pc
```
$ emerge --ask sys-apps/pciutils
```
![tba](imgs/i10.PNG)

### Usando genkernel
1. para utilizar el genkernel
```
$ emerge --ask sys-kernel/genkernel
```

2. editamos un archivo para darle la configuracion de boot
```
/dev/sda2	/boot	ext2	defaults	0 2
```

3. compilamos el kernel
```
$ genkernel all
```
![tba](imgs/i11.PNG)

4. Kernel Compilado exitosamente

![tba](imgs/i12.PNG)

5. Demás configuraciones puede consultar la documentación para ser más especifica.

## Instalación de Requerimientos de Práctica

### Conexion a Internet
1. Instalaremos el paquete 'dhcpcd'
```
$ emerge --ask dhcpcd
```
![tba](imgs/i14.PNG)

2. Buscamos nuestra interfaz
```
$ ip addr
```

![tba](imgs/i15.PNG)

3. Configuramos la interfaz que vemos en la salida anterior
```
$ dhcpcd enp0s3
```

![tba](imgs/i16.PNG)

4. Comprobamos conectividad

![tba](imgs/i17.PNG)

## Instalacion de SUDO y Creacion de Grupo
1. Instalamos SUDO con el siguiente comando

```
$ emerge --ask app-admin/sudo
```

![tba](imgs/i17.PNG)

2. Creamos el grupo Sopes2

```
$ groupadd sopes2
```

3. Agregamos root y u201503476 al grupo Sopes2
```
$ usermod -a -G sopes2 root
$ usermod -a -G sopes2 u201503476
```

## Creacion de la carpeta: practica1
1. Creamos la carpeta:
```
$ sudo mkdir practica1
```

2. Cambiamos el propietario de la carpeta
```
$ sudo chown -R u201503476:sopes2 practica1/
```

3. Para dar permisos de lectura, escritura y ejecucion al owner
```
$ sudo chmod +rwx practica1/
```

4. Para dar permisos de lectura, escritura y ejecucion al grupo
```
$ sudo chmod g+rwx practica1/
```

## Instalaciones de aplicaciones faltantes

1. Linux Headers
```
$ emerge --ask sys-kernel/linux-headres
```
2. Make
```
$ make --version
```
3. Perl
```
$ emerge --ask dev-lang/perl
```
4. gcc
```
$ gcc --version
```
5. build-essential

## Modificacion de parametros de kernel

![tba](imgs/i18.PNG)

## Screenshots
1. uname -a 

![tba](imgs/i19.PNG)

2. hostname

![tba](imgs/i20.PNG)

3. sysctl -a | grep sem

![tba](imgs/i21.PNG)

4. sysctl -a | grep shm

![tba](imgs/i22.PNG)

5. sysctl -a | grep file

![tba](imgs/i23.PNG)

6. xclock

![tba](imgs/i24.PNG)

7. cat /etc/passswd

![tba](imgs/i25.PNG)

8. car /etc/Group

![tba](imgs/i26.PNG)

9. free -m 

![tba](imgs/i27.PNG)

10. df -h

![tba](imgs/i28.PNG)

11. ping -c3 google.com

![tba](imgs/i29.PNG)