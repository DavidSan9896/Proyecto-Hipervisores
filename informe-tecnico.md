<div align="center">

# MIGRACIÓN A ENTORNOS VIRTUALES

**ANDRÉS CAMILO VARGAS SAAVEDRA**  
**DAVID SANTIAGO CUBILLOS MÉNDEZ**

**DOCENTE: FREY ALFONSO SANTAMARÍA BUITRAGO**  
**INGENIERO DE SISTEMAS**

**UNIVERSIDAD PEDAGÓGICA Y TECNOLÓGICA DE COLOMBIA**  
**INGENIERÍA DE SISTEMAS Y COMPUTACIÓN**  
**ELECTIVA I - IASS Y VIRTUALIZACIÓN**  
**TUNJA**  
**2026**

</div>
---

## 1. Introducción

El presente informe documenta el desarrollo del laboratorio práctico correspondiente a la asignatura Electiva I — IaaS, cuyo objetivo central fue diseñar e implementar una infraestructura virtualizada funcional que simulara la migración tecnológica de una empresa ficticia denominada ACME.

ACME representa una startup que necesita escalar su infraestructura de servidores físicos hacia un entorno virtualizado que garantice alta disponibilidad, mejor aprovechamiento de recursos, escalabilidad y reducción de costos. Para abordar este escenario, el equipo implementó tres entornos de virtualización en paralelo: Proxmox VE como hipervisor de tipo 1 (bare metal), y VMware Workstation junto con Oracle VirtualBox como hipervisores de tipo 2 (hosted).

La estrategia de comparación adoptada consistió en desplegar el mismo stack de servicios (Apache2, PHP y MySQL) en las tres máquinas virtuales bajo condiciones idénticas de hardware virtual y configuración, de modo que la única variable diferenciadora fuera el hipervisor subyacente. Esto permitió realizar una evaluación objetiva del rendimiento de cada plataforma bajo carga controlada.

---

## 2. Objetivos

### 2.1 Objetivo general

Implementar y comparar una infraestructura virtualizada compuesta por hipervisores de tipo 1 y tipo 2, desplegando servicios web homogéneos en cada entorno y evaluando su rendimiento mediante métricas técnicas cuantificables.

### 2.2 Objetivos específicos

- Configurar el modo de red Bridge en los tres hipervisores para lograr conectividad directa entre las máquinas virtuales.
- Asignar direccionamiento IP estático a cada VM para garantizar comunicación estable y predecible.
- Desplegar un stack LAMP (Linux, Apache, MySQL, PHP) idéntico en cada VM como servicio de referencia para las pruebas.
- Ejecutar pruebas de carga HTTP y ancho de banda de red sobre las tres VMs de manera simultánea y comparable.
- Recolectar y analizar métricas de rendimiento (CPU, RAM, latencia, peticiones por segundo) para fundamentar una recomendación técnica a ACME.

---

## 3. Marco teórico

### 3.1 Virtualización e IaaS

La virtualización es la tecnología que permite crear representaciones abstractas de recursos físicos de cómputo, tales como procesadores, memoria, almacenamiento y redes, mediante una capa de software denominada hipervisor. En el modelo de Infraestructura como Servicio (IaaS), esta tecnología constituye la base sobre la cual se proveen recursos computacionales bajo demanda.

### 3.2 Hipervisor tipo 1 vs. tipo 2

Los hipervisores se clasifican en dos categorías según su relación con el hardware físico:

**Tipo 1 — Bare Metal:** Se instala directamente sobre el hardware físico, sin sistema operativo intermediario. Esto le confiere acceso nativo al hardware, lo que se traduce en menor latencia, mayor eficiencia y mejor rendimiento bajo carga. Ejemplos: Proxmox VE, VMware ESXi, Microsoft Hyper-V.

**Tipo 2 — Hosted:** Se instala como una aplicación sobre un sistema operativo anfitrión (host). Introduce una capa adicional de abstracción que genera overhead de procesamiento. Su ventaja es la facilidad de uso y despliegue en equipos de escritorio. Ejemplos: VMware Workstation, Oracle VirtualBox.

### 3.3 Stack LAMP

LAMP es el acrónimo del conjunto de tecnologías de código abierto Linux, Apache, MySQL y PHP, ampliamente utilizado para el despliegue de aplicaciones web. En este laboratorio se utilizó como servicio de referencia por su facilidad de instalación, bajo consumo de recursos y capacidad de generar carga HTTP medible.

### 3.4 Modo de red Bridge

El modo Bridge (puente) conecta el adaptador de red virtual de la VM directamente al switch físico de la red, asignándole una dirección IP en el mismo segmento de red que los equipos físicos. A diferencia del modo NAT, el modo Bridge permite que las VMs sean accesibles desde cualquier equipo de la red y que se comuniquen entre sí sin restricciones.

---

## 4. Arquitectura implementada

### 4.1 Topología de red

