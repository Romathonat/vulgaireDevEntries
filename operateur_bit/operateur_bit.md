Lorsque l'on développe, la question de l'optimisation se pose. Pour beaucoup, optimiser signifie simplement réduire la complexité des différentes fonctions. Ici, nous allons essayer d'aller plus en profondeur, de voir l'optimisation des calculs grâce à l'utilisation des opérateurs bits à bits.

/!\ Pour cela, vous devez bien entendu maitriser la base binaire, puisque que l'ordinateur l'utilise pour faire ses calculs.

/!\ Pour plus de compréhension, on notera (42)10 un nombre décimal et (101010)2 en binaire

Cet article sera en C++,mais une fois le principe compris il est facilement réutilisable en d'autres langages. Je ne me prétends pas expert en manipulation de bits, donc n'hésitez pas à apporter votre pierre à l'édifice.

Sommaire de ce petit article :

    Les opérateurs
    Exercices

## 1. Les opérateurs

Mais quels sont ces opérateurs ?

Vous en connaissez déjà certains : les opérateurs &, | et ^ qui sont respectivement le AND, le OR et le XOR. Mais d'autres sont un peu moins connu : le << et le >> permettant respectivement de décaler les bits vers la gauche et vers la droite.

Voilà, vous savez tout ce qu'il faut savoir ! Ou presque, voyons cela un peu plus précisément.

#### Le décalage de bits vers la gauche :

Si l'on fait 4 << 1, cela donne (8)10, car :

    - (4)10 donne (100)2,
    - lorsque l'on décale de 1 bit vers la gauche, on ajoute un 0 derrière (100)2 et on obtient (1000)2, ce qui correspond à (8)10 en base 10.

En effet, lorsque l'on fait un décalage de x bits, on multiplie en fait par 2x. Quand on y réfléchit c'est logique : ajouter un zéro à droite (une dizaine) en base 10 multiplie le nombre par 10, et ajouter un zéro à droite en base 2 multiplie le nombre par 2 !

Dans notre exemple on a bien 4<<1 = 4*21 = 4*2.

#### Le décalage de bit vers la droite :

Le principe est le même que pour le décalage vers la gauche, sauf qu'au lieu d'une multiplication, on fait une division avec troncage.

Par exemple si l'on fait 7>>1, on obtient (3)10, car (7)10 donne (111)2. Si l'on décale de 1 vers la droite, on obtient (11)2 (le bit de la droite est enlevé), ce qui correspond à (3)10. On a bien fait une division par 21 en gardant seulement la partie entière.

#### Les opérateurs &, |, ^ et ~ :

L'opérateur & fait une comparaison bit par bit. Si chaque bits = 1, alors il retourne 1, sinon il retourne 0. Par exemple si l'on prend (8)10 & (7)10, on obtient 0 :

- (8)10 donne (1000)10, (7)10 donne (111)2
- Les 2 n'ont pas le même nombre de bits, on ajoute un 0 devant le (111)2, on a donc (1000)2 et (0111)2
- On regarde bit par bit, donc on compare si les deux bits de poids forts sont à 1, si oui on met 1 sinon 0. et ainsi de suite pour tous les bits
- Le résultat obtenu est 0

Pareillement, si l'on fait (12)10 & (7)10, on obtiendra (4)10, car (12)10 donne (1100)2 et (7)10 donne (0111)2. On obtient, après comparaison, (0100)2, soit (4)10.

Cet opérateur peut servir, par exemple, à obtenir le reste de divisions par une puissance de 2 (voir plus loin).

L'opérateur | fait une comparaison bit par bit. Si un des bits = 1, alors il retourne 1, sinon il retourne 0. Par exemple si l'on prend (8)10 | (7)10, on obtient (15)10 :

- (8)10 donne (1000)2, (7)10 donne (111)2
- Les 2 n'ont pas le même nombre de bits, on ajoute un 0 devant le (111)2, on a donc (1000)2 et (0111)2
- On regarde bit par bit, donc on compare le 1 avec le 0, le 0 avec le 1, ...
- Le résultat obtenu est (1111)2, soit (15)10

Pareillement, si l'on fait (12)10 | (7)10, on obtiendra (15)10, car (12)10 donne (1100) et (7)10 donne (0111)2. On obtient, après comparaison, (1111)2, soit (15)10.

L'opérateur ^ fait la même chose, mais avec un XOR au lieu d'un OR, c'est à dire que dans le cas où les deux bits valent 1, XOR donne 0, sinon il se comporte comme un OR.

Enfin, le ~ est le complémentaire (le NOT). Il retournera l'inverse de chaque bit (1 pour 0 et 0 pour 1).
## 2. Exercices

Il est temps de faire des petits exercices tout simples, voir si vous avez compris le principe !

### Exercice 1 :

Que fait cette fonction :

``` C
int fonction_q1(int x, int nombre){

    return (nombre & (( 1 << x ) -1 ));

}
```

Réponse : Elle retourne le reste (modulo) de la division entre nombre et 2x. C'est vrai que ça a l'air barbare, mais c'est ce n'est pas si compliqué. Décortiquons la :

