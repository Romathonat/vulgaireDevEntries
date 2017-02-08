En statistiques et en machine learning, on utilise souvent la notion de "faux positifs", "vrai positifs", rappel, précision ...
Nous allons expliquer ces concepts aujourd'hui.

## Contexte (classifieur binaire)
Admettons qu'on ait une application de machine learning, qui permette de [classifier les concombres](https://cloud.google.com/blog/big-data/2016/08/how-a-japanese-cucumber-farmer-is-using-deep-learning-and-tensorflow).
Dans un premier temps restons simple, gardons deux classes (classifieur binaire): les concombres et les non-concombres. Pour l'utilisateur, c'est très facile, il donne une photo au classifieur, et celle-ci lui-répond s'il s'agit d'un concombre ou pas. 
Bien.

## Des faux positifs ?

Notre application ne peut pas être parfaite, vous vous en doutez bien. Il y aura forcément des cas où elle détectera une image comme étant un concombre alors que ça n'en est pas un, ou l'inverse. C'est ici qu'interviennent ces notions de faux positifs, vrais négatifs, que voici:

- **Faux positifs**: les images de "non-concombre" détectées comme "concombres" par notre application (erreur) 
- **Vrai positifs**: les images de "concombre" détectées comme "concombres" (correct)
- **Faux négatifs**: les images de "concombre" detectées comme "non-concombres" (erreur)
- **Vrai négatifs**: les images de "non-concombre" détectées comme "non-concombres" (correct)

En vérité, retenir la proposition suivante vous assure de ne plus vous tromper: 

*En fait on se rend compte que l'utilisation du terme "positif" signifie que l'application a détecté l'image comme étant un concombre, et que la notion de vrai ou de faux indique si cette détection est juste ou pas.*

## Mesurer la qualité d'un classifieur

### La précision
La formule de la précision est simple :

$$\frac{Vrai positifs}{Vrai positifs + Faux positifs}$$

C'est le nombre de concombres réels détectés divisé par le nombre total de détections.

*La précision indique à quel point le classifieur est juste, càd le pourcentage de chance qu'il ne se trompe pas pour une classe donnée*

### Le rappel
La précision ne suffit pas à évaluer la qualité d'un classifieur, voici pourquoi. Imaginez que nous ayons 7 photos de concombres, et 3 photos de non-concombres. Admettons que notre classifieur détecte 2 concombres qui sont rééllement des concombres, et le reste en non-concombres. En reprenant la formule de la précision, on se retrouve avec 2 vrai positifs et 0 faux positifs, on à alors une précision de 100% pour la classe concombre, càd notre classifieur ne se trompe jamais quand il détecte un concombre. C'est vrai, mais en attendant il en a detecté très peu! C'est là qu'intervient le rappel:

$$\frac{Vrai positifs}{Vrai positifs + Faux négatifs}$$
C'est le nombre de concombre détectés qui sont de vrais concombres divisés pas le nombre total de concombre détectables.

*Le rappel indique à quel point le classifieur couvre les données, càd le pourcentage d'éléments qu'il détecte par rapport à l'ensemble d'éléments détectables pour une classe donnée*

### F-mesure
Avoir une seule mesure pour qualifier un classifieur est plus commode, c'est pourquoi on utilise souvent un indicateur appellé la F-mesure, dont voici la formule: 
$$F-mesure = 2*\frac{precision*rappel}{precision+rappel}$$



## Cas général (classifieur multiclasses)

Un classifieur multiclasses est un classifieur qui peut prédire la classe d'un élément parmi plus de deux classes. Dans notre cas ça pourrait être le fait de différencier des images de concombres en catégorisant de 1 à n, 1 étant un concombre de mauvaise qualité, et n un concombre de top qualité. Pour calculer la précision et le rappel, il faut calculer la précision et le rappel pour chacune des classes, et faire la moyenne :

$$precision = \sum\limits_{i=1}^n\frac{precision_i}{n}$$  
$$rappel = \sum\limits_{i=1}^n\frac{rappel_i}{n}$$



## Aller plus loin
(symetrie des tp/tn et fn/fp dans le cas du classifieur binaire, faire une belle courbe. Expliquer qu'on répartit dans 4 silots
les données. Mettre schéma wikipédia)
```
#0 -> concombre
#1 -> non-concombre

#en réalité 7 concombres, 3 non concombres
#l'outil nous donne 2 concombres (vrai), 8 non concombres

#detecte 2 concombres sur les 7, le reste est considere comme non-concombre
true_positive_0 = 2
true_negative_0 = 3 
false_negative_0 = 5
false_positive_0 = 0


true_positive_1 = 3
true_negative_1 = 2 
false_negative_1 = 0
false_positive_1 = 5

def recall(tp, tn, fn, fp):
  return tp/(tp+fn)
  
def precision(tp, tn, fn, fp):
  return tp/(tp+fp)

p_0 = precision(true_positive_0, true_negative_0, false_negative_0, false_positive_0)
r_0 = recall(true_positive_0, true_negative_0, false_negative_0, false_positive_0)

print('Pour la classe concombre, la precision est de {}\
  et le recall de {}'.format(p_0, r_0))
p_1 = precision(true_positive_1, true_negative_1, false_negative_1, false_positive_1)
r_1 = recall(true_positive_1, true_negative_1, false_negative_1, false_positive_1)

print('Pour la classe non-concombre, la precision est de {}\
  et le recall de {}'.format(p_1, r_1))

print('Precision moyenne: {}, Recall moyen: {}'.format((p_0+p_1)/2, (r_0+r_1)/2))
```
