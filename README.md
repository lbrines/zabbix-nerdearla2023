# **Instrucciones de Instalación para Workshop - Monitoreo con Zabbix**

¡Bienvenidos al emocionante mundo del monitoreo con Zabbix! 🚀 Este tutorial te guiará a través de la configuración paso a paso para tener tu propio entorno de laboratorio listo para monitorear. ¡No te preocupes si eres un novato en esto, estamos aquí para hacerlo divertido y fácil! 😄

## Índice

- [**Instrucciones de Instalación para Workshop - Monitoreo con Zabbix**](#instrucciones-de-instalación-para-workshop---monitoreo-con-zabbix)
  - [**Requisitos** 🛠️](#requisitos-️)
  - [**Direcciones IP de las VMs**](#direcciones-ip-de-las-vms)
  - [**Paso 1: Preparación del Entorno**](#paso-1-preparación-del-entorno)
  - [**Paso 2: Configuración del Zabbix Server**](#paso-2-configuración-del-zabbix-server)
  - [**Paso 3: Configuración del Zabbix Agent en VM1**](#paso-3-configuración-del-zabbix-agent-en-vm1)
  - [**Paso 4: Instalación del Agente Zabbix en Windows VM (Opcional)**](#paso-4-instalación-del-agente-zabbix-en-windows-vm-opcional)
  - [**Paso 5: Configuración del Autoregistro de Zabbix**](#paso-5-configuración-del-autoregistro-de-zabbix)

¡Disfruta del contenido! 📚



## **Requisitos** 🛠️

Antes de comenzar, asegúrate de tener instalados los siguientes programas en tu máquina:

- [Ansible](https://www.ansible.com/)
- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)

## **Direcciones IP de las VMs**

Asegúrate de tomar nota de las siguientes direcciones IP para acceder a tus máquinas virtuales en el laboratorio:

- Zabbix Server: 192.168.56.200
- Windows VM: 192.168.56.220
- VM vm1: 192.168.56.201
- VM vm2: 192.168.56.202
- VM vm3: 192.168.56.203
- VM vm4: 192.168.56.204

## **Paso 1: Preparación del Entorno**

1. Clona el repositorio del laboratorio.

2. Navega al directorio `linux` y ejecuta el comando `vagrant up` para levantar las máquinas virtuales Linux.

3. ¡Opcional y emocionante! Explora el directorio `windows` y también ejecuta `vagrant up` para levantar máquinas virtuales Windows.

## **Paso 2: Configuración del Zabbix Server**

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
