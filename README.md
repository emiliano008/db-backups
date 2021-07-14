# Backup MYSQL to DROPBOX

Este script comprime una a una las bases de mysql y manda a dropbox manteniendo 3 copias 

* en el script de backup tenemos que configurar la ruta, usuario y clave.

USERDIR="/home/s2wbackup"

MYUSER=usersololectura
MYPASS=clave

# Configuración

## USUARIO

* Creamos el grupo.  
    ```groupadd -g 499 backup```

* Creamos el usuario sin posibilidad de login.   
    ```useradd -d /home/backup -s /usr/sbin/nologin -c "Usuario para copias de seguridad" -u 499 -g 499 -G backup backup```

* Agregamos el usuario al grupo.  
    ```gpasswd -a backup backup```

* Creamos del directorio del usuario y los directorios base.    
    ```mkdir home/s2wbackup``` 
    ```mkdir carpetas home/s2wbackup/backup``` 
    ```mkdir carpetas home/s2wbackup/bin```

* Asignamos grupo y dueño
    ```chown backup.backup -R home/s2wbackup``` 


* Creamos un usuario de solo lectura MYSQL  
    ``` mysql -u root -p ```

   ``` mysql> create user 'backup'@'localhost' identified by 'clave';```
   ``` mysql> grant lock tables, select on *.* to 'backup'@'localhost';```

* Descargamos dropbox.                   
    ```curl "https://raw.githubusercontent.com/andreafabrizi/Dropbox-Uploader/master/dropbox_uploader.sh" -o dropbox_uploader.sh``` 

* Le damos permiso de ejecucion.                    
    ```chmod +x dropbox_uploader.sh```


## config cron para backup
SHELL=/bin/bash

PATH=/usr/bin:/bin:/home/s2wbackup/bin

\* 5 * * * backupdb > /tmp/backup.log