Las tres máquinas virtuales fueron configuradas en la misma subred mediante modo Bridge, quedando visibles entre sí y desde los equipos host del laboratorio.

<img width="633" height="405" alt="image" src="https://github.com/user-attachments/assets/e7d95cae-9282-4fc7-bb63-a31dc65d6155" />
 
Figura 1: Esquema de red

### 4.2 Direccionamiento IP

| VM | Hipervisor | Tipo | IP estática | Máscara de subred |
|---|---|---|---|---|
| VM-Proxmox | Proxmox VE | Tipo 1 | 192.168.1.91 | 255.255.255.0 |
| VM-VMware | VMware Workstation | Tipo 2 | 192.168.1.101 | 255.255.255.0 |
| VM-VirtualBox | Oracle VirtualBox | Tipo 2 | 192.168.1.111 | 255.255.255.0 |

*Tabla 1: Direccionamiento de las máquinas virtuales*

### 4.3 Especificaciones de las máquinas virtuales

Para poder garantizar los recursos de las tres máquinas virtuales, se configuraron bajo condiciones homogéneas de hardware virtual. Esto implica que cada VM cuenta con el mismo sistema operativo base y parámetros equivalentes.

| Parámetro | VM-Proxmox | VM-VMware | VM-VirtualBox |
|---|---|---|---|
| Sistema operativo | Ubuntu MATE | Ubuntu MATE | Ubuntu MATE |
| CPUs virtuales | 4 vCPU | 4 vCPU | 4 vCPU |
| RAM asignada | 4 GB | 4 GB | 4 GB |
| Almacenamiento | 25 GB | 25 GB | 25 GB |
| Adaptador de red | VirtIO / Bridge | Bridge (VMnet0) | Adaptador puente |

*Tabla 2: Recursos de configuración*

---

## 5. Procedimiento técnico

### 5.1 Fase 1: Configuración de red en modo bridge

#### 5.1.1 Bridge en VMware Workstation

Para habilitar el modo Bridge en VMware Workstation, se accedió a la configuración de la VM mediante la opción "Edit virtual machine settings", sección "Network Adapter". Se seleccionó la opción "Bridged: Connected directly to the physical network", lo que conecta el adaptador virtual directamente al adaptador físico del host a través del componente VMnet0.

<img width="419" height="349" alt="image" src="https://github.com/user-attachments/assets/db2ee7f9-0f51-47ef-8420-404535aed0bf" />

Figura 2: Configuración de red Bridge en VMware Workstation]**

#### 5.1.2 Bridge en VirtualBox

En Oracle VirtualBox, la configuración se realizó desde la sección "Red" en la configuración de la VM. Se cambió el modo del Adaptador 1 de NAT a "Adaptador puente", seleccionando el adaptador físico activo del equipo host como interfaz de enlace.

<img width="701" height="442" alt="image" src="https://github.com/user-attachments/assets/6289a928-2050-4584-92fb-6db8c0838a6e" />
 
Figura 3: Configuración de red Bridge en VirtualBox]**

#### 5.1.3 Bridge en Proxmox VE

Proxmox VE crea automáticamente un bridge virtual denominado **vmbr0** durante su instalación, enlazado a la interfaz física del servidor. Se verificó desde la interfaz web de administración que la VM tuviera asignado dicho bridge como dispositivo de red.

### 5.2 Fase 2: Asignación de IPs estáticas en las VM

La asignación de direcciones IP estáticas se realizó con el fin de garantizar la estabilidad en la comunicación de cada máquina virtual durante las pruebas. El uso de IP fijas permite mantener identificadores constantes y evitar conflictos de red.

Para ello, se utilizó el sistema de configuración de red Netplan, editando el archivo correspondiente en cada VM:

`/etc/netplan/01-netcfg.yaml`

**Configuración aplicada en VM-Proxmox**

En el archivo de configuración de Proxmox se definieron parámetros fijos, como se muestra a continuación:

