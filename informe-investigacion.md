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

## Tabla de contenido

- [1. Punto de partida](#1-punto-de-partida)
  - [1.1. Tema principal](#11-tema-principal)
  - [1.2. Pregunta inicial](#12-pregunta-inicial)
  - [1.3. ¿Qué sabemos y qué no sabemos? (detección de ideas previas)](#13-qué-sabemos-y-qué-no-sabemos-detección-de-ideas-previas)
- [2. Formación de equipos colaborativos](#2-formación-de-equipos-colaborativos)
- [3. Definición de producto final](#3-definición-de-producto-final)
  - [3.1. Producto a desarrollar](#31-producto-a-desarrollar)
  - [3.2. ¿Qué hay que saber?](#32-qué-hay-que-saber)
  - [3.3. Objetivos del aprendizaje](#33-objetivos-del-aprendizaje)
- [4. Organización y planificación](#4-organización-y-planificación)
  - [4.1. Asignación de roles](#41-asignación-de-roles)
  - [4.2. Definición de tareas y tiempos](#42-definición-de-tareas-y-tiempos)
  - [4.3. Habilidades blandas que implica el proyecto](#43-habilidades-blandas-que-implica-el-proyecto)
- [5. Búsqueda y recopilación de información](#5-búsqueda-y-recopilación-de-información)
- [6. Análisis y síntesis (Diseño de la solución)](#6-análisis-y-síntesis-diseño-de-la-solución)
- [7. Taller/Producción](#7-tallerproducción)
- [8. Presentación del proyecto y recomendación estratégica](#8-presentación-del-proyecto-y-recomendación-estratégica)
- [9. Respuesta colectiva a la pregunta inicial](#9-respuesta-colectiva-a-la-pregunta-inicial)
- [10. Evaluación y autoevaluación](#10-evaluación-y-autoevaluación)
- [11. Anexos](#11-anexos)

---

# Punto de partida

## Tema principal

Migración de infraestructura física a entornos virtualizados mediante IaaS, implementando y comparando hipervisores tipo 1 y tipo 2 en un escenario empresarial simulado.

El tema está ligado a una situación real:

Una startup (ACME) necesita escalar su infraestructura tecnológica migrando de servidores físicos a un entorno virtualizado que garantice:

- Alta disponibilidad
- Mejor aprovechamiento de recursos
- Escalabilidad
- Reducción de costos

Este tema permite desarrollar competencias técnicas reales del curso de IaaS: virtualización, redes virtuales, gestión de VMs, análisis de rendimiento y toma de decisiones técnicas.

## 1.2. Pregunta inicial

**¿Cómo diseñar e implementar una infraestructura virtual escalable para una startup, integrando hipervisores tipo 1 y tipo 2, garantizando conectividad, rendimiento y alta disponibilidad?**

## 1.3. ¿Qué sabemos y qué no sabemos? (detección de ideas previas)

| ¿Qué sabemos? | ¿Qué no sabemos? |
|---|---|
| ¿Qué es la virtualización y para qué se utiliza? | Qué hipervisor conviene elegir según el tipo de servidor o necesidad. |
| Diferencia básica entre hipervisores de tipo 1 y tipo 2. | Cómo interconectar máquinas virtuales que están en diferentes plataformas (por ejemplo, VirtualBox y Proxmox). |
| Arquitectura de una máquina virtual: CPU virtual, memoria RAM y almacenamiento virtual. | Cómo diseñar una red virtual funcional que conecte múltiples entornos de virtualización. |
| Instalación y uso básico de VirtualBox como hipervisor de tipo 2. | ¿Qué problemas reales pueden surgir durante la configuración de red y virtualización? |
| Conceptos básicos de red: IP, gateway y modos de conexión. | Cómo justificar técnicamente una recomendación estratégica sobre la infraestructura virtual. |

**Tabla 1. Lo que sabemos y no sabemos**

---

# Formación de equipos colaborativos

Para el desarrollo del proyecto se estableció una distribución básica de responsabilidades entre los integrantes del grupo, con el fin de organizar el trabajo y cumplir con cada una de las etapas planteadas en la planificación.

- Andrés Camilo Vargas Saavedra
- David Santiago Cubillos Méndez

---

# Definición de producto final

## 3.1. Producto a desarrollar

Se llevará a cabo la creación de una red con 3 máquinas virtuales conectadas entre sí. Estas máquinas se administrarán usando distintas herramientas, que incluirán al menos un hipervisor tipo 1 y uno tipo 2. Además, se presentará un informe técnico comparando las opciones y se dará una recomendación estratégica para la empresa ACME.

## 3.2. ¿Qué hay que saber?

- Arquitectura de virtualización.
- Diferencias entre hipervisor tipo 1 y tipo 2.
- Instalación y configuración de:
  - VirtualBox (Tipo 2)
  - VMware (Tipo 2)
  - Proxmox (Tipo 1)
- Configuración de redes virtuales:
  - NAT
  - Bridge
  - Red interna
- Asignación de recursos (CPU, RAM, almacenamiento).
- Monitoreo de rendimiento.
- Documentación técnica.
- Argumentación técnica basada en evidencia.

## 3.3. Objetivos del aprendizaje

### 3.3.4 Nivel técnico

- Implementar hipervisores bare metal (tipo 1) y hosted (tipo 2).
- Configurar redes virtuales en modo puente (bridged) para asegurar interoperabilidad.
- Desplegar servicios de servidor (stack web/BD) en entornos CLI (Linux).
- Configurar la conexión entre los 3 hipervisores y los hosts.
- Ejecutar pruebas para la conectividad entre las VMs del entorno.

### 3.3.5 Nivel analítico

- Evaluar el rendimiento del sistema mediante métricas de carga y latencia.
- Justificar la selección de herramientas según criterios de costo, disponibilidad y escalabilidad.

### 3.3.6 Nivel colaborativo

- Gestionar flujos de trabajo asíncronos (individuales) integrados en sesiones grupales.
- Resolver cuellos de botella técnicos mediante documentación compartida.

---

# Organización y planificación

## Asignación de roles

Los roles se distribuyen de acuerdo a áreas de conocimiento de la gestión de servicios de TI.

### A. Líder de Proyecto (Project Manager - PMBOK)

**Referencia:** Basado en el rol de Project Manager de la Guía PMBOK.  
**Responsabilidad:** Coordinar el cronograma, asegurar el cumplimiento de los objetivos estratégicos y gestionar la comunicación entre los niveles del proyecto. Es el responsable de la sustentación final y de la toma de decisiones.

### B. Administrador de Infraestructura (Infrastructure Specialist - ITIL)

**Referencia:** Basado en la función de Gestión Técnica de ITIL.  
**Responsabilidad:** Instalación y configuración de los hipervisores. Se encarga específicamente del hipervisor tipo 1 (Proxmox/ESXi), la creación de plantillas de servidores (web y BD) y la asignación de recursos (CPU/RAM).

### C. Especialista en Redes Virtuales (Network Analyst - ITIL)

**Referencia:** Basado en la función de Gestión de Operaciones de Red de ITIL.  
**Responsabilidad:** Implementación de la conectividad interna. Su tarea crítica es configurar el switch virtual (vSwitch) y asegurar que las VMs de diferentes hipervisores (tipo 1 y tipo 2) se comuniquen. Resuelve el "Nivel Técnico".

### D. Ingeniero de Integración y Documentación Técnica

**Referencia:** Basado en la práctica de Gestión del Conocimiento y Gestión de Configuración en ITIL.  
**Responsabilidad:** Apoyar la integración técnica entre los hipervisores tipo 1 y tipo 2, verificando que las configuraciones de red y recursos funcionen correctamente. Se encarga de organizar y consolidar la documentación técnica del proyecto (IPs, topología, métricas y configuraciones) y apoyar la redacción estructurada del informe final. Responde por la coherencia técnica y la formalización del trabajo realizado.

| Rol | Tarea principal | Nombre |
|---|---|---|
| Líder de proyecto | Planificación y viabilidad | David Santiago Cubillos Méndez |
| Analista de calidad | Benchmarking y lecciones aprendidas |  
| Ing. Integración y Documentación Técnica | Estandarización técnica, validación de configuración y consolidación del informe | |
| Adm. Infraestructura | Configuración de hipervisores | Andrés Camilo Vargas Saavedra |
| Esp. en redes | Interconexión y protocolos |  

**Tabla 2. Distribución de roles del equipo.**

## Definición de tareas y tiempos

Se construyó un proyecto dentro de Jira, que ayudará a llevar control del cronograma del proyecto. En la siguiente imagen se muestra una visualización; para poder acceder al proyecto, haz clic en este enlace: **[Cronograma Jira](https://anthonygs.atlassian.net/jira/software/projects/KAN/boards/1?atlOrigin=eyJpIjoiNDM1MzAxMjA3YmQxNDk4NmJiY2ZjNWQ4MDI5OGFkNjUiLCJwIjoiaiJ9)**

<img width="394" height="679" alt="image" src="https://github.com/user-attachments/assets/e48dcccb-b3c6-4e9b-8af8-4bf415b271cd" />

**Figura 1. Previsualización de semanas.**

<img width="1614" height="850" alt="image" src="https://github.com/user-attachments/assets/9d224ac7-6b97-49ea-85d3-3488357ef02b" /> 

**Figura 2. Previsualización del cronograma.**

---

# Habilidades blandas que implica el proyecto

Además de las competencias técnicas propias de la virtualización y la infraestructura IaaS, el proyecto fortalece diversas habilidades blandas fundamentales en el entorno profesional de TI. En primer lugar, se trabaja el trabajo en equipo, ya que la implementación requiere coordinación constante entre quienes configuran hipervisores, redes y pruebas de rendimiento. También se desarrolla la comunicación efectiva, tanto para explicar configuraciones técnicas como para documentar hallazgos y sustentar decisiones ante un público académico. El proyecto fomenta el pensamiento crítico y analítico, al comparar objetivamente hipervisores tipo 1 y tipo 2 y justificar recomendaciones estratégicas para la empresa ACME. Asimismo, impulsa la resolución de problemas, dado que la configuración de redes virtuales y la interoperabilidad entre plataformas suelen generar incidencias que deben abordarse de manera colaborativa. Finalmente, se fortalece la gestión del tiempo y la responsabilidad, al trabajar con un cronograma definido (diagrama de Gantt) y asumir compromisos individuales dentro de un objetivo común. Estas habilidades complementan la formación técnica y acercan el proyecto a un escenario real de gestión de infraestructura en una organización.

# Búsqueda y recopilación de información

Durante esta fase, el equipo investigó las características técnicas necesarias para cumplir con los objetivos de la startup ACME. Se analizó la arquitectura de los hipervisores seleccionados: Proxmox como representante de tipo 1 (bare metal), y VMware y VirtualBox como hipervisores de tipo 2 (hosted).

**Topologías de red físicas y virtuales:** Tras investigar los modos de conexión (NAT, Bridge y Red interna), se concluyó que la configuración en modo puente (Bridged), respaldada por un switch físico para interconectar los equipos anfitriones, es la única que permite la comunicación directa y transparente entre las máquinas virtuales alojadas en distintas plataformas físicas.

**Pila tecnológica (stack):** Se investigaron los requerimientos para desplegar aplicaciones web dinámicas utilizando (Linux, Apache, MySQL, PHP) y las mejores prácticas para asegurar el motor de base de datos.

# Análisis y síntesis (Diseño de la solución)

Con la información recopilada, el equipo procedió a estandarizar la configuración técnica. Se tomaron las siguientes decisiones de diseño y arquitectura:

**Sistema operativo:** Se definió el uso de Ubuntu MATE para las tres máquinas virtuales, garantizando un entorno idéntico en todos los hipervisores.

**Infraestructura y direccionamiento IP:** Se utilizarán 3 máquinas físicas conectadas mediante un switch físico con direccionamiento estático. La matriz de red asignada es la siguiente:

- Entorno Proxmox: Host físico (192.168.1.90) - VM (192.168.1.91)
- Entorno VMware: Host físico (192.168.1.100) - VM (192.168.1.101)
- Entorno VirtualBox: Host físico (192.168.1.110) - VM (192.168.1.111)

# Taller/Producción

Bajo la supervisión del Administrador de Infraestructura, se ejecutó la instalación de los hipervisores en los 3 equipos físicos y se desplegó Ubuntu MATE en las respectivas máquinas virtuales.

Posteriormente, se implementó el stack tecnológico para habilitar aplicaciones web dinámicas.

## 1. Instalación del servidor web y base de datos

Se actualizaron los repositorios y se instalaron los paquetes de MySQL Server (motor relacional), PHP y los módulos de integración para Apache (`libapache2-mod-php` y `php-mysql`).

## 2. Configuración de seguridad en MySQL

Para fortalecer la seguridad básica del servidor, se ejecutó la herramienta `sudo mysql_secure_installation`, aplicando acciones críticas como: eliminación de usuarios anónimos, deshabilitación del acceso remoto para el usuario root, supresión de la base de datos de pruebas y recarga de privilegios.

## 3. Creación de base de datos y datos de prueba

Accediendo al monitor de MySQL (`sudo mysql -u root`), se creó la base de datos `came_db` y un usuario con privilegios específicos para evitar el uso de root, Luego, se creó la tabla `clientes` y se insertaron registros de prueba (Juan Pérez, María Gómez, Carlos López).

## 4. Despliegue de aplicación web (PHP)

Para validar la conectividad del entorno, se desarrolló el archivo `/var/www/html/clientes.php` que realiza una consulta a la base de datos `came_db` e imprime los registros de los clientes en formato HTML.

# Presentación del proyecto y recomendación estratégica

Como producto final, se consolidó la topología física/virtual y se redactó el dictamen para la empresa ACME.

**Demostración:** Se comprobó el éxito del proyecto accediendo desde navegadores externos a las IPs de las VMs (ej. `http://192.168.1.91/clientes.php`), visualizando correctamente la lista de clientes generada desde la base de datos.

**Recomendación para ACME:** Se recomienda utilizar el entorno Proxmox (`192.168.1.90`) para alojar la base de datos principal (`came_db`) debido a su rendimiento nativo, mientras que los hipervisores de tipo 2 (VirtualBox y VMware) pueden emplearse como nodos de balanceo de carga para el servidor web Apache, o como entornos replicados para desarrollo y pruebas.

# Respuesta colectiva a la pregunta inicial

Al finalizar el proyecto, el equipo da respuesta a la pregunta inicial: **¿Cómo diseñar e implementar una infraestructura virtual escalable para una startup, integrando hipervisores tipo 1 y tipo 2, garantizando conectividad, rendimiento y alta disponibilidad?**

**Respuesta:** Una infraestructura escalable e interoperable se logra mediante una topología de red en modo puente (bridge) respaldada por un switch físico, lo que permite asignar direccionamiento estático del mismo segmento de red tanto a hosts como a VMs (ej. red 192.168.1.0/24). Al estandarizar el sistema operativo (Ubuntu MATE) y los servicios independientemente del hipervisor subyacente (Proxmox, VMware o VirtualBox), se garantiza la alta disponibilidad, permitiendo a la startup ACME migrar o escalar sus servicios web y de bases de datos de forma transparente.

# Evaluación y autoevaluación

Para el cierre del proyecto, el analista de calidad ejecutó las pruebas de conectividad y despliegue.

## Evaluación del producto

Se validó que el script PHP en cada una de las 3 máquinas virtuales lograra procesar la petición al motor MySQL local, demostrando la correcta integración del módulo `php-mysql` y la efectividad de las configuraciones de seguridad implementadas con `mysql_secure_installation`.

## Coevaluación y trabajo en equipo

La coordinación fue fundamental para mantener la consistencia lógica a pesar de utilizar plataformas físicas separadas. El equipo documentó exitosamente la asignación de IPs estáticas y las sentencias SQL, demostrando la capacidad de integrar conocimientos de redes, administración de servidores (Linux/Apache) y bases de datos.






