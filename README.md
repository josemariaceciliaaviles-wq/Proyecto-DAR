# Proyecto de Protocolo Cliente-Servidor para Gestión de Envíos

## 1. Descripción del protocolo implementado

Este proyecto implementa un **protocolo de comunicación cliente-servidor** orientado a la **gestión y trazabilidad de envíos**. El sistema permite que un cliente se conecte a un servidor, se autentique y, una vez validado, intercambie mensajes estructurados para consultar, crear y actualizar información asociada a envíos.

El protocolo definido sigue un esquema de comunicación basado en **mensajes de texto**, donde cada mensaje representa una orden o comando enviado por el cliente, y cada respuesta representa el resultado del procesamiento realizado por el servidor.

La comunicación se apoya en el modelo clásico **petición-respuesta**:

- El **cliente** envía un comando.
- El **servidor** interpreta ese comando.
- El **servidor** devuelve una respuesta adecuada según el estado de la sesión y la validez de la operación solicitada.

El sistema contempla al menos las siguientes funcionalidades:

- **Autenticación de usuario**
- **Mantenimiento de sesión**
- **Creación de envíos**
- **Consulta de envíos**
- **Actualización del estado de un envío**
- **Mensajes de control del protocolo** como `PING` o `LOGOUT`

Además, el protocolo incorpora un comportamiento basado en **estados**, de forma que determinadas operaciones solo pueden ejecutarse si el usuario ha iniciado sesión correctamente.

---

## 2. Arquitectura general del protocolo

La arquitectura implementada sigue un modelo **cliente-servidor centralizado**, compuesto por los siguientes elementos:

### 2.1. Servidor

El servidor es el componente encargado de:

- Escuchar conexiones entrantes en un puerto determinado.
- Aceptar clientes.
- Recibir mensajes desde el cliente.
- Procesar dichos mensajes según la lógica del protocolo.
- Mantener el control del estado de autenticación y operación.
- Generar respuestas adecuadas.

Dentro del servidor, el procesamiento del protocolo suele estar separado en una clase o módulo específico, por ejemplo:

- `Servidor`: gestiona la conexión de red.
- `Procesador`: interpreta y procesa los mensajes del protocolo.
- `Usuario`: representa las credenciales válidas o la identidad del usuario.
- `Envio`: representa los datos de cada envío gestionado por el sistema.

### 2.2. Cliente

El cliente es el componente encargado de:

- Conectarse al servidor mediante socket.
- Enviar comandos al servidor.
- Mostrar por pantalla las respuestas recibidas.
- Permitir la interacción del usuario a través de consola o interfaz simple.

### 2.3. Flujo general de funcionamiento

El flujo básico de interacción es el siguiente:

1. El servidor se inicia y queda a la espera de conexiones.
2. El cliente se conecta al servidor.
3. El cliente envía sus credenciales.
4. El servidor valida el acceso.
5. Si la autenticación es correcta, el cliente puede enviar comandos del protocolo.
6. El servidor procesa cada comando y devuelve una respuesta.
7. La sesión puede finalizar mediante un comando de cierre, como `LOGOUT`.

### 2.4. Modelo de estados

El protocolo sigue una lógica de estados sencilla:

- **Estado 0: No autenticado**
  - Solo se permite el envío de credenciales válidas.
  - Cualquier otro mensaje se considera inválido o no autorizado.

- **Estado 1: Autenticado**
  - El cliente puede ejecutar comandos permitidos del sistema.
  - Ejemplos: crear envío, consultar envío, actualizar estado, `PING`, `LOGOUT`.

Este modelo evita que un cliente sin autenticar realice operaciones sobre los envíos.

---

## 3. Requisitos de ejecución

Para ejecutar correctamente el proyecto es necesario preparar la comunicación entre las dos máquinas que participan en la aplicación: una actuando como **servidor** y otra como **cliente**.

### Requisitos de red

- Las dos máquinas deben estar **conectadas a la misma red**.
- La máquina cliente debe poder comunicarse con la máquina servidor.
- Es necesario conocer la **dirección IP de la máquina servidor**, ya que será la que utilice el cliente para establecer la conexión.
- El **puerto de comunicación** configurado en el servidor debe estar disponible y coincidir con el usado por el cliente.
- En caso necesario, debe comprobarse que el **cortafuegos** no bloquea la conexión entre ambas máquinas.

### Requisitos de ejecución

- Tener el proyecto compilado correctamente.
- Ejecutar primero el servidor en la máquina que actuará como servidor.
- Ejecutar después el cliente en la otra máquina, configurándolo para que se conecte a la IP del servidor.

## 4. Ejemplo de uso

Supongamos que el servidor se ejecuta en una máquina cuya dirección IP es:
  192.168.1.50
y que el puerto configurado para la comunicación es:
  5000
En ese caso:
  - En la máquina servidor se lanza el servidor y este queda esperando conexiones.
  - En la máquina cliente se lanza el cliente, que deberá conectarse a la IP 192.168.1.50 usando el puerto 5000.

De este modo, la comunicación no se realiza en local, sino entre dos máquinas diferentes dentro de la misma red, que es el escenario previsto para la práctica.
