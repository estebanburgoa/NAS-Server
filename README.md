# OpenMediaVault y Plex Media Server en Raspberry Pi
El sueño de muchos es tener un servidor NAS en casa pero la verdad es que cuestan mucho dinero y una posible solución mas económica es armar tu propio servidor NAS y que mejor que complementarlo con Plex que por si no lo sabes, Plex es un servidor multimedia.
Para este proyecto usé una Raspberry Pi 2, ya que con la versión 1 no funciona, pero incluso trasteando logre hacerlo funcionar en una Raspberry Pi Zero 2W.

### Materiales
- Raspberry Pi 2
- Disco HDD de 1Tb
- Un adaptador de USB a SATA
- Adaptador de corriente de la Raspberry
- (Opcional) Cable HDMI
- (Opcional) Teclado con trackpad.

## OpenMediaVault
Partimos creando una imagen de Raspbian con la aplicacion [Imager](https://www.raspberrypi.com/software/) en una tarjeta SD. 

Debemos seleccionar nuestro modelo de placa. Para el sistema yo elegí una versión de 32bits y sin escritorio. y luego la SD.

[IMAGEN]

Como recomendación, yo prefiero siempre editar los ajuste de nombre de usuario a "admin" y password a "password" (puedes poner lo que quieras aquí), ademaás de activar el servicio SSH.

[IMAGEN]

Una vez que ya terminó el proceso de creación de la imagen del sistema operativo en nuestra SD y haber iniciado el sistema, ingresamos con nuestras credenciales y procedemos a actualizar el sistema.
```
sudo apt update
sudo apt upgrade -y
```
Posterior a este proceso que no debería durar mucho si tenemos nuestra Raspberry conectada por cable de red, procedemos con la instalación de los paquetes necesarios para que nuestro NAS con OpenMediaVault cobre vida.
```
sudo wget -O - https://raw.githubusercontent.com/OpenMediaVault-Plugin-Developers/installScript/master/install | sudo bash
```
Este proceso demorará un poco y hasta reiniciará el sistema, así que ten paciencia.

Una vez que termite todo el proceso volvemos ingresar con nuestras credenciales para aprovechar de continuar con la instalación de Plex Media Server.

## Plex Media Server

Instalamos el paquete apt-transport-https
```
sudo apt install apt-transport-https
```
Para habilitar el repositorio de Plex Media Server solo se requiere ejecutar los siguientes dos comandos:
```
echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
```
```
curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
```
Actualizamos los paquetes
```
sudo apt update
```
Instalamos Plex Media Server 
```
sudo apt install plexmediaserver
```
Ahora vamos a modificar el usuario de plex con el mismo usuario que usamos cuando creamos la imagen de Raspbian
```
sudo nano /usr/lib/plexmediaserver/lib/plexmediaserver.default
```
Descomentamos la linea y sustituimos "plex" por "admin"

[IMAGEN]

Reiniciamos el servicio Plex
```
sudo systemctl restart plexmediaserver
```
Obtenemos la ip de nuestra Raspberry
```
hostname -I
```
Ahora desde un navegador conectado a nuestra red ingresamos en esa IP. Ingresamos nuestras credenciales y nos vamos al apartado Network/Interfaces para dejar la IP de forma estática.

[IMAGEN]

Fijamos la IP en Plex
```
sudo nano /boot/cmdline.txt
```
y añadimos ip="la ip fija que seteamos en el paso anterior" y reiniciamos el sistema
```
sudo reboot
```














