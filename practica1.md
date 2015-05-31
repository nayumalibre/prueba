
#Diplomado Software Libre GNU/LINUX - EGPP
## Modulo II
##Docente David Mendoza Paco

```
Contenido
1.	INSTALACION
2.	COMANDOS GENERALES
3.	CONFIGURACION DE RED
4.	REPOSITORIOS
5.	GITHUB
6. 	TECNOLOGIA DE VIRTUALIZACION KVM
7. 	INSTACION DE DEBIAN EN DISCO VIRTUAL
8. 	CREACION DE GRUPO
9. 	CREACION DE VOLUMENES LOGICOS
10. CONFIGURACION DINAMICA DE DHCP
11. REDIMESION DEL DISCO VIRTUAL
12. UTILITARIO EN MODO GRAFICO
13. CONFIGURACION DEL DNS
14. MANEJO DEL SSH
```

#INSTALACION DEBIAN JHESSY 8.0
## Ingresar al SETUP del Equipo
###Ir al menú de Boot
### Establecer el CD ROM como primario
```
También se puede iniciar desde USB cuyo caso se asignaría como PRIMARIO8
```
*Colocar el CD ROM instalador de Debian e iniciar la instalación
*Presionar ENTER en la opción resaltada INSTALL
* seleccionar  el lenguaje a ser usado (ESPAÑOL)
*seguidamente seleccionamos el país (BOLIVIA)
*posterior seleccionamos el Teclado a usar, recomendando que si es teclado para español elegir la opción LATINOAMERICANO
*Posteriormente emergerá un mensaje para configurar la red, se debe saltar este paso
*luego nos solicitara la contraseña del usuario ROOT
*ingresamos y confirmamos el PASSWORD
```
Para asignar la contraseña recordamos que el mismo sea de acuerdo a las buenas practicas establecidas en COBIT 
``` 
*Asignar el nombre completo del usuario
*A continuación nos sugiere el primer nombre del usuario descrito en el paso anterior, podemos ratificarlo o escribir uno nuevo.
*ingresamos y confirmamos el PASSWORD
###PARTICION DE DISCO
*existen diferentes opciones para la partición del disco, el mismo esta sujeto al uso que se le va a dar al equipo y/o a los sistemas pre instalados en el mismo, sin embargo se recomienda realizar el procedimiento eligiendo la forma MANUAL, 
establecidas en COBIT 
``` 
Para este ejemplo haremos uso de una particion de 100
establecidas en COBIT 
``` 
##Partición para el BOOT
a) Elegimos nueva partición de tipo PRIMARIO y elegir la opcion al PRINCIPIO
b) Asignamos el tamaño de la particion 2GB
c) 




      1. instalación manual
1.1 selección de disco duro virtual
1.2 si 
2. seleccionar espacio libre
2.1 crear una partición
2.1.1. le damos 256 MB (Dentro de ella se almacena el kernel de Linux)
2.1.1.1. partición primaria y principio
2.1.2. utilizar como: sistema de fichero ext2 (modo arranque rapido como FAT)
2.1.3. punto de montaje boot y finalizar la partición
3. Creando memoria virtual (si se tiene RAM de 4 GB o menos recomendable es de 2 gb de swap) 
3.1 Seleccionar espacio libre y nueva partición le damos 512 MB
3.2 que sea de partición lógica y principio
3.3 partición de disco como área de intercambio (Memoria virtual) y finalizar
4. Creando otra partición de 1 BG
4.1 lógica, principio de tipo ext4 
4.2 sistema de ficheros raíz  y finalizar.
5. Configurar el gestor de volúmenes lógicos (LVM)
5.1 le damos si a guardar cambio
5.2 Crear Grupo de Volúmenes “nombre”
5.3 Seleccionamos la partición libre
5.4 guardar cambios y crear Volumen logico, seleccionar el grupo
5.5 home le damos 500 MB y continuar
5.6 Crear otro volumen lógico con el nombre tmp con 200 MB.
5.7 En este caso solo se esta creando un grupo y dos particiones lógicas (home y tmp)
5.8 Terminar
6. Montamos las particiones
6.1 para home utilizar sistema ext4 y punto de montaje a home y terminar
6.2 para tmp utilizar ext4 y punto de montaje /tmp
6.3 y finalizar la partición
7. GRUB si, para el arranque del sistema
8. Para ver pariciones: df -h
9. volumenes de grupo: vgdisplay 
10. volumnes logicos de lvm: lvdisplay
11. pc aux

Desaparecer modo avión gnome- control-central 

