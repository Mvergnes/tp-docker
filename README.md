### TP Docker 17/10/2017 ###
                           
*Avant propos:
Les etapes 1 à 3 correspondent a la creation des images
Les etapes 4 à 6 correspondent a la mise en place des containers et au test final
Etape 7 correspond a comment push sur docker hub



__Etape 1:__
Creation de deux sous-dossier (sur la machine hote)  
-> reptom  
-> reppos

__Etape 2:__
telechargement des fichiers de l'appli via git clone


__Etape 3:__
Création de deux Dockerfile (afin de créer nos deux images pour nos conteneurs) à l'aide de la commande: vim Dockerfile (dans le repertoire souhaité)

	reptom/Dockerfile
		FROM tomcat:8-jre8						/* Récupération image sur le hub la premiere fois (image de base pour fabriquer la notre)*/
		COPY ./dbproject.war /usr/local/tomcat/webapps

		
	reppos/Dockerfile
		FROM postgres:9.5						/* Récupération image sur le hub la premiere fois (image de base pour fabriquer la notre)*/
		COPY ./init-db.sql docker-entrypoint-initdb.d/			/* copie du fichier init-db.sql dans le repertoire du container */
		VOLUME ["/var/lib/postgressql/data"]				/* Persistence des données de la base de données (on pourra le verifier a l'etape 6: test)*/


__Etape 4:__
Cette étape vas permettre de créer nos deux nouvelles images qui serviront pour nos constructeur, a partir des dockerfile créer précedement
a l'aide de la commande docker build

$ docker build -t postgres1710 .  
/* -t indique un tag , l'image aura pour nom postgres1710 */

$ docker build -t tomcat1710 .  
/* -t indique un tag , l'image aura pour nom tomcat1710 */


__Etape 5:__
Construction des conteneurs a partir des deux nouvelles images que l'ont vient de creer
a l'aide de la commande docker run

$ docker run -d --name db postgres1710  				
/* creation du conteneur nommé db a l'aide de l'image postgres1710

$ docker run -d --name cnttomcat17102 -p 8888:8080 --link db tomcat1710  
/* on créer le conteneur cnttomcat17102, ont indique le port 8888:8080 et on lie avec la base de donnée du conteneur postgres a l'aide de --link db (db = nom du contructeur de posgres)*/
                    

__Etape 6:__ test  
Pour vérifier aller sur l'adresse indiquer par docker (docker tools), http://192.168.99.100:8888/dbproject/accueil.jsp
afin de tester le bon fonctionnement de l'appli, entré un article en stock et verifier ca presence.
afin de verifier la persistence de la bdd, faites un 
$docker stop cnttomcat17102 
$docker stop db 
puis 
$docker start cnttomcat17102
$docker start db 
et retourner verifier la présence des informations de la base de données dans l'appli (stock de chaussures entré precedement)



__Etape 7:__ Upload image sur docker hub

$ docker tag postgres1710:latest mvergnes/postgres1710:1.0  		
/* Modification du Tag en vue de l'upload sur docker hub */

$ docker tag tomcat1710:latest mvergnes/tomcat1710:1.0  			
/* Modification du Tag en vue de l'upload sur docker hub */

$ docker push mvergnes/tomcat1710:1.0  
/* push de l'image sur dockerhub */

$ docker push mvergnes/tomcat1710:1.0  
/* push de l'image sur dockerhub */



