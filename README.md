# Search-similar-images-by-content
Recherche par le contenu se basant sur la couleur et la forme (Base d'exemples de logos).


     
## Outils 

 - Streamlit : pour l’interface web de sélection de l’image d’entrée et la présentation des images similaires

 - Spyder : éditeur de code

 - Opencv : bibliothèque de traitement des images

 - Python : Langage de développement
   
![Capture d'écran 2023-12-18 112751](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/f2a133c3-1b6f-47aa-b7fd-ae2bf73b0314)


## Développement 

Le développement du code suit le workflow suivant :

	Prétraitement des images  

Cette partie consiste à rendre en niveau de gris puis à appliquer le floutage Gaussien pour réduire le bruit dans l’image.

 ![Capture d'écran 2023-12-17 181853](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/dad58e28-8664-4a33-96c2-7ed3220c7b32)

Ensuite, la technique de seuillage automatique qui nous permet de transformer une image en binaire : ceci pour ne garder que les objets contenus dans une image.
 
![Capture d'écran 2023-12-17 182532](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/c35f89ab-b390-46c1-8bc5-864b9db5141c)

![Capture d'écran 2023-12-17 182553](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/0716146e-2e0b-4ef8-83a2-0dc41d928320)



 

	Enfin de prétraitement de l’image, nous appliquons la morphologie mathématique ( la fermeture d’image ).

Exemple d’application : 
 
   ![Capture d'écran 2023-12-18 113249](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/6443f8f6-8ae2-4a00-8e5f-72ab0a9ddb20)



## Etude de la similarité :

- Selon la forme 

Nous disposons de trois techniques pour réaliser cette tâche :
Les moments de Hu, ORB de opencv et la similarité structurale d’image (de la bibliothèque skimage).
Le moment de Hu donne de meilleurs résultats.
En premier, nous calculons les moments centraux basés sur la formule mathématique : 

 ![Capture d'écran 2023-12-17 192445](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/7a5d5ef1-54a2-441d-8322-71c4e667b7c9)


Avec l’instruction python :

 ![Capture d'écran 2023-12-17 192631](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/dcc84edd-b820-4b00-95fe-6d44643890c5)

Pour améliorer les résultats, nous calculons ces moments sur le contour le plus large des contours détectés sur l’image.
Suite à cela, nous calculons les moments de Hu :

 ![Capture d'écran 2023-12-17 192905](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/00b00d04-6f99-4330-86d0-4bda86e84a4f)

Avec nji :

 ![Capture d'écran 2023-12-17 192923](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/e8fdbca9-f7fa-407f-9454-582b56d0346f)

Les moments de Hu (ou plutôt les invariants des moments de Hu) sont un ensemble de 7 nombres calculés à l'aide des moments centraux qui sont invariants par rapport aux transformations de l'image. Il a été prouvé que les 6 premiers moments sont invariants par rapport à la translation, à l'échelle, à la rotation et à la réflexion. Le signe du 7e moment change en cas de réflexion de l'image.

- Selon les couleurs (les 3 canaux) :
    
    Ce paramètre d’étude de similarité consiste à comparer les couleurs présentes dans les images.
Pour ce faire, nous traçons les l’histogramme des trois canaux de chaque image puis nous calculons l’intersection des courbes couleur par couleur.

 
La méthode de comparaison complète des images est donc :

 ![Capture d'écran 2023-12-17 194130](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/7a37ec06-039f-4aff-8e42-684b20a9a0a5)



## APPLICATION 

Commande d’exécution : 
  
## streamlit run SearchApp.py


L'application Streamlit utilise les fonctions précédentes pour créer une interface utilisateur :

- L'utilisateur peut télécharger une image via l'interface.

- Les caractéristiques de l'image téléchargée sont extraites à l'aide des fonctions définies précédemment.
  
- Les caractéristiques de l'image téléchargée sont comparées à celles de la base de données d'images.
  
- Les images les plus similaires sont affichées à l'utilisateur.



La fonction principale :

- Lecture des images de base de logo se trouvant dans le répertoire /bondataset
 
![Capture d'écran 2023-12-04 225653](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/bfc72f29-d1ab-4330-a3d3-684e4af4538f)


-	 Interface demandant à l’utilisateur de sélectionner une image
 
![Capture d'écran 2023-12-18 114219](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/60c1a16b-4a32-4a9f-ad87-c77350ab9e71)
 
 
- Initialisation

 
Après le choix de l’image, nous sauvegardons temporairement cette image, la lisons avec cv2.imread.
Après cela, nous appelons la fonction represent_features qui appliquera :
preprocess_image()  extract_hu_moments () pour enfin retourner les moments de Hu de l’image.

- Itérations sur les images du dossier /bondataset

   ![Capture d'écran 2023-12-17 194525](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/a992188d-f4f4-49ba-818a-aec05e3782dd)


Le code itère sur les images de la base de données, calcule la similarité entre l'image téléchargée et chaque image de la base de données, trie la liste des images similaires en fonction de la similarité, puis affiche les images les plus similaires.

 
![Capture d'écran 2023-12-17 194611](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/30f60146-da00-4708-b106-4b3c31a55db3)

 

Résultat :

![Capture d'écran 2023-12-17 194611](https://github.com/kpatoukpakpodjro/Search-similar-images-by-content/assets/102617343/30f60146-da00-4708-b106-4b3c31a55db3)

 
 
 
## CONCLUSION

Le code fourni propose une approche complète pour la recherche par contenu, en se basant à la fois sur la couleur et la forme, spécifiquement dans le contexte d'une base d'exemples de logos. L'utilisation de techniques telles que la segmentation basée sur la forme (moments de Hu) et la comparaison des canaux RGB offre une méthodologie robuste pour identifier des similarités entre les logos.

L'interface utilisateur Streamlit facilite grandement l'utilisation de l'application en permettant à l'utilisateur de télécharger une image de référence et d'explorer rapidement les logos similaires dans la base de données. Les résultats affichés sont pertinents pour la recherche d'images de logos similaires en se basant sur les caractéristiques visuelles.

Des améliorations futures pourraient être envisagées pour rendre l'application encore plus flexible, telles que la personnalisation des seuils de similarité, l'ajout de fonctionnalités interactives, ou l'optimisation des performances pour une base de données plus importante. Dans l'ensemble, le code fourni constitue une base solide pour une application de recherche par contenu adaptée aux logos.

 
Références :
https://www.youtube.com/watch?v=16s3Pi1InPU
https://www.kongakura.fr/article/OpenCV_Python_Tutoriel

