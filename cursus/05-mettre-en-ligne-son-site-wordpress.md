# Mettre en ligne son site Wordpress

Maintenant que le site Wordpress commence à prendre forme, nous allons nous pencher sur la question de la migration et de la mise en ligne de ce dernier.
Du choix de l'hébergeur jusqu'au passage en HTTPS en passant par l'exportation du contenu du site, tous les aspects de cette ultime étape seront abordés.

## Choisir un hébérgeur

Les hébergeurs sont légion et ultimement le choix d'une formule d'hébergement est souvent une histoire de préfèrences ou d'habitudes.
Cependant, pour qu'un site Wordpress puisse être mis en ligne, la formule d'hébergement que vous choisirez doit proposer :

- Une base de données MySQL
- Une installation de PHP
- 100 Mo d'espace stockage minmum
- Un accès FTP pour téléverser les fichiers sur le serveur

Chez la plupart des hébergeurs 'sérieux', on retrouve ces configuration dans des formules d'hébergement mutualisé assez abordables.
La formule Perso de OVH, par exemple, propose ces configurations pour 2,99 euros/mois avec une possibilité d'installer Wordpress en quelques clics via l'interface de gestion du service.

On retrouve ce genre de formules chez les hébergeurs suivant :
- [OVH](https://www.ovh.com/fr/web/)
- [Amen](https://www.amen.fr/hebergement-web/linux/#pgc-3768-packTable-0)
- [Hostinger](https://www.hostinger.fr/hebergement-wordpress)
- [LWS](https://www.lws.fr/hebergement_web.php)
- et bien d'autres 

## Migrer le site local avec le plugin Duplicator

Pour effectuer une migration du site Wordpress avec Duplicator, nous aurons besoin des outils suivant :
- le plugin wordpress [Duplicator](https://wordpress.org/plugins/duplicator/).
- un logiciel de transfert FTP tel que [Filezilla](https://filezilla-project.org/) ou [Cyberduck](https://cyberduck.io/).
- une formule d'hébergement avec une base de données installée (penser à bien conserver les adresses, id et mot de passe de la base de donnée). [Procédure pour OVH](https://docs.ovh.com/fr/hosting/creer-base-de-donnees/).

### Créer le paquet d'export sur le site local

Une fois le plugin installé, vous pouvez accéder au configurations dans l'onglet 'Duplicator' dans la barre latérale de l'interface d'administration.
C'est ici que vous allez pouvoir créer ce qu'on appelle le paquet, c'est-à-dire un ensemble de fichiers d'exports de votre site Wordpress, en cliquant sur 'Create New'.

La fenêtre de configuration offre de nombreuses options à étudier. Pour un export complet, on peut simplement cliquer sur suivant pour passer à l'étape de Scan.
Si le résultat du scan est positif, on clique sur Build pour lancer la création du paquet d'export. Si certains indicateurs sont rouges ou affichent des alertes,
un message d'information indique ce qui peut être fait pour améliorer la qualité de l'export.

Une fois le paquet generé, il faut télécharger deux fichiers : `installer.php` et l'archive zippé des contenus du site.

### Déposer le contenu du paquet sur le serveur en ligne

Connecter vous à votre serveur à l'aide du logiciel de transfert FTP. Lors de l'achat de votre solution d'hébergement, l'hébergement vous a transmis vos adresses, id et mot de passe pour
une connexion FTP, vous en aurez besoin lors de la connection à votre serveur.

D'avantages d'informations sur l'accès FTP avec OVH ici : https://www.ovh.com/fr/hebergement-web/ftp.xml

Une fois connecté au serveur, déposer le fichier `installer.php` ainsi que l'archive du site dans le dossier de votre choix. 
Pour une installation sur votre domaine principale (www.romaricfargetton.fr par exemple), déposer simplement les fichiers dans le dossier `www` de votre serveur.

### Lancer l'installation du paquet

Pour lancer l'installation du paquet, il faut simplememt accéder a l'url de votre site en ajoutant `installer.php` ensuite.
Dans notre exemple : `htps://www.romaricfargetton.fr/installer.php`. Une fois sur cet url, un processus d'installation devrait se lancer.

Il n'y a pas grand chose à faire sur le premier panneau 'Deploy' à part s'assurer que tout est en ordre.
Cliquer sur suivant pour accéder à la connexion de base de données. C'est là que vous devrez insérer les infos (adresse, id et mdp) obtenues lors de la création de la base de données.
Une fois la base de données connectée, le site va s'installer et vous pourrez retrouver votre site en ligne, identique à celui que vous aviez en local.

## Migrer le site local de façon manuelle

Si jamais la marche à suivre avec Duplicator n'aboutit pas au résultat escompté, il faut suivre les étapes suivantes :

1. Exporter la base de donnée locale et l'importer sur le site Wordpress en ligne (via l'interface d'administration MySql)
2. Modifier le fichier `wp-config.php` pour y insérer les infos (adresse, id et mdp) de la base de données de l'hébergeur.
1. Déplacer l'ensemble des fichiers du site local sur le serveur, ainsi que le fichier `wp-config` que vous avez modifié.
3. Mettre à jour les urls du nouveau site (l'ancienne adresse du site local est encore présente dans la base de données)

La démarche est détaillée dans les articles suivant:
- https://wpmarmite.com/migrer-wordpress-manuellement/
- https://wpformation.com/comment-migrer-wordpress-local-vers-hebergeur/

## Passer le site en https (connexion securisée)

1. Déménager votre site de HTTP vers HTTPS. Pour ce faire, il faut simplement se rendre dans l'interface d'administration et accéder à Réglages > Général et changer l'url du site en HTTPS.
2. Éffectuer une redirection en insérant le code suivant dans le fichier .htaccess à la base du dossier site :
```
RewriteEngine on
RewriteCond% {HTTP_HOST} ^ votresite.com [NC, OR]
RewriteCond% {HTTP_HOST} ^ www.votresite.com [NC]
(*). RewriteRule ^ $ https: //www.votresite.com/$1 [L, R = 301, NC]
```
3. Forcer le HTTPS pour les urls canoniques avec le plugin Yoast SEO, en accédant à SEO > Permaliens.