```yaml
network:
  version: 2
  ethernets:
    ens3:
      dhcp4: no
      addresses:
        - 192.168.1.91/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

<img width="803" height="659" alt="image" src="https://github.com/user-attachments/assets/0bd7ce71-444a-4aea-b715-86e600218165" />

Figura 4: Configuración de red IP estática en Proxmox]

**Configuración aplicada en VM-VMware**

En el segundo hipervisor, VMware, se siguió el mismo procedimiento en la misma ruta de archivo, aplicando la configuración correspondiente.

<img width="746" height="535" alt="image" src="https://github.com/user-attachments/assets/61b4b02b-0f0c-4017-ab1a-70616ced75b6" />

Figura 5: Configuración de red IP estática en VM-VMware]

Tras guardar los cambios y reiniciar la máquina, se verificó la asignación de la IP mediante el comando `ip a`, como se puede observar en la figura 5.

<img width="937" height="533" alt="image" src="https://github.com/user-attachments/assets/1299eb14-e204-4056-95de-04bdb81c54a9" />

Figura 6: Confirmación de asignación]

**Configuración aplicada en VM-VirtualBox**

Finalmente, en el tercer hipervisor, VirtualBox, la configuración quedó de la siguiente manera:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.1.111/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

<img width="691" height="735" alt="image" src="https://github.com/user-attachments/assets/c5055a18-4d07-4df1-b376-fc8066bfe2e6" />

Figura 7: Confirmación de asignación]

### 5.3 Pruebas de conectividad entre VMs

Una vez asignadas las direcciones IP estáticas, se procedió a verificar la conectividad bidireccional entre las tres máquinas virtuales mediante el comando ping. Para ello, se realizaron pruebas desde cada VM hacia las demás y hacia el host correspondiente.

**Prueba desde VMware (IP 192.168.1.101)**

Se ejecutó ping desde la VM de VMware hacia:

- VirtualBox (192.168.1.111)
- Proxmox (192.168.1.91)
- Host de VMware (192.168.1.100)

Los paquetes fueron enviados y recibidos de manera estable y constante, confirmando la comunicación correcta.

<img width="685" height="385" alt="image" src="https://github.com/user-attachments/assets/d619cdcc-0ee9-4528-ab9e-98b7c65a8d8f" />

Figura 8: Ping del hipervisor VMware a los demás y a la máquina host]

Y ahora a la inversa, desde el host a la máquina VMware de IP 192.168.1.101.

<img width="852" height="627" alt="image" src="https://github.com/user-attachments/assets/64ed5a09-8aa5-4024-be72-cbb68f30f52e" />

Figura 9: Ping de la máquina host a la VMware]

**Prueba desde Proxmox (IP 192.168.1.91)**

Se verificó la conectividad desde la VM de Proxmox hacia:

- VirtualBox (192.168.1.111)
- VMware (192.168.1.101)
- Host de Proxmox (192.168.1.90)

La comunicación fue exitosa en todos los casos.

<img width="874" height="486" alt="image" src="https://github.com/user-attachments/assets/f06a4c2a-37d5-46d5-bb0f-e0e1e2a863f3" />

Figura 10: Ping desde Proxmox a las demás VMs y al host

Ahora vamos a verificar el ping desde el host a la máquina virtual de IP 192.168.1.91.

<img width="901" height="565" alt="image" src="https://github.com/user-attachments/assets/307f8903-3e50-4d8c-a703-e6fc61c793b9" />

Figura 11: Ping desde el host hacia la VM de Proxmox

**Prueba desde VirtualBox (IP 192.168.1.111)**

Finalmente, se comprobó la conectividad desde la VM de VirtualBox hacia:

- Proxmox (192.168.1.91)
- VMware (192.168.1.101)
- Host de VirtualBox (192.168.1.110)

Los resultados confirmaron la correcta comunicación entre todas las máquinas.

<img width="797" height="492" alt="image" src="https://github.com/user-attachments/assets/24a934bf-5853-4641-8d9d-909e88eaf967" />

Figura 12: Ping desde VirtualBox a las demás VMs y al host

### 5.4 Fase 4 — Instalación del stack LAMP

Se desplegó un stack LAMP completo e idéntico en las tres máquinas virtuales. El proceso se desarrolló en dos etapas principales: instalación de componentes y configuración de seguridad.

#### 5.4.1 Instalación de Apache, PHP y MySQL

El procedimiento seguido en cada VM fue el siguiente:

**1) Actualizar los paquetes del sistema:**

```bash
sudo apt update
```

<img width="709" height="468" alt="image" src="https://github.com/user-attachments/assets/5adf50ed-6b3a-4c62-8bfa-ba44de4f7e8b" />

Figura 13: Ejecución de comando update

**2) Instalar los componentes del stack LAMP:**

```bash
sudo apt install mysql-server php libapache2-mod-php php-mysql -y
```

Este comando instala:

- **MySQL Server:** motor de base de datos relacional para almacenar la información del sistema.
- **PHP:** Lenguaje de programación del lado del servidor para generar contenido dinámico.
- **libapache2-mod-php:** módulo que permite a Apache ejecutar scripts PHP.
- **php-mysql:** extensión que conecta PHP con bases de datos MySQL.

<img width="624" height="379" alt="image" src="https://github.com/user-attachments/assets/2baea743-319c-43c0-9f22-becbdb71f6d5" />

Figura 14: Ejecución de comando de instalación

**3) Reiniciar el servicio Apache para cargar correctamente el módulo PHP:**

```bash
sudo systemctl restart apache2
```

**4) Verificar el estado del servicio Apache:**

```bash
sudo systemctl status apache2
```

<img width="595" height="338" alt="image" src="https://github.com/user-attachments/assets/65cd3aa5-3555-4f3d-b45c-3fe2e120c95b" />

Figura 15: Salida del comando en VM-VirtualBox

**5) Instalar el servicio SSH para habilitar conexiones cifradas y acceso remoto seguro:**

```bash
sudo apt install openssh-server
```

<img width="608" height="318" alt="image" src="https://github.com/user-attachments/assets/b041fefb-93c5-4e67-a30e-4a959a2f3212" />

Figura 16: Proceso de instalación del SSH

#### 5.4.2 Configuración de seguridad de MySQL

Para mejorar la seguridad básica del servidor de base de datos, se ejecutó la herramienta de configuración inicial de MySQL usando el comando:

```bash
sudo mysql_secure_installation
```

Durante este proceso se aplicaron las siguientes medidas:

- Eliminación de usuarios anónimos.
- Deshabilitar el acceso remoto del usuario root.
- Eliminación de la base de datos de pruebas.
- Recarga de los privilegios del sistema.

Estas acciones reducen significativamente los riesgos de acceso no autorizado al servidor.

<img width="698" height="582" alt="image" src="https://github.com/user-attachments/assets/e0e818d5-4768-4056-946a-2945c1b8eea6" />

Figura 17: Proceso de configuración de seguridad en MySQL

#### 5.4.3 Creación de base de datos, usuario y datos de prueba

Dentro de la base de datos creada, se generó una tabla de ejemplo llamada `clientes`, con algunos registros de prueba para validar el funcionamiento del sistema.

Se accedió al monitor de MySQL con el siguiente comando:

```bash
sudo mysql -u root
```

Dentro del monitor de MySQL se ejecutaron las siguientes sentencias en cada VM:

```sql
CREATE DATABASE came_db;
CREATE USER 'came_user'@'localhost' IDENTIFIED BY '12345';
GRANT ALL PRIVILEGES ON came_db.* TO 'came_user'@'localhost';
FLUSH PRIVILEGES;

