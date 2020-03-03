# NOTAS 


### Qualcomm MSM (Mobile Station Modem)
MSM (Mobile Station Modem) is a series of 2G/3G/4G-capable system on chips designed by Qualcomm since the early 1990s to date.

Son los procesadores.

Qualcomm MSM based devices contain a special mode of operation, called Emergency Download Mode (EDL). In this mode, the device identifies itself as Qualcomm HS-USB 9008 through USB, and can communicate with a PC host. EDL is implemented by the SoC ROM code (also called PBL). The EDL mode itself implements the Qualcomm Sahara protocol, which accepts an OEM-digitally-signed programmer over USB. The programmer implements the Firehose protocol which allows the host PC to send commands to write into the onboard storage (eMMC, UFS).

As open source tool (for Linux) that implements the Qualcomm Sahara and Firehose protocols has been developed by Linaro, and can be used for program (or unbrick) MSM based devices, such as Dragonboard 410c or Dragonboard 820c.

### Qualcomm EDL (Emergency Download Mode)
https://alephsecurity.com/2018/01/22/qualcomm-edl-1/

There are many guides [1,2,3,4,5,6,7] across the Internet for ‘unbricking’ Qualcomm-based mobile devices. All of these guides make use of Emergency Download Mode (EDL), an alternate boot-mode of the Qualcomm Boot ROM (Primary Bootloader).

To make any use of this mode, users must get hold of OEM-signed programmers, which seem to be publicly available for various such devices. While the reason of their public availability is unknown, our best guess is that these programmers are often leaked from OEM device repair labs. Some OEMs (e.g. Xiaomi) also publish them on their official forums.

Qualcomm Secure Boot
An abstract overview of the boot process of Qualcomm MSM devices is as follows:

```
[Primary Bootloader (PBL)]
|
`---NORMAL BOOT---.
                  [Secondary Bootloader (SBL)]
                  |-. 
                  | [Android Bootloader (ABOOT)]
                  | `-.    
                  |   [boot.img]
                  |   |-- Linux Kernel
                  |   `-- initramfs
                  |       `-.
                  |         [system.img]
                  |
                  `-[TrustZone]
```
#### Primary Bootloader (PBL)
Primary or Single Bootloader (PBL): Primary Bootloader (PBL) is installed in the ROM and is the first block to execute on boot reset. The main function of the Primary Bootloader is to download the Secondary Bootloader in RAM and activate the SBL.


#### Secondary Bootloader (SBL)
Secondary Bootloader (SBL) is needed in order to initialize the execution environment for
running the application. This includes setting up the clocks, powering on the I/O peripherals,
initializing the secondary memory i.e. DDR, loading application images and booting slave cores.
Advanced Driver Assist Systems (ADAS) have very specific boot requirements regarding boot
time and functional safety. 

## MSMDownloadTool
Yo pensaba que esta herramienta escribe en toda la memoria. En XDA dicen que no, que es posible que no escriba todas las particiones que debería escribir.
Aquí hay un tío llorando con un caso similar al mío:

https://forum.xda-developers.com/oneplus-6t/help/bricked-oneplus-6t-msmdownloadtool-t3900145/page2

He desmontado el móvil y lo he vuelto a montar sin conseguir nada. Se sigue colgando en el inicio.

Se instaló Hydrogen el otro día con MSM, creo. No lo hice yo. He usado MSM para instalar Oxygen 9 (6T_MsmDownloadTool_v4.0.58_OOS_v9.0.5) y ahora la animación de inicio da más vueltas.
No sé qué habrá sucedido, quizás Hydrogen ha escrito sobre las particiones.

He entrado en el recovery y he hecho un Reset system settings and cache. Ahora se vuelve a colgar en el mismo punto, un par de vueltas de los círculos y muere.
Se ha reiniciado tras colgarse y vuelve a dar muchas vueltas, igual que antes de resetear la cache y el system settings.

https://github.com/phhusson/treble_experimentations/issues/55


On A/B partition devices, system must mark current slot as "bootable" after successful boot.
Otherwise bootloader counts down slot-retry-count and it reboots in bootloader recovery mode when slot-retry-count became 0.
https://source.android.com/devices/tech/ota/ab/ab_implement

Currently slot is not marked as bootable even if the device boots successfully.
I tested on Sharp AQUOS S2 SS2 (arm64).

Note: Don’t try to unlock SMT Download Mode as it will wipe Widevine certification and IMEI number of the smartphone. 
https://www.gizmochina.com/2019/11/19/unbrick-oneplus-7t-pro/

# Copia de seguridad
Utilizar el modo SMT borra tanto el IMEI como Widevine (utilizado en DRM) y puede dejar el teléfono inutilizable.
Se recomienda hacer una copia de seguridad de esos datos.

https://www.getdroidtips.com/how-to-backup-or-restore-qcn-efs-on-qualcomm-devices/


## Actualización 1/3/2020
Hoy lo he intentado iniciar como siempre y después de un par de vueltas la pantalla se ha quedado de color blanco. No sé qué ha pasado, no he podido volver a reproducirlo. Supongo que ha intentado mostrar alguna imagen de batería baja, pero cuando lo he enchufado al cargador mostraba el 29% de batería.
Lo he cargado con el adaptador de pared y funciona la carga rápida.
Imagen:
![alt text](https://github.com/miguelgargs/boot/blob/master/whitescreen.jpg "Pantalla en blanco")

## Actualización 3/3/2020
Hoy le he iniciado un par de veces como suelo hacer y se quedaba bloqueado y se apagaba.
Lo he encendido una tercera vez y cuando me he dado cuenta estaba en la pantalla de bienvenida.
Lo he cogido y lo he levantado y de repente se ha apagado.

- el software se escribe correctamente en el teléfono. No hace falta utilizar el modo smt de MSM (lo borraría todo)
- cuando cogí el teléfono estaba muy caliente por detrás, en la zona donde está el selector de modos de sonido.
- teniendo en cuenta lo anterior, parece ser algún fallo de hardware.

No tengo foto porque no me ha dado tiempo a hacerla. 

### Actualización 2
He quitado la tapa de atrás. He comprobado con Juan y resulta que la zona que estaba tan caliente es donde estaban la memoria y el procesador. 

Esta vez he apretado la parte de atrás y le he dado a encender. Se ha encendido. Y de repente se ha apagado.

Yo pienso que debe haber un mal contacto y por eso se arregla al apretar, Juan dice que puede ser un bus en mal estado.


![alt text](https://github.com/miguelgargs/boot/blob/master/arranque.jpeg "Pantalla de bienvenida")

### Actualización 3
Ya he descubierto dónde hay que pulsar. Pulsando donde el círculo rojo el teléfono inicia. Pero la pantalla no responde. Seguiré trabajando en ello.

![alt text](https://github.com/miguelgargs/boot/blob/master/dondepulsar.png "Dónde pulsar")