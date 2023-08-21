# **Instrucciones de Instalación para Workshop - Monitoreo con Zabbix**

¡Bienvenidos al emocionante mundo del monitoreo con Zabbix! 🚀 Este tutorial te guiará a través de la configuración paso a paso para tener tu propio entorno de laboratorio listo para monitorear. ¡No te preocupes si eres un novato en esto, estamos aquí para hacerlo divertido y fácil! 😄

## Índice

- [**Instrucciones de Instalación para Workshop - Monitoreo con Zabbix**](#instrucciones-de-instalación-para-workshop---monitoreo-con-zabbix)
  - [Índice](#índice)
  - [**Requisitos** 🛠️](#requisitos-️)
  - [**Preparativos** 🛠️](#preparativos-️)
      - [**1. Ansible**](#1-ansible)
      - [**2. VirtualBox**](#2-virtualbox)
      - [**3. Vagrant**](#3-vagrant)
      - [**4. Clona el repo**](#4-clona-el-repo)
      - [**5. Generar Clave SSH para Ansible**](#5-generar-clave-ssh-para-ansible)
      - [**6. Levantar las Máquinas Virtuales**](#6-levantar-las-máquinas-virtuales)
  - [**Direcciones IP de las VMs**](#direcciones-ip-de-las-vms)
  - [**Paso 1: Configuración del Zabbix Server**](#paso-1-configuración-del-zabbix-server)
  - [**Paso 3: Configuración del Zabbix Agent en VM1**](#paso-3-configuración-del-zabbix-agent-en-vm1)
  - [**Paso 4: Instalación del Agente Zabbix en Windows VM (Opcional)**](#paso-4-instalación-del-agente-zabbix-en-windows-vm-opcional)
  - [**Paso 5: Configuración del Autoregistro de Zabbix**](#paso-5-configuración-del-autoregistro-de-zabbix)

¡Disfruta del contenido! 📚



## **Requisitos** 🛠️

Antes de comenzar, asegúrate de tener instalados los siguientes programas en tu máquina:

- [Ansible](https://www.ansible.com/)
- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)

## **Preparativos** 🛠️

Antes de empezar con la configuración, es importante asegurarse de que tienes las herramientas adecuadas instaladas en tu computadora. No te preocupes si eres nuevo en esto, ¡te guiaré paso a paso!

#### **1. Ansible**

Ansible es una herramienta que te permitirá automatizar tareas en varios servidores de forma sencilla. Lo utilizaremos para configurar los agentes de Zabbix en nuestras máquinas virtuales.

Puedes instalar Ansible siguiendo estos pasos:

1. Abre una terminal en tu sistema.
2. Dependiendo de tu sistema operativo, ejecuta el siguiente comando para instalar Ansible:

   - En sistemas basados en Debian/Ubuntu:
     ```bash
     sudo apt update
     sudo apt install ansible
     ```

   - En sistemas basados en Red Hat/Fedora:
     ```bash
     sudo dnf install ansible
     ```

#### **2. VirtualBox**

VirtualBox es un software que te permite crear y gestionar máquinas virtuales en tu computadora. Usaremos VirtualBox para crear las máquinas virtuales donde configuraremos los agentes de Zabbix.

Para instalar VirtualBox:

1. Visita el sitio web de [VirtualBox](https://www.virtualbox.org/) y descarga el instalador correspondiente a tu sistema operativo.
2. Ejecuta el instalador descargado y sigue las instrucciones en pantalla para completar la instalación.

#### **3. Vagrant**

Vagrant es una herramienta que facilita la creación y configuración de entornos de desarrollo reproducibles. Utilizaremos Vagrant para automatizar la creación de nuestras máquinas virtuales.

Para instalar Vagrant:

1. Visita el sitio web de [Vagrant](https://www.vagrantup.com/) y descarga el instalador adecuado para tu sistema operativo.
2. Ejecuta el instalador descargado y sigue las instrucciones para finalizar la instalación.

#### **4. Clona el repo**
El repositorio del laboratorio contiene los archivos y configuraciones necesarios para realizar las tareas. Sigue estos pasos:

Abre una terminal en tu sistema.

Navega al directorio donde deseas guardar el repositorio utilizando el comando cd. Por ejemplo:

```bash
cd ruta/de/tu/directorio
```

Clona el repositorio ejecutando el siguiente comando:

```bash
git clone git@github.com:lbrines/zabbix-nerdearla2023.git
```

#### **5. Generar Clave SSH para Ansible**

Ahora que tienes las herramientas instaladas, necesitamos generar una clave SSH que Ansible utilizará para conectarse de forma segura a los agentes de Zabbix en las máquinas virtuales. Sigue estos pasos:

1. Abre una terminal en tu sistema.

2. Navega al directorio donde planeas trabajar con Ansible. Por ejemplo:
   ```bash
   cd ruta/del/directorio/linux/ansible
   ```

3. Genera una nueva clave SSH utilizando el siguiente comando. Esto creará un par de claves pública y privada:
   ```bash
   ssh-keygen -t rsa -b 4096 -f ansible_rsa
   ```

4. Durante la generación de la clave, se te pedirá que ingreses una contraseña opcional. Puedes dejarlo en blanco para no establecer una contraseña.

¡Listo! Ahora tienes una clave SSH llamada `ansible_rsa` en el directorio `linux/ansible` que Ansible usará para conectarse de manera segura a las máquinas virtuales y configurar los agentes de Zabbix.

#### **6. Levantar las Máquinas Virtuales**

Ahora que tienes el repositorio clonado, es hora de levantar las máquinas virtuales en las que configuraremos los agentes de Zabbix. Sigue estos pasos:

1. Abre una terminal.

2. Navega al directorio `linux` dentro del repositorio clonado:
   ```bash
   cd zabbix-nerdearla2023/linux
   ```

3. Ejecuta el siguiente comando para levantar las máquinas virtuales Linux:
   ```bash
   vagrant up
   ```

6. **Opcional:** Si también estás emocionado por explorar las máquinas virtuales Windows, navega al directorio `windows` dentro del repositorio clonado y ejecuta el mismo comando `vagrant up` para levantar las máquinas virtuales Windows.

¡Listo! Ahora estás listo para comenzar a configurar los agentes de Zabbix en las máquinas virtuales que has levantado.

## **Direcciones IP de las VMs**

Asegúrate de tomar nota de las siguientes direcciones IP para acceder a tus máquinas virtuales en el laboratorio:

- Zabbix Server: 192.168.56.200
- Windows VM: 192.168.56.220
- VM vm1: 192.168.56.201
- VM vm2: 192.168.56.202
- VM vm3: 192.168.56.203
- VM vm4: 192.168.56.204

## **Paso 1: Configuración del Zabbix Server**

1. Accede al Zabbix Server utilizando `vagrant ssh zabbix-server`.

2. Ejecuta los siguientes comandos uno por uno:

   ```bash
   wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu20.04_all.deb
   sudo dpkg -i zabbix-release_6.4-1+ubuntu20.04_all.deb
   sudo apt update
   ```

3. Instala Zabbix y MySQL Server:

   ```bash
   sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent mysql-server
   ```

4. Configura la base de datos MySQL:

   ```bash
   sudo mysql -uroot -p
   # Introduce la contraseña cuando se solicite
   create database zabbix character set utf8mb4 collate utf8mb4_bin;
   create user zabbix@localhost identified by 'password';
   grant all privileges on zabbix.* to zabbix@localhost;
   set global log_bin_trust_function_creators = 1;
   quit;
   ```

5. Instala la base de datos de Zabbix Server:

   ```bash
   sudo zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
   ```

6. Desactiva la creación de funciones:

   ```bash
   sudo mysql -uroot -p
   # Introduce la contraseña cuando se solicite
   set global log_bin_trust_function_creators = 0;
   quit;
   ```

7. Edita el archivo de configuración del Zabbix Server:

   ```bash
   sudo vim /etc/zabbix/zabbix_server.conf
   ```

8. Agrega la siguiente línea y guarda el archivo:

   ```conf
   DBPassword=password
   ```

9. Reinicia los servicios:

   ```bash
   sudo systemctl restart zabbix-server zabbix-agent apache2
   sudo systemctl enable zabbix-server zabbix-agent apache2
   ```

10. Accede a Zabbix en tu navegador utilizando: [http://192.168.56.200/zabbix/](http://192.168.56.200/zabbix/)

11. ¡Listo! Ahora sigue los pasos en pantalla para configurar la contraseña de la base de datos, la región y la zona horaria.

12. Ingresa con las siguientes credenciales:
    - Usuario: Admin
    - Contraseña: zabbix
    - Accede a [http://192.168.56.200/zabbix/](http://192.168.56.200/zabbix/)

## **Paso 3: Configuración del Zabbix Agent en VM1**

1. En la máquina `vm1`, ejecuta:

   ```bash
   vagrant ssh vm1
   ```

2. Ejecuta los siguientes comandos uno por uno:

   ```bash
   wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu20.04_all.deb
   sudo dpkg -i zabbix-release_6.4-1+ubuntu20.04_all.deb
   sudo apt update
   ```

3. Instala el agente de Zabbix y edita su archivo de configuración:

   ```bash
   sudo apt install zabbix-agent -y
   sudo vim /etc/zabbix/zabbix_agentd.conf
   ```

4. Agrega la siguiente línea (reemplaza `192.168.56.200` con la dirección IP del Zabbix Server) y guarda el archivo:

   ```conf
   Server=192.168.56.200
   ```

5. Opcional pero emocionante: instala la herramienta de estrés para simular consumo de memoria:

   ```bash
   sudo apt-get install stressapptest
   ```

6. Ejecuta la herramienta de estrés:

   ```bash
   stressapptest -s 3600
   ```

7. ¡Voilà! ¡Has completado la configuración del agente de Zabbix en `vm1`!

## **Paso 4: Instalación del Agente Zabbix en Windows VM (Opcional)**

1. Accede por RDP a la máquina virtual de Windows con la dirección IP `192.168.56.220`.

2. Instala `zabbix_agent-6.0.20-windows-amd64-openssl.msi` que se encuentra en `C:/vagrant`.

3. Cuando lo solicite, coloca la dirección IP del Zabbix Server `192.168.56.200`.

4. Agrega el host siguiendo los mismos pasos del Paso 3, pero utiliza el template llamado "Zabbix Agent Windows".

## **Paso 5: Configuración del Autoregistro de Zabbix**

1. Entra al directorio `linux/ansible` y ejecuta:

   ```bash
   ansible config-agent.yml
   ```

2. En el frontend de Zabbix, ve a Configuración → Acciones, selecciona "Autoregistro" como fuente de eventos y haz clic en "Crear acción":

3. En la pestaña "Acción", ponle un nombre a tu acción.

4. Opcionalmente, especifica condiciones. Puedes hacer coincidencias de subcadenas o expresiones regulares en las condiciones para el nombre del host/metadatos del host. Si vas a usar la condición "Metadatos del host", consulta la siguiente sección.

5. En la pestaña "Operaciones", agrega operaciones relevantes, como "Agregar host", "Agregar a grupo de hosts" (por ejemplo, hosts descubiertos), "Vincular a plantillas", etc.

¡Y eso es todo, master! 🎉 Ahora tienes un entorno de laboratorio configurado con Zabbix para comenzar tu emocionante viaje en el mundo del monitoreo. ¡Diviértete explorando, modificando y aprendiendo! 😃📊🔍
