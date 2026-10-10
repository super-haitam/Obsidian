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
$$ \forall \sigma \in \Sigma,\ p(\sigma) = u_\sigma $$ ![[Pasted image 20261010181731.png]]
*Figure 1 - Aperçu de l'approche symbolique pour le contrôle*
### 1.1.2 Approximation de la dynamique
Les ensembles d'états et de commandes symboliques $\Xi$ et $\Sigma$ étant maintenant proprement définis, il nous reste a spécifier la fonction de transition $g$. Pour un état et d'une commande symbolique $\xi \in \Xi,\ \sigma \in \Sigma,\ g(\xi, \sigma)$ dénote l'ensemble des états symboliques vers lesquels le modelé symbolique peut évoluer.
	**Définition.** Un contrôleur est, d'après ma compréhension, la fonction qui donne, d'après l'état $x$ ( ou symbolique $\xi$ ) a l'instant $t$, la commande $u$ ( ou symbolique $\sigma$ ) qui permet d'aller au prochain état ( a travers la fonction $f$ ou $g$ suivant le modèle dynamique/symbolique ), tout en respectant les spécifications indiquées.

Pour que le modèle symbolique soit utilisable pour synthétiser des contrôleurs pour le system décrit dans [[The Mighty Pepper#^1|(1)]], il faut que la dynamique symbolique capture l'ensemble des comportements possible de [[The Mighty Pepper#^1|(1)]].
	Cela veut seulement dire qu'on doit avoir une bijection entre le modèle dynamique et symbolique concernant les états et commandes.

>[!note] Definition (3)
> Pour ce faire, il est nécessaire de calculer pour chaque $\xi \in \Xi,\ \sigma \in \Sigma$ un ensemble $\mathbb{Y}_{\xi,\sigma} \subseteq \mathbb{R}^{n_x}$ vérifiant l'inclusion ci-dessous:
> $$ \mathbb{Y}_{\xi,\sigma} \supseteq f(cl(\mathbb{X}_\xi),u_\sigma,\mathbb{W}) := \{f(x, u_\sigma, w)\ |\ x \in cl(\mathbb{X}_\xi),\ w \in \mathbb{W}\} \tag{3}$$
ou $cl(\mathbb{X}_{\xi,\sigma})$ denote l'adherence de l'ensemble $\mathbb{X}_\xi$ . 
>
>Ainsi, $\mathbb{Y}_{\xi,\sigma}$ est un ensemble contenant l'ensemble des états vers lequel [[The Mighty Pepper#^1|(1)]] peut evaluer depuis un etat $x \in cl(\mathbb{X}_\xi)$, sour l'action de la commande $u_\sigma$ et l'effet d'une perturbation $w \in \mathbb{W}$ . 

>Le calcul des ensembles $\mathbb{Y}_{\xi,\sigma}$ représente le _principal point de difficulté_ dans le calcul de modèle symbolique ; plusieurs méthodes ont été développées a cet effet [[The Mighty Pepper#1. Abstraction symbolique d'un system complexe#1.1 Méthodologie d'abstraction symbolique#1.1.3 Analyse d'atteignabilité|(section 1.1.3)]].

Ensuite, la dynamique du modele symbolique peut etre definie de la maniere suivante:
$$g(\xi,\sigma)=\{\xi^{+} \in \Xi\ |\ \mathbb{Y}_{\xi,\sigma} \cap cl(\mathbb{X}_{\xi^{+}}) \neq \emptyset \}.$$

### 1.1.3 Analyse d'atteignabilité
