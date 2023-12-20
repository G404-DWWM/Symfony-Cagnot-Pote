# 🎼 Introduction Symfony

## 🎦 Présentation MVC

## 🎦 Live coding

Installation d'un projet Symfony. Explication de l'arborescence du framework.

### Lien symbolique à partir de public vers htdocs

```bash
ln -s public htdocs
```

### Installation du pack apache pour laragon

```bash
composer require symfony/apache-pack
```

Utilisation de la documentation pour créer un contrôleur et la view associée.

[Symfony Documentation](https://symfony.com/doc/current/index.html)

```bash
php bin/console make:controller
```

Utilisation du contrôleur pour faire un traitement PHP, démonstration du système de routing.

```php
<?php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class HomeController extends AbstractController
{
    #[Route('/', name: 'app_home')]
    public function index(): Response
    {
        $variable = 'Totoooooo';

        return $this->render('home/index.html.twig', [
            'controller_name' => 'HomeController',
            'variable' => $variable,
        ]);
    }
}
```

Syntaxe du langage twig et utilisation des variables injecté.

```twig
{% if variable is not null %}
    {{ variable }}
{% else %}
    Je n'existe pas !!!
{% endif %}
```

## 🛠 MINI-TP Maquette interactive

Pour bien prendre en main la manipulation du MVC dans Symfony, nous allons créer une "maquette" interactive.

### Installation du projet :

- Installer Symfony sur Devilbox grâce à la [documentation Symfony](https://symfony.com/doc/current/index.html)
- Faire un lien symbolique entre le dossier public et htdocs

### Création d'un controller à la main :

Pour toute création de nouvelles classes PHP, on utilisera généralement la console de Symfony qui embarque une boîte à outils complète pour nous aider à aller plus vite.

Avec cette commande vous pouvez créer des controller custom :
```bash
php bin/console make:controller
```

- Vous l'appellerez Home (il s'appellera HomeController dans le dossier des controller) et il vous sera utile pour le routage de la page d'accueil.
- Vérifiez que le chemin du template mène à `index.html.twig` du dossier `home`.

### 🎨 Intégration

1. Récupérer les pages HTML du projet ici : [payetonpote](https://gitlab.com/simplon-roanne/paiement-collaboratif)
2. Copier le dossier "assets" dans le dossier Symfony `public` (c'est la racine du serveur web).
3. Copier toute la page `index.html` et la coller dans le fichier `base.html.twig` tout en
conservant le code existant. On va en réutiliser une partie (les balises twig !)
4. Repérer l'endroit où termine le header et où commence le footer. Couper ce code et le
coller dans la partie "block body" du template `index.html.twig` dans le dossier `home`.
5. De retour dans `base.html.twig`, il nous reste à replacer les blocks "stylesheets", "title",
"body", et "javascripts". **ATTENTION !** pour les blocks stylesheets et javascripts, on les
place après les scripts déjà existants. C'est parce qu'ils seront utilisés sur toutes les
pages. De plus, rajoutez un "/" aux liens de script dans le footer et de style dans le header.
6. Attention à mettre `{% block stylesheets %}{% endblock %}` sous les lignes d'appel du style
CSS (et à ne pas les englober) dans le head et `{% block javascripts %}{% endblock %}` sous
les lignes d'appel du Javascript (et à ne pas les englober) dans le footer.

La racine du projet doit mener sur la page `index.html.twig` du dossier home que vous venez
d'intégrer.

## 🏆 Objectifs

- J'ai compris le chemin du code dans un environnement MVC.
- Je sais router les pages de mon projet dans un controller.

# 💰 TP d'apprentissage Payetonpote

Vous allez créer un projet de paiement collaboratif. Ce sera une preuve de concept pour un site de type leetchi, mais pourquoi pas aussi pour des paris entre potes, des courses communes, etc.

**Analyse des besoins :**

- **En tant qu'utilisateur**, je crée une campagne de financement collaborative avec les champs suivants :
  - *Nom de la campagne* : 120 caractères maximum
  - *Contenu de la campagne* : texte libre via [éditeur markdown](https://simplemde.com/)
  - *Objectif de cagnotte* : nombre qui servira de simple indicateur
  - *Nom*
  - *Email*

- **En tant qu'utilisateur**, j'accède à une campagne partagée par l'auteur de la campagne via URL sécurisée et aléatoire.

- **En tant que participant**, je peux modifier le contenu de la campagne et le budget, mais pas le titre.

- **En tant que participant**, je peux payer n'importe quel montant. En plus de ma carte, je suis invité à saisir mon email. Le paiement est ensuite comptabilisé dans la cagnotte et visible dans une liste.

**Note importante :** Il y aura une limitation majeure : le destinataire des paiements ne sera pas dynamique : ce sera une seule personne pour toute l'application. Après avoir réussi à terminer ce projet, il serait possible de le continuer avec un système qui relie les créateurs de campagnes avec leur compte bancaire. Mais nous n'irons pas jusque-là dans un premier temps.

**Lexique :**

- **Utilisateur :** Un visiteur ayant accès à la page d'accueil.

- **Participant :** Un utilisateur qui participe à une campagne. Participer à une campagne signifie avoir payé ou enregistré une dépense.

- **Campagne :** Un projet de financement créé dans l'application.

- **Cagnotte :** La tirelire commune calculée en fonction des paiements et dépenses de chacun.

- **Objectif de cagnotte :** Nombre indicatif qui n'a aucun effet.

- **Dépenses :** Liste composée de montants en € associés à des participants.

# ✨ Backend Symfony

Vous avez commencé l’intégration du front-end de l’application dans le mini-tp de la maquette, nous allons maintenant développer les fonctionnalités dans le backend PHP.

## 👉 Designer la base de données

Voici le code de la BDD : [[SQL] payetonpote - Pastebin.com.](https://pastebin.com/NMihQkN5)
Reliez votre bdd en modifiant le .env du projet symfony.
Prenez le temps d'étudier l'architecture de la BDD. Essayez de visualiser l'interaction entre les tables ainsi que leurs contenus pour le projet.

## 🧙‍♂️ Création des Entités

Deux commandes vont suffire à générer les modèles du projet, appelés Entity sur Symfony. Magique !
La première commande permet de générer les classes Entity. La dernière commande permet de générer les getter et setters.
Il faut les utiliser dans la console du projet en ouvrant la console directement sur VS Code, par exemple.

```bash
php bin/console doctrine:mapping:import App\Entity annotation --path=src/Entity
php bin/console make:entity --regenerate App
```

## 🎛 Création des controllers

Vous pensez que Symfony, c'est déjà magique ? Ce n'est pas fini ! Nous allons désormais créer en une seule ligne de commande le Controller d'un Model, toutes les principales fonctions (C.R.U.D. Create, Read, Update, Delete) ainsi que les Views (templates) associés à ces méthodes !

```bash
php bin/console make:crud
```

La console vous demandera l’entité souhaitée, vous choisirez Campaign. Et vous
nommerez le controller associé avec le même nom.

Prenez le temps d'étudier tout le code que vous avez généré... c'est bon, vous avez tout
compris ? Bien, il est temps de passer à l'intégration !

# 🎨 Intégration en Symfony

Dans cette étape, nous allons profiter du `CampaignController` que vous venez de créer
pour finaliser les routes et le rendu de toutes les pages. Le travail ici consiste à
remplacer le code généré de symfony par le code HTML fournis dans les 4 pages du TP.

Vous devrez ajouter une page `payment.html.twig` dans le dossier templates/campaign
et le controller `PaymentController` dans le dossier Controller.

## Les Formulaires Symfony

### Une chose importante à savoir : Les formulaires de Symfony sont eux aussi des objets à part entière !

Lors de la création du CRUD de campaign, un fichier `CampaignType.php` à été créé dans le
dossier Form de `src`.

Lors de l'affichage de la view `new.html.twig`, une variable contenant le formulaire a été envoyé
depuis le controller !

```php
    #[Route('/new', name: 'app_campaign_new', methods: ['GET', 'POST'])]
    public function new(Request $request, EntityManagerInterface $entityManager): Response
    {
        $campaign = new Campaign();
        $form = $this->createForm(CampaignType::class, $campaign); // ICI
        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            $entityManager->persist($campaign);
            $entityManager->flush();
            return $this->redirectToRoute('app_campaign_index', [], Response::HTTP_SEE_OTHER);
        }

        return $this->render('campaign/new.html.twig', [
            'campaign' => $campaign,
            'form' => $form, // ET LA
        ]);
    }
```

Cette variable contient une instance de CampaignType fait avec la méthode de symfony `createForm()`.

Cette variable est utilisée ici, dans `_form.html.twig`

```twig
{% block body %}
    <h1>Create new Campaign</h1>
    {{ include('campaign/_form.html.twig') }}  
    <a href="{{ path('app_campaign_index') }}">back to list</a>
{% endblock %} 
```

Le formulaire ressemble à ceci pour l'instant :

```twig
{{ form_start(form) }}
    {{ form_widget(form) }}
    <button class="btn">{{ button_label|default('Save') }}</button>
{{ form_end(form) }}
```

Il va falloir y mettre celui qui est fourni dans les pages html de la maquette

CampaignType a été généré automatiquement pour remplir toutes les colonnes de votre table. il
va falloir le modifier pour enlever les champs que l'on ne souhaite pas remplir comme
`updated_at` et `created_at`.

```php
public function buildForm(FormBuilderInterface $builder, array $options): void
{
    $builder
        ->add('title')
        ->add('content')
        ->add('createdAt') // A RETIRER
        ->add('updatedAt') // A RETIRER
        ->add('goal')
        ->add('name')
    ;
}
```

Ce "Formbuilder" est la pour générer un objet à partir des champs du formulaire, ce qui nous
permettra de les valider ou de spécifier des caractéristiques comme des longueurs maximales de
champs
À savoir que pour l'affichage dans les templates, nous ne sommes pas obligés d'utiliser la syntax
{{ form_start(form) }}

### À retenir :

Il est indispensable d'utiliser la gestion des formulaires de symfony, mais vous pouvez les écrire
en html classique.

# 🕵️‍♂️ Le code Métier

Les bases du projet sont prêtes, nous allons maintenant développer les fonctionnalités !

## Modification de l'Entity Campaign

L'enregistrement de la campagne provoquera une erreur tant qu'on n'indique pas à
Symfony que :
- On souhaite nous même assigner la propriété ID (au lieu de l'auto-increment habituellement utilisé)
- La colonne `id` ne possède pas de setter donc nous devrons le rajouter manuellement dans le fichier Entity\Campaign.php

```php
/**
* @var string
* @ORM\Column(name="id", type="string", length=32, nullable=false)
* @ORM\Id
* @ORM\GeneratedValue(strategy="NONE")
*/
private $id;

public function setId(string $id): self
{
    $id = md5(random_bytes(50));
    $this->id = $id;
    return $this;
}
```

## Modification de CampaignController et de la méthode new

Dans le if de cette méthode, nous allons ajouter le setId de campaign :

```php
if ($form->isSubmitted() && $form->isValid()) {
    $campaign->setId(); //il faut définir l'id de la campagne !
    $entityManager->persist($campaign);
    $entityManager->flush();

    return $this->redirectToRoute('app_campaign_index', [], Response::HTTP_SEE_OTHER);
}
```

### Une chose importante à savoir : Les formulaires de symfony sont eux aussi des objets à part entière !

# 🎮 A vous de jouer

## Ajout du formulaire de paiement

Le code html du formulaire de paiement d'une campagne se trouve dans les maquettes du TP.
vous devrez créer une page `payment.html.twig` qui contiendra ce formulaire.

## 🤷 Création de la fonction new dans PaymentController

Cette fonction devra se faire en plusieurs étapes :
- Récupération du montant du paiement.
- Instanciation la campagne.
- Enregistrement du participant.
- Enregistrement du paiement.
- Redirection vers le show de la campagne.

## 🏄 Dynamiser les templates

Maintenant que vous savez enregistrer un paiement, il va falloir changer toutes les données en
dure dans les templates :
- La barre de progression doit correspondre au pourcentage de complétion de l'objectif de la campagne.
- Le nombre de participants.
- Le montant total récolté pour une campagne.
- La liste des participants avec leur participation.

## 🕵 Fonctionnalités d'anonymat

Les utilisateurs du projet doivent désormais avoir la possibilité de ne pas afficher leur identité ou
le montant de leur paiement s'ils cochent les champs correspondants
- Ces ajouts de fonctionnalités impliquent de faire des modifications dans la base de données.

## 💸 Fonctionnalités de paiement

- Il n'est pas possible d'enregistrer un paiement négatif
- Si un paiement dépasse le goal de la campagne, il n'est plus possible de participer