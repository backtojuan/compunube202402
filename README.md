# compunube202402

└── compunube
    ├── Practica1
    └── Practica2
    └── Practican

## CONFIGURACIÓN ENTORNO VIRTUALIZADO CON VAGRANT

1. Instalar última versión de VirtualBox
•	Descargue e instale VirtualBox 7.0.20 del sitio https://www.virtualbox.org/wiki/Downloads

2. Instalar última versión de Vagrant
•	Descargue e instale la ultima versión de Vagrant desde https://releases.hashicorp.com/vagrant/ 
•	Por ejemplo: https://releases.hashicorp.com/vagrant/2.4.1/
NOTA: En ese caso, usaremos la versión 2.4.1 para 64 bits (vagrant_2.4.1_windows_amd64.msi) (para usuarios mac con tecnología intel el archivo con extensión .dmg vagrant_2.4.1_darwin_amd64.dmg)
3. Abra una consola de Windows y verifique la versión de Vagrant
 	 
![image](https://github.com/user-attachments/assets/191983ff-03c9-4212-8c6f-bcfe96c6cf28)

    
5. Configuración el entorno virtualizado
Muchos servicios de red usan el modelo cliente-servidor. Usaremos dos maquinas virtuales, una que alojará los servicios configurados y otra que los consumirá. 
Configurar el Vagrantfile de la siguiente manera (ver instrucciones abajo en “CONFIGURACION” ). 
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define :clienteUbuntu do |clienteUbuntu|
    clienteUbuntu.vm.box = "bento/ubuntu-22.04"
    clienteUbuntu.vm.network :private_network, ip: "192.168.100.2"
    clienteUbuntu.vm.hostname = "clienteUbuntu"
  end

  config.vm.define :servidorUbuntu do |servidorUbuntu|
    servidorUbuntu.vm.box = "bento/ubuntu-22.04"
    servidorUbuntu.vm.network :private_network, ip: "192.168.100.3"
    servidorUbuntu.vm.hostname = "servidorUbuntu"
  end
end

Este Vagrantfile define dos maquinas virtuales, una llamada servidor con dirección ip 192.168.100.3 y la otra cliente con dirección ip 192.168.100.2, ambas instanciadas desde un box en el repositorio de bento llamado bento/Ubuntu-20.04.

Nota: SOLO SI ES USUARIO MAC OSX:
En la maquina anfitrión crear el directorio /etc/vbox  y dentro el archivo networks.conf  (/etc/vbox/networks.conf): con el siguiente contenido:
* 192.168.100.0/24

CONFIGURACION
1.	Cree un directorio llamado “prueba”
2.	Dentro del directorio prueba cree un archivo llamado “Vagrantfile”.
Para crear un Vagrantfile de ejemplo puede usar el siguiente comando dentro de la carpeta prueba:
vagrant init
Modifique el Vagrantfile para que tenga UNICAMENTE el contenido mostrado en el ejemplo de arriba (al inicio de la sección)
3.	Cree y configure las maquinas mediante el comando vagrant up ejecutado desde consola
4.	Verifique el Puerto de reenvío para cada maquina virtual. En el ejemplo mostrado abajo, el puerto de reenvío es el 2222
$ vagrant up
5.	Verifique el estado de las maquinas creadas con el comando
vagrant status
6.	Establezca una sesión ssh con la maquina servidor
vagrant ssh servidorUbuntu
7.	Autenticarse como super usuario
sudo –i
8.	Instalar algunas herramientas para configuración de la red
apt-get install net-tools
9.	Instalar el editor Vim
apt-get install vim
10.	Repita los pasos 5 a 8 para la maquina cliente.
11.	Confirme la ip de las maquinas virtuales usando ifconfig y pruebe conectividad con el comando ping.

6. Detener o suspender una maquina virtual

vagrant suspend guarda el estado actual de la maquina y la detiene. Para volver a la maquina desde el punto en que la suspendio puede ejecutar vagrant up.

Vagrant halt apaga la maquina virtual de manera segura conservando los contenidos del disco y permitiendo un inicio seguro de nuevo. Para levantar la maquina de nuevo puede usar vagrant up.

Vagrant destroy remueve la maquina guest del sistema compelamente. Para levantar la maquina de nuevo puede usar vagrant up.

7. Publicar Boxes en Vagrant Cloud
Una vez modificada la maquina virtual podemos crear un nuevo box y subirlo a Vagrant Cloud para usarlo posteriormente. Para eso seguiremos los siguientes pasos:

1.	Re empaquetar la maquina virtual en un nuevo Vagrant Box

vagrant package servidorUbuntu --output mynew.box
2.	Agregar el box creado a su instalación de Vagrant.

El comando anterior creara un archivo mynew.box. Con el siguiente comando agregaremos el box a nuestra instalación de Vagrant:

vagrant box add mynewbox mynew.box
Esto permitirá usar el box desde cualquier ubicación en su computador.

3.	Publique el box en Vagrant Cloud

•	Diríjase a https://app.vagrantup.com/boxes/new
•	Ingrese un nombre y una descripción para su box
•	Cree la primera versión del box. Esta versión debe cumplir con el formato [0-9].[0-9].[0-9]. Por ejemplo 0.0.1.
•	Cree un provider para el box. Virtualbox es el provider mas común.
•	Cargue el archivo .box que corresponde al provider creado
•	Una vez cargado el box puede lo puede encontrar en la sección de boxes de https://app.vagrantup.com/
•	Antes de usar una versión del box, debe liberarlo “release”
•	Una vez creado y liberado un box, puede liberar nuevas versiones dando click en “create new version” en el menú de versiones de la pagina del box.

Referencias

Creating a New Vagrant Box. https://atlas.hashicorp.com/help/vagrant/boxes/create
