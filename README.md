# Search-similar-images-by-content
Recherche par le contenu se basant sur la couleur et la forme (Base d'exemples de logos).


     
Outils 
	Streamlit : pour l’interface web de sélection de l’image d’entrée et la présentation des images similaires
	Spyder : éditeur de code
	Opencv : bibliothèque de traitement des images
	Python : Langage de développement 


Développement 
Le développement du code suit le workflow suivant :
	Prétraitement des images  
Cette partie consiste à rendre en niveau de gris puis à appliquer le floutage Gaussien pour réduire le bruit dans l’image.
 
Ensuite, la technique de seuillage automatique qui nous permet de transformer une image en binaire : ceci pour ne garder que les objets contenus dans une image.
 



 

	Enfin de prétraitement de l’image, nous appliquons la morphologie mathématique ( la fermeture d’image ).

Exemple d’application : 
 
   =========  


	Etude de la similarité :
_ Selon la forme 
Nous disposons de trois techniques pour réaliser cette tâche :
Les moments de Hu, ORB de opencv et la similarité structurale d’image (de la bibliothèque skimage).
Le moment de Hu donne de meilleurs résultats.
En premier, nous calculons les moments centraux basés sur la formule mathématique : 
 
Avec l’instruction python :
 
Pour améliorer les résultats, nous calculons ces moments sur le contour le plus large des contours détectés sur l’image.
Suite à cela, nous calculons les moments de Hu :
 
Avec nji :
 
Les moments de Hu (ou plutôt les invariants des moments de Hu) sont un ensemble de 7 nombres calculés à l'aide des moments centraux qui sont invariants par rapport aux transformations de l'image. Il a été prouvé que les 6 premiers moments sont invariants par rapport à la translation, à l'échelle, à la rotation et à la réflexion. Le signe du 7e moment change en cas de réflexion de l'image.



    __ Selon les couleurs (les 3 canaux) :
    Ce paramètre d’étude de similarité consiste à comparer les couleurs présentes dans les images.
Pour ce faire, nous traçons les l’histogramme des trois canaux de chaque image puis nous calculons l’intersection des courbes couleur par couleur.

 
La méthode de comparaison complète des images est donc :

 


	APPLICATION 
Commande d’exécution : 
  

L'application Streamlit utilise les fonctions précédentes pour créer une interface utilisateur :
- L'utilisateur peut télécharger une image via l'interface.
- Les caractéristiques de l'image téléchargée sont extraites à l'aide des fonctions définies précédemment.
- Les caractéristiques de l'image téléchargée sont comparées à celles de la base de données d'images.
- Les images les plus similaires sont affichées à l'utilisateur.



La fonction principale :
	Lecture des images de base de logo se trouvant dans le répertoire /bondataset
 

	 Interface demandant à l’utilisateur de sélectionner une image
 

 
 
	Initialisation
 
Après le choix de l’image, nous sauvegardons temporairement cette image, la lisons avec cv2.imread.
Après cela, nous appelons la fonction represent_features qui appliquera :
preprocess_image()  extract_hu_moments () pour enfin retourner les moments de Hu de l’image.

	Itérations sur les images du dossier /bondataset

 

Le code itère sur les images de la base de données, calcule la similarité entre l'image téléchargée et chaque image de la base de données, trie la liste des images similaires en fonction de la similarité, puis affiche les images les plus similaires.

 

 

Résultat :


 
===== 
 
 
CONCLUSION

Le code fourni propose une approche complète pour la recherche par contenu, en se basant à la fois sur la couleur et la forme, spécifiquement dans le contexte d'une base d'exemples de logos. L'utilisation de techniques telles que la segmentation basée sur la forme (moments de Hu) et la comparaison des canaux RGB offre une méthodologie robuste pour identifier des similarités entre les logos.

L'interface utilisateur Streamlit facilite grandement l'utilisation de l'application en permettant à l'utilisateur de télécharger une image de référence et d'explorer rapidement les logos similaires dans la base de données. Les résultats affichés sont pertinents pour la recherche d'images de logos similaires en se basant sur les caractéristiques visuelles.

Des améliorations futures pourraient être envisagées pour rendre l'application encore plus flexible, telles que la personnalisation des seuils de similarité, l'ajout de fonctionnalités interactives, ou l'optimisation des performances pour une base de données plus importante. Dans l'ensemble, le code fourni constitue une base solide pour une application de recherche par contenu adaptée aux logos.

 
Références :
https://www.youtube.com/watch?v=16s3Pi1InPU
https://www.kongakura.fr/article/OpenCV_Python_Tutoriel

