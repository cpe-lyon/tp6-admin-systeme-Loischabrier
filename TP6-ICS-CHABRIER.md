Loïs CHABRIER

# _TP 6 - Gestion des disques / Tâches d’administration_

## Exercice 1. Disques et partitions

<span style='color:red'>1.</span> Dans l’interface de configuration de votre VM, créez un second disque dur, de 5 Go dynamiquement
alloués ; puis démarrez la VM.


<span style='color:red'>2.</span> Vérifiez que ce nouveau disque dur est bien détecté par le système

`fdisk -l` pour vérifier : le nouveau disque est bien détecté.

<span style='color:red'>3.</span> Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83),
et une seconde partition de 3 Go en NTFS (n°7)

Pour créer la partition de type Linux de 2Go : `sudo fdisk /dev/sdb/` puis entrer "n" puis entrer jusqu'à "Last sector" et entrer "+2G"
Pour créer la partition de type NTFS de 3Go : `sudo fdisk /dev/sdb/` puis entrer "n" puis entrer jusqu'à la fin

Comme on peut le voir, on a une partition de type Linux de 3Go qui est créée. Pour la reformater en NTFS, entrer `sudo fdisk /dev/sdb/` puis "t", puis "2", puis "7" dans Hex Code.
Entrer ensuite "w" pour enregistrer les modifications.

<span style='color:red'>4.</span> A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers. A l’aide de la commande mkfs, formatez vos deux partitions (pensez à consulter le manuel !)

Pour la partition de type Linux : `sudo mkfs.ext4 /dev/sdb1/`
Pour la partition de type NTFS : `sudo mkfs.ntfs /dev/sdb2/`

<span style='color:red'>5.</span> Pourquoi la commande df -T, qui affiche le type de système de fichier des partitions, ne fonctionne-t-
elle pas sur notre disque ?

Elle ne fonctionne pas parce que nos partitions ne sont pas montées.

<span style='color:red'>6.</span> Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la
machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des
UUID en raison de l’impossibilité d’effectuer des copier-coller)

Exécuter la commande `nano /etc/fstab/`puis ajouter ces lignes : 

    /dev/sdb1      /data      ext4     defaults     0       0
    /dev/sdb2      /win       ext4     defaults     0       0

<span style='color:red'>7.</span> Utilisez la commande mount puis redémarrez votre VM pour valider la configuration

Après redémarrage, exécuter la commande `df -T`pour vérifier que les 2 partitions apparaissent.

<span style='color:red'>8.</span> Montez votre clé USB dans la VM


<span style='color:red'>9.</span> Créez un dossier partagé entre votre VM et votre système hôte (rem. il peut être nécessaire d’installer les Additions invité de VirtualBox)


## Exercice 2. Partitionnement LVM

<span style='color:red'>1.</span> On va réutiliser le disque de 5 Gio de l’exercice précédent. Commencez par démonter les systèmes de
fichiers montés dans /data et /win s’ils sont encore montés, et supprimez les lignes correspondantes
du fichier /etc/fstab


<span style='color:red'>2.</span> Supprimez les deux partitions du disque, et créez une patition unique de type LVM


<span style='color:red'>3.</span> A l’aide de la commande pvcreate, créez un volume physique LVM. Validez qu’il est bien créé, en
utilisant la commande pvdisplay.


<span style='color:red'>4.</span> A l’aide de la commande vgcreate, créez un groupe de volumes, qui pour l’instant ne contiendra que
le volume physique créé à l’étape précédente. Vérifiez à l’aide de la commande vgdisplay.


<span style='color:red'>5.</span> Créez un volume logique appelé lvData occupant l’intégralité de l’espace disque disponible.


<span style='color:red'>6.</span> Dans ce volume logique, créez une partition que vous formaterez en ext4, puis procédez comme dans
l’exercice 1 pour qu’elle soit montée automatiquement, au démarrage de la machine, dans /data.


<span style='color:red'>7.</span> Eteignez la VM pour ajouter un second disque (peu importe la taille pour cet exercice). Redémarrez
la VM, vérifiez que le disque est bien présent. Puis, répétez les questions 2 et 3 sur ce nouveau disque.


<span style='color:red'>8.</span> Utilisez la commande vgextend <nom_vg> <nom_pv> pour ajouter le nouveau disque au groupe de
volumes

<span style='color:red'>9.</span> Utilisez la commande lvresize (ou lvextend) pour agrandir le volume logique. Enfin, il ne faut pas
oublier de redimensionner le système de fichiers à l’aide de la commande resize2fs.

## Exercice 3. Exécution de commandes en différé : at et cron

<span style='color:red'>1.</span> Programmez une tâche qui affiche un rappel pour la réunion qui aura lieu dans 3 minutes. Vérifiez
entre temps que la tâche est bien programmée.


<span style='color:red'>2.</span> Est-ce que le message s’est affiché ? Si la réponse est non, essayez de trouver la cause du problème (par exemple en vous aidant des logs, du manuel...)


<span style='color:red'>3.</span> Pour tester le fonctionnement de cron, commencez par programmer l’exécution d’une tâche simple,
l’affichage de “Il faut réviser pour l’examen !”, toutes les 3 minutes.


<span style='color:red'>4.</span> Programmez l’exécution d’une commande tous les jours, toute l’année, tous les quarts d’heure


<span style='color:red'>5.</span> Programmez l’exécution d’une commande toutes les cinq minutes à partir de 2 (2, 7, 12, etc.) à 18
heures les 1er et 15 du mois :


<span style='color:red'>6.</span> Programmez l’exécution d’une commande du lundi au vendredi à 17 heures


<span style='color:red'>7.</span> Modifiez votre crontab pour que les messages ne soient plus envoyés par mail, mais redirigés dans un
fichier de log situé dans votre dossier personnel


<span style='color:red'>8.</span> Videz votre crontab
