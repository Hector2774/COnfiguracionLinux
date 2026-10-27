## Configuración de ssh

Para configurar ssh en un servidor y una maquina seguí los siguientes pasos:

1. Instalar OpenSSH Server

   ```sh
   sudo apt update
   sudo apt install openssh-server -y
   ```

2. Verificar e inicar el servicio

   ```sh
   sudo systemctl enable ssh
   sudo systemctl start ssh
   sudo systemctl status ssh
   ```

Para conectarse al servidor desde la maquina se ejecuta así

```sh
ssh user@host
```

### SSH con claves

Un ssh con claves es mas seguro y recomendado. Para configurar un ssh con claves se siguen los siguientes pasos:

1. Generar el par de claves SSH

   ```sh
   ssh-keygen -t ed25519 -C "comentario" 
   ```

La opcion -t permite permite elegir un tipo de clave, se recomienda e algoritmo ed25519 porque es rápido, seguro y genera claves mas cortas. La opción permite escribir un comentario. De forma opcional, nos solicita introducir una contraseña para cifrar la clave, si decidimos hacerlo, tendremos que ingresar está contraseña en cada inicio con ssh que hagamos.

Esto generará 2 archivos en el directorio ~/.ssh/:

- id_ed25519 (Esta es la clave privada y debemos guardarla)

- id_ed25519.pub (Esta es la clave pública que debemos subir al servidor)

  
2. Copiar la clave pública en el servidor

   La forma más rápida de hacerlo es es utilizando la herramienta ssh-copy-id

   ```sh
   ssh-copy-id usuario@IP_DEL_SERVIDOR
   ```

Si por algún motivo ssh-copy-id no está disponible podemos hacerlo manualmente conel siguiente comando

```sh
cat ~/.ssh/id_ed25519.pub | ssh usuario@IP_DEL_SERVIDOR "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

Para acceder al servidor con la clave se ejecuta el comando así:

```sh
ssh -i ./.ssh/id_ed25519 user@host
```

#### Configurar un archivo ~/.ssh/config

Para no tener que escribir la IP, el usuario o la ruta de la clave, podemos crear un archivo config en nuestra máquina.

```sh
sudo nano /etc/ssh/sshd_config
```

Y añadimos un bloque así

```sh
Host mibox
    HostName 192.168.1.50
    User tu_usuario
    IdentityFile ~/.ssh/id_ed25519
```

Guardamos los cambios y le damos permisos restrictivos

```sh
chmod 600 ~/.ssh/config
```

Y con esto solo bastara con ejecutar shh con el nombre del host en .ssh/config

```sh
ssh mibox
```



### Endurecer la seguridad del servidor

Se pueden tomar diferentes medidas para mejorar la seguridad del servidor. Como desactivar por completo el acceso con contraseña.

1. Conectarse al servidor

2. Abrir el archivo de configuración de SSH (con sudo)

   ```sh
   sudo nano /etc/ssh/sshd_config
   ```

3. Buscar y modificar los siguientes parametros

   ```sh
   PubkeyAuthentication yes
   PasswordAuthentication no
   PermitRootLogin prohibit-password
   ```

4. Guardar el archivo

5. Verificar que la sintaxis sea correcta y reinicar el servicio ssh

   ```sh
   sudo sshd -t
   sudo systemctl restart ssh
   ```



### Configuración extra

#### Cambiar el puerto del servicio ssh

Para cambiar el puerto del servicio ssh debemos modificar la siguiente linea del archivo /etc/ssh/sshd_config y cambiarlo por el numero de puerto que deseamos

```sh
#Port 22
```

Cuando cambiamos el puerto, para conectarnos con ssh debemos especificar el numero de puerto donde corre el servicio

```sh
ssh -p puerto user@host
```

#### Copiar manualmente una clave de autenticación al servidor

Cuando el servidor tiene configurado ssh con claves, para poder conectarnos a él desde una nueva computadora debemos generar las claves y copiarlas directamente al servidor desde la maquina con la cual tenemos acceso

1. Desde la nueva máquina generamos el par de claves

   ```sh
   ssh-keygen -t ed25519 -C "comentario"
   ```

2. Mostramos el contenido de la llav publica y lo copiamos

   ```sh
   cat ~/.ssh/id_ed25519.pub
   ```

3. Nos conectamos al servidor desde la máquina que tiene acceso

4. Abrimos el archivo de claves autorizadas con cualquier editor de texto y pegamos la clave pública

   ```sh
   nano ~/.ssh/authorized_keys
   ```

5. Guardamos
