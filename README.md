# Bajar la imagen desde el repositorio oficial de Oracle

Para habilitar un contendor con la base de datos de oracle tenemos que descargar la imagen desde su repositorio principal. 

Esta imagen no está en Dockerhub, sin embargo, podemos hacer la integración con Docker de manera transparente, como cualquier otra imagen.

Para poder hacer uso de nuestro contenedor podemos crear nuestra imagen de esta manera:

```
sudo docker run --name oracle-db-1 \
-p 1521:1521 -p 5500:5500 \
-e ORACLE_PWD='password' \
-e ORACLE_CHARACTERSET=utf8 \
container-registry.oracle.com/database/express:21.3.0-xe
```

- Donde el puerto 1521 es el puerto por defecto de la base de datos de oracle y el puerto 5500 es el puerto por defecto de la interfaz web.
- La configuración para la contraseña y la codificación de caracteres se hace con las variables de entorno ORACLE_PWD y ORACLE_CHARACTERSET respectivamente.
- La imagen que estamos utilizando es la versión express de la base de datos de oracle.


# Crear una base de datos

A continuación se presentará la configuración de la base de datos de oracle, para ello, se debe de tener en cuenta que se debe de tener instalado el cliente de oracle en el equipo local.

Una vez que nos conectamos a la base de datos por el puerto especificado, procedemos a crear una base de datos con el siguiente script:

```
CREATE TABLESPACE SSIOC DATAFILE '/opt/oracle/oradata/SSIOC.DBF' SIZE 2G;

alter session set "_ORACLE_SCRIPT"=true;
CREATE USER SSIOC IDENTIFIED BY "redine" DEFAULT TABLESPACE "SSIOC" PROFILE DEFAULT QUOTA UNLIMITED ON "SSIOC";

GRANT
    ALL
    PRIVILEGES
    TO
    SSIOC;

CREATE OR REPLACE DIRECTORY DUMP AS '/backup/dumps';
GRANT READ, WRITE ON DIRECTORY DUMP TO SSIOC;
```