USE came_db;

CREATE TABLE clientes (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL
);

INSERT INTO clientes (nombre) VALUES ('Juan Pérez'), ('María Gómez'), ('Carlos López');
```

<img width="718" height="249" alt="image" src="https://github.com/user-attachments/assets/141c20c7-6f63-4518-b362-e5e891eed0b9" />

Figura 18: Confirmación de creación de base de datos

Como podemos observar, tenemos la tabla de datos creada en uno de los hipervisores.

<img width="706" height="359" alt="image" src="https://github.com/user-attachments/assets/f43429cd-9ee0-40b9-beee-01be277200dc" />

Figura 19: Ejecución de sentencias SQL en VM-VMware

<img width="768" height="479" alt="image" src="https://github.com/user-attachments/assets/e340d131-613f-4502-8fd1-00526c296a5d" />

Figura 20: Ejecución de sentencias SQL en VM-VirtualBox

#### 5.4.4 Creación del archivo PHP de prueba

Se creó el archivo `/var/www/html/clientes.php` en cada VM de los hipervisores para probar la conectividad entre Apache, PHP y MySQL; se desarrolló un archivo PHP que consulta los datos almacenados en la base de datos y los muestra en el navegador.

El archivo fue creado en el directorio raíz del servidor web Apache usando lo siguiente:

```bash
cd /var/www/html/
sudo nano clientes.php
```

<img width="855" height="68" alt="image" src="https://github.com/user-attachments/assets/8d8a7f79-4113-4073-a58d-e750870cf689" />

Figura 21: Ingreso de los comandos php

El archivo tiene el siguiente contenido, teniendo en cuenta que al `h1` le cambiamos para poder identificarlos en cada máquina.

```php
<?php
$conexion = new mysqli("localhost", "came_user", "TuClaveSegura", "came_db");

if ($conexion->connect_error) {
    die("Error de conexión: " . $conexion->connect_error);
}

$resultado = $conexion->query("SELECT nombre FROM clientes");

echo "<h1>Lista de Clientes CAME de la VMware</h1>";
echo "<ul>";
while ($fila = $resultado->fetch_assoc()) {
    echo "<li>" . htmlspecialchars($fila['nombre']) . "</li>";
}
echo "</ul>";

$conexion->close();
?>
```

Con esto se conecta a la base de datos, ejecuta una consulta SQL y muestra los resultados en una página web.

#### 5.4.5 Verificación del servicio PHP-MySQL

Se accedió a la URL `http://localhost/clientes.php` desde el navegador de cada VM para verificar que el stack completo funcionara correctamente: Apache sirviendo PHP que se conecta a MySQL y devuelve datos.

<img width="522" height="292" alt="image" src="https://github.com/user-attachments/assets/028a23eb-6eb6-469b-9794-23c478663f6c" />

Figura 22: Navegador mostrando la página clientes.php en VM-VMware

<img width="393" height="346" alt="image" src="https://github.com/user-attachments/assets/90251f80-44ad-4f98-8393-01e4133384e8" />

Figura 23: Navegador mostrando la página clientes.php en VM-VirtualBox

#### 5.4.6 Verificación de acceso desde las demás VM

Para comprobar la correcta comunicación entre los hipervisores y la base de datos, se accedió al listado de clientes desde cada máquina virtual. La verificación se realizó ingresando en el navegador la URL: `http://<IP>/clientes.php`

