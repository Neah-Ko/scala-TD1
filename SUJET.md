Dans ce premier TD d’introduction, nous commençons par entrer quelques expressions _Scala_ simples pour nous familiariser avec le language. 

Dans la deuxième partie, nous nous intéressons au calcul des termes d’une suite récurrente simple, la suite de Fibonacci. 
La première méthode de calcul « naïve » ne permet pas d’obtenir les valeurs des termes de la suite, car le temps de calcul nécessaire est trop important.
Nous implémenteron ensuite une méthode plus efficace. Enfin, dans la dernière partie, nous écrivons quelques fonctions pour calculer les termes de suites récurrentes de la forme 
![equation](http://latex.codecogs.com/gif.latex?U_{n+1} = f(U_{n}))


Liens utiles: 
* Essayez Scala dans votre Browser
  * [scalafiddle.io](https://scalafiddle.io/sf/gKgxQY0/1)
  * [scalakata.com](http://www.scalakata.com/)
  * [scastie.org](http://scastie.org/)
* [First Steps to Scala (www.artima.com)](http://www.artima.com/scalazine/articles/steps.html)
* [scala-lang.org](http://www.scala-lang.org)


# 1 Premières expressions Scala

## Expressions 
Saisissez par exemple 2 + 3 et validez par entrée. Scala vous répond 
```scala
Res0: Int = 5 
```
Ce qui signifie que vous avez entré une valeur, qui est de type entier et qui vaut 5. On peut écrire des expressions plus compliquées, _Scala_ respecte les priorités habituelles des opérateurs. 
Essayez par exemple : 
* (11 + 4) * 3 + 2 + 6 * 2 / 3
* 23 / 3
* 23 % 3   // Reste de la division entière
* 23 / 3.0

## Variables
De même qu’en mathématiques on écrit « soit s la somme des nombres 1, 2 et 3 », on écrit en Scala 
```scala
var S = 1 + 2 + 3 
``` 
Scala vous répond que vous venez de définir une variable `S` qui est un entier (Int) et qui vaut 6. Vous pouvez maintenant utiliser s dans toutes vos expressions. 
Essayez par exemple :
* (2 + s ) * s + 3  
* var p = 2 * s + s * s

Les définitions de noms que nous venons de voir sont permanentes : elles restent valides tant que vous n’abandonnez pas le système _Scala_. On dit qu’elles sont globales. 
On peut aussi définir **des constantes**, ce sont des **variables immuables**.
Essayez par exemple :
* val constante = 17
* constante = 12 // ceci produit une erreur car on essaie de modifier une variable déclarée par "val"
* p = 17 (p n’est pas une constante)

## Types de base
_Scala_ a un ensemble de types de bases prédéfinis. il sont résumer dans ce qui suit :

Nom     | Definition 
---     | --- 
Byte    |  entier 8-bit signé en complément à 2 (-128 à 127)
short   |  entier 16-bit signé en complément à 2 (-32 768 à 32 767)
Int     |  entier 32-bit signé en complément à 2 (-2 147 483 648 à 2 147 483 647)
Long    |  entier 64-bit signé en complément à 2 (-2^63 à 2 2^63 -1)
Float   |  flottant 32-bit IEEE 754 en précision simple
Double  |  flottant 64-bit IEEE 754 en précision double
char    |  caractère Unicode 16-bit non signé
String  |  chaîne de Caractères unicode
Boolean |  true/false

##  Fonctions
> Le Hello World
> ```scala
> def hello() 
> {
>    println("Hello, world!")
}
> ```
> qui a pour signature `hello: ()Unit` ce qui signifie que 
> * elle a pour non hello
> * le **()** signifie qu'elle ne prend aucun parametre en entrée 
> * le **Unit** signifie qu'elle ne renvoie rien

La syntaxe des fonctions en _Scala_ est proche de la notation mathématique habituelle. 
Pour définir une fonction carre qui calcule le carré d’un réel x, on peut principalement utiliser l’écriture suivante :
```scala
def carre(x :Int) :Int = x*x 
```
Qui a pour signature `carre: (x: Int)Int`


Après avoir défini cette fonction, il vous suffit d’entrer `carre(11) `
pour calculer le carré  de 11 qui a pour valeur 121. On définit très souvent en _Scala_ des fonctions de manière récursive. Par exemple, on peut écrire une fonction qui, étant donnés deux entiers x et n, calcule ![equation](http://latex.codecogs.com/gif.latex?X^{n}) de manière récursive. Voici 2 syntaxes possibles :
```scala
def puissance(x:Int,n:Int):Int = 
{
    if (n == 1) 
        x 
    else 
        x*puissance(x,(n-1))
}
```
solution utilisant le [**Pattern matching (link)**](http://fr.wikipedia.org/wiki/Filtrage_par_motif)

> Filtrage par motif:
> Le filtrage par motif est la vérification de la présence de constituants d'un motif par un programme informatique, ou parfois par un matériel spécialisé.
> Par contraste avec la reconnaissance de forme, les motifs sont complètement spécifiés. 
> De tels motifs concernent conventionnellement des séquences ou des arbres.

```scala
def puissance2(x:Int,n:Int):Int = 
{
    n match 
    {
        case 1 => x
        case _ => x*puissance2(x,(n-1))
    }
}
```
Pour calculer 2^10, il suffit alors d’entrer `puissance(2,10)`
 
> **Question 1.1**
> * Combien de multiplications effectue t-on pour calculer ![equation](http://latex.codecogs.com/gif.latex?X^{n}) en utilisant cette méthode ?
> * Connaissez-vous une autre méthode de calcul plus efficace?
> * Implémenter celle-ci ( [aide (link)](http://fr.wikipedia.org/wiki/Exponentiation_rapide) )

## Fonction anonyme
Avantage : notation plus courte, idéal pour les fonctions à usage unique
pour créer une fonction anomyme

```scala
(x: Int) => x + 1 // This function adds 1 to an Int named x.
res1: (Int) => Int = <function1>

scala> res1(1)
res2: Int = 2
```
il est aussi possible de créer une fonction avec plusieurs paramètres
```scala
    (x: Int, y: Int) => "(" + x + ", " + y + ")"
```
ou sans paramètre 
```scala
    () => { System.getProperty("user.dir") }
    
```
## Les fonctions d’ordre supérieur

Une fonction d’ordre supérieur est une fonction qui a au moins l’une des deux caractéristiques suivantes :
* prend en paramètre une ou plusieurs fonctions
* sa valeur de retour est une fonction

Les fonctions d’ordre supérieur aident à l’expressivité du code en mettant l’attention sur la tâche à accomplir plutôt que sur la façon de l’accomplir, le code dévient ainsi plus déclaratif.
```scala
def display(name : String)
{
	println(name)
}

def doSomeThingWith(name : String , action : String => Unit )
{
	action(name)
}

doSomeThingWith("Raoul", display)
```
Le programme show affiche une liste de nombres sur la console :
```scala
def show(xs: List[Int]) {
  for (x <- xs) {
    println(x)
  }
}
```
Le programme showSquares affiche les carrés d’une liste de nombres sur la console :
```scala
def showSquares(xs: List[Int]) {
  for (x <- xs) {
    println(x * x)
  }
}
```
Observez comme les programmes show et showSquares sont similaires : leur seule différence réside dans la façon dont le nombre va être transformé avant d’être affiché. Peut-on factoriser la parties communes de ces deux programmes ? Pour cela il faut transformer ce qui les différencie en un paramètre. Dans notre cas, il s’agit d’une fonction qui transforme un nombre en un autre nombre.

> Question 1.2 : Implémentez la fonction d’ordre supérieur show(f: Int => Int, xs: List[Int]), 
> qui affiche tous les nombres de la liste xs après les avoir transformés avec la fonction f. 
> Exemples d’utilisation :
>```scala
show(x => x, List(1, 2, 3))
show(x => x * x,List( 1, 2, 3))
```

# 2 La suite de Fibonacci
La suite de Fibonacci est une suite d’entiers ![equation](http://latex.codecogs.com/gif.latex?U_{n}) définie récursivement par les relations :

![equation](http://latex.codecogs.com/gif.latex?%20F_%7Bn%7D%20%20%3D%5Cbegin%7Bcases%7D0%20%26%20n%20%3D%200%5C%5C1%20%26%20n%20%3D%201%5C%5CF_%7Bn-1%7D%2BF_%7Bn-2%7D%20%26%20x%20%3E%201%5Cend%7Bcases%7D%20&)

## 2.1 Calcul en temps exponentiel
> Temps exponentiel: 
> * dès que n devient « grand » le calcul prend un temps excessif.

On souhaite pouvoir calculer à l’aide de _Scala_ n’importe quel terme de cette suite. _Scala_ permettant de définir simplement des fonctions de manière récursive, une idée naturelle est de s’inspirer directement de la définition de la suite.

> **Question 2**
>  En utilisant la définition récursive de la suite ![equation](http://latex.codecogs.com/gif.latex?U_{n}) donnée ci-dessus implémenter
> * Une fonction fib telle que fib(n) calcule ![equation](http://latex.codecogs.com/gif.latex?U_{n})         
> * Qui a pour signature `fibExp : (n :Int) Long`

> **Question 3**
>  * Calculez les premiers entiers de Fibonacci, puis calculez ![equation](http://latex.codecogs.com/gif.latex?U_{40}). Comment expliquez-vous que le temps de calcul soit si long ?

## 2.2 Calcul en temps linéaire
On peut effectuer un calcul plus efficace des termes de la suite en écrivant une fonction fib2 qui retourne une paire de deux termes consécutifs de la suite,
c’est-à-dire telle que fib2 n donne la paire ![equation](http://latex.codecogs.com/gif.latex?(U_{n},U_{n+1})) chaque appel de la fonction fib2 ne nécessitera alors plus qu’un seul appel récursif.

> **Question 4**
> * Implémenter une telle fonction fibLin.( [aide (link)](http://fr.wikipedia.org/wiki/Suite_de_Fibonacci) ) 
> * Qui a pour signature :  `fibLin : (n: Int, b: Long, a: Long) Long`
> * Combien d’additions effectue-t-on pour calculer ![equation](http://latex.codecogs.com/gif.latex?U_{n}) avec cette méthode ?

#3 Suites «![equation](http://latex.codecogs.com/gif.latex?U_{n+1}=f(U_{n}))»
Soit f une fonction d’un ensemble A dans lui-même et a un élément de A. On définit alors une suite ![equation](http://latex.codecogs.com/gif.latex?U_{n})  par

* ![equation](http://latex.codecogs.com/gif.latex?U_{0}= a)
* ![equation](http://latex.codecogs.com/gif.latex?U_{n+1}= f(u_{n})) 

Remarquons qu’une telle suite est définie par la donnée de la paire (f, a).
 
> **Question 5** 
> * Écrivez une fonction term qui prend pour arguments la paire (f, a) et l’entier n et calcule ![equation](http://latex.codecogs.com/gif.latex?U_{n})
> * Qui a pour signature `term (f: Long => Long, a: Long, n: Int) Long`

On souhaite définir une fonction qui calcule la liste  ![equation](http://latex.codecogs.com/gif.latex?[U_{m}; ... ; U_{0}]) des termes successifs de la suite.

On pourrait pour cela écrire un algorithme qui appelle successivement la fonction term pour des valeurs de n variant de 0 à m.

Cependant, cette méthode n’est pas efficace : pour chaque élément de la liste résultat, on reprend les calculs de zéro alors qu’il est facile de calculer un élément de la 
liste à partir de l’élément précèdent.

> **Question 6**
> En tenant compte de cette remarque, écrivez
> * Une fonction list telle que list(f , a, m) retourne la liste ![equation](http://latex.codecogs.com/gif.latex?[U_{m}; ... ; U_{0}])
> * Qui a pour signature `list : (f: Long => Long, a: Long, m: Int)List[Long]`

On veut maintenant obtenir les éléments dans l’ordre inverse. On propose pour cela deux méthodes différentes.
 
> **Question 7** 
> * Ecrivez une fonction map telle que map f ![equation](http://latex.codecogs.com/gif.latex?[U_{0}; ... ; U_{m}])  retourne ![equation](http://latex.codecogs.com/gif.latex?[f(U_{o}); ... ; f(U_{m})])
> * Déduisez-en une fonction listRev telle que listRev (f , a, n) calcule la liste ![equation](http://latex.codecogs.com/gif.latex?[U_{0}; ... ; U_{m}])
> * Signature ` map: (f: Long => Long, q: List[Long])List[Long]`
> * Signature ` listRev: Long => Long, a: Long, n: Int)List[Long]`
 
> **Question 8**
> * Ecrivez une fonction rev calculant le miroir d’une liste i.e. telle que rev ![equation](http://latex.codecogs.com/gif.latex?[U_{0}; ... ; U_{m}]) retourne ![equation](http://latex.codecogs.com/gif.latex?[U_{m}; ... ; U_{0}])
> * Déduisez-en une autre implémentation de la fonction fonction list rev. 
> * Quelle version vous semble la plus efficace ?
> * Signature : `rev: (list: List[Long])List[Long]`
> * Signature : `listRev : f: Long => Long, a: Long, n: Int)List[Long]`





