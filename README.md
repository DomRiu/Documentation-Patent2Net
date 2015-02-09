# Documentation-Patent2Net
installation et fonctionnement de Patent2Net

Patent2Net

**********           1.	Création d’un compte sur le site d’ESPACENET       ******************

L’URL pour s’enregistrer sur ESPACNET est :    https://developers.epo.org/user/register

Il faut s’enregistrer comme self employed et indiquer academia.
un numéro est généré, c’est celui qui permettra de se logger
(attention Il est possible que l’inscription soit refusée dans un premier temps et que l’EPO demande des précisions supplémentaires quant à l’utilisation qui sera faite des données extraites- 
Répondre en anglais et ne pas hésiter à communiquer en plus son adresse mail de l’université et une copie de sa carte étudiant).
Lorsque l’incription est acceptée on reçoit un mail indiquant:
Your registration number xxxxx  has been accepted by the European Patent Office as a Non-paying user.
Login to the site using https://developers.epo.org/user/login
Please register your application(s) here https://developers.epo.org/user/xxxxx/apps.
Sur cette page on va ajouter une nouvelle application qu’on appellera Patent2Net
cela permettra en cliquant dessus le non de l’application Patent2Net d’arriver à une page
https://developers.epo.org/user/xxxxx/app-detail/Patent2Net
sur laquelle on aura les 2 clefs suivantes

Clefs
Ce sont les clefs de votre domaine d’application.
Consumer Key: XXXXXVVXCntdtzndghhrkrop
Consumer Secret Key: 325TFRghppetdk
qui seront à recopier dans un fichier txt créé avec Notepad++ sous le format suivant
XXXXXVVXCntdtzndghhrkrop,325TFRghppetdk 
(une virgule entre les 2 clefs, aucun espace après la virgule ou après la saisie de caractères).
le fichier créé sera sauvegardé sous le nom « clefs-epo.txt » 


Par la suite l’accès peut se faire à l’adresse suivante 
http://www.epo.org/searching/free/ops_fr.html


*****************      2.	Installation de Patent2Net    ***************************

Dans Github, on va ensuite aller chercher le programme Patent2Net pour l’installer sur son ordinateur

https://github.com/patent2net/Patent2Net

Le fichier compile.bat génèrera une version compilée de Patent2Net dans le répertoire distWE32.

Après l’installation de la version compilée de Patent2Net, 
3 opérations sont à réaliser :

1-	L’installation de votre fichier clefs d’identification « clefs-epo.txt » dans ce répertoire distWE32

2-	La modification du fichier « requete.cql » en fonction de la requête que vous souhaitez faire dans la base des brevets (cf. ci après)

3-	Le lancement du fichier « CollecteEttraite.bat » qui exécute directement certains programmes de Patent2Net et génère le répertoire de stockage DONNEES où les informations recueillies et retraitées seront stockées dans les répertoires suivants.
-	« GephifilesV5 » (stockage des fichiers GEPHI créés –thème, inventeurs, déposants, pays…)
-	« Patentbiblios » (requête et familles de la requête)
-	« Patentcontents » (répertoire par requête ventilé en sous répertoires résumé, revendications, contenu) 
-	« PatentscontentsHTML » (fichiers générés sur la requête par les différents programmes de P2N en format cvs, json et html)
-	« Patentlists » (liste des brevets de la requête effectuée)
-	« tempo » (temps de réalisation de la requête)

*********
requete.cql
	
requete.cql est le fichier dans lequel vous devez renseigner la requête que vous souhaitez faire sur la base de données des brevets
i.e. par exemple “foie gras”, “banana peel”….
La requête peut être faite aussi en utilisant des opérateurs booléens AND, OR, NOT 
i.e. par exemple « foie gras AND France »

********* 	
CollecteEttraite.ba

CollecteEttraite.bat lance l’exécution de différents sous-programmes de Patent2Net afin de :

1. Collecter les brevets du thème de la requête dans la base EPO
2. En extraire et retraiter certaines données 
(famille de brevets, analyse lexicale, inventeurs, déposants, liens entre les déposants)

Ces données retraitées seront ensuite utilisées dans Gephi pour réaliser une visualisation des réseaux d’inventeurs et de déposants et pouvoir en interpréter la signification des nœuds les plus importants.


*********  Liste des différents programmes contenus dans Patent2Net et résumé de ce qui est réalisé avec chacun d’entre eux  *******

OPSGatherPatentsv2.py

	Ce script 
1-	charge la requête du fichier “requete.cql »
2-	construit la liste des brevets correspondant à cette requête
3-	les sauvegarde dans le répertoire../DONNEES/PatentLists
Ensuite les données bibliographiques associées à chaque brevet dans la liste des brevets sont collectées et stockées dans un fichier dans directory ../DONNEES/PatentBiblio, dont le nom correspond à la requête effectuée.
i.e. par exemple “foie gras”, “banana peel”….

--------------------

OPSGather-BiblioPatent.py
	
(non trouvé dans le répertoire development)

--------------------
 
OPSGatherContentsv1.py

Après avoir chargé la liste des brevets (créée depuis OPSGather-BiblioPatent ) ce script collecte les éléments suivants:
-	numéro du brevet
-	pays du depot du brevet
-	revendications
-	description
-	texte complet du brevet

--------------------- 
	
OPSGatherContentsv1-Iramuteq.py

Ce script réalise une analyse lexicale IRAMUTEQ du texte du brevet

---------------------
 
OPSGatherAugment-Families.py
	
Après avoir chargé la liste de brevets créée par OPSGather-BiblioPatent, ce script vérifie pour chaque brevet s’il est orphelin ou s’il appartient à une famille de brevets.
Dans le 2ème cas, le brevet est ajouté à la liste de la famille et une hiérarchie est créée entre le plus ancien et les autres brevets apparentés.

----------------------

OPS3.py

Ce script traite toutes les informations contenues dans les brevets récupérés, en ventile tous les éléments, 
nettoie toutes les données texte et les stocke de façon à en rendre le traitement le plus propre 
et le plus facile possible par les autres programmes qui iront récupérer les données stockées dans les différentes biblios.

 ----------------------
 
OPS2NetUtils2.py
Ce script réalise les appariements des brevets, leur pondération en fonction de leur citation et génère les réseaux. 
Il Crée les nœuds corrects pour l’exploitation ultérieure des données dans Gephi

 -----------------------
 
 P2N-Applicants.py
 
Ce script génère le réseau des déposants de brevets
 
--------------------

P2N-ApplicantsCrossTech.py

     A complèter

--------------------- 

P2N-Authors.py

Ce script génère le réseau des inventeurs des brevets
 
--------------------

P2N-InventorCrossTech.py

     A complèter

--------------------- 

P2N-AuthorsApplicants.py

     A complèter

--------------------- 

P2N-CountryCrossTech.py

     A complèter

--------------------- 
 
 P2N-families.py

     A complèter

--------------------- 
 P2N-familiesHierarc.py

     A complèter

--------------------- 
 P2N-V5.py

     A complèter

--------------------- 
 
 FormateExport.py
 
  A complèter

--------------------- 

 FormateExportFamilies.py
 
  A complèter

--------------------- 

 Fusion.py
 
  A complèter

--------------------- 

 FusionIramuteq.py
 
  A complèter

--------------------- 

 

 

 

 

 

 