De esta manera, se validó que cada VM pudiera consultar los datos almacenados en las demás.

**Verificación desde Proxmox**

Se accedió a la dirección correspondiente y se confirmó la conexión a las bases de datos de las demás máquinas virtuales.

<img width="651" height="439" alt="image" src="https://github.com/user-attachments/assets/b639353c-4de4-490d-a632-669794c5414e" />

 Figura 24: Verificación de conexión desde Proxmox

**Verificación desde VMware**

Se realizó el mismo procedimiento desde la VM de VMware, comprobando la correcta obtención de datos desde las demás VMs.

<img width="662" height="358" alt="image" src="https://github.com/user-attachments/assets/c0ba8ddf-32ba-492a-ba22-4f30a6a41f1b" />

Figura 25: Verificación de conexión VMware

**Verificación desde VirtualBox**

Finalmente, se verificó el acceso desde la VM de VirtualBox, confirmando la conexión estable con las bases de datos de los otros hipervisores.

<img width="771" height="406" alt="image" src="https://github.com/user-attachments/assets/9a3b5233-592d-404b-a384-f59a972b6b29" />

Figura 26: Verificación de conexión VirtualBox

### 5.5 Fase 5 — Pruebas de rendimiento
 
#### 5.5.1 Instalación de Apache Benchmark
 
```bash
sudo apt update
sudo apt install iperf3 apache2-utils htop -y
```
 
<img width="522" height="289" alt="image" src="https://github.com/user-attachments/assets/a425a78f-3540-4e28-955c-f19e14323db5" />

Figura 27: Proceso de instalación de herramientas de administración
 
#### 5.5.2 Prueba de ancho de banda de red con iperf3
 
Se midió el ancho de banda disponible entre cada par de VMs. Primero se ejecutó el comando `iperf3 -s` en cada máquina virtual y después desde cada cliente Windows que ejecutó el comando `iperf3.exe -c 192.168.1.X`.
 
**Resultado de iperf3 hacia VM-Proxmox (192.168.1.91)**
 
Prueba 1:
```bash
.\iperf3.exe -c 192.168.1.91
```
 
<img width="520" height="275" alt="image" src="https://github.com/user-attachments/assets/5b9a5ac8-04f8-488b-a46c-93ce598ae4df" />

Figura 28: Resultados de prueba 1 VM-Proxmox (192.168.1.91)
 
Prueba 2 (más paralelismo):
```bash
.\iperf3.exe -c 192.168.1.91 -P 4
```
 
<img width="502" height="308" alt="image" src="https://github.com/user-attachments/assets/fac69d28-2b9e-430c-8255-17dde3edba88" />
Figura 29: Resultados de prueba 2 VM-Proxmox (192.168.1.91)
 
Prueba 3 (más tiempo):
```bash
.\iperf3.exe -c 192.168.1.91 -t 20
```
 
<img width="505" height="306" alt="image" src="https://github.com/user-attachments/assets/7ff0d638-4637-4f4a-a17f-a646ec241225" />

Figura 30: Resultados de prueba 3 VM-Proxmox (192.168.1.91)
 
Se realizaron pruebas de rendimiento de red utilizando la herramienta iperf3 entre la máquina anfitriona (Windows 11) y la máquina virtual Ubuntu configurada como servidor. En la prueba base se obtuvo una velocidad promedio de **444 Mbps**, evidenciando un buen rendimiento de la red virtual. Posteriormente, se realizaron pruebas con múltiples flujos concurrentes (`-P 4`), alcanzando un ancho de banda agregado de **1.06 Gbps**, lo que demuestra una mejora significativa al simular múltiples conexiones simultáneas. Finalmente, se ejecutó una prueba extendida de 20 segundos (`-t 20`), obteniendo un promedio de **815 Mbps**, lo cual indica un comportamiento relativamente estable en el tiempo, aunque con ligeras variaciones debido a la virtualización.
 
---
 
**Resultado de iperf3 hacia VM-VMware (192.168.1.101)**
 
Prueba 1:
```bash
.\iperf3.exe -c 192.168.1.101
```
 
<img width="520" height="249" alt="image" src="https://github.com/user-attachments/assets/60ca4cf8-61e2-4e10-a6f5-9add41b50cc6" />

Figura 31: Resultados de prueba 1 VM-VMware (192.168.1.101)
 
Prueba 2 (más paralelismo):
```bash
.\iperf3.exe -c 192.168.1.101 -P 4
```
 
<img width="470" height="282" alt="image" src="https://github.com/user-attachments/assets/5e7971f0-1bdc-493f-9f33-30fe3fe23304" />

Figura 32: Resultados de prueba 2 VM-VMware (192.168.1.101)
 
Prueba 3 (más tiempo):
```bash
.\iperf3.exe -c 192.168.1.101 -t 20
```
 
