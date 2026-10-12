# 1. Abstraction symbolique d'un system complexe
Dans cet article, nous considérons un système dynamique non linéaire de la forme suivante:

>[!note] Equality (1)
>L'equation du systeme dynamique non lineaire:
>$$x(t+1)\ =\ f(x(t), u(t), w(t)),\ x(t) \in \mathbb{X},\ u(t) \in \mathbb{U},\ w(t) \in \mathbb{W} \tag{1}$$
>ou $x(t),\ u(t),\ w(t)$ denotent l'etat, la commande et la perturbation du systeme a l'instant $t \in \mathbb{N}$.
>Les ensembles $\mathbb{X} \subseteq \mathbb{R}^{n_{x}},\ \mathbb{U} \subseteq \mathbb{R}^{n_{u}}\ \mathbb{W} \subseteq \mathbb{R}^{n_{w}}$, supposes compacts (c'est-a-dire fermes et bornes) qui sont les contraintes sur l'état, la commande et la perturbation du systeme.
>^1

Dans cet article, nous ne considérons que des systèmes dynamiques en temps discret.
Un **modèle symbolique** est une $\ll$abstraction finie$\gg$ de la dynamique de [[The Mighty Pepper#1. Abstraction symbolique d'un system complexe|(1)]]. Donc, il n'existe qu'un nombre fini de valeurs possibles pour l'état de la commande du modèle symbolique.

>[!note] Notation (2)
>Ainsi, un modele symbolique peut s'ecrire sous la forme suivante:
>$$ \xi(t+1) \in g(\xi(t), \sigma(t)),\ \xi(t) \in \Xi,\ \sigma(t) \in \Sigma \tag{2}$$
>ou $\xi(t)$ et $\sigma(t)$ denotent l'etat et la commande du modele symbolique a l'instant $t \in \mathbb{N},\ \Xi\ et\ \Sigma$ sont des ensembles finis.
>^2

On déduit que pour un état $\xi$ et une commande symbolique $\sigma$, $g(\xi, \sigma)$ et un sous-ensemble de $\Xi$ représentant l'ensemble des états successeurs potentiels.
## 1.1 Méthodologie d'abstraction symbolique
Voila une approche pour calculer un modèle symbolique de système décrit dans [[The Mighty Pepper#^1|(1)]]. L'**abstraction du système** se fait en deux étapes:
1. Définition des ensembles d'états et de commandes symboliques $\Xi$ et $\Sigma$ par une discrétisation des ensembles d'états et de commandes de [[The Mighty Pepper#^1|(1)]];
2. Définition de la fonction de transition $g$ par approximation précise de la dynamique de [[The Mighty Pepper#^1|(1)]].
### 1.1.1 Discrétisation de l'état et de la commande
Considérons une partition de l'ensemble d'états $\mathbb{X}$: soit $\mathbb{X}_{1}, ..., \mathbb{X}_{m}$ une collection de sous-ensembles de $\mathbb{X}$ vérifiant:
$$ \mathbb{X}=\mathbb{X}_{1}\cup...\cup\mathbb{X}_{m}\ \text{ et }\ \mathbb{X}_{i}\cap\mathbb{X}_{j}=\emptyset,\ \forall i \neq j$$
De plus, on note $\mathbb{X}_{0}=\mathbb{R}^{n_{x}}\backslash\mathbb{X}$ .

On définit alors l'ensemble d'états symboliques comme $\Xi = \{0,...,m\}$ ou l'état symbolique $\xi \in \Xi$ représente l'ensemble des états $x\in\mathbb{X}_{\xi}$ . 
On associe également l'**interface d'abstraction** $q:\ \mathbb{R}^{n_{x}} \rightarrow \Xi$ qui associe a tout $x\in\mathbb{R}^{n_{x}}$ l'état symbolique lui correspondant:
$$ \forall x \in \mathbb{R}^{n_{x}},\ \forall \xi \in \Xi,\ (q(x) = \xi \Longleftrightarrow x \in \mathbb{X}_{\xi}).$$
Pour la définition de l'ensemble de commandes symboliques, on considère un nombre fini de commandes $u_1,...u_l \in \mathbb{U}$ . On définit alors l'ensemble des commandes symboliques comme $\Sigma = \{1,...,l\}$ ou la commande symbolique $\sigma \in \Sigma$ représente la commande $u_{\sigma} \in \mathbb{U}$ . 
On définit ainsi l'**interface de concrétisation** $p:\ \Sigma \rightarrow \mathbb{U}$ telle que:
$$ \forall \sigma \in \Sigma,\ p(\sigma) = u_\sigma $$![[Pasted image 20261011004132.png]]
*Figure 1 - Aperçu de l'approche symbolique pour le contrôle*
### 1.1.2 Approximation de la dynamique
Les ensembles d'états et de commandes symboliques $\Xi$ et $\Sigma$ étant maintenant proprement définis, il nous reste a spécifier la fonction de transition $g$. Pour un état et d'une commande symbolique $\xi \in \Xi,\ \sigma \in \Sigma,\ g(\xi, \sigma)$ dénote l'ensemble des états symboliques vers lesquels le modelé symbolique peut évoluer.
	**Définition.** Un contrôleur est, d'après ma compréhension, la fonction qui donne, d'après l'état $x$ ( ou symbolique $\xi$ ) a l'instant $t$, la commande $u$ ( ou symbolique $\sigma$ ) qui permet d'aller au prochain état ( a travers la fonction $f$ ou $g$ suivant le modèle dynamique/symbolique ), tout en respectant les spécifications indiquées.

Pour que le modèle symbolique soit utilisable pour synthétiser des contrôleurs pour le system décrit dans [[The Mighty Pepper#^1|(1)]], il faut que la dynamique symbolique capture l'ensemble des comportements possible de [[The Mighty Pepper#^1|(1)]].
	Cela veut seulement dire qu'on doit avoir une bijection entre le modèle dynamique et symbolique concernant les états et commandes.

>[!info] Definition (3)
> Pour ce faire, il est nécessaire de calculer pour chaque $\xi \in \Xi,\ \sigma \in \Sigma$ un ensemble $\mathbb{Y}_{\xi,\sigma} \subseteq \mathbb{R}^{n_x}$ vérifiant l'inclusion ci-dessous:
> $$ \mathbb{Y}_{\xi,\sigma} \supseteq f(cl(\mathbb{X}_\xi),u_\sigma,\mathbb{W}) := \{f(x, u_\sigma, w)\ |\ x \in cl(\mathbb{X}_\xi),\ w \in \mathbb{W}\} \tag{3}$$
ou $cl(\mathbb{X}_{\xi,\sigma})$ denote l'adherence de l'ensemble $\mathbb{X}_\xi$ . 
>
>Ainsi, $\mathbb{Y}_{\xi,\sigma}$ est un ensemble contenant l'ensemble des états vers lequel [[The Mighty Pepper#^1|(1)]] peut evaluer depuis un etat $x \in cl(\mathbb{X}_\xi)$, sour l'action de la commande $u_\sigma$ et l'effet d'une perturbation $w \in \mathbb{W}$ . 

**Que signifie $\mathbb{Y}_{\xi,\sigma}$ ?
	Une surapproximation de l'ensemble atteignable:** il contient tous les états ou le système [[The Mighty Pepper#^1|(1)]] peut arriver en un pas, en partant de n'importe quel $x$ dans la cellule $\xi$ (adhérence comprise), avec la commande $u_\sigma$ et n'importe quelle perturbation $w \in \mathbb{W}$.

>Le calcul des ensembles $\mathbb{Y}_{\xi,\sigma}$ représente le _principal point de difficulté_ dans le calcul de modèle symbolique ; plusieurs méthodes ont été développées a cet effet [[The Mighty Pepper#1. Abstraction symbolique d'un system complexe#1.1 Méthodologie d'abstraction symbolique#1.1.3 Analyse d'atteignabilité|(section 1.1.3)]].

Ensuite, la dynamique du modèle symbolique peut être définie de la manière suivante:
$$g(\xi,\sigma)=\{\xi^{+} \in \Xi\ |\ \mathbb{Y}_{\xi,\sigma} \cap cl(\mathbb{X}_{\xi^{+}}) \neq \emptyset \}.$$
La procédure d'approximation de la dynamique de [[The Mighty Pepper#^1|(1)]] est illustrée dans la [[The Mighty Pepper#^figure2|figure 2]].

![[Pasted image 20261011004920.png]]
*Figure 2 - Calcul de la dynamique symbolique par approximation de la dynamique de (1)*^figure2

>[!note] Remarque 1
>Les choix des discrétisations de l'état et de la commande sont importants pour le succès de l'approche symbolique. Notons qu'il existe des methodes de raffinement iteratif des discretisations afin de trouver le bon compromis precision/complexite (section 4.1.1). Nous decrivons ci-dessous un critere simple pour determiner si une discretisation est trop grossiere. Supposons que [[The Mighty Pepper#^1|(1)]] satisfais la condition de mobilite suivante:
>$$ \forall x \in \mathbb{X},\ \exists u \in \mathbb{U},\ \forall w \in \mathbb{W},\ x \neq f(u, v, w) \tag{4}$$

>[!note] Remarque 1 *(Suite)*
>La condition ci-dessus signifie qu'il est toujours possible de choisir une commande de [[The Mighty Pepper#^1|(1)]] pour modifier la valeur courante de l'etat du systeme, une condition encessaire pour satisfaire des proprietes autres que celles de surete. Pour pouvoir utiliser le modele symbolique pour ce type de proprietes, il est necessaire qu'il satisfasse egalement la condition de mobilite:
>$$ \forall \xi \in \Xi \setminus \{0\},\ \exists \sigma \in \Sigma,\ \xi \notin g(\xi, \sigma). \tag{5} $$
>	Si les discretisations de l'etat et de la commande sont trop grossieres, il est possible que (5) ne soit pas verifiee meme si (4) l'est. En revanche si (4) est verifiee, la condition (5) le sera egalement pour toute discretisation suffisamment fine.

>[!note] Remarque 2
>Le modele symbolique construit suivant la procedure decite dans cette secion est en fait valable pour n'importe quel systeme de la forme (1) ou $f$ est remplacee par une fonction $\tilde{f}$ verifiant pour chaque $\xi \in \Xi, \sigma \in \Sigma$
>$$ \{ \xi^+ \in \Xi\ |\ \tilde{f}(cl(\mathbb{X}_\xi, u_\sigma, \mathbb{W}) \cap cl(\mathbb{X}_{\xi^+}) \neq \emptyset \} \subseteq g(\xi, \sigma) .$$
>Ainsi, il apparait qu'un modèle symbolique ne représente pas uniquement une abstraction d'un seul système dynamique mais d'un ensemble de systèmes dynamiques. Les contrôleurs synthétisés par les approches symboliques disposent alors par construction de certaines marges de robustesse.

**Un exemple simple : le thermostat** 
Considérons un modèle de l'évolution de température dans une pièce décrit par :
$$ x(t+1) = x(t) + (w(t) - x(t))a + u(t)(T - x(t))b, $$
ou $x\in\mathbb{R}$ est la température moyenne de la pièce
- $u\in\{0,1\}$ est la commande du thermostat (arrêt/marche), 
- $w\in[10,15]$ est la température extérieure, 
- $T=30$ est la température du chauffage quand il est en marche, 
- et $a=0.1$ et $b=0.2$ sont les constantes thermiques respectivement associées a la perte de température extérieure et au chauffage lorsqu'il est allume.

Notre objectif est d'appliquer la méthode d'abstraction symbolique présentée dans cette section pour maintenir la température $x$ dans l'intervalle $\mathbb{X}=[17,23]$ malgré la perturbation $w\in[10,15]$. L'intervalle $\mathbb{X}$ est partitionne en trois sous-intervalles de même taille $\mathbb{X}_1=[17,19],\ \mathbb{X}_2=[19,21], \mathbb{X}_3=[21,23]$ représentés par les états symboliques $\xi=1,2,3$. L'état symbolique $\xi=0$ représente alors toutes les autres températures que l'on souhaite éviter $\mathbb{X}_0 = \mathbb{R} \setminus \mathbb{X}$ . 
L'ensemble des commandes $\mathbb{U} = \{0,1\}$ est déjà un ensemble discret que nous conservons ainsi par simplicité en dénotant $\Sigma = \mathbb{U}$ .

La dynamique du système étant très simple (linéaire et de dimension 1), nous sommes dans un des rares cas ou les ensembles atteignables du système peuvent êtres évalués de manière exacte, sans avoir recours aux méthodes de surapproximation de la [[The Mighty Pepper#1. Abstraction symbolique d'un system complexe#1.1 Méthodologie d'abstraction symbolique#1.1.3 Analyse d'atteignabilité|section 1.1.3]]. 
La fonction de transition $g$ du modèle symbolique obtenu après l'application de la [[The Mighty Pepper#1. Abstraction symbolique d'un system complexe#1.1 Méthodologie d'abstraction symbolique#1.1.2 Approximation de la dynamique|section 1.1.2]] peut ainsi être décrite par le graph ci-dessous (les transitions rouges sont celles vers l'état symbolique $\xi=0$ non représenté, que l'on souhaite éviter).

![[Pasted image 20261011165210.png]]
	La synthèse d'un contrôleur de sureté pour maintenir la température dans $\mathbb{X} = [17,23]$ (décrite plus en détails dans la section 2.1) consiste alors a interdire la commande $\sigma=0$ depuis l'état symbolique $\xi=1$ et la commande $\sigma=1$ depuis $\xi=3$.
### 1.1.3 Analyse d'atteignabilité
	Ceci m'a semble un peu trop complique pour la clarte ajoutee au contenu. D'ou la Claudification suivante:

Cette page répond a la question laissée ouverte en [[The Mighty Pepper#1. Abstraction symbolique d'un system complexe#1.1 Méthodologie d'abstraction symbolique#1.1.2 Approximation de la dynamique|1.1.2]] : **comment calculer concrètement $\mathbb{Y}_{\xi,\sigma}$**, la surapproximation de l'ensemble atteignable depuis $cl(\mathbb{X}_\xi)$. Elle donne deux méthodes a base d'intervalles (boites):
- **Systèmes monotones (coopératifs),** ou les dérivées partielles de f sont positives. Il suffit alors d'évaluer $f$ en seulement deux points, le coin inferieur $(\underline{x}, u_\sigma, \underline{w})$ et le coin supérieur $(\overline{x}, u_\sigma, \overline{w})$, pour obtenir la plus petite boite contenant l'ensemble atteignable [[The Mighty Pepper#^figure3|(fig. 3a)]]. C'est exact en termes d'inclusion, mais limite aux systèmes monotones.
- **Systèmes a dérivées bornées** (classe beaucoup plus large). On calcule le successeur de centre, $f(x^{*},u_\sigma,w^{*})$, et on l'entoure d'une boite de demi largeur $D_x\delta_x + D_w\delta_w$, ou $D_x$ et $D_w$ bornent les dérivées partielles [[The Mighty Pepper#^figure3|(fig. 3b)]]. Le terme ajoute représente la variation maximale possible autour du centre, a cause de la taille de la cellule $(\delta_x)$ et de la perturbation $(\delta_w)$.

![[Pasted image 20261011172040.png]]
*Figure 3 - Deux méthodes de surapproximation (intervalle rouge) de l'ensemble atteignable (bleu) a partir d'un intervalle d'états initiaux (gris)*^figure3

>[!note] Remarque 3
>L'approche decrite dans cette section peut etre appliquee pour tous les elements de la partition $\mathbb{X}_\xi$ avec $\xi \neq 0$. Pour $\xi = 0$, nous rappelons que $\mathbb{X}_0 = \mathbb{R}^{n_x} \setminus \mathbb{X}$. Il est d'usage dans ce cas particulier d'utiliser la surapproximation $f(cl(\mathbb{X}_0),u_\sigma,\mathbb{W}) \subseteq \mathbb{R}^{n_x}.$
## 1.2 Contrôleur symbolique et concrétisation

>[!info] Contrôleur Symbolique
>Un contrôleur est la règle qui décide quelle commande appliquer au système a chaque instant, en fonction de ce qu'il observe. Ici, il travail dans le monde symbolique : il recoit l'état symbolique $\xi(t)$ et renvoie une commande symbolique $\sigma(t)$.

Considérons maintenant un contrôleur pour le modèle symbolique [[The Mighty Pepper#^2|(2)]] de la forme : ^6
$$
\begin{cases}
\psi(t) = h_1(\psi(t-1),\ \xi(t)), \\
\sigma(t) = h_2(\psi(t),\ \xi(t))
\end{cases}
\tag{6}
$$
ou $\psi(t) \in \Psi$ est une variable interne du contrôleur et $\Psi$ est un ensemble fini. 

Ce contrôleur possède une mémoire représentée par la variable interne $\psi(t)$ et est donc un _contrôleur dynamique_. Le cas d'un _contrôleur statique_, c'est-a-dire sans mémoire, peut être vu comme un cas particulier ou $h_2(\psi,\xi)=h(\xi)$ . Dans ce cas, la commande symbolique $\sigma(t)=h(\xi(t))$ ne dépend que de l'état symbolique a l'instant $t$.

On note $\mathcal{T}_{symb}(\xi_0)$ l'ensemble des trajectoires du système symbolique en boule fermée [[The Mighty Pepper#^2|(2)]]-[[The Mighty Pepper#^6|(6)]] avec la condition initiale $\xi(0)=\xi_0$, c'est un ensemble de *trajectoires entières*, c'est-a-dire de suites $\xi(0), \xi(1), \xi(2),...$ a valeurs dans $\Xi$. Formellement, $\mathcal{T}_{symb}(\xi_0)$ est l'ensemble des suites $\xi(\cdot)$ telles que :
- $\xi(0)=\xi_0$
- $\sigma(t)=h_2(\psi(t),\ \xi(t))$, avec $\psi(t)=h_1(\psi(t-1),\ \xi(t))$ (la commande vient du contrôleur);
- $\xi(t+1) \in g(\xi(t),\ \sigma(t))$ pour tout $t \geq 0$. 
Notons qu'il peut exister plusieurs trajectoires pour une condition initiale a cause du caractère multivalué de la fonction de transition $g$. Ceci est du au fait que $\xi^+$ a plusieurs choix comme élément de l'ensemble $g(\xi, \sigma)$.

Nous décrivons maintenant la procédure de **concrétisation du contrôleur** [[The Mighty Pepper#^6|(6)]] qui nous permettra de contrôler le système dynamique [[The Mighty Pepper#^1|(1)]]. Ainsi, nous considérons le contrôleur suivant : ^7
$$
\begin{cases}
\psi(t) = h_1(\psi(t-1),\ q(x(t))) \\
u(t) = p(h_2(\psi(t),\ q(x(t))))
\end{cases}
\tag{7}
$$
ou $q : \mathbb{R}^{n_x} \rightarrow \Xi$ et $p : \Sigma \rightarrow \mathbb{R}^{n_u}$ sont les interfaces d'abstraction et de concrétisation définies en [[The Mighty Pepper#1. Abstraction symbolique d'un system complexe#1.1 Méthodologie d'abstraction symbolique#1.1.1 Discrétisation de l'état et de la commande|section 1.1.1]].

On note $\mathcal{T}_{conc}(x_0)$ l'ensemble des trajectoires du système en boucle fermée [[The Mighty Pepper#^1|(1)]]-[[The Mighty Pepper#^7|(7)]] avec la condition initiale $x(0)=x_0$, c'est un ensemble de *trajectoires entières*, c'est-a-dire de suites $x(0),x(1),x(2),...$ a valeurs dans $\mathbb{X}$. Formellement, $\mathcal{T}_{conc}(x_0)$ est l'ensemble des suites $x(\cdot)$ telles que:
- $x(0)=x_0$
- $u(t)=p(h_2(\psi(t),\ q(x(t))))$, avec $\psi(t)=h_1(\psi(t-1),\ q(x(t)))$ ;
- $x(t+1) = f(x(t), u(t), w(t))$ avec $w(t) \in \mathbb{W}$, pour tout $t \geq 0$.
En raison de la présence de la perturbation $w(\cdot)$, il peut exister plusieurs trajectoires pour une condition initiales. On note aussi :
$$ q(\mathcal{T}_{conc}(x_0)) = \{ \xi(\cdot)\ |\ \xi(\cdot) = q(x(\cdot)),\ x(\cdot) \in \mathcal{T}_{conc}(x_0) \} $$
Cet ensemble correspond aux trajectoires du système en boucle fermée [[The Mighty Pepper#^1|(1)]]-[[The Mighty Pepper#^7|(7)]], observées via l'interface d'abstraction.

Nous pouvons maintenant énonce le théorème fondamental du contrôle symbolique (voir également [[The Mighty Pepper#^figure4|figure 4]]).

>[!note] Théorème 1
>Pour tout $x_0\in\mathbb{R}^{n_x}$, on a $q(\mathcal{T}_{conc}(x_0)) \subseteq \mathcal{T}_{symb}(q(x_0))$. ^theorem1

![[Pasted image 20261011224819.png]]
*Figure 4 - Architecture de contrôle symbolique et illustration du théorème fondamental du control symbolique*^figure4

Intuitivement, le [[The Mighty Pepper#^theorem1|théorème 1]] stipule que les trajectoires du système en boucle fermée [[The Mighty Pepper#^1|(1)]]-[[The Mighty Pepper#^7|(7)]] forment un sous-ensemble des trajectoires du système symbolique en boucle fermée [[The Mighty Pepper#^2|(2)]]-[[The Mighty Pepper#^6|(6)]].

Cela implique que si un contrôleur symbolique [[The Mighty Pepper#^6|(6)]] synthétisé comme dans la [[The Mighty Pepper#^section2|section 2]] permet au modèle symbolique en boucle fermée de satisfaire des spécifications données, alors le contrôleur concrétisé [[The Mighty Pepper#^7|(7)]] permettra aussi au système initial en boucle fermée de satisfaire les mêmes spécifications.

Notez toutefois que l'échec de synthèse d'un contrôleur symbolique n'implique pas pour autant que le système initial est incapable de satisfaire les spécifications (contrairement aux abstractions complètes évoquées dans la section 4.2). 

En effet, puisque l'étape d'abstraction symbolique s'appuie sur des méthodes de surapproximation du système [[The Mighty Pepper#^1|(1)]], cet échec peut être dû au fait d'avoir créé un modèle symbolique trop grossier pour capturer les comportements importants du système, ou à la présence de trop nombreuses transitions d'un état symbolique vers lui-même qui empêchent la progression du modèle symbolique vers les objectifs de contrôle. Ces problèmes peuvent parfois être résolus en adaptant les paramètres de discrétisation du système dans la [[The Mighty Pepper#1. Abstraction symbolique d'un system complexe#1.1 Méthodologie d'abstraction symbolique#1.1.1 Discrétisation de l'état et de la commande|section 1.1.1]] (voir remarque 1 et section 4.1.1).

>[!info] A retenir
>- Un modèle symbolique est un système dynamique dont les ensembles d'états $\Xi$ et de commandes $\Sigma$ sont des ensembles finis, et qui représente une *abstraction* d'un système dynamique continu.
>- Un modèle symbolique peut être calculé par partition de l'espace des états $\mathbb{X}$, discrétisation de l'espace des commandes $\mathbb{U}$, et **surapproximation** des ensembles atteignables $\mathbb{Y}_{\xi,\sigma}$ .
>- Un contrôleur symbolique peut être *concretisé* de manière à ce que l'ensemble des trajectoires du système continu en boucle fermée soit inclus dans l'ensemble des trajectoires du modèle symbolique en boucle fermée.
# 2. Synthèse d'un contrôleur symbolique ^section2