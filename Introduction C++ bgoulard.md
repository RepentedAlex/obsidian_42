---
aliases: 
tags:
  - CPP
---
# Les classes

En gros le C++, c'est pareil que le C sauf que tu as des [[Classes (C++)]] :
```C++
class classA
{
private:/* data */
	int position;
public:
	classA(/* args */);
	~classA();
	void walk(int distance);
};
```

Alors plusieurs trucs a expliquer. Déjà la fonction qui a le meme nom c'est le constructeur il se charge d'initialiser la structure de ta classe pense en gros que les classes c'est des structures avec des fonction attaches genre la j'ai attache la fonction walk et la data position le truc avec le `~` c'est le destructeur il se charge de libérer les sous objets en mode si dans ma classe j'ai besoin d'un char * alloue c'est le role du ~ de le free le constructeur c'est la fonction avec le meme nom toujours en mode class toto {} le constructeur se sera la fonction toto() le destructeur se sera la aussi toujours ~toto() constructeur -> meme nom que la classe destructeur -> meme nom que la classe avec un ~ devant toujours et comme les constructeurs renvoient un objet (wink wink) ils ont pas besoin de dire ce qu'il renvoient. pareil pour les destructeur ils sont tous void.

donc il y a pas

`class classA { private:     int position; public:     classA classA();     void ~classA();     void walk(int distance); };`