<img width="456" height="278" alt="image" src="https://github.com/user-attachments/assets/3ecd4c1d-c536-4d8d-a98f-39606bfbbdb3" />

Figura 33: Resultados de prueba 3 VM-VMware (192.168.1.101)
 
Se realizaron pruebas de rendimiento de red utilizando la herramienta iperf3 entre la máquina anfitriona (Windows 11) y la máquina virtual Ubuntu configurada como servidor. En la prueba base se obtuvo una velocidad promedio de **49.9 Gbps**, evidenciando un buen rendimiento de la red virtual. Posteriormente, se realizaron pruebas con múltiples flujos concurrentes (`-P 4`), alcanzando un ancho de banda agregado de **107 Gbps**, lo que demuestra una mejora significativa al simular múltiples conexiones simultáneas. Finalmente, se ejecutó una prueba extendida de 20 segundos (`-t 20`), obteniendo un promedio de **49.9 Gbps**, lo cual indica un comportamiento relativamente estable en el tiempo, aunque con ligeras variaciones debido a la virtualización.
 
---
 
**Resultado de iperf3 hacia VM-VirtualBox (192.168.1.111)**
 
Prueba 1:
```bash
.\iperf3.exe -c 192.168.1.111
```
 
<img width="472" height="245" alt="image" src="https://github.com/user-attachments/assets/3732e639-f06a-40c6-b408-fb59cc36bd4c" />

Figura 34: Resultados de prueba 1 VM-VirtualBox (192.168.1.111)
 
Prueba 2 (más paralelismo):
```bash
.\iperf3.exe -c 192.168.1.111 -P 4
```
 
<img width="492" height="302" alt="image" src="https://github.com/user-attachments/assets/959aba16-e577-48e6-8555-4573eae6f348" />

Figura 35: Resultados de prueba 2 VM-VirtualBox (192.168.1.111)
 
Prueba 3 (más tiempo):
```bash
.\iperf3.exe -c 192.168.1.111 -t 20
```
 
<img width="490" height="298" alt="image" src="https://github.com/user-attachments/assets/39f97a91-f657-4798-b35c-c3d01829826a" />

Figura 36: Resultados de prueba 3 VM-VirtualBox (192.168.1.111)
 
Se realizaron pruebas de rendimiento de red utilizando la herramienta iperf3 entre la máquina anfitriona (Windows 11) y la máquina virtual Ubuntu configurada como servidor. En la prueba base se obtuvo una velocidad promedio de **444 Mbps**, evidenciando un buen rendimiento de la red virtual. Posteriormente, se realizaron pruebas con múltiples flujos concurrentes (`-P 4`), alcanzando un ancho de banda agregado de **1.06 Gbps**, lo que demuestra una mejora significativa al simular múltiples conexiones simultáneas. Finalmente, se ejecutó una prueba extendida de 20 segundos (`-t 20`), obteniendo un promedio de **815 Mbps**, lo cual indica un comportamiento relativamente estable en el tiempo, aunque con ligeras variaciones debido a la virtualización.
 
#### 5.5.3 Prueba de carga HTTP con Apache Benchmark
 
Se ejecutaron 2 000 000 peticiones con 2000 conexiones concurrentes con el siguiente comando, con el fin de determinar cuánto soporta cada servidor:
 
```bash
ab -n 2000000 -c 2000 http://<IP>/clientes.php
```
 
**Reporte completo de Apache Benchmark contra VM-Proxmox**
 
<img width="551" height="139" alt="image" src="https://github.com/user-attachments/assets/86f51c00-fd86-4628-81c8-1f56b782249d" />

Figura 37: Ejecución de Apache Benchmark
 
**Reporte completo de Apache Benchmark contra VM-VMware**
 
<img width="479" height="376" alt="image" src="https://github.com/user-attachments/assets/c91fe35a-b2f8-43f6-abf0-db1afa48fcb9" />

Figura 38: Ejecución de Apache Benchmark
 
**Reporte completo de Apache Benchmark contra VM-VirtualBox**
 
<img width="578" height="435" alt="image" src="https://github.com/user-attachments/assets/0ef044ce-1e47-47c2-84c4-1bcd0d30ee83" />

Figura 39: Ejecución de Apache Benchmark
 
#### 5.5.4 Monitoreo de CPU y RAM durante la prueba de carga
 
Simultáneamente con la ejecución de Apache Benchmark, se monitorea el consumo de recursos en cada VM con `htop` y se registró una captura instantánea.
 
**htop en VM-Proxmox durante la prueba de carga**
 
<img width="574" height="417" alt="image" src="https://github.com/user-attachments/assets/6394208b-d172-4014-8fc5-7eaf33667aa8" />

Figura 40: Monitoreo de recursos
 
**htop en VM-VMware durante la prueba de carga**
 
<img width="598" height="310" alt="image" src="https://github.com/user-attachments/assets/d81327af-eb85-4a8a-aced-c7648f856070" />

Figura 41: Monitoreo de recursos
 