- 1 << x correspond à 1 * 2x
- On enlève 1 à ce resultat. En effet, une puissance de 2 a 1 bit à 1 et tout les autres à 0, donc une puissance de 2 moins 1 aura 1 bit de moins et tous ses bits à 1 (exemple : (8)10 donne (1000)2 et (7)10 donne (111)2).
- On utilise l'opérateur & entre le nombre et la puissance de 2 moins 1.
- La magie des bits fait que cela retourne le reste de la division. Si l'on prend par exemple :
    * nombre = (15)10
    * x = (3)10 (i.e on va diviser par 8)
    * (15)10 donne (1111)2
    * (8)10 donne (1000)2 en binaire, 8-1 = 7 donne donc (0111)2
    * On applique l'opérateur &
    * On obtient (0111)2, soit (7)10
    * On vérifie : le reste de 15 / 8 est bel et bien 7 !

Pour bien comprendre pourquoi cela fonctionne, passons en base 10. Les opérations précédentes correspondent à : prendre une puissance de 10, puis ne conserver que les chiffres de cette puissance de dix dans le nombre initial !

Exemple: (15532)10 et (100)10. Si on applique les régles précédentes, on obtient 32 (grâce au &), ce qui est bien le reste de la division de (15532)10 par (100)10

L'avantage de cette fonction est que ce calcul sera plus rapide qu'un modulo.

### Exercice 2 :

Prenons le code suivant :

``` C
if(x%2 == 0){
    cout << "Pair";
}
else{
    cout << "Impair";
}
```

Ce code vérifie que x est pair. Pour cela on utilise le modulo. Mais n'y aurait-il pas un moyen plus efficace de le faire ?

Réponse :

``` C
if(x & 1){
    cout << "Impair";
}
else{
    cout << "Pair";
}
```
En binaire, le premier bit est celui qui indique si le nombre est pair ou pas. C'est ce que l'on regarde ici.

### Exercice 3 :

Imaginons que vous deviez stocker 8 booléens. Trouvez une structure permettant de les stocker avec le moins de mémoire utilisée possible, et de les manipuler facilement.

Solution :

Il suffirait par exemple d'utiliser un char. En effet, un char est codé sur 1 octet, soit 8 bits. Cela signifie donc 8 choses dont la valeur peut être 1 ou 0, comme les booléens.

Pour récupérer un bit, vous pourriez utiliser la fonction suivante :

``` C
unsigned short int recupererTelBit(unsigned int position, char lesBools)
{
    return ((1 << position) & lesBools) >> position;
}
```

Comme vous pouvez le voir, la fonction retourne un unsigned short int. Le unsigned est là car les 2 valeurs retournables (0 et 1) ne sont pas négatives. Le short permet simplement de ne pas prendre trop de place en mémoire (un short étant sur 2 octets alors qu'un int est sur 4 octets).

Attention, cette fonction manipule le char comme s'il était un tableau de droite à gauche, on a donc pour 8 octets les index dans l'ordre 7,6,5,4,3,2,1,0. Pour que cette fonction retourne le 1er bit quand on met position = 1, il suffit de changer les 2 appel à position en 8-position.

Voyons ce que fait cette fonction :

- Prenons position = 2 et lesBools = 'a' (a est codé (97)10 = (01100001)2)
- 1 << position retourne ici (100)2
- On a donc (100)2 & lesBools, i.e (100)2 & (01100001)2. Comme ils n'ont pas le même nombre de bits, on rajoute des 0. On a donc : (00000100)2 & (01100001)2, ce qui donne (00000000)2.
- Finalement, on a 00000000 >> 2, ce qui donne 000000, soit 0.
- Le 3 ème bits est donc un 0

Maintenant, il faut aussi savoir comment changer tel ou tel bit.

Voyons 3 fonctions différentes :

``` C
unsigned int changerTelBit(unsigned int position, unsigned char lesBools)
{
    return (lesBools ^ (1 << position));
}
```
Cette fonction change un bit. Si celui-ci était un 0, il deviendra 1, et inversement. Décortiquons la :

- 1 << position permet de se placer au bit voulu
- lesBools ^ (1 << position) fait un XOR sur le bit voulu. Si on a lesBools = (1111)2 et position = 2, on aura comme résultat (1011)2

``` C
unsigned int changerTelBitEn1(unsigned int position, unsigned char lesBools)
{
    return (lesBools | (1 << position));
}
```
Cette fonction force un bit à prendre la valeur 1. Décortiquons la :

- 1 << position permet de se placer au bit voulu
- Le | (OR) nous permet de changer ce bit. Comme x | 1 (où x = 0 ou 1) donnera toujours 1, le bit à la position deviendra un 1 obligatoirement.

``` C
unsigned int changerTelBitEn0(unsigned int position, unsigned char lesBools)
{
    return (lesBools & ~(1 << position));
}
```

Enfin, cette fonction force un bit à prendre la valeur 0. Décortiquons la elle aussi :

- 1 << position permet de se placer au bit voulu
- Le ~ retourne l'inverse, du coup au lieu d'avoir un 1 et sept 0, on a sept 1 et un 0
- Le & (AND) nous permet de changer ce bit. Comme x & 0 (où x = 0 ou 1) donnera toujours 0, le bit à la position voulue deviendra un 0 obligatoirement.

Voilà, vous savez maintenant créer un tableau de booléens prenant moins de place en mémoire et parcourable sans boucles.

En utilisant les types, vous pourrez, avec cette technique, créer des tableaux de booléens jusqu'a une taille de 64 (long long).
