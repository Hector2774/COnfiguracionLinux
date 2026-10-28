## Configuración de UFW

UFW sirve para bloquear el tráfico de red que no queremos y dejar pasar el que sí necesitas. 

Para la configuración de UFW en el servidor debian seguí los siguientes pasos:

1. Instalación

   Nos conectamos el servidor y ejecutamos

   ```sh
   sudo apt install ufw
   ```

2. Cerrar todo por defecto

   Ejecutamos los siguientes comandos:

   ```sh
   sudo default deny incoming
   sudo default allow outgoing
   ```

 

- **`default deny incoming`**: Significa *"Nadie entra a menos que yo le dé permiso explícito con nombre y apellido"*. (Cierras todas las puertas con llave).

- **`default allow outgoing`**: Significa *"Tus programas dentro del servidor pueden salir a internet a comprar cosas (descargar actualizaciones, conectar a APIs), sin ataduras"*.
3. Permitir ssh

   Si activamos el firewall antes de permitir ssh, UFW va a bloquear ssh y nos desconectará del servidor

   ```sh
   sudo ufw allow ssh
   ```

4. Abrir los servicios que el mundo si debe ver

   ejemplo

   ```sh
   sudo ufw allow 80/tcp
   ```

5. Activar UFW

   ```sh
   sudo ufw enable
   ```

6. Comprobar el estado y las reglas

   ```sh
   sudo ufw status verbose
   sudo ufw status numbered
   ```