**htop en VM-VirtualBox durante la prueba de carga**
 
<img width="503" height="303" alt="image" src="https://github.com/user-attachments/assets/856b399a-edb5-4ba1-9d0e-7ae36672ca62" />

Figura 42: Monitoreo de recursos
 
---

## 6. Resultados obtenidos
 
### 6.1 Métricas de red
 
| Métrica | VM-Proxmox (Tipo 1) | VM-VMware (Tipo 2) | VM-VirtualBox (Tipo 2) |
|---|---|---|---|
| Ancho de banda base | 444 Mbps | 49.9 Gbps | 444 Mbps |
| Ancho de banda con paralelismo (-P 4) | 1.06 Gbps | 107 Gbps | 1.06 Gbps |
| Ancho de banda extendido (-t 20) | 815 Mbps | 49.9 Gbps | 815 Mbps |
 
*Tabla 3: Métricas de red*
 
### 6.2 Métricas HTTP
 
| Métrica | VM-Proxmox (Tipo 1) | VM-VMware (Tipo 2) | VM-VirtualBox (Tipo 2) |
|---|---|---|---|
| Requests per second (req/s) | Mayor | Intermedio | Menor |
| Time per request — media (ms) | Menor | Intermedio | Mayor |
| Failed requests | 0 | 0 | 0 |
| Transfer rate (KB/s) | Alto | Medio | Medio |
 
*Tabla 4: Métricas HTTP*
 
### 6.3 Métricas de sistema
 
| Métrica | VM-Proxmox (Tipo 1) | VM-VMware (Tipo 2) | VM-VirtualBox (Tipo 2) |
|---|---|---|---|
| CPU% pico | Menor | Alto | Más alto |
| CPU% promedio | Menor | Medio | Alto |
| RAM usada bajo carga (MB) | Menor | Media | Mayor |
 
*Tabla 5: Métricas de sistema*
 
---
 
## 7. Análisis comparativo
 
### 7.1 Rendimiento de red
 
Con base en los resultados obtenidos mediante iperf3, se observa que VM-Proxmox y VM-VirtualBox presentan un comportamiento consistente, alcanzando valores de hasta **1.06 Gbps** con múltiples flujos y manteniendo estabilidad en pruebas prolongadas (≈815 Mbps).
 
Por otro lado, VMware reporta valores significativamente superiores (hasta **107 Gbps**). Sin embargo, estos resultados no son comparables directamente, ya que probablemente corresponden a optimizaciones internas del entorno virtual o mediciones dentro del mismo host, lo que no refleja condiciones reales de red.
 
En este sentido, Proxmox demuestra un rendimiento más estable y realista, coherente con su arquitectura tipo 1.
 
### 7.2 Rendimiento del servicio HTTP
 
Durante las pruebas con Apache Benchmark, se evidenció que VM-Proxmox logra procesar un mayor número de solicitudes por segundo, con menor tiempo de respuesta. VMware presenta un rendimiento intermedio, mientras que VirtualBox muestra el menor desempeño bajo carga.
 
La diferencia observada indica que el hipervisor tipo 1 optimiza mejor el manejo de múltiples conexiones concurrentes, lo cual es clave en aplicaciones web reales.
 
### 7.3 Eficiencia en uso de recursos
 
El monitoreo con `htop` permitió observar que:
 
- **Proxmox** utiliza menos CPU para procesar la misma carga.
- **VMware** incrementa el consumo.
- **VirtualBox** presenta el mayor uso de CPU.
 
Esto confirma que los hipervisores tipo 2 generan overhead adicional, ya que dependen del sistema operativo anfitrión. En memoria RAM, la tendencia es similar: Proxmox resulta más eficiente, mientras que VirtualBox presenta el mayor consumo.
 
### 7.4 Síntesis del análisis
 
| Criterio | Mejor resultado | Hipervisor |
|---|---|---|
| Rendimiento de red estable | 1.06 Gbps | Proxmox / VirtualBox |
| Resultados más realistas | Consistentes | Proxmox |
| Mayor throughput HTTP | Superior | Proxmox |
| Menor consumo de CPU | Más eficiente | Proxmox |
| Mayor estabilidad de red | Óptimo | Proxmox |
 
---
 
## 8. Recomendación estratégica para ACME
 
### 8.1 Contexto empresarial
 
ACME es una startup en fase de crecimiento que requiere una infraestructura tecnológica capaz de escalar de forma progresiva, garantizando la disponibilidad de sus servicios y manteniendo un control eficiente de los costos operativos. En este contexto, la elección de una plataforma de virtualización no solo impacta el rendimiento del sistema, sino también la eficiencia en el uso de recursos, la facilidad de administración y la viabilidad económica a largo plazo.
 
### 8.2 Hallazgos técnicos relevantes
 
Los resultados obtenidos durante el laboratorio permiten identificar diferencias claras entre los hipervisores evaluados:
 
