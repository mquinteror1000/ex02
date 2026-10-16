## EX01
## PV
Crear particion con
```bash
sudo cfsisk
```

```bash
sudo vgcreate martin /dev/vda4
sudo lvcreate -L 500M -n america martin
sudo lvcreate -L 500M -n europa martin
sudo lvcreate -L 500M -n asia martin
```
dar formato
```bash
sudo mkfs.ext4 /dev/martin/america
sudo mkfs.ext4 /dev/martin/europa
sudo mkfs.ext4 /dev/martin/asia
```
montar
```bash
sudo mkdir /america
sudo mkdir /asia
sudo mkdir /europa
```

```shellsession
[martin@localhost ~]$ sudo mount /dev/martin/america /america
[martin@localhost ~]$ sudo mount /dev/martin/asia /asia
[martin@localhost ~]$ sudo mount /dev/martin/europa /europa

```
en fstab
```bash
/dev/martin/america       /america        ext4    defaults    0 0
/dev/martin/asia    /asia    ext4    defaults    0 0
/dev/martin/europa    /europa        ext4    defaults    0 0
```
montar
```bash
sudo mount -a
```

## GRUPOS
```bash
sudo groupadd -g 5000 america
sudo groupadd -g 6000 europa
sudo groupadd -g 7000 asia
```
usuarios
```shellsession
[martin@localhost ~]$ sudo useradd -u 5001 -g america mexico
[martin@localhost ~]$ sudo useradd -u 5002 -g america brasil
[martin@localhost ~]$ sudo useradd -u 5003 -g america canada
[martin@localhost ~]$ sudo useradd -u 5004 -g america argentina
[martin@localhost ~]$ sudo useradd -u 6001 -g europa espana
[martin@localhost ~]$ sudo useradd -u 6002 -g europa francia
[martin@localhost ~]$ sudo useradd -u 6003 -g europa italia
[martin@localhost ~]$ sudo useradd -u 7001 -g asia japon
[martin@localhost ~]$ sudo useradd -u 7002 -g asia china
[martin@localhost ~]$ sudo useradd -u 7003 -g asia corea
```
atlantida
```shellsesssion
[martin@localhost ~]$ sudo groupadd -g 8000 atlantida
[martin@localhost ~]$ sudo useradd -u 8000 -g atlantida atlantida
useradd: warning: the home directory /home/atlantida already exists.
useradd: Not copying any file from skel directory into it.
Creando el fichero del buzón de correo: El fichero ya existe
[martin@localhost ~]$ 
[martin@localhost ~]$ id atlantida
uid=8000(atlantida) gid=8000(atlantida) grupos=8000(atlantida)

```
se queja pero si lo hace


Cambiar duaño a la carpeta raiz
```shellsession
[martin@localhost ~]$ sudo chown root:america /america
[martin@localhost ~]$ sudo chown root:asia /asia
[martin@localhost ~]$ sudo chown root:europa /europa
```
permisos especiales

```shellsession
[martin@localhost ~]$ sudo chmod 2770 /america
[martin@localhost ~]$ sudo chmod 2770 /asia
[martin@localhost ~]$ sudo chmod 2770 /europa

```
Permios para atlantida mediante ACL
```shellsession
[martin@localhost ~]$ sudo setfacl -m u:atlantida:rwx /america
[martin@localhost ~]$ sudo setfacl -m u:atlantida:rwx /asia
[martin@localhost ~]$ sudo setfacl -m u:atlantida:rwx /europa

```
ultimo check, para que herede atlantida los permisos cuando creen nuevos directorios
```shellsesison
[martin@localhost ~]$ sudo setfacl -d -m u:atlantida:rwx /america
[martin@localhost ~]$ sudo setfacl -d -m u:atlantida:rwx /asia
[martin@localhost ~]$ sudo setfacl -d -m u:atlantida:rwx /europa
```



## DNF
intalar epel-release
```bash
sudo dnf install epel-release
sudo dnf install htop
sudo dnf install tree
```
## PODAMAN
```bash
podman network create red-martin
podman volume create vol-martin

```

### Construir imagen apartir del containerfile
moverse a home y ahí crear un containerfile
```shellsession
[martin@localhost ~]$ cd --
[martin@localhost ~]$ vim Containerfile 
```
Colocar el contendido 
```bash
# Containerfile · Examen DevOps 2026 · Módulo 1
FROM docker.io/library/httpd:2.4
LABEL curso="devops2026" examen="devops2026-m1"
LABEL alumno="Martin Quintero"
RUN sed -i 's/^#ServerName .*/ServerName examen.local:80/' /usr/local/apache2/conf/httpd.conf
EXPOSE 80
```

### Construir la imagen basada en el containerfile
```shellsesion
[martin@localhost ~]$ podman build -t examen-web:1.0 .
## crear el container
podman run -d --name web-martin --network red-martin -v vol-martin:/usr/local/apache2/htdocs:Z -p 8090:80 examen-web:1.0
```

#### permitir acceso a puerto 8090 por tcp
```shellsession
sudo firewall-cmd --permanent --add-port=8090/tcp
```

probar
```shellsesion
[martin@localhost ~]$ curl http://localhost:8090
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<title>It works! Apache httpd</title>
</head>
<body>
<p>It works!</p>
</body>
</html>
```
si responde

Modificar el index en el contenedor para poner el nombre
```shellsession
[martin@localhost ~]$ podman volume ls
DRIVER      VOLUME NAME
local       vol-martin
[martin@localhost ~]$ podman volume inspect vol-martin
[
     {
          "Name": "vol-martin",
          "Driver": "local",
          "Mountpoint": "/home/martin/.local/share/containers/storage/volumes/vol-martin/_data",
          "CreatedAt": "2026-10-16T08:39:16.915886949-06:00",
          "Labels": {},
          "Scope": "local",
          "Options": {},
          "MountCount": 0,
          "LockNumber": 2
     }
]
[martin@localhost ~]$


```
Entondes modificar el index
con podman unshare para que no hay problemas de permisos
```shellsession
[martin@localhost _data]$ cd /home/martin/.local/share/containers/storage/volumes/vol-martin/_data
[martin@localhost _data]$ podman unshare
[root@localhost _data]# vim index.html 

```
deje asi el index.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<title>Martin Quintero Rubio</title>
</head>
<body>
<p>Martin Quintero Rubio</p>
</body>
</html>

```

