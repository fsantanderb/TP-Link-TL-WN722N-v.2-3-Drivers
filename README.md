# TP-Link TL-WN722N v.2-3 Drivers
Instalación en Kali Linux de los Drivers TP-Link TL-WN722N v.2-3

## Requisitos:
- Windows 10-11 con VirtualBox
- Kali Linux Versión 2022.4 (Última versión del 2022)
- Tarjeta TP-Link TL-WN722N v.2-3 (La versión 1 no necesita cambio de drivers)

![alt text](https://github.com/fsantanderb/TP-Link-TL-WN722N-v.2-3-Drivers/blob/main/imagenes/TP-Link_WN722N.jpg)

## Procedimiento:

### 1.- Preparación de VirtualBox para conectar tarjeta WiFi

a. En virtualBox al configurar la Máquina Virtual de Kali Linux, en el apartado USB, agregar nuevo filtro USB

![alt text](https://github.com/fsantanderb/TP-Link-TL-WN722N-v.2-3-Drivers/blob/main/imagenes/VirtualBoxUSB.jpg)

b.- Seleccionar dispositivo USB correcpondiente a la tarjeta WiFi

![alt text](https://github.com/fsantanderb/TP-Link-TL-WN722N-v.2-3-Drivers/blob/main/imagenes/VirtualBoxUSB2.jpg)

c.- Luego en la interfaz gráfica de Kali Linux, seleccionar el dispositivo USB, tal como se observa en la siguiente imágen:

![alt text](https://github.com/fsantanderb/TP-Link-TL-WN722N-v.2-3-Drivers/blob/main/imagenes/VirtualBoxUSB3.jpg)

Con esto ya debería estar diponible el dispositivo en nuestro Kali Linux, verificar con el comando `iwconfig`

![alt text](https://github.com/fsantanderb/TP-Link-TL-WN722N-v.2-3-Drivers/blob/main/imagenes/VirtualBoxKali.jpg)

### 2.- Configuración en Kali Linux

--- Ejecutar los siguientes comandos sin conectar la tarjeta ---
```
sudo apt-get update
sudo apt-get upgrade -y
sudo apt install bc
```
--- Reiniciar Kali Linux ---
```
sudo apt-get install build-essential
sudo apt-get install libelf-dev
sudo apt-get install linux-headers-amd64
sudo apt install dkms
```
--- Conectar tarjeta al equipo ---
```
sudo rmmod r8188eu.ko
git clone https://github.com/aircrack-ng/rtl8188eus
cd rtl8188eus
sudo su (Cambiar a modo root)
cd /etc/modprobe
echo "blacklist r8188eu" > realtek.conf 
exit (salir de root)
```
--- Reiniciar Kali Linux ---
```
sudo apt-get update 
cd rtl8188eus
```
--- El siguiente comando realiza una corrección a los archivos antes de compilar ---
```
sed -i 's/$(pwd)/$(srctree)\/$(src)/'g Makefile && rm -rf *patch && wget https://github.com/MrRob0-X/rtl8188eus/commit/5e3cfb4be0258dbca0ad553be5dc04d0829dbd04.patch && patch -p1 < *patch && rm -rf *patch
```
--- Compilar e instalar ---
```
sudo make
sudo make install
sudo modprobe 8188eu
```
### 3.- Verificar

```
iwconfic (verifica el modo y el ID de interfaz, ejemplo wlan0)
sudo airmon-ng check kill
sudo ip link set wlan0 down
sudo iw dev wlan0 set type monitor
```

`iwconfig` (nuevamente para verificar el cambio a modo monitor)

![alt text](https://gitcnc.investigaciones.cl/FSB/tp-link-tl-wn722n-v.2-3-drivers/-/raw/master/Imagenes/Modo_Monitor.JPG)

`sudo airodump-ng wlan0` (Para verificar recepción de señales)