car on trouverait ca repetitif maintenant comment jappelle ma fonction de ma classe ? nom de ma classe :: nom de ma fonction ici classA::classA() -> appele le constructeur "mais c trop relou de devoir prefacer toujour par la classe" oui mais 90% des methodes (fonctions de l'objet) interagissse avec celui ci genre la fonction walk genre un main

`int main(int argc, char const *argv[]) {     classA a = classA();     std::cout << "Hello World !" << std::endl;     a.walk(10);     return 0; }`

(je ne suis pas pedantic et je call pas classA::classA()) mais tu vois le delire ne t'inquitte pas pour l'instant de "cout << ... " mais tu vois je precise pas le classA::walk() car le compilo comprend que c'est le walk de variable a du type classA donc il lie tout seul les 2


en gros, chaque classe est un espace imagine : tu a 2 createur (oui c'est possible) et je call classA() lequel il va prendre ? celui de default mais jai code ma fonction en penssant au classA::classA() donc pas super si quelqu'un change le constructeur par default my bad dailleur apres un test il me crie a la figure

`classA a = classA::classA();`

error: cannot call constructor ‘classA::classA’ directly

"a fully qualified constructor call is not allowed"

"il y a des flags de compil aussi en cpp ?" oui le compilo de c++ c'est g++ decendant de gcc il suporte pas mal de flag similaires, genre -Wall -Wextra, etc.. ici, a 42, on utilise clang++ mais c'est la meme "les makefiles sont differents aussi j'ai l'impression, je suis en train de regarder l'exercice Megaphone" oui il y a parfoit des dinguerie mais pour 90% du temps, quand tu fait du c avec parfoit des trucs en plus, en mode parfois tu oublie que cest du cpp. Maintenant que on a vu le main et l'interface (le point h qui decrit comment interagir avec la classe) on va voir comment on implemente (code en dur en vrai) celle ci. voici le constructeur de notre example:

`classA::classA() {     std::cout << "Class A creator" << std::endl;     position = 0; }`

(plus propre sans le /*args*/) en gros le premier classA il dit pour quelle classe est la fonction le second le nom de la fonction le reste tu connais

"mais comment on sait le retour de la fonction?" c'est un constructeur - meme nom 2 fois du coup le cpp comprend tout seul le retour de ta fonction par contre la fonction walk elle est pas constructeur donc elle, elle ressemble a ca :

`void classA::walk(int distance) {     std::cout << "Class A walk " << distance << " meters" << std::endl;     position += distance; }`

Ca ressemble un peu a du C ca non ? Problème: d'ou vient le position? il vient de l'objet lui meme notre .h declare un int position dans la classe donc quand le compilateur ne voit pas la var position il va chercher dans la classe faire position += distance c'est pareil que this->position += distance; si tu as fait du python cest la meme chose que self sauf en c++ cest this (modifié)


maitenant il faut que javoue tout jai menti (modifié)


`classA::classA(int position) {     std::cout << "Class A creator" << std::endl;     this->position = position; }`

c'est ca notre constructeur "mais alors le main comment il sait la position a envoyer dans la fonction ou tu a changer le main aussi?" non le main a pas changer classA a = classA(); // marche toujour] pourquoi? grasse au parametre par default classA(int position = 0);

`class classA { private:     int position; public:     classA(int position = 0);     ~classA();     void walk(int distance); };`

on specifie dans notre interface pour notre classe que si tu provide pas de position ben on prendra 0 comme valeur ca cest le debut du 'function overloading' en c++ tu peux faire ce genre de dinguerie

`int main(int argc, char const *argv[]) {     classA a = classA();     std::cout << "Hello World !" << std::endl;     a.walk(10);     foobar();     foobar("toto");     foobar(42);     return 0; }`

et la foobar c'est pas le meme dependant des arguments qui sont call (edited)

`void foobar(void) {     std::cout << "foobar" << std::endl; }  void foobar(std::string str) {     std::cout << "foobar " << str << std::endl; }  void foobar(int nb) {     std::cout << "foobar " << nb << std::endl; }`

jai cree 3 foobar et le compilo va link le bon tout seul


tu peux faire pareil pour le constructeur en mode

`classA::classA(std::string str) {     std::cout << "Class A creator" << std::endl;     std::cout << "str = " << str << std::endl;     position = 0; }`

un constructeur pour creer avec une string hop on lajoute a l'interface classA(std::string str); et voila utilisable depuis partout maintenant probleme "trop relou de devoir faire position = 0 + la ligne daffichage" "on pourait pas call le constructeur davant?" si on peut reduire notre fonction a juste ca

`{     std::cout << "str = " << str << std::endl; }`

avec les ':'

`classA::classA(std::string str) : classA()`

les : disent au compilo refer a un autre constructeur pour le call avec les trucs dans les parentheses (modifié)


tu as devant toi le premier des grans 3 pilier de l'objet le polymorphisme le compilo comprend que tu as plusieur fois le meme nom de fonction pour pas les memes arguments et redirige tout seul vers la bonne fonction

pour entrer dans le detail si tu veux? le cpp ajoute au nom de fonction le nom des args pour lequel la fonction est en mode foo devient foo_int()


en vrai, le deuxieme pillier du cpp : tu la deja vu c'est les classes le fait d'attacher une structure avec des champs de donnes (des proprietes) avec des fonctions (des methodes) et la on va voir le 3eme pilier du cpp
# Héritage

Si on fait
```C++
struct a
{
	int a;
	int b;
	int c;
};
```
C'est une struct en C classique, si je fait (toujours en C) : 
```C
struct a
{
	int a;
	int b;
	int c;
	int (*somme)(struct a this)
};
```
C'est une structure `a` avec 3 ints et un pointeur de fonction.

ce pointeur prend une struct a sous le nom de this en argument et il renvoie un int ben en C++ on peut faire pareil mais en beaucoup plus simple

```C++
struct a
{
	int a, b, c;
	int somme();
};
```
La c'est la meme chose mais en c++ (tu peux aussi faire le a, b, c en c c'est juste moins de frappe clavier) la fonction somme est declaree dans la structure a et elle renvoie un int

>*Pourquoi le * de somme a disparu ?*

Car le C++ fonctionne selon un système de **classes** et d'**objets**. Une classe est *toujours* un objet mais **tous les objets ne sont pas des classes**. Les structures sont aussi des objets et les objets sont une collection de proprietes et de fonctions. Les proprietes c'est des types simples ou d'autres objets et les fonctions sont des méthodes qui prennent par default implicitement le pointeur de l'objet qui apelle en mode somme a pas besoin de `struct a this` spécifié dans la version C++ il le prend silencieusement par default genre ca

```C++
struct struct_a
{
	int a, b, c;
	int somme()
	{
		return this->a + this->b + this->c;
	};
};
```

this est jamais specifie il est pris par default

# Privé et Public
ok next pillier l'heritage

```C++
class classB : public classA
{
private:
public:
	classB();
	~classB();
};
```

bonjour a la classe classB heritant publiquement de la classA je peux donc faire toutes les methodes publiques de classA avec la classB et pareil que le constructeur de classA pour les string je peux referncer les methodes de classA dans mon implementation de classB

`classB::classB() : classA() {     std::cout << "Class B creator" << std::endl; }`

ici jai donc pas besoin de init position classA s'en charge pour moi


`int main(int argc, char const *argv[]) {     classB b = classB();     b.walk(10);     return 0; }`


mais du coup notre main il rend ca

`Class A creator Class B creator Class A walk 10 meters Class B destructor Class A destructor`

le walk de b cest le walk de a et comme la classe b est un super set de la classe a elle call le destructor de a a la fin


"j'ai du mal a comprendre l'utilite de B dans ce contexte du coup" elle est nulle pour linstant mais cest une base abstraite pour l'heritage "ah, parceque B = heritier de A?" oui


donc si je creer la fonction fly dans a ben b elle a directement sans rien faire cette fonction aussi donc en c++ tu fait une class toute en haut full abstraite avec des fonction en mode print et derrierre tu fait des enfants en mode entier, string etc... et tu fait des enfants a ceux la en mode entier_positif, path_as_string, base_string ... et les enfants herite des methode de print et les petits enfants aussi tu verra souvent l'example de la classe Animaux avec l'enfant chat chien etc... avec des methodes genre 'talk' 'moove'

voici un example dans notre cas je veux plus voir "classA walk" quand je call walk avec la classe b jajoute ceci a mon interface pour classB

  `void walk(int distance);`

une fonction en plus

`void classB::walk(int distance) {     std::cout << "Class B walk " << distance << " meters" << std::endl;     position += distance; }`

mais la AIE ca compile pas
``` shell
classB.cpp:29:2: error: ‘int classA::position’ is private within this context    29 |  position += distance;       |  ^~~~~~~~ In file included from classB.hpp:4,                  from classB.cpp:14: classA.hpp:21:6: note: declared private here    21 |  int position;       |      ^~~~~~~~
```
mince le gars qui a declarer la classe a il a mis position en privee jy ai pas access
```C++
class classA
{
private:
	int position;
public:
	...
};
```


hop on a passe position en public et hop ca conpile si on reprend notre example des animeaux on peut faire un truc sur les classe on peut dire les classes qui herite de celle ci doivent implementer les fonction x y z etc... example

```C++
class classAnimeaux
{
public:
	classAnimeaux() {};
	~classAnimeaux() {};
	virtual void walk(int distance) = 0;
	};
```
Ici on définit une classe `classAnimeaux` avec un constructeur vide un destructeur vide et une fonction virtuelle cette dernière dit en gros il y a une méthode `walk` non implémentée (le = 0) qui a le prototype suivant: `void walk(int distance)`.

En gros la table virtuelle c'est une table séparée pour les méthodes qui dit je vais faire des trucs spéciaux et ici on dit la méthode avec son prototype est pas implémentée dans la classe dons si j'essaye de compiler avec juste la classe `classAnimeaux` le compilateur ne va pas être ok src.
```C++
classAnimeaux a = classAnimeaux();
```
>Erreur.
```shell
test.cpp:35:34: error: invalid cast to abstract class type ‘classAnimeaux’    35 |  classAnimeaux a = classAnimeaux();       |                                  ^ In file included from test.cpp:14: classAnimeaux.hpp:4:7: note:   because the following virtual functions are pure within ‘classAnimeaux’:     4 | class classAnimeaux       |       ^~~~~~~~~~~~~ classAnimeaux.hpp:11:15: note:     ‘virtual void classAnimeaux::walk(int)’    11 |  virtual void walk(int distance) = 0;       |               ^~~~ test.cpp:35:16: error: cannot declare variable ‘a’ to be of abstract type ‘classAnimeaux’    35 |  classAnimeaux a = classAnimeaux();       |                ^
```

si on implémente un enfant de `classAnimeaux` on doit specifier une implementation de walk sinon on aura toujours la meme erreur

example d'implementation

```C++
class classChien : public classAnimeaux
{
public:
	classChien(/* args */);
	~classChien();
	void walk(int distance) override {
		std::cout << "Class Chien walk " << distance << " meters" << std::endl;
		position += distance;
	};
};
```
Ici le keyword `override` est specifie pour dire qu'on surcharge on ecrit par dessu on remplace le symbole `walk` de depart une fois cette methode mise dans la VTABLE (table des methodes virtuelles) on peut la changer dans chacun des enfants, exemple:
```C++
#include "classChien.hpp"
class classPetitChien : public classChien
{
private:
	/* data */
public:
	classPetitChien(/* args */) {};
	~classPetitChien() {};
	void walk(int distance) override {
		distance = distance / 2;
		std::cout << "Class Petit Chien walk " << distance << " meters" << std::endl;
		position += distance;
	};
};
```
Mais que ce passe-t-il si moi grand manitou et genie ultime je trouve je bosse avec des nuls/mal intentiones? il existe un mot clees pour toi grand manitou le mot clee final final fige l'implementation dune methode dans la VTABLE et override devient alors une erreur pour le compilateur qui hurle "ah!" example
```C++
void walk(int distance) final {
	distance = distance / 2;
	std::cout << "Class Petit Chien walk " << distance << " meters" << std::endl;
	position += distance;
};
```
Ici on passe le walk de petit chien en final donc si cree la classe `ToutPetitChien` je ne pourrais pas override le walk bon bah je sais pas pk mais ca devrait pas marcher mais ca compile [cpp/language/final](https://en.cppreference.com/w/cpp/language/final).
```C++
class classPetitChien : public classChien
{
public:
	classPetitChien(/* args */) {};
	~classPetitChien() {};
	void walk(int distance) final {
		distance = distance / 2;
		std::cout << "Class Petit Chien walk " << distance << " meters" << std::endl;
		position += distance;
	};
};

class classToutPetitChien final : public classChien
{
public:
	classToutPetitChien(/* args */) {};
	~classToutPetitChien() {};
	void walk(int distance) override {
		distance = distance / 4;
		std::cout << "Class Tout Petit Chien walk " << distance << " meters" << std::endl;
		position += distance;
	};
};
```
Ca ne devrait pas compiler mdr mais la pas derreur ici a 42 lol ¯\_(ツ)_/¯ idk

```C++
struct Base
{
	virtual void foo();
};

struct A : Base
{
	void foo() final; // Base::foo is overridden and A::foo is the final override
};

struct B final : A // struct B is final
{
	void foo() override; // Error: foo cannot be overridden as it is final in A
};
```

```shell
main.cpp:14:10: error: virtual function 'virtual void B::foo()' overriding final function    14 |     void foo() override; // Error: foo cannot be overridden as it is final in A       |          ^~~ main.cpp:8:10: note: overridden function is 'virtual void A::foo()'     8 |     void foo() final; // Base::foo is overridden and A::foo is the final override       |          ^~~
```
Littéralement notre example dans la doc lol idk


bon mais en gros tu as les bases du c++ sur les classes avec virtual tu peux aussi le metre dans les heritage de fonction en mode class classB : final classA c'est l'heritage de classA et rien ne peut herite de class B car cest declarer comme l'implentation finale de l'arbre d'heritage tu peux faire pareil avec virtual et heriter des classes virtuellement (edited) la doc cppreference est vraiment cool maintenant la dernier truc


les references en gros les mecs qui on fait le c++ se sont dit les pointeurs c cool, c'est rapide mais... il y a plein derreur due a eux donc on va faire un truc on va ajouter un type (edited) la reference ca fonctione comme un pointeur mais ca peut pas etre null et le symbole est constant cest toujours le &

"Mais rien à voir avec le & qu’on utilise pour récupérer les adresse en C du coup" ben c pareil que le c ca prend ladresse mais ca check le fait de pas etre null ici une fonction foobar avec le type chien

```C++
void foobar(classChien &chien)
{
	std::cout << "foobar " << chien.position << std::endl;
}
```
un appel dans le main:
```C++
foobar(chien);
```
en gros tu dit a ton compilo occupe toi des adresses. Si je test avec
```C++
foobar(NULL);
```
jai direct l'erreur

```shell
test.cpp: In function ‘int main(int, const char**)’: test.cpp:53:9: error: invalid initialization of non-const reference of type ‘classChien&’ from an rvalue of type ‘long int’    53 |  foobar(NULL);       |         ^~~~ test.cpp:36:25: note: in passing argument 1 of ‘void foobar(classChien&)’    36 | void foobar(classChien &chien)       |             ~~~~~~~~~~~~^~~~~
```
voila voila voila

fin


![Type de fichier joint : archive](https://discord.com/assets/5ff8ffaa3831478d2a28.svg "archive")

[tuto_cpp.tar.gz](https://cdn.discordapp.com/attachments/1143161845562212513/1186297892433035354/tuto_cpp.tar.gz?ex=6592bd01&is=65804801&hm=e3e731a49f92ba1e625691567d99155e0caa29165ac0d4d00c786472d609194e&)
source code