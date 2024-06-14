Commandes afin de partionner les disques de 6 GB et 4 GB. ( fdisk )

![Checkpoints1_ex1_1](https://github.com/Blazeuhh/Quetes_WCS/blob/main/images/Checkpoints1_ex1_1.png?raw=true)

Mkfs help afin de savoir quel commandes utilisé pour mettre en swap et en ext4

![Checkpoints1_ex1_2](https://github.com/Blazeuhh/Quetes_WCS/assets/156552845/88db1dad-ef80-4d3d-9f72-7a31a46ae76d)

`mkfs.ext4 -L DATA /dev/sdb1` et sudo `mkswap -L SWAP /dev/sdb2` pour formater en ext4 et SWAP

![Checkpoints1_ex1_3](https://github.com/Blazeuhh/Quetes_WCS/assets/156552845/03c4e41b-c491-4179-9424-ae3ce1819fc3)

`swapoff /dev/sda2` et `sudo swapon /dev/sda2` pour configurer le swap, `lsblk` pour vérifier si c'est en SWAP et en EXT4

![Checkpoints1_ex1_4](https://github.com/Blazeuhh/Quetes_WCS/assets/156552845/8c027c09-5f99-434c-9d5d-a04fa4a908de)

Créer le point de montage :

`mkdir -p /mnt/data` pour créer un dossier où la partition sera monté
`blkid /dev/sdb1` pour trouver l'UUID de la partition DATA
Copier et coller l'UUID dans `etc/fstab`
`UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /mnt/data ext4 defaults 0 2`

![Checkpoints1_ex1_5](https://github.com/Blazeuhh/Quetes_WCS/assets/156552845/394486bb-ec6c-4b3f-8da6-1193f5cc9fde)
Puis faire mount -a pour monter la partition
Vérification du montage avec df -h /mnt/data

![Checkpoints1_ex1_6](https://github.com/Blazeuhh/Quetes_WCS/assets/156552845/0135959c-2865-4df5-a1ad-5b236ca1c694)