Jueves 21 de Mayo
1. Sacamos una copia de la imagen (ubicamos el lugar de instalacion)
1. ls -l (para ver el archivo)
2. sudo cp debian.img debian.img.bak => COPIA
2. Ver particiones df -h
1. aumentamos memoria para tmp 100 megas mas
2. vgdisplay (para ver espacio libre)
3. vgs para ver del grupo
4. lvs Muestra la datos de los discos
5. lvextend -L+100M (/dev/mapper/gpsistemas-tmp)
6. ahora redimencionamos sistemas de archivos
7. umount /tmp (desmontamos la particion para que actualice los datos) si no funciona forzamos a que se desmonte umoun -l /tmp
8. e2fsck -f  /dev/mapper/gpsistemas-tmp (para ver si todo esta correcto)
9. resize2fs dev/mapper/gpsistemas-tmp (para redimencionar)
10. mount /tmp (montamos la particion) y df -h para ver estado de la particion
11. Aumentamos par home /dev/mapper/gpsistemas-home
3. Reducimos tamaño a tmp
1. umount /dev/mapper/gpsistemas-tmp
2. e2fsck -f /dev/
3. resize2fs /dev/mapper/gpsistenas-tmp 100M
4. mount /tmp
5. lvreduce -L 100M /dev/mapper/gpsistemas/tmp

Viernes 22 de Mayo
1. Aumentamos disco, almacenamiento, nuevo volumen, dar nombre, formato qcow2 (para q sea dinamico) de 2 GB
2.  En la maquina virtual le damos abrir (buscar el foquito) abajo en adicionar hardware (storage), en explorar buscamos el nuevo disco creado.
3. En tipo dispositivo Virtio disk
4. Modo cache: default
5. storage format: raw
6. TEMA DISCO
1. fdisk -l (| more)
2. cfdisk /dev/vdb 
3. si sale 0 MB ir a VITIO DISK 2, DISK BUS: SCSI. Storage format qcow2
4. cfdisk /dev/sda 
5. vgdisplay (mestra el numero de grupos)
6. pvdisplay (muestra donde esta el grupo de sistemas /dev/vda7)
7. ahora crear un nuevo grupo.
1. Crear particion fisica: pvcreate /dev/sda
2. cfdisk /dev/sda (creamos nueva particion 1BG)
1. tipo primario, 1GB, principio
2. opcion escribir
3. fdisk -l /dev/sda (para la informacion)
3. ahora: pvcreate /dev/sda1 (Crear fisico)
4. pvdisplay
5. CREAMO NUEVO VOLUMEN
1. vgcreate grupoA /dev/sda1 (Segun el nombre en este caso sda1)
2. lvcreate -L 500M -n volumen01 grupoA
1. lvs (informacion de los grupos)
3. mkfs.ext3 -m 0 /dev/mapper/grupoA-volumen01 (-m 0.. cero bloques) (es como formatear el disco)
4. mkdir /mnt/volument01 (creamos carpeta para montar la particion)7
5. mount /dev/mapper/grupoA-volumen01 /mnt/volumen01
6. mkdir -p /mnt/volumen-01/datos/{a1,a2,a3,b1,b2}
7. reiniciar el equipo
8. nano /etc/fstab (permite montar automaticamente las particiones)
1. dev/mapper/gpsistemas-home
2. dev-mapper/gpsistemas-tmp /tmp
3. dev/mapper/grupoA-volumen01 /mnt/voumen-01 ext3 defaults$
9. TAREA
1. Crear grupoB de 1024M
2. crear volumen log vol-b01 de 700M
3. crear volumen log vol-b02 de 200M

Lunes 25 de Mayo
REDIMENCIONANDO DISCO 
	1. qemu-img resize “RUTA IMG” +7G (En terminal principal)
2. cfdisk /dev/sda (en virtual)
3. fdisk -l /dev/sda 
3. 192.168.55.100/iso (Desgargamos archivo) copiamos el archivo donde esta el repositorio
4. en la terminal principal (apagar maquina) adicionar storage, Tipo Dsipositivo CDROM y seleccionar el archivo, en la maquina virtual seleccionar BOOT OPTIONS DCROM (bootear con gparted.iso)
1. Instalamos Seleccionar segunda Opcion (DONT....)  
2. 25 (idioma)
3. 0 modo grafico, Hacemos las acciones para redimencionar el disco
1. expandir la particion extendidad desde el modo grafico
2. expandir la particion LVM, desde modo grafico
4. fdisk -l /dev/sda
1. PRACTICA
1. aumentar tamaño del disco duro virutual a 15 G y crear un nuevo grupo LVM “Servidores”


