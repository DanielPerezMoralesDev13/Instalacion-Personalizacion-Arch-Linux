<!-- Autor: Daniel Benjamin Perez Morales -->
<!-- GitHub: https://github.com/DanielPerezMoralesDev13 -->
<!-- Correo electrónico: danielperezdev@proton.me  -->

# ***Instalacion desde la uefi de Arch Linux***

**Indices:**

- [***Instalacion desde la uefi de Arch Linux***](#instalacion-desde-la-uefi-de-arch-linux)
  - [***Configurar la distribucion del teclado***](#configurar-la-distribucion-del-teclado)
  - [***Conexión Inalámbrica***](#conexión-inalámbrica)
  - [***Crear y formatear particiones (comandos usados en mi caso)***](#crear-y-formatear-particiones-comandos-usados-en-mi-caso)
  - [***Instalar sistema***](#instalar-sistema)
  - [***Configurar sistema***](#configurar-sistema)
  - [***Instalar Entorno de Escritorio KDE-Plasma***](#instalar-entorno-de-escritorio-kde-plasma)
  - [***Configurar Particiones (Reconozca Windows, y acceder asus discos)***](#configurar-particiones-reconozca-windows-y-acceder-asus-discos)

## ***Configurar la distribucion del teclado***

```bash
loadkeys es
```

```bash
loadkyes la-latin1
```

> *El comando `loadkeys` es utilizado en sistemas operativos basados en Linux para cargar o cambiar la configuración del teclado.*

- *Cuando ejecutas `loadkeys es`, estás indicando que deseas cargar la configuración del teclado español. Esto significa que el sistema utilizará el layout de teclado correspondiente al español, con las teclas configuradas de acuerdo a este idioma.*

- *Del mismo modo, al ejecutar `loadkeys la-latin1`, estás cargando la configuración del teclado para el layout latino1. Este layout está diseñado para lenguajes basados en el alfabeto latino, como el español, el francés, el italiano, entre otros.*

## ***Conexión Inalámbrica***

> *Este conjunto de comandos está relacionado con la gestión de redes inalámbricas en sistemas Linux utilizando `iwctl`, una herramienta de línea de comandos para controlar y configurar dispositivos Wi-Fi. Aquí está la explicación de cada comando:*

- *`iwctl`: Este es el comando principal que invoca la herramienta `iwctl`, que es una interfaz de línea de comandos para configurar y administrar conexiones Wi-Fi en sistemas Linux.*

```bash
iwctl
```

- *`device list`: Este comando lista todos los dispositivos Wi-Fi disponibles en el sistema. Proporciona información sobre los adaptadores inalámbricos presentes en la computadora.*

```bash
device list
```

- *`station Nombre_Dispositivo scan`: Este comando solicita al dispositivo Wi-Fi especificado (identificado por `Nombre_Dispositivo`) que realice un escaneo de redes disponibles. Esto recopila información sobre las redes inalámbricas a las que el dispositivo puede conectarse.*

```bash
station Nombre_Dispositivo scan  # El nombre sale del comando anterior
```

- *`station Nombre_Dispositivo get-networks`: Después de realizar el escaneo, este comando muestra una lista de las redes Wi-Fi detectadas por el dispositivo especificado (`Nombre_Dispositivo`). Proporciona información detallada sobre cada red, como su nombre (SSID), intensidad de la señal, tipo de seguridad, etc.*

```bash
station Nombre_Dispositivo get-networks
```

- *`station Nombre_Dispositivo connect Nombre_Router`: Este comando establece una conexión a una red Wi-Fi específica llamada `Nombre_Router` a través del dispositivo especificado (`Nombre_Dispositivo`). Esto configura el dispositivo para conectarse a la red inalámbrica deseada.*

```bash
station Nombre_Dispositivo connect Nombre_Router
```

- *`exit`: Este comando sale de la interfaz de línea de comandos `iwctl`, regresando al símbolo del sistema habitual.*

```bash
exit
```

- *`ping -c 1 archlinux.org`: Este comando envía un solo paquete de solicitud de eco ICMP (también conocido como "ping") al dominio archlinux.org. La opción `-c 1`. Este comando se usa comúnmente para verificar la conectividad de red con un host remoto. Si todo está configurado correctamente y el host es accesible a través de la red, deberías recibir una respuesta del host.*

```bash
ping -c 1 archlinux.org
```

- *`timedatectl set-ntp true`: Este comando configura el servicio de protocolo de tiempo de red (NTP, por sus siglas en inglés) para sincronizar automáticamente el reloj del sistema con servidores NTP externos. NTP se utiliza para sincronizar el reloj del sistema con una fuente de tiempo confiable en Internet. Al configurar esto en "true", estás activando la sincronización automática del tiempo del sistema.*

```bash
timedatectl set-ntp true
```

## ***Crear y formatear particiones (comandos usados en mi caso)***

- *El comando `lsblk` se utiliza para listar información sobre todos los dispositivos de bloque (como discos duros y unidades USB) disponibles en el sistema, así como sus particiones. Al ejecutar `lsblk`, obtendrás una lista que muestra los dispositivos y sus particiones, junto con información como su tamaño, tipo de disco y sistema de ficheros.*

```bash
lsblk
```

```bash
lsblk /dev/nvme1n1
```

- *El comando `cfdisk` se utiliza para particionar discos en sistemas Linux. Al ejecutar `cfdisk`, se abrirá una interfaz de usuario en la terminal que te permite crear, eliminar y modificar particiones en un disco específico. El valor por defecto que toma `cfdisk` puede variar según el sistema y la configuración específica, pero generalmente se elige la opción más segura o recomendada por el sistema.*

```bash
cfdisk
```

```bash
cfdisk /dev/nvme1n1
```

- *El comando `mkfs.fat` se utiliza para formatear una partición con el sistema de ficheros FAT32.*

```bash
mkfs.fat -F 32 /dev/nvme1n1p1
```

- *El comando `mkfs.ext4` se utiliza para formatear una partición con el sistema de ficheros ext4, que es comúnmente utilizado en sistemas Linux.*

```bash
mkfs.ext4 /dev/nvme1n1p2
```

```bash
mkfs.ext4 /dev/nvme1n1p3
```

- *El comando `mkswap` se utiliza para configurar una partición como área de intercambio en sistemas Linux. Esta área de intercambio se utiliza para aumentar la memoria virtual del sistema.*

```bash
mkswap /dev/nvme1n1p4
```

- *El comando `swapon` se utiliza para activar la partición de intercambio configurada anteriormente.*

```bash
swapon /dev/nvme1n1p4
```

- *El comando `mount` se utiliza para montar una partición en un directorio específico del sistema de ficheros. Por ejemplo, `mount /dev/nvme1n1p2 /mnt` montará la partición `/dev/nvme1n1p2` en el directorio `/mnt`.*

```bash
mount /dev/nvme1n1p2 /mnt
```

```bash
mkdir /mnt/home
```

```bash
mount /dev/nvme1n1p3 /mnt/home
```

```bash
mkdir /mnt/boot
```

```bash
mount /dev/nvme1n1p1 /mnt/boot
```

- *Los comandos `mkdir` y `--mkdir` se utilizan para crear directorios en el sistema de ficheros. Por ejemplo, `mkdir /mnt/home` crea un directorio llamado "home" dentro de "/mnt". En cambio, `--mkdir` se utiliza en combinación con `mount` para crear automáticamente el directorio de montaje si no existe previamente.*

```bash
mount -m /dev/nvme1n1p3 /mnt/home
```

```bash
mount --mkdir /dev/nvme1n1p3 /mnt/home
```

```bash
mount -m /dev/nvme1n1p1 /mnt/boot
```

```bash
mount --mkdir /dev/nvme1n1p1 /mnt/boot
```

## ***Instalar sistema***

```bash
pacstrap -K /mnt base linux linux-firmware
```

- **`pacstrap -K /mnt base linux linux-firmware`**:

   > *Este comando instala un conjunto básico de paquetes en un directorio raíz especificado. Aquí está la explicación de cada parte:*

  - *`pacstrap`: Es el comando que se utiliza en Arch Linux para instalar paquetes en un sistema de ficheros raíz.*

  - *`-K`: Es una opción que indica a `pacstrap` que debe utilizar la cache de paquetes descargados previamente si están disponibles. Esto puede ayudar a acelerar el proceso de instalación si ya has descargado los paquetes previamente.*

  - *`/mnt`: Es el directorio raíz en el que se instalarán los paquetes. Es importante asegurarse de que este directorio esté correctamente montado antes de ejecutar `pacstrap`.*

  - *`base linux linux-firmware`: Son los nombres de los paquetes que se van a instalar. `base` es un grupo de paquetes que incluye el mínimo necesario para tener un sistema funcional. `linux` es el kernel de Linux y `linux-firmware` son los firmware necesarios para el hardware.*

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

- **`genfstab -U /mnt >> /mnt/etc/fstab`**:

   > *Este comando genera una tabla de montaje (fstab) para el sistema de ficheros raíz y la agrega al fichero fstab dentro del directorio raíz. Aquí está la explicación de cada parte:*

  - *`genfstab`: Es el comando que se utiliza en Arch Linux para generar la tabla de montaje.*

  - *`-U`: Es una opción que indica a `genfstab` que utilice UUIDs en lugar de rutas de dispositivo para identificar las particiones en la tabla de montaje. Usar UUIDs es una práctica recomendada ya que garantiza que los dispositivos se monten correctamente incluso si los nombres de los dispositivos cambian.*

  - *`/mnt`: Especifica el directorio raíz del sistema de ficheros para el que se generará la tabla de montaje.*

  - *`>> /mnt/etc/fstab`: Redirige la salida del comando `genfstab` al fichero fstab dentro del directorio raíz. El operador `>>` agrega la salida al final del fichero fstab en lugar de sobrescribirlo. Esto es importante para conservar cualquier configuración existente en fstab.*

## ***Configurar sistema***

```bash
arch-chroot /mnt
```

> *El comando `arch-chroot` se utiliza para cambiar el entorno de raíz del sistema actual al directorio especificado, lo que te permite trabajar dentro de un sistema de directorios raíz montado sin salir del sistema actual. Aquí está la explicación de cada parte del comando:*

- *`arch-chroot`: Es el comando utilizado en Arch Linux para cambiar el entorno de raíz.*

- *`/mnt`: Es el directorio al que se cambiará el entorno de raíz. En este caso, se asume que `/mnt` es el directorio donde has montado el sistema de directorios raíz de tu instalación de Arch Linux.*

- *Cuando ejecutas este comando, se cambia el directorio raíz del sistema actual al directorio especificado (`/mnt` en este caso). Esto significa que todos los comandos que ejecutes a partir de ese momento se realizarán dentro del contexto del sistema de directorios raíz en `/mnt`. Esto es útil cuando necesitas realizar tareas de configuración o mantenimiento en la instalación de Arch Linux que está montada en `/mnt`, como instalar o configurar paquetes, configurar el gestor de arranque, entre otros.*

```bash
ln -sf /usr/share/zoneinfo/America/Manangua /etc/localtime
```

> *El comando `ln` crea enlaces entre ficheros. La opción `-sf` significa:*

- *`-s`: Crea un enlace simbólico en lugar de una copia física del fichero.*

- *`-f`: Elimina cualquier fichero existente con el mismo nombre de destino antes de crear el enlace.*

**La sintaxis general del comando `ln` es:**

```bash
ln [opciones] fichero_existente enlace_creado
```

```bash
hwclock --systohc
```

> *El comando `hwclock` se utiliza para configurar el reloj de hardware del sistema. La opción `--systohc` se utiliza para sincronizar el reloj de hardware con el tiempo del sistema.*

**La sintaxis general del comando `hwclock` es:**

```bash
hwclock [opciones]
```

> *El comando `pacman` es el gestor de paquetes utilizado en Arch Linux. Pacman significa "package manager" (gestor de paquetes) y es una herramienta fundamental para instalar, actualizar y administrar software en sistemas basados en Arch Linux.*

```bash
pacman -Syu --noconfirm nano
```

```bash
pacman --sync nano
```

- *La opción `-S` se utiliza para instalar un paquete y sus dependencias desde los repositorios de Arch Linux. En otras palabras, `-S` significa "synchronize" (sincronizar), indicando que se sincronizará el sistema con los repositorios de software de Arch Linux para instalar el paquete especificado y todas las dependencias necesarias.*

**La sintaxis general del comando `pacman` para instalar un paquete es:**

```bash
pacman -Syu --noconfirm nombre_del_paquete
```

```bash
pacman --sync nombre_del_paquete
```

> *El comando `nano` se utiliza para editar ficheros de texto en la línea de comandos. En este caso, se está abriendo el fichero `/etc/locale.gen` para editarlo.*

**La sintaxis general del comando `nano` para abrir un fichero es:**

```bash
nano ruta/al/fichero
```

> *Una vez que el fichero está abierto en Nano, puedes usar las funciones de búsqueda para encontrar las líneas específicas que necesitas. En este caso, estás buscando las líneas que contienen `en_US.UTF-8 UTF-8` y `es_ES.UTF-8 UTF-8`.*

*Una vez que hayas encontrado estas líneas, puedes descomentarlas (eliminar el símbolo `#` al principio de la línea) si están comentadas, o asegurarte de que estén presentes si no lo están. Estas líneas se utilizan para configurar las localizaciones del sistema para inglés estadounidense y español de España, respectivamente.*

```bash
nano /etc/locale.gen  # Buscar en_US.UTF-8 UTF-8 y es_ES.UTF-8 UTF-8
```

> *El comando `locale-gen` se utiliza para generar las configuraciones de localización especificadas en el fichero `/etc/locale.gen`. Este fichero contiene una lista de configuraciones de localización que están disponibles para su uso en el sistema.*

**La sintaxis general del comando `locale-gen` es simplemente:**

```bash
locale-gen
```

> *El comando `echo` se utiliza para imprimir texto en la salida estándar. En este caso, se está utilizando para escribir la cadena `LANG=es_ES.UTF-8` en el fichero `/etc/locale.conf`.*

**La sintaxis general del comando `echo` es:**

```bash
echo "texto_a_imprimir"
```

- *El operador `>` se utiliza para redirigir la salida del comando `echo` al fichero especificado. En este caso, la cadena `LANG=es_ES.UTF-8` se escribirá en el fichero `/etc/locale.conf`, sobrescribiendo cualquier contenido existente en el fichero.*

- *La variable `LANG` especifica la configuración regional y de idioma predeterminada para el sistema. En este caso, se está configurando en español de España (`es_ES`) con codificación UTF-8.*

- *Es importante tener en cuenta que la configuración de `LANG` en `/etc/locale.conf` afectará a todos los usuarios del sistema, estableciendo la configuración regional predeterminada para el entorno del sistema.*

```bash
echo "LANG=es_ES.UTF-8" > /etc/locale.conf
```

> *El comando `echo` se utiliza para imprimir texto en la salida estándar. En este caso, se está utilizando para escribir la cadena `KEYMAP=es` en el fichero `/etc/vconsole.conf`.*

**La sintaxis general del comando `echo` es:**

```bash
echo "texto_a_imprimir"
```

- *El operador `>` se utiliza para redirigir la salida del comando `echo` al fichero especificado. En este caso, la cadena `KEYMAP=es` se escribirá en el fichero `/etc/vconsole.conf`, sobrescribiendo cualquier contenido existente en el fichero.*

- *La variable `KEYMAP` especifica el mapeo de teclado que se utilizará en el sistema. En este caso, se está configurando en español (`es`).*

- *Es importante tener en cuenta que la configuración de `KEYMAP` en `/etc/vconsole.conf` afectará al mapeo de teclado predeterminado para el sistema.*

```bash
echo "KEYMAP=es" > /etc/vconsole.conf
```

> *El comando `echo` se utiliza para imprimir texto en la salida estándar. En este caso, se está utilizando para escribir la cadena "asus" en el fichero `/etc/hostname`.*

**La sintaxis general del comando `echo` es:**

```bash
echo "texto_a_imprimir"
```

- *El operador `>` se utiliza para redirigir la salida del comando `echo` al fichero especificado. En este caso, la cadena "asus" se escribirá en el fichero `/etc/hostname`, sobrescribiendo cualquier contenido existente en el fichero.*

- *El nombre del host, que se establece en `/etc/hostname`, es el nombre que identifica de manera única a un sistema en una red. En este caso, el nombre del host se establecerá en "asus".*

- *Es importante tener en cuenta que cambiar el nombre del host puede afectar a la configuración de red y a la resolución de nombres en el sistema.*

```bash
echo "asus" > /etc/hostname
```

> *El comando `nano` se utiliza para editar ficheros de texto en la línea de comandos. En este caso, se está abriendo el fichero `/etc/hosts` para editarlo y agregar las entradas necesarias.*

**La sintaxis general del comando `nano` para abrir un fichero es:**

```bash
nano ruta/al/fichero
```

- *Una vez que el fichero `/etc/hosts` está abierto en Nano, puedes agregar o modificar las entradas según sea necesario. Las entradas en este fichero se utilizan para mapear direcciones IP a nombres de host.*

**Aquí está el contenido que deberías agregar al fichero `/etc/hosts`:**

```plaintext
127.0.0.1   localhost
::1         localhost
127.0.1.1   asus.localhost asus
```

- *Estas entradas se utilizan comúnmente en sistemas Linux. La primera entrada asigna `localhost` a la dirección IP `127.0.0.1`, la segunda entrada hace lo mismo pero para `::1` (la dirección IPv6 de loopback), y la tercera entrada asigna el nombre del host `asus.localhost` a la dirección IP `127.0.1.1`.*

- *Es importante tener en cuenta que `/etc/hosts` es utilizado por el sistema operativo para resolver nombres de host localmente, por lo que cualquier cambio que hagas aquí afectará cómo se resuelven los nombres de host en tu sistema.*

```bash
nano /etc/hosts
```

> *Para cambiar la contraseña del usuario `root`, el comando `passwd` se utiliza. Aquí está el comando para cambiar la contraseña del usuario `root`:*

- *El comando `passwd` se utiliza para cambiar las contraseñas de usuario en sistemas Linux. Para cambiar la contraseña del usuario `root`, simplemente ejecuta el siguiente comando y sigue las instrucciones:*

```bash
passwd root
```

- *Después de ejecutar este comando, se te pedirá que ingreses la nueva contraseña para el usuario `root`. Ten en cuenta que al escribir la contraseña, no verás ningún carácter en la pantalla, esto es normal para mayor seguridad. Luego, se te pedirá que ingreses la contraseña nuevamente para confirmarla. Una vez que confirmes la nueva contraseña, se actualizará y el cambio será efectivo.*

- *Es importante tener en cuenta que se requieren privilegios de administrador para cambiar la contraseña de `root`. Si no tienes acceso de administrador, no podrás ejecutar este comando.*

> *El comando `pacman` es el gestor de paquetes en sistemas Arch Linux y derivados, como Manjaro. Se utiliza para instalar, actualizar y administrar paquetes de software en el sistema.*

- *La opción `-S` se utiliza para instalar un paquete desde los repositorios de Arch Linux. Cuando se utiliza `-S`, `pacman` buscará el paquete especificado en los repositorios y lo instalará junto con sus dependencias.*

- *En este caso, el paquete especificado es `networkmanager`. Este es el paquete que proporciona el administrador de conexiones de red en sistemas Arch Linux. NetworkManager es una herramienta que facilita la configuración y gestión de conexiones de red, incluidas conexiones Ethernet, Wi-Fi, VPN y móviles.*

- *Por lo tanto, al ejecutar `pacman -Syu --noconfirm networkmanager`, el sistema buscará el paquete `networkmanager` en los repositorios de Arch Linux y lo instalará en el sistema junto con cualquier dependencia necesaria.*

- *Es importante tener en cuenta que se requieren privilegios de administrador para ejecutar `pacman`, por lo que generalmente se usa con el prefijo `sudo` para ejecutarlo como usuario root.*

```bash
pacman -Syu --noconfirm networkmanager
```

> *El comando `systemctl` se utiliza para administrar servicios del sistema en sistemas basados en systemd, como Arch Linux y sus derivados. Puede iniciar, detener, habilitar, deshabilitar y administrar el estado de los servicios del sistema.*

- *La opción `enable` se utiliza para habilitar un servicio para que se inicie automáticamente en el arranque del sistema. Cuando se habilita un servicio, systemd crea enlaces simbólicos en los directorios `/etc/systemd/system` y `/etc/systemd/user` que apuntan al fichero de servicio correspondiente.*

- *En este caso, el servicio especificado es `NetworkManager`. NetworkManager es el administrador de conexiones de red que instalamos previamente con el comando `pacman -Syu --noconfirm networkmanager`. Al habilitar el servicio NetworkManager, systemd configurará el sistema para que se inicie automáticamente al arrancar.*

- *Por lo tanto, al ejecutar `systemctl enable NetworkManager`, el servicio NetworkManager se habilitará y se configurará para iniciarse automáticamente en el arranque del sistema.*

- *Es importante tener en cuenta que se requieren privilegios de administrador para ejecutar `systemctl`, por lo que generalmente se usa con el prefijo `sudo` para ejecutarlo como usuario root.*

```bash
systemctl enable NetworkManager
```

> *El comando `pacman -Syu --noconfirm grub efibootmgr efivar os-prober` se utiliza en sistemas basados en Arch Linux y derivados para instalar una serie de paquetes relacionados con el gestor de arranque y la gestión de EFI (Interfaz de Firmware Extensible). Aquí tienes una explicación sobre cada uno de los paquetes:*

- **grub**: *Este paquete proporciona GRUB (Grand Unified Bootloader), que es un gestor de arranque comúnmente utilizado en sistemas Linux. GRUB se encarga de cargar el sistema operativo durante el proceso de arranque.*

- **efibootmgr**: *Este paquete proporciona la utilidad `efibootmgr`, que se utiliza para administrar la configuración de arranque en sistemas EFI. Permite crear, eliminar y modificar entradas de arranque en la tabla de particiones EFI.*

- **efivar**: *Este paquete proporciona la utilidad `efivar`, que se utiliza para leer y escribir variables de firmware en sistemas EFI. Estas variables se utilizan para almacenar configuraciones y datos de bajo nivel relacionados con el hardware y el firmware.*

- **os-prober**: *Este paquete proporciona el script `os-prober`, que se utiliza para detectar otros sistemas operativos instalados en el sistema y agregarlos a la configuración de arranque de GRUB. Es útil cuando tienes múltiples sistemas operativos instalados en tu computadora y deseas arrancar entre ellos.*

- *En resumen, al ejecutar `pacman -Syu --noconfirm grub efibootmgr efivar os-prober`, instalarás estos paquetes en tu sistema, lo que te permitirá configurar y administrar el gestor de arranque GRUB en sistemas EFI, así como detectar otros sistemas operativos instalados en tu computadora.*

```bash
pacman -Syu --noconfirm grub efibootmgr efivar os-prober
```

> *El comando `grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB` se utiliza para instalar GRUB como el gestor de arranque en sistemas basados en EFI (Interfaz de Firmware Extensible). Aquí tienes una explicación sobre cada una de las opciones utilizadas en este comando:*

- **`grub-install`**: *Este es el comando que se utiliza para instalar GRUB en el disco de arranque del sistema.*

- **`--target=x86_64-efi`**: *Esta opción especifica que el destino de la instalación de GRUB es el firmware EFI en arquitectura x86_64. En sistemas EFI, GRUB puede tener diferentes objetivos dependiendo de la arquitectura del firmware EFI. En este caso, `x86_64-efi` se refiere a la arquitectura de 64 bits utilizada en la mayoría de los sistemas EFI modernos.*

- **`--efi-directory=/boot`**: *Esta opción especifica el directorio donde se monta la partición EFI del sistema. En sistemas basados en EFI, la partición EFI generalmente se monta en `/boot`. Esta opción le dice a `grub-install` dónde buscar la partición EFI para instalar los ficheros de arranque de GRUB.*

- **`--bootloader-id=GRUB`**: *Esta opción especifica el identificador único para el cargador de arranque GRUB en la tabla de particiones EFI. Este identificador se utiliza para identificar el cargador de arranque GRUB durante el proceso de arranque del sistema. En este caso, el identificador se establece en `GRUB`.*

- **`--recheck`**: *En el comando "grub-install" fuerza al programa a escanear nuevamente los dispositivos en busca de los ficheros necesarios para la instalación del gestor de arranque GRUB. Esto puede ser útil si se han realizado cambios en los discos o particiones desde la última vez que se ejecutó el comando.*

- *Al ejecutar este comando, GRUB se instalará como el gestor de arranque en el sistema EFI, utilizando la partición EFI montada en `/boot` y configurando el identificador del cargador de arranque como `GRUB`.*

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --recheck
```

> *El comando `grub-mkconfig -o /boot/grub/grub.cfg` o `grub-mkconfig --output=/boot/grub/grub.cfg` se utiliza para generar el fichero de configuración de GRUB, llamado `grub.cfg`, en sistemas basados en Linux. Aquí tienes una explicación sobre cómo funciona este comando:*

- **`grub-mkconfig`**: *Este comando es una utilidad de GRUB que se utiliza para generar dinámicamente el fichero de configuración de GRUB basado en la configuración actual del sistema y los ficheros de configuración específicos del usuario.*

- **`-o /boot/grub/grub.cfg`**: *Esta opción especifica la ruta y el nombre del fichero de configuración de GRUB que se generará. En este caso, el fichero se guardará en `/boot/grub` con el nombre `grub.cfg`. La forma no abreviada de -o en el contexto de este comando es --output. Por lo tanto, el comando completo sería: `grub-mkconfig --output=/boot/grub/grub.cfg`*

- *Al ejecutar `grub-mkconfig -o /boot/grub/grub.cfg`, GRUB buscará la configuración actual del sistema, incluidos los ficheros de configuración de GRUB en `/etc/grub.d/` y `/etc/default/grub`, así como cualquier otro fichero de configuración específico del usuario. Luego, generará un fichero de configuración `grub.cfg` que contiene las entradas de menú de arranque para todos los sistemas operativos instalados y opciones de arranque adicionales.*

- *Es importante tener en cuenta que `grub-mkconfig` debe ejecutarse con privilegios de superusuario, ya que necesita acceso a ciertos ficheros del sistema y la capacidad de modificar el fichero de configuración de GRUB en `/boot/grub/grub.cfg`.*

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

```bash
grub-mkconfig --output=/boot/grub/grub.cfg
```

> *El comando `useradd -m usuario` se utiliza para crear un nuevo usuario en sistemas basados en Linux. Aquí está la explicación de cada parte del comando:*

- **`useradd`**: *Este es el comando utilizado para agregar un nuevo usuario al sistema.*

- **`-m`**: *Esta opción indica a `useradd` que debe crear automáticamente el directorio de inicio del nuevo usuario. Esto asegura que se creen los directorios de configuración y los ficheros necesarios en el directorio de inicio del usuario. La forma no abreviada de -m en el contexto de este comando es --create-home. Por lo tanto, el comando completo sería: `useradd --create-home usuario`*

- **`usuario`**: *Este es el nombre del nuevo usuario que se va a crear. Puedes reemplazar "usuario" con el nombre que desees para el nuevo usuario.*

*Por lo tanto, al ejecutar `useradd -m usuario`, se creará un nuevo usuario con el nombre especificado y se creará automáticamente su directorio de inicio en `/home/usuario`.*

- *Es importante tener en cuenta que `useradd` es un comando que generalmente se ejecuta con privilegios de superusuario, ya que implica la modificación del sistema y la creación de ficheros y directorios en áreas restringidas del sistema.*

```bash
useradd -m usuario
```

```bash
useradd --create-home usuario
```

> *El comando `passwd usuario` se utiliza para establecer una nueva contraseña para un usuario específico en sistemas basados en Linux. Aquí está la explicación de cada parte del comando:*

- **`passwd`**: *Este es el comando utilizado para cambiar las contraseñas de los usuarios en el sistema.*

- **`usuario`**: *Este es el nombre del usuario para el cual se desea cambiar la contraseña. Debes reemplazar "usuario" con el nombre del usuario para el cual deseas cambiar la contraseña.*

- *Al ejecutar `passwd usuario`, el sistema te pedirá que ingreses y confirmes la nueva contraseña para el usuario especificado. Es importante tener en cuenta que este comando generalmente se ejecuta con privilegios de superusuario, ya que implica la modificación de la contraseña de otro usuario en el sistema.*

```bash
passwd usuario
```

> *El comando `usermod -aG games scanner power wheel audio optical storage video usuario` se utiliza para modificar los grupos a los que pertenece un usuario específico en sistemas basados en Linux. Aquí está la explicación de cada parte del comando:*

- **`usermod`**: *Este es el comando utilizado para modificar los atributos de un usuario en el sistema.*

- **`-aG`**: *Estas opciones se utilizan para agregar al usuario a grupos adicionales sin eliminarlos de los grupos existentes. La forma no abreviada de -aG en el comando usermod es --append --groups. Por lo tanto, la forma no abreviada del comando completo sería: `usermod --append --groups games,scanner,power,wheel,audio,optical,storage,video usuario`*

- **`games scanner power wheel audio optical storage video`**: *Estos son los nombres de los grupos a los que se está agregando al usuario. Puedes agregar tantos grupos como desees, separándolos por espacios.*

- **`usuario`**: *Este es el nombre del usuario al que se están aplicando los cambios. Debes reemplazar "usuario" con el nombre del usuario al que deseas modificar los grupos.*

- *Al ejecutar `usermod -aG games scanner power wheel audio optical storage video usuario`, el usuario especificado será agregado a los grupos `games`, `scanner`, `power`, `wheel`, `audio`, `optical`, `storage` y `video`, sin eliminarlos de los grupos a los que ya pertenece.*

- *Es importante tener en cuenta que este comando generalmente se ejecuta con privilegios de superusuario, ya que implica la modificación de los atributos de otro usuario en el sistema.*

```bash
usermod -aG games, scanner, power, wheel, audio, optical, storage, video, usuario
```

```bash
usermod -a -G games, scanner, power, wheel, audio, optical, storage, video, usuario
```

```bash
usermod --append -G games, scanner, power, wheel, audio, optical, storage, video, usuario
```

```bash
usermod -a --groups games, scanner, power, wheel, audio, optical, storage, video, usuario
```

```bash
usermod --append --groups games, scanner, power, wheel, audio, optical, storage, video, usuario
```

> *El comando `pacman -Syu --noconfirm sudo` se utiliza en sistemas basados en Arch Linux para instalar el paquete `sudo`. Aquí está la explicación de cada parte del comando:*

- **`pacman`**: *Este es el administrador de paquetes utilizado en Arch Linux para instalar, actualizar y administrar software en el sistema.*

- **`-S`**: *Esta opción se utiliza para sincronizar los paquetes del repositorio con el sistema e instalar nuevos paquetes. En este caso, `sudo` es el nombre del paquete que se va a instalar.*

- **`sudo`**: *Este es el nombre del paquete que se va a instalar en el sistema. `sudo` es un programa que permite a los usuarios ejecutar comandos con privilegios de otro usuario, generalmente el superusuario (root).*

- *Por lo tanto, al ejecutar `pacman -Syu --noconfirm sudo`, se instalará el paquete `sudo` en el sistema, lo que permitirá a los usuarios utilizar el comando `sudo` para ejecutar comandos con privilegios elevados.*

```bash
pacman -Syu --noconfirm sudo
```

- *En el fichero `/etc/sudoers`, la línea que generalmente se descomenta es la que permite que los usuarios del grupo `wheel` utilicen el comando `sudo`. La línea se ve así:*

```bash
# %wheel ALL=(ALL:ALL) ALL
```

**Para descomentar esta línea, simplemente elimina el símbolo `#` al principio de la línea. Después de descomentarla, se verá así:**

```bash
%wheel ALL=(ALL:ALL) ALL
```

- *Esta configuración permite a cualquier usuario que pertenezca al grupo `wheel` ejecutar cualquier comando utilizando `sudo`.*

- *Es importante tener cuidado al editar el fichero `/etc/sudoers`, ya que cambios incorrectos pueden bloquearte el acceso al sistema. Siempre es una buena práctica hacer una copia de seguridad del fichero antes de editarlo.*

```bash
nano /etc/sudoers
```

> *El comando `exit` se utiliza para cerrar la sesión actual del intérprete de comandos, ya sea Bash u otro shell. Cuando ejecutas el comando `exit`, el intérprete de comandos se cierra y vuelves al shell desde el que lo invocaste. Si estás utilizando una terminal o una sesión de conexión remota, el comando `exit` cerrará la terminal o la sesión de conexión.*

- *Es útil cuando deseas salir de una sesión de shell o terminal, o cuando deseas cerrar una ventana de terminal.*

- *Por ejemplo, si estás dentro de una sesión de Bash y ejecutas el comando `exit`, la sesión de Bash se cerrará y volverás al shell desde el que iniciaste esa sesión.*

```bash
exit
```

> *El comando `umount -R /mnt` se utiliza para desmontar recursivamente todos los puntos de montaje que se encuentran bajo el directorio `/mnt`.*

- **`umount`**: *Es el comando utilizado para desmontar (remover) puntos de montaje del sistema de ficheros.*

- **`-R`**: *Esta opción indica que el desmontaje debe realizarse de forma recursiva, es decir, desmontará todos los puntos de montaje que se encuentren dentro del directorio especificado y sus subdirectorios. La forma no abreviada de la opción -R es --recursive. Por lo tanto, el comando completo sería: `umount --recursive /mnt`*

- **`/mnt`**: *Es el directorio raíz desde el cual se desmontarán los puntos de montaje. En este caso, se desmontarán todos los puntos de montaje que están bajo el directorio `/mnt`.*

- *Este comando es útil cuando se desea desmontar varios puntos de montaje que están ubicados dentro de un directorio específico, como en el caso de desmontar varios dispositivos montados dentro de un directorio de montaje compartido.*

```bash
umount -R /mnt
```

```bash
umount --recursive /mnt
```

> *El comando `shutdown -h now` se utiliza para apagar el sistema inmediatamente. Aquí está la explicación de cada parte del comando:*

- **`shutdown`**: *Este es el comando utilizado para programar un apagado o reinicio del sistema.*

- **`-h`**: *Esta opción indica que se realizará un apagado (`halt`) del sistema. Significa "halt" en inglés. La forma no abreviada de la opción -h en el comando shutdown es --halt. Por lo tanto, el comando completo sería: `shutdown --halt now`*

- **`now`**: *Esta es una indicación de tiempo que significa "ahora". Indica que el apagado debe ocurrir inmediatamente.*

*Por lo tanto, cuando ejecutas `shutdown -h now`, el sistema se apagará inmediatamente.*

- *Es importante tener en cuenta que este comando generalmente requiere privilegios de superusuario (root) para ejecutarse correctamente. Por lo tanto, es posible que necesites usar `sudo` antes del comando si no estás ejecutando como root.*

*Espero que esta explicación te sea útil. Si tienes alguna otra pregunta, no dudes en preguntar.*

```bash
shutdown -h now
```

```bash
shutdown --halt now
```

```plaintext
Sacar USB y arrancar PC
```

## ***Instalar Entorno de Escritorio KDE-Plasma***

> *El comando `nmcli device wifi list` se utiliza para mostrar una lista de las redes Wi-Fi disponibles. Aquí está la explicación de cada parte del comando:*

- **`nmcli`**: *Este es el comando para NetworkManager Command Line Interface, que es una herramienta de línea de comandos para controlar NetworkManager, un programa de gestión de redes en sistemas basados en Linux.*

- **`device`**: *Esta subcomando de `nmcli` se utiliza para operaciones relacionadas con dispositivos de red, como Wi-Fi, Ethernet, etc.*

- **`wifi`**: *Especifica que estamos interesados en las redes Wi-Fi.*

- **`list`**: *Especifica que queremos listar las redes Wi-Fi disponibles.*

- *Por lo tanto, cuando ejecutas `nmcli device wifi list`, obtendrás una lista de todas las redes Wi-Fi disponibles que tu dispositivo puede detectar, junto con información como el nombre de la red (SSID), el canal, el modo de seguridad, la intensidad de la señal, etc.*

```bash
nmcli device wifi list
```

> *El comando `nmcli device wifi connect <nombre> <password> <contraseña>` se utiliza para conectarse a una red Wi-Fi específica utilizando NetworkManager a través de la línea de comandos. Aquí está la explicación de cada parte del comando:*

- **`nmcli`**: *Este es el comando para NetworkManager Command Line Interface, que es una herramienta de línea de comandos para controlar NetworkManager, un programa de gestión de redes en sistemas basados en Linux.*

- **`device`**: *Esta subcomando de `nmcli` se utiliza para operaciones relacionadas con dispositivos de red, como Wi-Fi, Ethernet, etc.*

- **`wifi`**: *Especifica que estamos interesados en las redes Wi-Fi.*

- **`connect`**: *Especifica que queremos conectarnos a una red Wi-Fi específica.*

- **`<nombre>`**: *Reemplaza "nombre" con el nombre (SSID) de la red Wi-Fi a la que deseas conectarte.*

- **`<password>`**: *Indica que se proporcionará una contraseña para autenticarse en la red Wi-Fi.*

- **`<contraseña>`**: *Reemplaza "contraseña" con la contraseña de la red Wi-Fi a la que deseas conectarte.*

- *Por lo tanto, al ejecutar este comando con el nombre y la contraseña correctos de la red Wi-Fi, tu dispositivo intentará conectarse a esa red Wi-Fi específica utilizando NetworkManager.*

- *Es importante tener en cuenta que este comando generalmente requiere privilegios de superusuario (root) para ejecutarse correctamente. Por lo tanto, es posible que necesites usar `sudo` antes del comando si no estás ejecutando como root.*

```bash
nmcli device wifi connect <nombre> <password> <contraseña>
```

> *`ping -c 1 archlinux.org`: Este comando envía un solo paquete de solicitud de eco ICMP (también conocido como "ping") al dominio archlinux.org. La opción `-c 1`. Indica que solo se enviará un paquete. Este comando se usa comúnmente para verificar la conectividad de red con un host remoto. Si todo está configurado correctamente y el host es accesible a través de la red, deberías recibir una respuesta del host.*

```bash
ping -c 1 archlinux.org
```

*El comando `sudo pacman -Syu --noconfirm xorg` se utiliza en sistemas Arch Linux para instalar el conjunto base del servidor gráfico Xorg. Aquí está la explicación de cada parte del comando:*

- **`sudo`**: *Este es un comando que se utiliza en sistemas basados en Unix para ejecutar otro comando con privilegios de superusuario (root). Permite que el comando siguiente (`pacman`) se ejecute con los privilegios necesarios para instalar software en el sistema.*

- **`pacman`**: *Es el gestor de paquetes de Arch Linux, utilizado para instalar, actualizar y administrar paquetes de software.*

- **`-S`**: *Esta opción indica a `pacman` que se debe instalar un paquete o una lista de paquetes en el sistema. La opción `-S` significa "synchronize" (sincronizar), ya que sincroniza la lista de paquetes disponibles con los repositorios antes de instalar.*

- **`xorg`**: *Este es el nombre del paquete que se va a instalar. En este caso, se refiere al servidor gráfico Xorg, que proporciona la infraestructura básica para el entorno gráfico en sistemas Linux.*

*Por lo tanto, cuando ejecutas `sudo pacman -Syu --noconfirm xorg`, el gestor de paquetes `pacman` buscará en los repositorios de Arch Linux el paquete `xorg` y sus dependencias, lo descargará e instalará en tu sistema.*

- *Es importante tener en cuenta que la instalación de `xorg` es solo el primer paso para tener un entorno gráfico funcional. Después de instalar `xorg`, normalmente se instalan también otros componentes como un gestor de ventanas (por ejemplo, `i3`, `GNOME`, `KDE`, etc.) y un gestor de inicio de sesión (como `lightdm` o `gdm`) para obtener un entorno gráfico completo.*

```bash
sudo pacman -Syu --noconfirm xorg
```

> *El comando `sudo pacman -Syu --noconfirm plasma-meta kde-applications-meta` se utiliza en sistemas Arch Linux para instalar el entorno de escritorio Plasma KDE junto con un conjunto de aplicaciones del proyecto KDE. Aquí está la explicación de cada parte del comando:*

- **`sudo`**: *Este es un comando que se utiliza en sistemas basados en Unix para ejecutar otro comando con privilegios de superusuario (root). Permite que el comando siguiente (`pacman`) se ejecute con los privilegios necesarios para instalar software en el sistema.*

- **`pacman`**: *Es el gestor de paquetes de Arch Linux, utilizado para instalar, actualizar y administrar paquetes de software.*

- **`-S`**: *Esta opción indica a `pacman` que se debe instalar un paquete o una lista de paquetes en el sistema. La opción `-S` significa "synchronize" (sincronizar), ya que sincroniza la lista de paquetes disponibles con los repositorios antes de instalar.*

- **`plasma-meta`**: *Este es un meta-paquete que instala el entorno de escritorio Plasma KDE junto con un conjunto básico de componentes necesarios para su funcionamiento. Instala el entorno de escritorio principal y algunos componentes esenciales.*

- **`kde-applications-meta`**: *Este es otro meta-paquete que instala un conjunto de aplicaciones del proyecto KDE, que incluye una amplia gama de programas y utilidades diseñadas para ser utilizadas con el entorno de escritorio KDE.*

*Por lo tanto, cuando ejecutas `sudo pacman -Syu --noconfirm plasma-meta kde-applications-meta`, el gestor de paquetes `pacman` buscará en los repositorios de Arch Linux los paquetes `plasma-meta` y `kde-applications-meta`, junto con sus dependencias, los descargará e instalará en tu sistema.*

*Este comando te proporcionará un entorno de escritorio completo basado en KDE, junto con un conjunto de aplicaciones diseñadas para funcionar con él.*

```bash
sudo pacman -Syu --noconfirm plasma-meta kde-applications-meta
```

> *El comando `sudo systemctl enable sddm` se utiliza en sistemas Linux con systemd (como Arch Linux) para habilitar el servicio de SDDM (Simple Desktop Display Manager). Aquí está la explicación de cada parte del comando:*

- **`sudo`**: *Este es un comando que se utiliza en sistemas basados en Unix para ejecutar otro comando con privilegios de superusuario (root). Permite que el comando siguiente (`systemctl`) se ejecute con los privilegios necesarios para administrar servicios del sistema.*

- **`systemctl`**: *Es una herramienta de systemd que se utiliza para controlar el sistema y administrar servicios. Permite habilitar, deshabilitar, iniciar, detener y administrar servicios del sistema, entre otras funciones.*

- **`enable`**: *Esta es una subcomanda de `systemctl` que se utiliza para habilitar un servicio para que se inicie automáticamente al arrancar el sistema.*

- **`sddm`**: *Este es el nombre del servicio que se va a habilitar. SDDM (Simple Desktop Display Manager) es un gestor de inicio de sesión gráfico utilizado por defecto en los entornos de escritorio KDE Plasma y otros basados en Qt.*

*Por lo tanto, cuando ejecutas `sudo systemctl enable sddm`, estás habilitando el servicio de SDDM para que se inicie automáticamente cada vez que se arranque el sistema. Esto asegura que el gestor de inicio de sesión gráfico esté disponible y listo para que los usuarios inicien sesión en el entorno de escritorio KDE Plasma.*

```bash
sudo systemctl enable sddm
```

> *El comando `bash -c "sleep 0 && reboot"` es una forma de reiniciar el sistema utilizando el shell Bash. Aquí está la explicación de cada parte del comando:*

- **`bash`**: *Es el nombre del intérprete de comandos o shell que se utilizará para ejecutar el comando especificado. En este caso, se está utilizando el shell Bash.*

- **`-c`**: *Es una opción de Bash que se utiliza para indicar que se ejecutará un comando proporcionado como argumento. La forma no abreviada de la opción -c en el comando bash es -command. Por lo tanto, el comando completo sería: `bash --command "sleep 0 && reboot"`*

- **`"sleep 0 && reboot"`**: *Es el comando que se ejecutará dentro del shell Bash. Consiste en dos partes separadas por el operador `&&`:*

  - **`sleep 0`**: *Este comando hace que el shell espere durante 0 segundos antes de continuar con el siguiente comando. En la práctica, esto no hace nada, pero puede ser útil para introducir un pequeño retraso antes de ejecutar el siguiente comando.*

  - **`reboot`**: *Este es el comando que realmente reinicia el sistema. Cuando se ejecuta, reinicia el sistema operativo.*

P*or lo tanto, al ejecutar `bash -c "sleep 0 && reboot"`, se espera brevemente y luego se reinicia el sistema.*

*Es importante tener en cuenta que reiniciar el sistema de esta manera puede ser potencialmente peligroso, especialmente si hay procesos en ejecución que aún no han completado sus tareas. Se recomienda tener precaución al utilizar este tipo de comandos y asegurarse de que no haya ningún trabajo importante en progreso antes de reiniciar el sistema de esta manera.*

```bash
bash -c "sleep 0 && reboot"
```

```bash
bash --command "sleep 0 && reboot"
```

---

## ***Configurar Particiones (Reconozca Windows, y acceder asus discos)***

> *Para montar la partición EFI de Windows y las particiones de Windows (C y D), así como configurar el sistema para que detecte Windows automáticamente al reiniciar, podemos seguir los siguientes pasos:*

```bash
fdisk -l
```

```plaintext
root@asus:/home/d4nitrix13
> fdisk -l
Disk /dev/sda: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: WD Blue SA510 2.
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: C11508EC-AA78-4E25-BE46-61703E3874A1

Device     Start        End    Sectors   Size Type
/dev/sda1     34      32767      32734    16M Microsoft reserved
/dev/sda2  32768 1953521663 1953488896 931.5G Microsoft basic data


Disk /dev/nvme0n1: 465.76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: WDC WDS500G2B0C-00PXH0
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 961D34A3-3901-485E-9BF6-A6B1470AD9C6

Device             Start       End   Sectors  Size Type
/dev/nvme0n1p1      2048    206847    204800  100M EFI System
/dev/nvme0n1p2    206848    239615     32768   16M Microsoft reserved
/dev/nvme0n1p3    239616 975364095 975124480  465G Microsoft basic data
/dev/nvme0n1p4 975364096 976771071   1406976  687M Windows recovery environment


Disk /dev/nvme1n1: 232.89 GiB, 250059350016 bytes, 488397168 sectors
Disk model: HP SSD EX900 250GB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: DB40C8A1-D9A1-444E-94B0-AA485DDC0864

Device             Start       End   Sectors   Size Type
/dev/nvme1n1p1      2048   1050623   1048576   512M EFI System
/dev/nvme1n1p2   1050624 211185663 210135040 100.2G Linux filesystem
/dev/nvme1n1p3 211185664 421320703 210135040 100.2G Linux filesystem
/dev/nvme1n1p4 421320704 488396799  67076096    32G Linux swap
```

*Para identificar una partición EFI y un disco, puedes seguir estas indicaciones:*

1. **Partición EFI**:
   - *La partición EFI generalmente tiene un tamaño pequeño, a menudo alrededor de 100 MB o 200 MB.*
   - *Por lo general, tiene un tipo de partición llamado "EFI System" o similar.*
   - *Puede tener una etiqueta de tipo "EFI System" en la columna "Type" al ejecutar `fdisk -l`.*
   - *En tu salida de `fdisk -l`, las particiones EFI se muestran con el tamaño y el tipo de partición. Por ejemplo:*

     ```plaintext
     /dev/nvme0n1p1      2048    206847    204800  100M EFI System
     /dev/nvme1n1p1      2048   1050623   1048576   512M EFI System
     ```

   - *En este caso, `/dev/nvme0n1p1` y `/dev/nvme1n1p1` son particiones EFI.*

2. **Disco**:
   - *Para identificar un disco, generalmente puedes observar el tamaño total del disco y el modelo del disco.*
   - *Los discos tienen tamaños significativamente mayores que las particiones, como varios cientos de gigabytes o terabytes.*
   - *Además del tamaño, a menudo puedes ver el modelo del disco, que generalmente describe el fabricante y el modelo del disco.*
   - *En tu salida de `fdisk -l`, puedes ver información sobre el modelo del disco en la línea que comienza con "Disk model". Por ejemplo:*

     ```plaintext
     Disk model: WDC WDS500G2B0C-00PXH0
     Disk model: HP SSD EX900 250GB
     ```

- *Para identificar una partición EFI, busca una partición con el tipo "EFI System" y un tamaño pequeño, típicamente al principio del disco. Para identificar un disco, busca un tamaño grande y el modelo del disco. En tu caso, las particiones `/dev/nvme0n1p1` y `/dev/nvme1n1p1` son particiones EFI, y los discos son `/dev/sda` (modelo "WD Blue SA510 2") y `/dev/nvme0n1` (modelo "WDC WDS500G2B0C-00PXH0" ) y `/dev/nvme1n1` (modelo "HP SSD EX900 250GB").*

- **"Microsoft basic data"** *es una etiqueta que indica que la partición es utilizada por el sistema operativo Windows para almacenar datos. Esta etiqueta es comúnmente utilizada por Windows para sus particiones de datos, incluidas las particiones del sistema y las particiones de almacenamiento de ficheros.*

- *En la salida de `fdisk -l` que proporcionaste, las particiones etiquetadas como "Microsoft basic data" son aquellas que se utilizan para almacenar datos del sistema operativo Windows. Estas particiones pueden contener ficheros del sistema, programas instalados, datos de usuario y otros ficheros relacionados con Windows.*

- *Por lo tanto, cuando ves la etiqueta "Microsoft basic data" en la salida de `fdisk -l`, significa que esa partición está siendo utilizada por el sistema operativo Windows para almacenar datos.*

> *Para instalar `ntfs-3g` en Arch Linux, puedes usar el administrador de paquetes `pacman`. Ejecuta el siguiente comando en tu terminal:*

```bash
sudo pacman -Syu --noconfirm ntfs-3g
```

**Esto instalará el paquete `ntfs-3g` en tu sistema. Una vez instalado, podrás montar y acceder a particiones NTFS en tu sistema Arch Linux.**

1. **Montar la partición EFI de Windows** (`/dev/nvme0n1p1`):

   ```bash
   sudo mkdir /mnt/windows-efi
   ```

   ```bash
   sudo mount /dev/nvme0n1p1 /mnt/windows-efi
   ```

   *Esto montará la partición EFI de Windows en el directorio `/mnt/windows-efi`.*

2. **Montar la partición de Windows (C)** *(`/dev/nvme0n1p3`)*:

   ```bash
   sudo mkdir /mnt/windows-c
   ```

   ```bash
   sudo mount -t ntfs-3g /dev/nvme0n1p3 /mnt/windows-c
   ```

   ```bash
   sudo mount --mkdir -t ntfs-3g /dev/nvme0n1p3 /mnt/windows-c
   ```

   ```bash
   sudo mount -m --type ntfs-3g /dev/nvme0n1p3 /mnt/windows-c
   ```

   ```bash
   sudo mount --mkdir --type ntfs-3g /dev/nvme0n1p3 /mnt/windows-c
   ```

   ```bash
   sudo mount -m -t ntfs-3g /dev/nvme0n1p3 /mnt/windows-c
   ```

   *Esto montará la partición de Windows (C) en el directorio `/mnt/windows-c`.*

3. **Montar la partición de Windows (D)** *(`/dev/sda2`)*:

   ```bash
   sudo mkdir /mnt/windows-d
   ```

   ```bash
   sudo mount -t ntfs-3g /dev/sda2 /mnt/windows-d
   ```

   ```bash
   sudo mount --type ntfs-3g /dev/sda2 /mnt/windows-d
   ```

   ```bash
   sudo mount --mkdir --type ntfs-3g /dev/sda2 /mnt/windows-d
   ```

   ```bash
   sudo mount -m --type ntfs-3g /dev/sda2 /mnt/windows-d
   ```

   ```bash
   sudo mount -m -t ntfs-3g /dev/sda2 /mnt/windows-d
   ```

   *Esto montará la partición de Windows (D) en el directorio `/mnt/windows-d`.*

4. **Editar el fichero `/etc/fstab` para que las particiones se monten automáticamente al arrancar**:

   ```bash
   sudo nano /etc/fstab
   ```

   - *Agrega las siguientes líneas al fichero si no las tiene:*

   ```plaintext
   root@asus:/home/d4nitrix13
   cat /etc/fstab
   # Static information about the filesystems.
   # See fstab(5) for details.

   # <file system> <dir> <type> <options> <dump> <pass>
   # /dev/nvme1n1p2
   UUID=75785f7f-d6ba-43b6-bb86-bad9755c2d43 /          ext4       rw,relatime 0 1

   # /dev/nvme1n1p3
   UUID=e48dffed-422a-4c72-9e95-7527bf01889f /home      ext4       rw,relatime 0 2

   # /dev/nvme1n1p1
   UUID=B298-8318       /boot      vfat       rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro 0 2

   # /dev/nvme0n1p1
   UUID=201A-8A8B       /windows   vfat       rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro 0 2

   # /dev/nvme1n1p4
   UUID=d306d702-3f1f-4265-89e1-b18b694a3174 none       swap       defaults   0 0


   # /dev/nvme0n1p3
   UUID=D8561E3A561E19B8 /mnt/C ntfs defaults 0 2

   # dev/sda2
   UUID=AE4CC7BB4CC77D19 /mnt/D ntfs defaults 0 2
   ```

   *Esto montará automáticamente la partición EFI de Windows, así como las particiones C y D de Windows al iniciar el sistema.*

5. **Configurar GRUB para que detecte Windows automáticamente**:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

*Esto actualizará la configuración de GRUB para que detecte automáticamente los sistemas operativos instalados en el sistema.*

*Con estos pasos, las particiones de Windows deberían montarse automáticamente al arrancar el sistema, y GRUB debería detectar Windows y mostrarlo como una opción de arranque.*

**Si deseas realizar los montajes de las particiones antes de ejecutar `arch-chroot /mnt`, puedes hacerlo manualmente como lo has mencionado. Aquí está el proceso:**

1. **Montar las particiones**: *Ejecuta los comandos de montaje para cada partición en el directorio `/mnt`:*

   ```bash
   mount /dev/nvme1n1p1 /mnt/boot
   ```

   ```bash
   mount /dev/nvme1n1p2 /mnt 
   ```

   ```bash
   mount -m /dev/nvme1n1p3 /mnt/home
   ```

   ```bash
   mount -m /dev/nvme0n1p1 /mnt/windows-efi
   ```

   ```bash
   mount -m /dev/nvme0n1p3 /mnt/windows-c
   ```

   ```bash
   mount -m /dev/sda2 /mnt/windows-d 
   ```

   - *Aquí, `/mnt` es el punto de montaje raíz donde se montarán todas las demás particiones.*

2. **Verificar los montajes**: Puedes verificar que las particiones se han montado correctamente ejecutando el comando `mount` o `df`. Por ejemplo:

   ```bash
   mount
   ```

   ```plaintext
   proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
   sys on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
   dev on /dev type devtmpfs (rw,nosuid,relatime,size=8106800k,nr_inodes=2026700,mode=755,inode64)
   run on /run type tmpfs (rw,nosuid,nodev,relatime,mode=755,inode64)
   efivarfs on /sys/firmware/efi/efivars type efivarfs (rw,nosuid,nodev,noexec,relatime)
   /dev/nvme1n1p2 on / type ext4 (rw,relatime)
   securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
   tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,inode64)
   devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
   cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot)
   pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
   bpf on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime,mode=700)
   systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=37,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=11718)
   mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime)
   hugetlbfs on /dev/hugepages type hugetlbfs (rw,nosuid,nodev,relatime,pagesize=2M)
   debugfs on /sys/kernel/debug type debugfs (rw,nosuid,nodev,noexec,relatime)
   tracefs on /sys/kernel/tracing type tracefs (rw,nosuid,nodev,noexec,relatime)
   fusectl on /sys/fs/fuse/connections type fusectl (rw,nosuid,nodev,noexec,relatime)
   configfs on /sys/kernel/config type configfs (rw,nosuid,nodev,noexec,relatime)
   tmpfs on /tmp type tmpfs (rw,nosuid,nodev,nr_inodes=1048576,inode64)
   /dev/nvme0n1p3 on /mnt/C type fuseblk (ro,nosuid,nodev,relatime,user_id=0,group_id=0,allow_other,blksize=4096)
   /dev/nvme0n1p1 on /windows type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro)
   /dev/nvme1n1p1 on /boot type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro)
   /dev/sda2 on /mnt/D type fuseblk (ro,nosuid,nodev,relatime,user_id=0,group_id=0,allow_other,blksize=4096)
   /dev/nvme1n1p3 on /home type ext4 (rw,relatime)
   tmpfs on /run/user/1000 type tmpfs (rw,nosuid,nodev,relatime,size=1627156k,nr_inodes=406789,mode=700,uid=1000,gid=1000,inode64)
   ```

   ```bash
   df
   ```

   ```plaintext
   Filesystem     1K-blocks      Used Available Use% Mounted on
   dev              8106800         0   8106800   0% /dev
   run              8135788      1312   8134476   1% /run
   efivarfs             128        27        97  22% /sys/firmware/efi/efivars
   /dev/nvme1n1p2 102832040   7472240  90090040   8% /
   tmpfs            8135788      6964   8128824   1% /dev/shm
   tmpfs            8135788      3776   8132012   1% /tmp
   /dev/nvme0n1p3 487562236  83664800 403897436  18% /mnt/C
   /dev/nvme0n1p1     98304     33072     65232  34% /windows
   /dev/nvme1n1p1    523248    190252    332996  37% /boot
   /dev/sda2      976744444 520447656 456296788  54% /mnt/D
   /dev/nvme1n1p3 102832040   6421968  91140312   7% /home
   tmpfs            1627156        12   1627144   1% /run/user/1000
   ```

   *Esto mostrará todas las particiones montadas actualmente y sus puntos de montaje.*

   ```bash
   df -Th /mnt/boot
   ```

   - *El comando `df` (abreviatura de "disk free") se utiliza para mostrar el espacio disponible en los sistemas de ficheros en Linux. La opción `-Th` especifica el formato de salida para que incluya información sobre el tipo de sistema de ficheros (`-T`) y presente los tamaños en un formato legible para humanos (`-h`). `/mnt/boot` es el directorio que especifica qué sistema de ficheros o dispositivo se debe examinar. Por lo tanto, `df -Th /mnt/boot` mostrará información sobre el espacio disponible en el sistema de ficheros montado en `/mnt/boot`, incluido el tipo de sistema de ficheros y el tamaño disponible en un formato fácil de entender.*

3. **Luego, procede con `arch-chroot /mnt`**: *Una vez que todas las particiones estén montadas correctamente, puedes ejecutar el comando `arch-chroot /mnt` para chrootear en el sistema instalado y continuar con la instalación y configuración de Arch Linux.*

*Este enfoque te permite configurar las particiones manualmente antes de chrootear en el sistema instalado, lo que puede ser útil si deseas realizar alguna configuración adicional o personalizada antes de continuar con la instalación.*