- **Proxmox VE**, como hipervisor de tipo 1, demostró un mejor rendimiento general, especialmente en términos de eficiencia en el uso de CPU y estabilidad en el rendimiento de red, alcanzando valores de hasta **1.06 Gbps** en pruebas con múltiples flujos y manteniendo un comportamiento consistente en el tiempo.
- En comparación, los hipervisores de tipo 2 (VMware Workstation y VirtualBox) presentaron un rendimiento funcional pero con mayor variabilidad y consumo de recursos, debido al overhead introducido por el sistema operativo anfitrión.
- Aunque VMware reportó valores de ancho de banda significativamente superiores (hasta **107 Gbps**), estos resultados no son directamente comparables en un entorno real, ya que responden a optimizaciones internas del entorno virtualizado.
- En cuanto al uso de recursos, se evidenció que el consumo de CPU bajo carga es mayor en los hipervisores tipo 2, lo que implica que se requieren más recursos físicos para sostener el mismo nivel de servicio en comparación con un hipervisor tipo 1.
- Adicionalmente, el comportamiento observado en las pruebas de carga HTTP muestra que Proxmox gestiona de manera más eficiente las conexiones concurrentes, ofreciendo menores tiempos de respuesta y mayor capacidad de procesamiento.
 
### 8.3 Recomendación
 
Para el entorno de producción de ACME, se recomienda adoptar **Proxmox VE** como plataforma principal de virtualización, debido a las siguientes razones:
 
En primer lugar, desde el punto de vista técnico, Proxmox ofrece una mayor eficiencia en el uso de los recursos de hardware, al eliminar la capa intermedia del sistema operativo anfitrión. Esto se traduce en menores tiempos de respuesta, mayor estabilidad y mejor rendimiento bajo carga, aspectos críticos en entornos productivos.
 
En segundo lugar, desde la perspectiva económica, Proxmox VE es una solución de código abierto sin costos de licenciamiento, lo que representa una ventaja significativa para una startup que busca optimizar su inversión en infraestructura.
 
Finalmente, Proxmox incorpora funcionalidades avanzadas como clustering, alta disponibilidad (HA), snapshots y gestión centralizada de máquinas virtuales, lo que facilita la escalabilidad del sistema y se alinea directamente con las necesidades de crecimiento de ACME.
 
Para los entornos de desarrollo y pruebas, se recomienda mantener el uso de **VMware Workstation** y **VirtualBox**, ya que permiten la creación rápida de entornos locales sin necesidad de infraestructura dedicada, favoreciendo la productividad del equipo de desarrollo.
 
### 8.4 Plan de migración sugerido
 
| Fase | Descripción | Plataforma |
|---|---|---|
| Fase 1 — Corto plazo | Desarrollo y pruebas de nuevas funcionalidades en equipos locales | VirtualBox / VMware Workstation |
| Fase 2 — Mediano plazo | Despliegue de servicios en producción sobre servidor dedicado | Proxmox VE |
| Fase 3 — Largo plazo | Escalado mediante clustering de nodos Proxmox con alta disponibilidad | Proxmox VE Cluster |
 
---
 
## 9. Conclusiones
 
A lo largo del laboratorio se logró implementar de manera exitosa una infraestructura virtualizada compuesta por tres hipervisores distintos, interconectados en una red común mediante modo Bridge, con un stack de servicios web homogéneo desplegado en cada entorno.
 
La estrategia de comparar los tres hipervisores en igualdad de condiciones, corriendo el mismo servidor LAMP (Apache, PHP y MySQL) y sometiéndolos a la misma carga mediante Apache Benchmark e iperf3, permitió obtener datos objetivos sobre el comportamiento de cada plataforma.
 
Los resultados obtenidos confirman la diferencia teórica entre hipervisores tipo 1 y tipo 2. Desde el punto de vista del aprendizaje, el laboratorio permitió al equipo desarrollar competencias prácticas en configuración de redes virtuales, administración de servicios en Linux, diagnóstico de problemas de conectividad y análisis de rendimiento de infraestructura, habilidades directamente aplicables al ejercicio profesional en entornos de TI empresariales.
 
---
 
## 10. Referencias
 
- Proxmox Server Solutions GmbH. (2024). *Proxmox VE Administration Guide*. https://pve.proxmox.com/pve-docs/
- Oracle Corporation. (2024). *Oracle VM VirtualBox User Manual*. https://www.virtualbox.org/manual/
- VMware Inc. (2024). *VMware Workstation Pro Documentation*. https://docs.vmware.com/en/VMware-Workstation-Pro/
- The Apache Software Foundation. (2024). *Apache HTTP Server Documentation*. https://httpd.apache.org/docs/
- PHP Group. (2024). *PHP Manual*. https://www.php.net/manual/
- MySQL AB. (2024). *MySQL 8.0 Reference Manual*. https://dev.mysql.com/doc/refman/8.0/en/
- Tanenbaum, A. S., & Bos, H. (2015). *Modern Operating Systems* (4th ed.). Pearson.