Martes 26 de Mayo
REDES Y DHCP
1. /etc/network/interfaces
2. gnome-control-center network
3. nm-connection-editor (muestra consola de Conexiones de red)
4. ls /sys/class/net (muestra las interfaces de red de la maquina)
5. apagando la red cirtual en Network Agregar hardware Red Virtual Dfault. (PARA PRUEBA)
6. nano /etc/network/interfaces (Conf de red)
7. para virtual quitar allow-hotplug
8. reiniciar servicio => service networking restart o /etc/init.d/networking restart
9. ip addr show => muestra todas las interfaces o ip addr show eth0
10. CONFIGURANDO MANUAL
1. auto eth0
2. iface eth0 inet static
3. address 192...
4. netmask 255..
5. gateway 192,168
6. CREAR RED VIRTUAL
1. En redes virtuales
2. agregar red virtual
3. Desabilitar DHCP
4. Reenvio a la red fisica
1. Cualquier Dispositivo
7. para ver el numero de error => nano +9 /etc/network/interfaces
8. nano /etc/resolv.conf => configurando DNS (mismo del gateway por ser virtual)
9. nano /etc/apt/sources.list (confituramos el repositorio) http://192,168,55,2/ftp.us.debian.org/debian wheezy main contrib (luego apt-get update)
10. REMOTAMENTE
1. apt-get install ssh (instalamos ssh)
2. netstat -natp => para ver puertos disponibles
3. para ver solo el puerto => netstat -natp | grep 22
4. nano -c  /etc/ssh/sshd_config => buscamos PubkeyAuthentication (linea 31 cambiar por no, para que no exija clave publica)
5. service ssh restart => para guardar cambios
6. En terminal de la maquina princiapal => ssh edwin@192,168,3,100
11. En maquina principal => echo “hola mundo” >  hola.txt
12. scp hola.txt root@192.168.3.100:/tmp => copia el archivo remotamente al virtual
13. mkdir -p /tmp/datos_egpp/{aaa,bbb,ccc,ddd}
14. scp -r /tmp/datos_egpp/ root@192.168.3.100:/srv
15. en maquina virtual desbilitar el root => en la linea 27 desabilitar PermitRootLogin no
16. reinicio => service ssh restart

Miercoles 27 de Mayo
1. de la maquina principal ingresamos por ssh
2. mkdir -p /tmp/test/{aaa,bbb,ccc,ddd} en virtual
3. scp -r /tmp/test/ root@192.168.55,4:/tmp
4. whoo => usuarios conectados desde la virtual
5. en maquina princiapal abrir terminarl para crear llave publica e ingresar solo una vez
1. mkdir ~/.ssh
2. ssh-keygen -t rsa -b 2048 (el archivo por defecto y le se crea una contraseña)
3. ls -l ~/.ssh => muestra las llaves privadas
4. ssh-copy-id -i /home/edwin/.ssh/id_rsa.pub root@192.168.3.100 O ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.3.100=> copia de archivo de la clave al remoto
5. en maquina virtual ls -l /root/.ssh
6. En Mauina virtual
1. nano -c /etc/ssh/ssh_config => linea 31  PubkeyAuthentication . cambiar a yes
2. service ssh restart
7. para ver si las copias estan correctas
1. head ~/.ssh/authorized_keys => tomar encuenta la ruta en principal y virutal, los numeros deben ser iguales
8. conectamos a la maquina virtual
1. ssh -i ~/.ssh/id_rsa edwin@192.168.3.100
2. y no debe pedir contraseña
9. sudo apt-get install terminator (en caso de no ser root usar sudo)
10. PERMISOS
1. r read => 4
2. w write => 2
3. x ejecution => 1
1. chmod o+w prueba/
2. chmod o-rwx prueba/ => se agrega a (o otros)
3. chmod -rwx prueba/ => quita todos los permisos
4. chmod 437 prueba/ => 
5. chown root:edwin prueba/ => permite cambiar de usuario
6. CREAR USUARIO
1. sudo useradd usuario1
2. sudo passwd usuario1
3. getent passwd => muestra los usuarios del sistema
4. sudo su – usuario1
5. USUARIO CON OPCIONES
1. sudo adduser usuario2
11. davis.men.pa@gmail.com

EXAMEN
1. Crear la red virtual 192.168.40.0
2. Asignar la direccion IP: 192.168.40.150 al VM
3. Crear el grupo LVM 'egpp' en la VM => vgremove my_volume_group
4. Crear el volumen logico 'vol100' en 'egpp'

TAREAS

Documentar todo el proceso en github, con capturas de pantalla
1. Crear un disco duro virual de 8 G
2. Crear el grupo LVM 'sistemas-kvm01'
3. Crear el grupo LVM 'sistemas-kvm02'
4. Crear 3 volumenes logicos dentro de 'sistemas-kvm'