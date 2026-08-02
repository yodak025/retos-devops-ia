# Despliegue de Flask con Ansible y Nginx

## Criterios de calificación

### Creación y configuración del inventario y playbook de Ansible

**Peso: 40 %**

- **Inventario en formato YAML:** el archivo `inventory.yml` está correctamente definido en formato YAML, define al menos un host o grupo de hosts para el despliegue e incluye la variable `flask_port`.
- **Playbook `deploy.yml`:** incluye todas las tareas necesarias para desplegar Nginx y la aplicación Flask, utiliza la variable `flask_port` en las configuraciones y el despliegue, maneja los servicios mediante systemd e implementa handlers para reiniciarlos cuando sea necesario.

### Implementación de Nginx como proxy inverso

**Peso: 40 %**

- Configura correctamente Nginx para actuar como proxy inverso hacia la aplicación Flask.
- Utiliza la variable `flask_port` para redirigir las solicitudes del puerto `80` al puerto de la aplicación Flask.
- La sintaxis del archivo de configuración es correcta y Nginx se reinicia sin errores.
- La variable `flask_port` está correctamente definida y se utiliza en todas las configuraciones necesarias de Nginx y Flask.
- El puerto puede modificarse fácilmente sin necesidad de cambiar varios archivos de forma manual.

### Desarrollo de la aplicación Flask

**Peso: 20 %**

- Contiene una aplicación Flask que devuelve `"Hola Mundo"` en la ruta `/`.
- Está configurada para ejecutarse en el puerto y la dirección definidos mediante la variable `flask_port`.
- Se ejecuta como un servicio gestionado por systemd, por lo que permanece en ejecución y se inicia automáticamente al reiniciar el sistema.

## Descripción

En este ejercicio deberás utilizar Ansible para configurar un servidor Nginx como proxy inverso de una aplicación Flask. Crearás cuatro archivos:

- Un inventario.
- Un playbook en YAML.
- Un archivo de configuración de Nginx.
- Una aplicación Flask en Python que muestre `"Hola Mundo"`.

Desarrolla el proyecto en tu ordenador, súbelo a un repositorio público de GitHub y conéctalo a este ejercicio para evaluarlo.

## Requisitos

Desarrolla un proyecto utilizando Ansible que cumpla los siguientes requisitos:

### 1. Inventario de hosts (`inventory`)

Crea un archivo de inventario en formato YAML que defina al menos un host o grupo de hosts donde se desplegará la aplicación.

### 2. Variables de configuración (`vars`)

Define una variable en Ansible para especificar el puerto en el que escuchará la aplicación Flask. Esta variable se utilizará tanto en la configuración de Nginx como en la aplicación Flask. El puerto predeterminado suele ser el `5000`.

### 3. Playbook de Ansible (`deploy.yml`)

Escribe un playbook en YAML que realice las siguientes tareas:

- **Instalación de Nginx:** instala Nginx en el host remoto utilizando el módulo adecuado de Ansible.
- **Instalación de Python y dependencias:** instala Python y las dependencias necesarias para ejecutar una aplicación Flask, por ejemplo mediante pip.
- **Configuración de variables:** define una variable para el puerto de la aplicación Flask.
- **Copiado de archivos de configuración:**
  - Copia `nginx.conf` desde el controlador al directorio adecuado del host remoto.
  - Copia `app.py` al directorio apropiado del host remoto.
- **Configuración y arranque de servicios:**
  - Inicia y habilita el servicio de Nginx.
  - Inicia la aplicación Flask en el puerto configurado mediante systemd para que se ejecute en segundo plano y arranque automáticamente al reiniciar el sistema.

### 4. Configuración de Nginx (`nginx.conf`)

- Crea un archivo de configuración para que Nginx actúe como proxy inverso de la aplicación Flask.
- Utiliza la variable de Ansible definida para especificar el puerto al que Nginx redirigirá las solicitudes HTTP.

### 5. Aplicación Flask (`app.py`)

- Desarrolla una aplicación sencilla en Python con Flask que devuelva el mensaje `"Hola Mundo"` al acceder a la ruta raíz (`/`).
- Configura la aplicación para que escuche en todas las interfaces (`0.0.0.0`) y en el puerto especificado mediante la variable de Ansible.

### 6. Ejecutar el playbook

- Ejecuta el playbook `deploy.yml` contra los hosts definidos en el inventario.
- Verifica que no se produzcan errores durante la ejecución.

### 7. Probar la aplicación

- Una vez completado el despliegue, abre un navegador y accede a la dirección IP o al nombre de dominio del host mediante el puerto `80`.
- Comprueba que se muestra el mensaje `"Hola Mundo"`.

## Instrucciones

### Crear el archivo de inventario

1. Crea un archivo llamado `inventory.yml` en formato YAML.
2. Especifica el host o grupo de hosts donde se aplicará el playbook.
3. Define la variable `flask_port` en el inventario o en un archivo de variables separado.

### Definir las variables de configuración

Crea un archivo de variables, por ejemplo `vars/main.yml`, y define en él la variable `flask_port`.

### Desarrollar el playbook de Ansible

1. Crea un archivo llamado `deploy.yml`.
2. Define en el playbook el objetivo (`hosts`) utilizando el inventario creado.
3. Incluye todas las tareas necesarias para instalar Nginx, Python y las dependencias de Flask, copiar los archivos de configuración y configurar los servicios.
4. Utiliza la variable `flask_port` en las plantillas de configuración.

### Configurar Nginx como proxy inverso

Escribe `nginx.conf` como una plantilla Jinja2 que utilice la variable `flask_port` para redirigir las solicitudes al puerto correcto.

### Desarrollar la aplicación Flask

Crea un archivo `app.py` con una aplicación básica de Flask que utilice la variable `flask_port` para definir el puerto de escucha.

### Ejecutar el playbook

Ejecuta el siguiente comando y verifica que no se produzcan errores:

```bash
ansible-playbook -i inventory.yml deploy.yml
```

### Probar la aplicación

1. Una vez completado el despliegue, abre un navegador y accede a la dirección IP o al nombre de dominio del host mediante el puerto `80`.
2. Comprueba que se muestra el mensaje `"Hola Mundo"`.
