## EX01
## PV
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
