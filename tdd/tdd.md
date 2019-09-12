## Contexte
 Dans la vie réelle, les applications informatiques durent dans le temps (on ne jette pas le code à la fin de la journée contrairement à un TP). De plus, les spécifications et les entrées du programme évoluent. A partir du moment où le code contient plus de 2 ou 3 fonctions, il va falloir faire attention aux "effets de bords", c-à-d que la modification du programme pour répondre à cette nouvelle spécification ne détruise pas d'autres fonctionnalités du logiciel.  

## Solution
 Le Test Driven Development (TDD) est un paradigme ("une façon de faire") où on cherche à écrire les tests d'un code informatique avant d'écrire ledit code. Ainsi, lorsqu’on voudra changer le code, il suffira d'écrire de nouveaux tests pour tester les nouveaux cas, et relancer les anciens tests. On minimise les erreurs en se forçant à faire des fonctions courtes, qui répondent à une spécifications précises dont on test les cas limites le plus possible. En général ça permet de faire du meilleur code, plus maintenable, plus concis, mieux testé.

Le cycle du TDD est le suivant:  

1. Ecrire le test
2. Lancer les tests. Ca doit échouer
3. Ecrire le code
4. Lancer les tests. Ca doit fonctionner
5. Refactor. La modification du programme peut faire qu'il faille le "nettoyer" pour qu'il soit plus simple à maintenir à l'avenir.

## Activité:  FizzBuzz
Pour faire nos tests, nous utiliserons [pytest](https://docs.pytest.org/en/latest/getting-started.html). L'arborescence des fichiers est simple:  
├── TDD_example  
│   ├── `fizzbuzz.py`  
│   └── `test_fizzbuzz.py`  


## Cycle numéro 1 
Le programme doit fonctionner de la manière suivante:  
**Entrée**: 1  
**Sortie**: 1

Lancer le cycle TDD: Ecrire les tests, les lancer, écrire le code, relancer les test.  

**Solution**:


```python  
# test_fizzbuzz.py
from fizzbuzz import fizzbuzz  
  
def test_process_number():  
    assert fizzbuzz(1) == 1  

```

```python  
# fizzbuzz.py
def fizzbuzz(number):  

if number == 1:
    return 1  
```

Pour lancer les tests avec pytest c'est simple, en étant dans le répertoire:

```bash
pytest
```

On écrit le code minimal qui répond à la spécification. On lance les tests. Si tout fonctionne, on a fait un cycle de TDD.

## Cycle numéro 2
**Entrée**: 1, 2 (1 ou 2)  
**Sortie**: 1, 2

**Solution**:


```python  
# test_fizzbuzz.py
from fizzbuzz import fizzbuzz  
  
def test_process_number():  
    assert fizzbuzz(1) == 1
    assert fizzbuzz(2) == 2  
```
```python  
# fizzbuzz.py
def fizzbuzz(number):  
    return number  
```


On a modifié fizzbuzz, il répond à la nouvelle spécification, mais on vérifie aussi (et facilement) que les spécifications précédentes sont validées. On à la garantie qu'on a pas cassé le fonctionnement du programme testé.  

## Cycle numéro 3
**Entrée**: 1, 2,3  
**Sortie**: 1, 2, fizz  

**Solution**:

```python  
# test_fizzbuzz.py
from fizzbuzz import fizzbuzz  
  
def test_process_number():  
    assert fizzbuzz(1) == 1
    assert fizzbuzz(2) == 2
    assert fizzbuzz(3) == 'fizz'  
```


```python  
# fizzbuzz.py
def fizzbuzz(number):
    if number == 3:
        return 'fizz'  
    return number  
```
On a un nouveau cas, qu'on gère facilement avec un if.

## Cycle numéro 4

**Entrée**: 1, 2, 3, 5  
**Sortie**: 1, 2, fizz, buzz

**Solution**:

```python  
# test_fizzbuzz.py
from fizzbuzz import fizzbuzz  
  
def test_process_number():  
    assert fizzbuzz(1) == 1
    assert fizzbuzz(2) == 2
    assert fizzbuzz(3) == 'fizz'
    assert fizzbuzz(5) == 'buzz'
```

```python  
# fizzbuzz.py
def fizzbuzz(number):
    if number == 3:
        return 'fizz'
    if number == 5:
        return 'buzz'  
    return number  
```
Encore un nouveau cas, qu'on a géré avec un autre if.

## Cycle numéro 5
**Entrée**: 1, 2, 3, 5, 6, 10  
**Sortie**: 1, 2, fizz, buzz, fizz, buzz  

**Solution**:

```python  
# test_fizzbuzz.py
from fizzbuzz import fizzbuzz  
  
def test_process_number():  
    assert fizzbuzz(1) == 1
    assert fizzbuzz(2) == 2
    assert fizzbuzz(3) == 'fizz'
    assert fizzbuzz(5) == 'buzz'
    assert fizzbuzz(6) == 'fizz'
    assert fizzbuzz(10) == 'buzz'
```

```python  
# fizzbuzz.py
def fizzbuzz(number):
    if number % 3 == 0:
        return 'fizz'
    if number % 5 == 0:
        return 'buzz'  
    return number  
```
Cette fois-ci on se rend compte que c'est les multiples de 3 qui doivent retourner "fizz" et les multiples de 5 qui doivent donner "buzz".

## Cycle numéro 6

**Entrée**: 1, 2, 3, 5, 6, 10, 15  
**Sortie**: 1, 2, fizz, buzz, fizz, buzz, fizzbuzz  

  **Solution**:
  
```python  
# test_fizzbuzz.py
from fizzbuzz import fizzbuzz  
  
def test_process_number():  
    assert fizzbuzz(1) == 1
    assert fizzbuzz(2) == 2
    assert fizzbuzz(3) == 'fizz'
    assert fizzbuzz(5) == 'buzz'
    assert fizzbuzz(6) == 'fizz'
    assert fizzbuzz(10) == 'buzz'
    assert fizzbuzz(15) == 'fizzbuzz'
```

```python  
# fizzbuzz.py
def fizzbuzz(number):
    if number % 3 == 0 and number % 5 == 0:
        return 'fizzbuzz'  
    if number % 3 == 0:
        return 'fizz'
    if number % 5 == 0:
        return 'buzz'
    return number  
```
On a encore un nouveau cas: les nombres multiples de 3 et 5 doivent afficher 'fizzbuzz'. On le gère dans ce nouveau cycle TDD

Les tests fonctionnent bien, on peut "refactor" le code pour avoir quelque chose de plus élégant. On ajoute une doc pour expliquer ce que fait la fonction, utile quand on voudra reprendre le code des mois/années plus tard ou pour expliquer rapidement à un autre développeur qui travaillerait sur le projet.  

```python  
# fizzbuzz.py
def fizzbuzz(number):
    '''
    :param number: number
    :return: 'fizz' if number is multiple of 3, 'buzz' if number is multiple of 5, 'fizzbuzz' is multiple of both, or number in the default case.
    '''
    multiple_3 = number % 3 == 0
    multiple_5 = number % 5 == 0
    
    if multiple_3 and multiple_5:
        return 'fizzbuzz'  
    elif multiple_3:
        return 'fizz'
    elif multiple_5:
        return 'buzz'
    return number  
```


