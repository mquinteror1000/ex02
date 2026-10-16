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

