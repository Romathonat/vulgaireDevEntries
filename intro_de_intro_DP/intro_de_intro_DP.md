## Intro

J'ai fait un article sobrement nommé [introduction à la DP](http://vulgairedev.fr/blog/article/intro_dp). On m'a très justement fait
remarquer que l'exemple du sac à dos est trop compliqué pour bien comprendre la programmation dynamique si on en a jamais fait.
Qu'à cela ne tienne, voici l'intro de l'intro à la DP !

## Suite de Fibonnaci
Un problème plus simple pour illustrer la programmation dynamique est celui de la suite de Fibonacci.

![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/intro_de_intro_DP/Fibonacci2.jpg)  
*[Leonardo Fibonacci](https://fr.wikipedia.org/wiki/Leonardo_Fibonacci), un BG du croix baton baton baton siècle*

La suite de Fibonnaci est définie comme suit:  
$$Fibo(n) = Fibo(n-1) + Fibo(n-2)$$

A la base, elle est utilisée pour modéliser l'évolution d'une population de lapins, avec les règles suivantes:

- Un couple de lapins donne naissance à un autres couple de lapins
- Un couple de lapins qui vient de naître doit attendre un an avant de pouvoir se reproduire.

Combien aura-t-on de lapins au bout de n années ?

Et bien la solution c'est la suite de Fibonacci, et le plus simple pour le comprendre est de regarder le schéma suivant (fait avec amour):

![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/intro_de_intro_DP/lapins.png) 
Les lapins en gris ne peuvent pas encore se reproduire, les autres oui. Au rang n, on a le nombre de lapins du rang n-1, auquel on ajoute tous les nouveaux nées, ce qui correspond au nombre de lapins du rang n-2. En effet, parmis les lapins du rang n-1, certains ne peuvent pas encore se reproduire, par contre tous ceux du rang n-2 le peuvent.

## Elle est où la DP ?
J'y arrive. Si on pose le problème "Combien vaut la suite de Fibonacci au rang n"
