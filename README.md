# Documentación de Klipper en UTN FRRo


Este repositorio tiene la intencion de establecer la documentacion pertinente a la instalación, configuracion y guia de uso del firmware Klipper en las impresoras 3D de la universidad.

Toda la informacion utilizada en este proceso fue suministrada por la pagina oficial de [Klipper](https://www.klipper3d.org/), su [repositorio](https://github.com/Klipper3d/klipper) y distintos foros públicos como [Reddit](https://www.reddit.com/r/klippers/) y la [Wiki](https://tronxy.fandom.com/wiki/Installing_Klipper) de Tronxy para instalar Klipper.

## 0. Información previa

 Antes de realizar cualquier manipulacion del firmware original de las impresoras ([Marlin](https://github.com/MarlinFirmware/Marlin)), es recomendable realizar/obtener una copia del mismo por cuestiones de seguridad. 

 Tener preparado un dispositivo capaz de correr una distribucion de Linux cualquiera, esta a su vez debe de ser capaz de correr Python 3.8 (sujeto a modificaciones segun Klipper, en un futuro puede ser alguna version mas nueva). Esta distribción se justa a limitaciones del dispositivo que lave a ejecutar o gusto de la persona.

Hasta el momento no hay requisitos mínimos muy claros, pero segun experimentos hechos de forma autónoma para ejecutar una sola interfaz (1 dispositivo, 1 impresora) debe ser un sistema que tenga las siguientes especificaciones:

- Arquitectuta x86_64 o x64
- Procesador de 2 núcleos
- 2GB de ram DDR3

Tambien tener cable para realizar la conexion entre la impresora y el dispositivo.

Cabe aclarar que se menciona "dispositivo" porque es posible correrlo en un dispositivo Android.


## 1. Instalación

Teniendo preparados los requisitos previos, se utilizó la interfaz de instalación [KIAUH](https://github.com/dw-0/kiauh), de ésta se instala minimamente en el dispositivo a ejecutar el host:

-  Klipper
-  Moonraker (Servidor web basado en Python)
-  Una interfaz gráfica (Mainsail o Fluidd).

Posteriormente se debe generar el archivo .bin que contiene el firmware Klipper para la impresora; dentro de la carpeta kiauh se ejecuta el comando:
 
 ~~~
 make menuconfig
 ~~~

 Se despliega una interfaz gŕafica que permite seleccionar los parametros de la motherboard de la impresora. Dichos parametros van variando segun la placa y el procesador que contiene la misma. Para nuestro caso las placas son:

 - CXY-446-V6-191121
 - CXY-446-V10
  
Siguiendo la guia de la wiki, se copia la configuracion para una placa tipo 446 con bootloader de 64Kb,se guardan los cambios y se ejecuta el comando:

~~~
make
~~~

> [!WARNING]
> Si se utiliza el dispositivo para generar multiples configraciones distintas, al finalizar, se debe volver a generar la configuracion de la impresora a la cual va a estar conectado.

Este proceso genera varios archivos en la carpeta /klipper/out, solo se copia el .bin y se pone en una sd en una carpeta llamada "update", se conecta la sd a la impresora, se prende y si todo salio correctamente, la pantalla queda con una barra de carga en la parte inferior quedando inutilizable.

## 2. Configuración inicial

Ya teniendo la impresora conectada, se accede a esta a través de la interfaz grafica instalada via navegador por medio de "localhost".

Dentro de ésta, se accede a la pestaña de "Configuración" y se procede a modificar el archivo "printer.cfg"

La configuracion de la impresora depende pura y exclusivamente de la placa, el procesador y el modelo de la impresora. Con el pasar de los años se encuentran muchas configuraciones gratuitas disponibles en el repositorio de Klipper o en foros.

Lo que importan de éstas son el seteo de pines para los movimientos de los ejes, sensores de temperatura, flujo del extrusor, etc.

Lo que se debe configurar dependiendo la impresora son las dimensiones, estas estan escritas en mm.

> [!NOTE]
> Antes de realizar cualquier moviemiento de ejes, configurar dimensiones y probar moviendo uno a la vez. En caso de mal movimiento, presionar el boton de "frenado" que se encuentra en la esquina superior derecha

Si se aprecia que todo se encuentra correctamente funcionando y no se presentan errores, Klipper ya se encuentrar funcionando.

## 3. Calibración

Por lo general esto se hace 1 sola vez, se puede hacer mas veces en casos de mantenimiento o cuando se considere conveniente.

La calibración depende del tipo de impresora y si presenta sensor o no. Existe la opción de calibracion en su pestaña propia y se puede apreciar un grafico de desnivel en las coordenadas que se toma valor de distancia del z_offset (que tecnicamente es el desface del z_endstop).

En el caso de las D01, no tienen sensor, por lo que no se puede apreciar dicho grafico y se hace el "paper test" mencionado en la pagina de Klipper y como mucho, en casos de desniveles muy grotescos, se ajustan los tornillos debajo de la cama segun requiera.

Para la X5SA, se puede hacer el paper test pero debido a las dimensiones es mejor realizar un ajuste manual mediante "probe", ya que esta si presenta sensor y ajustar los tornillos segun el desfazaje que muestre el grafico.