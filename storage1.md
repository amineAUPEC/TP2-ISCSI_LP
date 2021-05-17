
# storage1 :
## install

- VM 2GB RAM, 64 bits, Freenas version, 2 VirtualDISK for VIRTUALBOX 
- boot from ISO
- install it
- remove ISO
- reboot
- wait for IP and all things
- GUI from the IP



## activer le service 
- from GUI :
- services
- start ISCSI


## exporter le disque 
```yml
- sharing -> block(ISCSI)  
- Portals -> add -> with default settings  
- Targets -> target: lun0 ; groupid: 1   
-  Extents -> ADD :  
    - extent_name: ext1
    - extent_type: device
    - device : ada1
-  Associated Targets -> ADD :  
    - target:lun0
    - lun_id:0
    - extent:ext1
```

# workstation1:

## install open-iscsi :
    sudo apt-get update -y  &&  sudo apt-get install -y open-iscsi  
## Découverte Iscsi
    sudo iscsiadm -m discovery -t sendtargets -p 192.168.0.13 
> output : \
> target : \ ((with :lun0))
**dddddd.xxxx.Xxx:lun0**

## Créer une session iSCSI avec la target $target présente sur le serveur $serveur : 

```bash
sudo iscsiadm -m node -l -T dddddd.xxxx.Xxx:lun0 -p $serveur
```


## Afficher les propriétés des sessions iSCSI établies :

    sudo iscsiadm -m session -o show && sudo cat /proc/net/iet/session

## afficher les propriétées bloc
    sudo cat /proc/partitions

## créer une table des partitions
    sudo fdisk /dev/sdb <<  
    < n  
    < p  
    < 2048  
    <  <<enter>>  
    < t  
    < 83  

## formater en ext4
    sudo mkfs.ext4 /dev/sdb1

## Formatez (en ext4) cette partition primaire

    sudo mkdir /mnt/lun0


## montez-la dans le répertoire 
    sudo mount /dev/sdb1 /mnt/lun0

## Vérifiez que vous pouvez créer et lire des fichiers dans ce dossier. 

    cd /mnt/lun0 && mkdir TP && touch A && ls


## démonter :

    cd /mnt && sudo umount /dev/sdb1 /mnt/lun0



## Fermer la session iSCSI établie avec la target $target :

    sudo iscsiadm -m node -u -T $target

## Fermer toutes les sessions iSCSI :

    sudo iscsiadm -m node -u


>> comme un disque dur virtuelle -- freenas --serveur NFS, NAS, ISCSI
>> TP  6.2 


## Copyright Bricecotte --Brice Augustin -- AmineAUPEC
<!-- #

#

#

# -->

<!-- > [!WARNING]
> Dtest
1. **d**
1. d
1. 
1. 


![display](../) -->


<!-- ## exporter le disque
- sharing -> block(ISCSI)  
- Portals -> add -> with default settings  
- Targets -> target: lun0 ; groupid: 1   
-  Extents -> ADD :  
    - extent_name: ext1
    - extent_type: device
    - device : ada1
-  Associated Targets -> ADD :  
    - target:lun0
    - lun_id:0
    - extent:ext1
 -->
