# **Taller de Monitoreo con Zabbix: Guía de Instalación**

¡Bienvenidos al emocionante mundo del monitoreo con Zabbix! 🚀 En esta guía, te acompañaremos paso a paso para configurar tu propio entorno de laboratorio y adentrarte en el fascinante universo del monitoreo. No te preocupes si eres un novato en el tema, ¡estamos aquí para que disfrutes de un proceso divertido y sencillo! 😄

## Índice

- [**Taller de Monitoreo con Zabbix: Guía de Instalación**](#taller-de-monitoreo-con-zabbix-guía-de-instalación)
  - [Índice](#índice)
  - [**Requisitos** 🛠️](#requisitos-️)
  - [**Preparativos** 🛠️](#preparativos-️)
      - [**1. VirtualBox**](#1-virtualbox)
      - [**2. Vagrant**](#2-vagrant)
      - [**3. Ansible**](#3-ansible)
      - [**4. Clonar el Repositorio**](#4-clonar-el-repositorio)
      - [**5. Generar una Clave SSH para Ansible**](#5-generar-una-clave-ssh-para-ansible)
      - [**6. Levantar las Máquinas Virtuales**](#6-levantar-las-máquinas-virtuales)
  - [**Direcciones IP de las Máquinas Virtuales**](#direcciones-ip-de-las-máquinas-virtuales)
  - [**Paso 1: Configuración del Zabbix Server**](#paso-1-configuración-del-zabbix-server)
  - [**Paso 3: Configuración del primer host VM1**](#paso-3-configuración-del-primer-host-vm1)
    - [Crear el host](#crear-el-host)
    - [Configurar el agente en el host VM1](#configurar-el-agente-en-el-host-vm1)
  - [**Paso 4: Instalación del Agente Zabbix en Windows VM (Opcional)**](#paso-4-instalación-del-agente-zabbix-en-windows-vm-opcional)
  - [**Paso 5: Configuración del Autoregistro de Zabbix**](#paso-5-configuración-del-autoregistro-de-zabbix)

¡Ahora sí, a disfrutar del contenido! 📚

## **Requisitos** 🛠️

Antes de comenzar, asegúrate de tener instalados los siguientes programas en tu máquina:

- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)
- [Ansible](https://www.ansible.com/)

## **Preparativos** 🛠️

Antes de sumergirnos en la configuración, vamos a asegurarnos de que cuentas con las herramientas necesarias instaladas en tu computadora. No importa si eres nuevo en esto, ¡te acompañaremos en cada paso!

#### **1. VirtualBox**

VirtualBox es un software que te permite crear y gestionar máquinas virtuales en tu computadora. Usaremos VirtualBox para crear las máquinas virtuales donde configuraremos los agentes de Zabbix.

La instalación de VirtualBox es simple:

1. Visita el sitio web de [VirtualBox](https://www.virtualbox.org/wiki/Downloads) y descarga el instalador correspondiente a tu sistema operativo.
2. Ejecuta el instalador descargado y sigue las instrucciones en pantalla para completar la instalación.

#### **2. Vagrant**

Vagrant es una herramienta que facilita la creación y configuración de entornos de desarrollo reproducibles. Utilizaremos Vagrant para automatizar la creación de nuestras máquinas virtuales.

La instalación de Vagrant es sencilla:

1. Visita el sitio web de [Vagrant](https://developer.hashicorp.com/vagrant/downloads) y descarga el instalador adecuado para tu sistema operativo.
2. Ejecuta el instalador descargado y sigue las instrucciones para finalizar la instalación.

#### **3. Ansible**

[Ansible](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html) es una herramienta que te permite automatizar tareas en múltiples servidores de manera sencilla. Lo utilizaremos para configurar los agentes de Zabbix en nuestras máquinas virtuales.

#### **4. Clonar el Repositorio**
El repositorio del laboratorio contiene todos los archivos y configuraciones necesarios para llevar a cabo las tareas. Sigue estos pasos:

En tu sistema, abre una terminal.

Navega al directorio donde deseas almacenar el repositorio usando el comando `cd`. Por ejemplo:

```bash
cd ruta/de/tu/directorio
```

Clona el repositorio con el siguiente comando:

```bash
git clone https://github.com/lbrines/zabbix-nerdearla2023.git
```
```bash
├── ansible
│   ├── ansible.cfg
│   ├── ansible.log
│   ├── config-agent.yml
│   ├── inventory.yml
│   ├── key_rsa
│   ├── key_rsa.pub
│   └── templates
│       └── zabbix_agentd.conf.j2
├── linux
│   └── Vagrantfile
└── windows
    ├── Vagrantfile
    └── zabbix_agent-6.0.20-windows-amd64-openssl.msi
```

Entra al repo clonado:

```bash
cd zabbix-nerdearla2023/
```

#### **5. Generar una Clave SSH para Ansible**

Ahora que cuentas con las herramientas instaladas, necesitamos generar una clave SSH que Ansible utilizará para conectarse de manera segura a los agentes de Zabbix en las máquinas virtuales. Sigue estos pasos:

1. Genera una nueva clave SSH con el siguiente comando. Esto creará un par de claves pública y privada:
   ```bash
   ssh-keygen -t rsa -b 4096 -f ansible/key_rsa
   ```

2. Durante la generación de la clave, se te solicitará ingresar una contraseña opcional. Puedes dejarlo en blanco para no establecer una contraseña.

¡Listo! Ahora tienes una clave SSH llamada `key_rsa` en el directorio `ansible`, que Ansible utilizará para conectarse de manera segura a las máquinas virtuales y configurar los agentes de Zabbix.

#### **6. Levantar las Máquinas Virtuales**

Con el repositorio clonado, es hora de levantar las máquinas virtuales en las que configuraremos los agentes de Zabbix. Sigue estos pasos:

1. Abre una terminal.

2. Navega

 al directorio `linux` dentro del repositorio clonado:
   ```bash
   cd linux
   ```

3. Ejecuta el siguiente comando para levantar las máquinas virtuales Linux:
   ```bash
   vagrant up
   ```

4. **Opcional:** Si también deseas explorar las máquinas virtuales Windows, accede al directorio `windows` dentro del repositorio clonado y ejecuta el mismo comando `vagrant up` para levantar las máquinas virtuales Windows.

¡Excelente! Ahora estás listo para comenzar a configurar los agentes de Zabbix en las máquinas virtuales que has levantado.

## **Direcciones IP de las Máquinas Virtuales**

Asegúrate de tomar nota de las siguientes direcciones IP para acceder a tus máquinas virtuales en el laboratorio:

```conf
- Zabbix Server: 192.168.56.200
- Windows VM: 192.168.56.220
- Linux VM1: 192.168.56.201
- Linux VM2: 192.168.56.202
- Linux VM3: 192.168.56.203
- Linux VM4: 192.168.56.204
```

## **Paso 1: Configuración del Zabbix Server**

1. Accede al Zabbix Server mediante el siguiente comando:

   ```bash 
   vagrant ssh zabbix-server 
   ```

2. Ejecuta los siguientes comandos uno a uno:

   ```bash
   wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu20.04_all.deb
   ```
   ```bash
   sudo dpkg -i zabbix-release_6.0-4+ubuntu20.04_all.deb
   ```
   ```bash
   sudo apt update
   ```

3. Instala Zabbix y MySQL Server:

   ```bash
   sudo apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent mysql-server
   ```

4. Configura la base de datos MySQL:

   ```bash
   sudo mysql -uroot -p
   ```
   ```bash
   create database zabbix character set utf8mb4 collate utf8mb4_bin;
   ```
   ```bash
   create user zabbix@localhost identified by 'password';
   ```
   ```bash
   grant all privileges on zabbix.* to zabbix@localhost;
   ```
   ```bash
   set global log_bin_trust_function_creators = 1;
   ```
   ```bash
   quit;
   ```

5. Instala la base de datos de Zabbix Server:

   ```bash
   sudo zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
   ```

6. Desactiva la creación de funciones:

   ```bash
   sudo mysql -uroot -p
   ```
   ```bash
   set global log_bin_trust_function_creators = 0;
   ```
   ```bash
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
![Welcome to Zabbix](img/zabbix_1.png)

11. Verifica que todos los pre-requisitos estén OK
![Check of pre-requisites](img/zabbix_2.png)
12. Configuración de la base de datos:
![Configure DB connection](img/zabbix_3.png) 
13. Configuración de nombre y zona horaria:
![Settings](img/zabbix_4.png) 
14. Revisión de la configuración
![Pre-installation summary](img/zabbix_5.png)
15. Finaliza la instalación
![Install](img/zabbix_6.png)
16. Ingresa con las siguientes credenciales:
    - Usuario: Admin
    - Contraseña: zabbix
    - Accede a [http://192.168.56.200/zabbix/](http://192.168.56.200/zabbix/)
![Access Zabbix Server](img/zabbix_7.png)

## **Paso 3: Configuración del primer host VM1**

### Crear el host
1. Accede al menú principal a `Configuration/Hosts`  ![Create host](img/config_1.png)
2. Haz clic en el botón "Create host".
3. Completa los datos básicos del formulario.
   ![New host](img/config_2.png)
   ```conf
   Host name: vm1
   Template: Linux by Zabbix agent
   Groups: Linux servers
   Click Add -> Agent
   IP address: 192.168.56.201
   Click en el botón Add
   ```
   Este sería el resultado esperado:
   ![Create host](img/config_3.png)

### Configurar el agente en el host VM1
4. En tu directorio `zabbix-nerdearla2023/linux`, ejecuta:

   ```bash
   vagrant ssh vm1
   ```

5. Ejecuta los siguientes comandos uno por uno:

   ```bash
   wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu20.04_all.deb
   ```
   ```bash
   sudo dpkg -i zabbix-release_6.0-4+ubuntu20.04_all.deb
   ```
   ```bash
   sudo apt update
   ```

6. Instala el agente de Zabbix y edita su archivo de configuración:

   ```bash
   sudo apt install -y zabbix-agent
   ```
   ```bash
   sudo vim /etc/zabbix/zabbix_agentd.conf
   ```

7. Agrega la siguiente línea (reemplaza `192.168.56.200` con la dirección IP del Zabbix Server) y guarda el archivo:

   ```conf
   Server=192.168.56.200
   ```

8. Reinicia el servicio:
   ```bash
   sudo systemctl restart zabbix-agent.service
   ```

9. Opcional pero emocionante: instala la herramienta de estrés para simular consumo de memoria:

   ```bash
   sudo apt-get install stressapptest
   ```
11. Verificamos que tengamos datos del Zabbix Server
    ![Latest data](img

/config_11.png)

12. Ejecuta la herramienta de estrés (Opcional):

   ```bash
   stressapptest -s 3600
   ```

13. ¡Voilà! ¡Has completado la configuración del agente de Zabbix en `vm1`!

## **Paso 4: Instalación del Agente Zabbix en Windows VM (Opcional)**

1. Accede por RDP a la máquina virtual de Windows con la dirección IP `192.168.56.220`.

2. Instala `zabbix_agent-6.0.20-windows-amd64-openssl.msi` que se encuentra en `C:/vagrant`.
![Explorer](img/win_1.png)

3. Cuando lo solicite, coloca la dirección IP del Zabbix Server `192.168.56.200`.
![Explorer](img/win_2.png)

4. Agrega el host siguiendo los mismos pasos del Paso 3.
![New host win](img/win_3.png)
   ```conf
   Host name: Win1
   Template: Windows by Zabbix agent
   Groups: Windows servers
   Click Add -> Agent
   IP address: 192.168.56.220
   Click en el botón Add
   ```
![Create host win](img/win_4.png)

## **Paso 5: Configuración del Autoregistro de Zabbix**

1. Entra en el menú principal a `Configurations/Actions/Autoregistration actions`.
2. Haz clic en el botón "Create action". ![Create host win](img/config_4.png)
3. Completa los datos básicos del formulario.
   ![Create action](img/config_5.png)
   ```conf
   Name: Linux Servers
   Conditions: Click Add
   ```
   ![Add conditions](img/config_6.png)
   ```conf
   Type: Host metadata
   Operator: matches
   Value: Linux Servers
   Click en el botón Add
   ```
   ![Operations](img/config_7.png)
   ```conf
   Click en la solapa: Operations
   Operations: click al link Add
   ```
   ![Operations add](img/config_8.png)
   ```conf
   Operation: Add to host group
   ```

   ![Operations add to host group](img/config_9.png)
   ```conf
   Operation: Add to host group
   Host groups "Linux Server".
   Click botón Add
   ```
   ![Operations Link to template](img/config_10.png)
   ```conf
   Operation: Link to template
   Host groups "Linux by Zabbix agent".
   Click botón Add
   ```
   ![Operations Link to template](img/config_12.png)
   ```conf
   Click botón Add
   ```
   ¡Listo! Ahora tienes el Zabbix Server listo para recibir datos de autoregistro.

4. Entra al directorio `ansible` y ejecuta:

   ```bash
   ansible-playbook config-agent.yml
   ```

Al finalizar la ejecución, todos tus nuevos hosts deben estar activos en el Zabbix Server.

¡Y eso es todo, maestro! 🎉 Ahora tienes un entorno de laboratorio configurado con Zabbix para comenzar tu emocionante viaje en el mundo del monitoreo. ¡Diviértete explorando, modificando y aprendiendo! 😃📊🔍

1. Documentación oficial de Zabbix: https://www.zabbix.com/la/manuals
2. Documentación de instalación para distintos OS: https://www.zabbix.com/la/download

---

**Integraciones:**

- **Kubernetes**: [Zabbix Integration](https://www.zabbix.com/integrations/kubernetes)
- **Grafana**: [Zabbix Plugin](https://grafana.com/grafana/plugins/alexanderzobnin-zabbix-app/)

---