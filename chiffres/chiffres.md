Nous allons essayer de résoudre informatiquement le problème des chiffres. Pour rappel, ou information, les consignes de ce jeu sont les suivantes :

- un nombre à 3 chiffres est tiré au hasard.
- six nombres de 1 à 3 chiffres sont tirés au hasard

**Objectif** : à l'aide des opérateurs mathématiques de base et en combinant les 6 nombres tirés, atteindre le premier nombre tiré.

**Règles**:

- On utilise seulement les 4 opérateurs de base : "+ - / *".
- Un nombre ne peut être utilisé qu'un seule fois
- La division n'est acceptée que si elle retourne un nombre entier
- La combinaison de deux nombres en donne un nouveau qui peut être utilisé pour combiner avec les nombres restants.

Exemple:

Nombre initial : 126 Nombres utilisables : 4 | 3 | 10 | 7 | 5 | 75

Une manière d'arriver au résultat est :

5 * 10 = 50

75 + 50 = 125

4 - 3 = 1

125 + 1 = 126

Pour se rendre compte de l'étendu du problème, essayons de calculer le nombre de combinaisons possibles avec ces nombres. On notera n le nombre d'éléments qu'on peut sélectionner (6 ici).

Pour commencer, on prend 2 nombres parmi les 6 initiaux, auquels ont applique un des 4 opérateurs. On a donc :

possibilités.

NB: A ce moment là, on peut remarquer qu'en principe il n'y a pas 4 mais 5 opérations possibles. En effet, le "+", le "*" sont commutatifs, mais le "/" ne l'est pas ( a/b est différent de b/a). Cependant, une des règles du jeu nous mène à la conclusion qu'on ne peut avoir que des nombres entiers. Soit les deux nombres choisis sont égaux, et auquel cas leur divison vaut 1 quel que soit l'ordre dans lequel elle est faite, soit il ne sont pas égaux, et dans ce cas l'un est plus grand que l'autre et donc on ne peut diviser que dans un sens (l'autre sens donne un nombre strictement inférieur à 1 donc non entier). De même, le "-" ne peut se faire que dans un sens (on n'autorise pas les nombres négatifs)

On a donc bien que 4 opérations possibles au maximum, et encore la division nous donnera souvent un nombre non entier.

Après cette première étape, on obtient donc n-1 nombres : on en avait n, on retire les deux qu'on a utilisé, et on ajoute le résultat de l'opération effectuée avec ces deux nombres. Si le résultat est le nombre recherché, on s'arrête, sinon on recommence l'opération, il y aura donc:

possibilités… etc jusqu'à ce qu'il ne reste plus que 2 nombres.

Ainsi, le nombre total de combinaisons possibles est égale à :

Pour n = 6 ici, on trouve un résultat de 2 764 800 possibilités, dans le pire des cas. Pour un ordinateur c'est largement faisable, on peut donc faire un algorithme bruteforce (on teste toutes les possiblités). Par contre si on commence à utiliser plus de nombres, mettons 8 au lieu de 6, ça commence à devenir plus compliqué : environ 36 milliards de possibilités, ce qui est tout de même beaucoup plus important.

Voici donc un code python permettant de résoudre le problème, en brute-forçant la chose.

``` python
import sys

#on prend la premiere solution pour le moment
def calculRec(nombres, aim, solutionCourante, solutions, solutionPlusProche):
    #on test si on a trouvé le bon nombre
    for nombre in nombres:
        if nombre == aim:
            solutions.append(solutionCourante[:])
            return

        #si jamais on arrive pas à la solution, on garde la solution la plus proche qu'on ai trouvé
        if abs(aim - nombre) < abs(aim - solutionPlusProche[0]):
            #on remplace l'ancienne valeur
            del solutionPlusProche[:]
            solutionPlusProche.append(nombre)
            solutionPlusProche.append(solutionCourante[:])

    #si on arrive à un seul nombre (et qu'il n'est pas aim), on termine
    if(len(nombres) == 1):
        return

    #pour tous les nombres disponibles, on liste toutes les façons d'en prendre 2
    for i in range(len(nombres)):
        if(nombres[i] > 0):
            for j in range(i):
                if(nombres[j] > 0):
                    nombre1 = nombres[i]
                    nombre2 = nombres[j]

                    #on invalide ces deux nombres de la liste en les mettant négatifs
                    nombres[i] = -nombres[i]
                    nombres[j] = -nombres[j]

                    #on lance la récurence pour les 4 opérations possibles
                    nombres.append(nombre1+nombre2)
                    solutionCourante.append(str(nombre1)+"+"+str(nombre2)+"="+str(nombre1+nombre2))
                    calculRec(nombres,aim,solutionCourante,solutions, solutionPlusProche)
                    solutionCourante.pop()
                    nombres.pop()

                    nombres.append(max(nombre1,nombre2)-min(nombre1,nombre2))
                    solutionCourante.append(str(max(nombre1,nombre2))+"-"+str(min(nombre1,nombre2))+"="+str(max(nombre1,nombre2)-min(nombre1,nombre2)))
                    calculRec(nombres,aim,solutionCourante,solutions, solutionPlusProche)
                    solutionCourante.pop()
                    nombres.pop()

                    nombres.append(nombre1*nombre2)
                    solutionCourante.append(str(nombre1)+"*"+str(nombre2)+"="+str(nombre1*nombre2))
                    calculRec(nombres,aim,solutionCourante,solutions,solutionPlusProche)
                    solutionCourante.pop()
                    nombres.pop()

                    #on ne fait le résultat que si la division est un entier
                    resultatDivision = max(nombre1,nombre2)/min(nombre1,nombre2)
                    if(isinstance(resultatDivision, int)):
                        nombres.append(resultatDivision)
                        solutionCourante.append(str(max(nombre1,nombre2))+"/"+str(min(nombre1,nombre2))+"="+str(resultatDivision))
                        calculRec(nombres,aim,solutionCourante,solutions,solutionPlusProche)
                        solutionCourante.pop()
                        nombres.pop()

                    #on réactive ces nombres maintenant qu'on a fini de travailler avec eux
                    nombres[i] = -nombres[i]
                    nombres[j] = -nombres[j]

try:
    aim = int(input("Nombre visé: "))
    nombres = []
    for i in range(6):
        nombres.append(int(input("Entrez un nombre: ")))
except ValueError:
    print("Merci d'entrer des uniquement des nombres entiers !")
    sys.exit()

#contient une paire (valeur, plusProcheSolutionCorrespondante)
solutionPlusProche = [0,[]]
solutions =[]

calculRec(nombres,aim,[],solutions,solutionPlusProche)

#on essaie d'afficher la premiere solution : si elle n'existe pas, on file la solution la plus proche
try:
    print("Solution exact :"+'\n'.join(solutions[0]))
except IndexError:
    print("Solution la plus proche trouvee:"+'\n'.join(solutionPlusProche[1]))

```

Voilà. Il y aurait plein d'optimistations potentielles, la première étant d'arréter le calcul si on trouve une solution.

Et pour vous montrer que ça marche, vous pouvez remplir les champs suivants et tester par vous même : j'ai réécrit la solution en javascript. Pourquoi? Parce que comme ca c'est votre ordinateur qui travaille pour faire les calculs plutôt que mon serveur. Si j'avais directement mis le code python en backend, des petits malins pourraient facilement faire un script envoyant des requêtes à la suite qui mettrait mon serveur en PLS.




