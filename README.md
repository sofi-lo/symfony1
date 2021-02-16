# formation dev fullstack

    https://github.com/form2021/formation

## liveshare

    jeudi 11/02

https://prod.liveshare.vsengsaas.visualstudio.com/join?1C8C4471E6B72B3ED303526D4EE12A4B64D9

## questions ??

## bases de symfony: créer un site de quelques pages

    Accueil         /
    Galerie         /galerie
    Contact         /contact

    ouvrir un terminal dans le dossier symfony/
    php bin/console make:controller

    Choose a name for your controller class (e.g. BravePizzaController):
    > Site

    created: src/Controller/SiteController.php
    created: templates/site/index.html.twig


    Success!


    ENSUITE, ON CREE LES ROUTES POUR CHAQUE PAGE...
    ET AUSSI LES TEMPLATES TWIG POUR CHAQUE PAGE...

```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class SiteController extends AbstractController
{
    #[Route('/', name: 'index')]
    public function index(): Response
    {
        return $this->render('site/index.html.twig', [
            'controller_name' => 'SiteController',
        ]);
    }

    #[Route('/galerie', name: 'galerie')]
    public function galerie(): Response
    {
        return $this->render('site/galerie.html.twig', [
            'controller_name' => 'SiteController',
        ]);
    }

    #[Route('/contact', name: 'contact')]
    public function contact(): Response
    {
        return $this->render('site/contact.html.twig', [
            'controller_name' => 'SiteController',
        ]);
    }

}

```

### CREER DES LIENS VERS LES ROUTES DANS TWIG


    https://symfony.com/doc/current/templates.html#linking-to-pages

    https://symfony.com/doc/current/reference/twig_reference.html#path

```twig

        <nav>
            <a href="{{ path('index') }}">accueil</a>
            <a href="{{ path('galerie') }}">galerie</a>
            <a href="{{ path('contact') }}">contact</a>
        </nav>

```

## CREER DES URLS POUR LES FICHIERS CSS, JS, IMAGES, etc...

    https://symfony.com/doc/current/templates.html#linking-to-css-javascript-and-image-assets

```twig
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mon Projet Symfony</title>
    <link rel="stylesheet" href="{{ asset('css/style.css') }}">
</head>
<body>
    <header>
        <nav>
            <a href="{{ path('index') }}">accueil</a>
            <a href="{{ path('galerie') }}">galerie</a>
            <a href="{{ path('contact') }}">contact</a>
        </nav>
    </header>
    <main>
        <section>
            <h1>MON TITRE1</h1>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Nisi, numquam in, mollitia culpa quia nostrum eius amet modi ipsam minus inventore assumenda eum ipsum voluptates, totam quibusdam similique consequatur expedita?</p>
            <img src="{{ asset('images/photo1.jpg') }}" alt="photo1">
        </section>
    </main>
    <footer>
        <p>tous droits réservés</p>
    </footer>
    <script src="{{ asset('js/script.js') }}"></script>
</body>
</html>

```


    PAUSE ET REPRISE A 11H15...


## EXO EN AUTONOMIE (60 MINUTES)

    AJOUTER DES BLOCS DANS LE TEMPLATE PARENT
    ET REMPLIR LES BLOCS DANS LES TEMPLATES ENFANTS

    OBJECTIF: ARRIVER A CONSTRUIRE UN SITE AVEC DU CONTENU DIFFERENT SUR LES 3 PAGES...

    NE PAS HESITER A POSER DES QUESTIONS...

## BASE DE DONNEES ET SYMFONY

    https://symfony.com/doc/current/doctrine.html

    https://www.doctrine-project.org/

    * VIDEOS TUTOS (CERTAINES PAYANTES...)
    https://symfonycasts.com/screencast/symfony-doctrine

    SYMFONY UTILISE LE CODE DU PROJET DOCTRINE POUR GERER LA PARTIE AVEC LA DATABASE...


    AJOUTER LA LIGNE DE CONFIG DANS LE FICHIER .env

```
###> doctrine/doctrine-bundle ###
# Format described at https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/configuration.html#connecting-using-a-url
# IMPORTANT: You MUST configure your server version, either here or in config/packages/doctrine.yaml
#
# DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
# DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name?serverVersion=5.7"
# DATABASE_URL="postgresql://db_user:db_password@127.0.0.1:5432/db_name?serverVersion=13&charset=utf8"
# AVEC VOTRE SERVEUR LARAGON
# DATABASE_URL="mysql://root:@localhost:3306/symfony?serverVersion=5.7"
# DANS MON CAS 
DATABASE_URL="mysql://root:@localhost:3306/symfony?serverVersion=mariadb-10.4.17"
###< doctrine/doctrine-bundle ###

```
    ET ENSUITE DANS LE TERMINAL LANCER LA COMMANDE

    php bin/console doctrine:database:create

    ET SI TOUT SE PASSE BIEN, ON PEUT VERIFIER AVEC PHPMYADMIN QUE LA DATABASE EST CREEE...

    Created database `symfony` for connection named default


    PAUSE ET REPRISE A 14H...


## AJOUTER UNE TABLE SQL POUR ENREGISTRER LES INSCRIPTIONS A UNE NEWSLETTER

    Table SQL newsletter
        id                  INT             INDEX=PRIMARY   A_I
        nom                 VARCHAR(255)
        email               VARCHAR(255)
        date_inscription    DATETIME


    https://symfony.com/doc/current/doctrine.html#creating-an-entity-class

    DANS SYMFONY, ON PASSE PAR UNE LIGNE DE COMMANDE QUI VA CREER UNE CLASSE PHP
    ET ENSUITE, D'AUTRES LIGNES DE COMMANDES VONT CREER LA TABLE SQL...

    ON AURA UNE CLASSE Newsletter
    => ON APPELLE ENTITE/ENTITY UNE CLASSE QUI EST RELIEE A UNE TABLE SQL 
        (PERSISTENCE...)

    ET DANS NOTRE CLASSE, ON AURA DES PROPRIETES 
    => CES PROPRIETES VONT DEVENIR DES COLONNES DANS NOTRE TABLE SQL
        (ORM Object Relationship Mapping)

    EN PHP, ON A                EN SQL, ON A
    UNE CLASSE Newsletter       UNE TABLE newsletter
    UNE PROPRIETE               UNE COLONNE
        id                          id
        nom                         nom
        email                       email
        dateInscription             date_inscription


    php bin/console make:entity

    => ASSISTANT POUR CREER LA CLASSE ET LES PROPRIETES

    ENSUITE QUAND ON EST BON SUR LA CLASSE ENTITE Newsletter.php
    ON PEUT LANCER LA COMMANDE SUIVANTE...

    php bin/console make:migration

    => CREE UN FICHIER DANS LE DOSSIER migrations
    => IL Y A LE CODE SQL QUI PERMET DE CREER LA TABLE SQL

    ENSUITE, IL FAUT LANCER LA COMMANDE POUR EXECUTER LA REQUETE SQL

    php bin/console doctrine:migrations:migrate

## GENERER UN CRUD A PARTIR D'UNE ENTITE

    NOUVELLE VERSION DEPUIS 2018
    https://symfony.com/blog/new-and-improved-generators-for-makerbundle#added-a-new-make-crud-generator

    ON A UNE LIGNE DE COMMANDE QUI PERMET DE GENERER LE CODE POUR UN CRUD A PARTIR D'UNE ENTITE

    php bin/console make:crud

    The class name of the entity to create CRUD (e.g. BravePopsicle):
    > Newsletter
    Newsletter

    created: src/Controller/NewsletterController.php
    created: src/Form/NewsletterType.php
    created: templates/newsletter/_delete_form.html.twig
    created: templates/newsletter/_form.html.twig
    created: templates/newsletter/edit.html.twig 
    created: templates/newsletter/index.html.twig
    created: templates/newsletter/new.html.twig       
    created: templates/newsletter/show.html.twig      

            
    Success! 
            

    Next: Check your new CRUD by going to /newsletter/


    PAUSE ET REPRISE A 15H45...


    Create      => FORMULAIRE POUR AJOUTER UNE NOUVELLE LIGNE
    Read        => AFFICHAGE LISTE ET AFFICHAGE UNE SEULE LIGNE
    Update      => FORMULAIRE POUR MODIFIER UNE LIGNE EXISTANTE
    Delete      => FORMULAIRE POUR SUPPRIMER UNE LIGNE EXISTANTE


```php
// ON AJOUTERA LE PREFIXE /admin A TOUTES LES ROUTES OBTENUES AVEC LE make:crud
// => ON PREPARE LA PROTECTION POUR AUTORISER L'ACCES SEULEMENT LES ADMINISTRATEURS

#[Route('/admin/newsletter')]                                                        // PREFIXE COMMUN POUR LES URLS DANS LA CLASSE
class NewsletterController extends AbstractController
{
    #[Route('/', name: 'newsletter_index', methods: ['GET'])]                       // URL DANS LE NAVIAGATEUR /admin/newsletter/
    public function index(NewsletterRepository $newsletterRepository): Response
    {
    }

    #[Route('/new', name: 'newsletter_new', methods: ['GET', 'POST'])]              // URL DANS LE NAVIGATEUR /admin/newsletter/new
    public function new(Request $request): Response
    {
    }

}
```


    LES METHODES CONTROLLER SONT RELIEES A DES TEMPLATES TWIG
    QUI HERITENT DE base.html.twig

    => ATTENTION AU CODE DANS base.html.twig
        IL FAUT GARDER LES BLOCS title ET body


## ACTIVATION DU MODE BOOTSTRAP POUR LA PARTIE ADMIN

    https://symfony.com/doc/current/form/bootstrap4.html

    MODIFIER LE FICHIER config/packages/twig.yaml

```yaml

twig:
    default_path: '%kernel.project_dir%/templates'
    form_themes: ['bootstrap_4_layout.html.twig']

```

    ET ENSUITE COMPLETER base.html.twig POUR CHARGER LE CODE DE BOOTSTRAP

    https://getbootstrap.com/docs/4.6/getting-started/introduction/


```twig
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>{% block title %}Welcome!{% endblock %}</title>
        {# Run `composer require symfony/webpack-encore-bundle`
           and uncomment the following Encore helpers to start using Symfony UX #}

        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css" integrity="sha384-B0vP5xmATw1+K9KRQjQERJvTumQW0nPEzvF6L/Z6nronJ3oUOFUFpCjEUQouq2+l" crossorigin="anonymous">
   
        {% block stylesheets %}
            {#{{ encore_entry_link_tags('app') }}#}
        {% endblock %}
        
    </head>
    <body>
        <div class="container">
            {% block body %}{% endblock %}
        </div>

        <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-Piv4xVNRyMGpqkS2by6br4gNJ7DXjqk09RmUpJ8jgGtD7zP9yug3goQfGII0yAns" crossorigin="anonymous"></script>

        {% block javascripts %}
            {#{{ encore_entry_script_tags('app') }}#}
        {% endblock %}
    </body>
</html>

```

## LIER SON DOSSIER GIT AVEC UN REPO SUR GITHUB.COM

    IL FAUT UN COMPTE GITHUB.COM
    ET ENSUITE CREER UN REPO VIDE SUR GITHUB.COM

    ENFIN ON VA CONNECTER NOTRE DOSSIER GIT AVEC LE REPO GITHUB.COM

    ET DANS VSCODE, ON PEUT AJOUTER LES LIGNES DE COMMANDE 
    POUR LIER NOTRE DOSSIER AVEC LE REPOSITORY GITHUB.COM

    git remote add origin https://github.com/form2021/symfony.git
    git branch -M main
    git push -u origin main

    ______________________________________________________


# formation dev fullstack

    https://github.com/form2021/formation

## liveshare

    lundi 15/02

https://prod.liveshare.vsengsaas.visualstudio.com/join?B41D3B9E942814ABD44E222C1CE311B08097

## questions ??

## GESTION DES UTILISATEURS ET SECURITE DANS SYMFONY

    https://symfony.com/doc/current/security.html

    https://symfony.com/doc/current/security.html#a-create-your-user-class

    LANCER DANS LE TERMINAL (DANS LE DOSSIER symfony/)

    php bin/console make:user

    ON A UNE BASE DE CODE POUR L'ENTITE User
    MAIS IL MANQUE DES PROPRIETES

    email           string(255)      VARCHAR(255)
    dateCreation    datetime         DATETIME
    ...

    LANCER LA COMMANDE POUR COMPLETER LES PROPRIETES...

    php bin/console make:entity

    SE POSER DES QUESTIONS SUR LE RGPD ET LA LEGALITE DES INFOS SUR LES UTILISATEURS...

    * CONNECTER NOTRE ENTITE AVEC LE SYSTEME DE SECURITE DE SYMFONY

    https://symfony.com/doc/current/security/form_login_setup.html#generating-the-login-form


    * ON VA LANCER LA COMMANDE 

    php bin/console make:registration-form

    CA VA GENERER LE CODE...
    https://symfonycasts.com/screencast/symfony-forms/registration-form


    LE SITE A CASSE CAR IL ME MANQUE UN BUNDLE POUR L'ENVOI D'EMAIL DE CONFIRMATION

    DANS LE TERMINAL (ET DANS LE DOSSIER syfmony/)

    composer require symfonycasts/verify-email-bundle

    SI ON ESSAIE D'ALLER SUR LA PAGE /register POUR CREER UN COMPTE
    ON A UNE ERREUR SUR LA CONFIG MAILER_DSN

    https://symfony.com/doc/current/mailer.html


    PAUSE ET REPRISE A 11H25...

    LA PAGE /register S'AFFICHE MAIS ON N'A PAS LA TABLE SQL

    php bin/console make:migration

    php bin/console doctrine:migrations:migrate

    AJOUTER LE CHAMP email DANS LE FORMULAIRE

    AJOUTER LA DATE PAR DEFAUT POUR LA PROPRIETE dateCreation

    => ON A UN FORMULAIRE DE CREATION QUI FONCTIONNE

    ENSUITE VERIFIER LA PAGE /login

    => IL FAUT COMPLETER LE CODE PHP POUR REDIRIGER VERS LA BONNE PAGE

   PAUSE DEJEUNER ET REPRISE A 14H...

## PROTECTION DE LA PARTIE ADMIN

    RAJOUTER UNE LIGNE DANS LE FICHIER 
    config/packages/security.yaml

```yaml

    # Easy way to control access for large sections of your site
    # Note: Only the *first* access control that matches will be used
    access_control:
        - { path: ^/admin, roles: ROLE_ADMIN }
        # - { path: ^/admin, roles: ROLE_ADMIN }
        # - { path: ^/profile, roles: ROLE_USER }

```


## PROTEGER LES FORMULAIRES EN AJOUTANT DES CONTRAINTES

    * LISTE DES CONTRAINTES DISPONIBLES
    https://symfony.com/doc/current/reference/constraints.html

    * LIGNE UNIQUE
    https://symfony.com/doc/current/reference/constraints/UniqueEntity.html

    * PROPRIETE EMAIL
    https://symfony.com/doc/current/reference/constraints/Email.html

    ON PEUT AJOUTER DES CONTRAINTES SUR LES ENTITES AVEC DES ANNOTATIONS
    MAIS AUSSI DANS LES FORMULAIRES AVEC DU CODE PHP

```php
// ...
use Symfony\Bridge\Doctrine\Validator\Constraints\UniqueEntity;

// ...
use Symfony\Component\Validator\Constraints as Assert;


/**
 * @ORM\Entity(repositoryClass=UserRepository::class)
 * @UniqueEntity(fields={"username"}, message="There is already an account with this username")
 * @UniqueEntity(fields={"email"}, message="il y a déjà un compte avec cet email")
 */
class User implements UserInterface
{
    // ...
    /**
     * @ORM\Column(type="string", length=255)
     * @Assert\Email(
     *     message = "désolé '{{ value }}' n'est pas un email valide."
     * )
     */
    private $email;

    // ...

}

```


## PERSONNALISER LES FORMULAIRES AVEC TWIG

    * PLUSIEURS NIVEAUX DE DETAILS SONT DISPONIBLES 
        POUR PRENDRE LA MAIN SUR LE CODE HTML DES FORMULAIRES

    https://symfony.com/doc/current/form/form_customization.html


    PAUSE ET REPRISE A 15H50



## PROJET POUR LA SEMAINE


    SITE DE PETITES ANNONCES

    UN VISITEUR PEUT CREER UN COMPTE
    ENSUITE IL PEUT SE CONNECTER
    ET UNE FOIS CONNECTE, IL A ACCES A UN ESPACE MEMBRE
    DANS SON ESPACE MEMBRE, 
        IL PEUT CREER DES ANNONCES
        IL NE PEUT VOIR QUE SES ANNONCES
        IL NE PEUT MODIFIER QUE SES ANNONCES
        IL NE PEUT SUPPRIMER QUE SES ANNONCES

        RELATION ONE TO MANY ENTRE User ET Annonce
        (UNE ANNONCE EST CREEE PAR UN USER)
        (UN USER PEUT CREER PLUSIEURS ANNONCES)

        RELATION MANY TO MANY ENTRE Annonce ET Categorie
        UNE ANNONCE PEUT ETRE CLASSEE DANS PLUSIEURS CATEGORIES
        UNE CATEGORIE PEUT CONTENIR PLUSIEURS ANNONCES

    SUR LA PARTIE PUBLIQUE
        AJOUTER UNE PAGE QUI AFFICHE TOUTES LES ANNONCES
        ET CHAQUE ANNONCE A SA PROPRE PAGE

        AJOUTER UN MOTEUR DE RECHERCHE 
            POUR CHERCHER LES ANNONCES QUI CONTIENNENT UN MOT CLE

    DANS LA PARTIE ADMIN    (make:crud et compléter...)
        AJOUTER LA GESTION 
        DE TOUS LES USERS
        DE TOUS LES ANNONCES
        DE TOUTES LES CATEGORIES
        ...




    CREER LES ENTITES ET LE CRUD 
    (make:entity ET make:crud)

## ENTITE Categorie

    id
    label
    description

    
## ENTITE Annonce

    id
    titre
    contenu
    image
    datePublication

    UNE FOIS LE make:crud FAIT
    ON PEUT CREER DES ANNONCES DANS LA PARTIE /admin/annonce

    * AJOUTER DANS LA PARTIE PUBLIQUE UNE PAGE /annonces
        QUI VA AFFICHER LES ANNONCES POUR LES VISITEURS 

    RAJOUTER LES RELATIONS DANS UN 2E TEMPS
    user_id     => ONE TO MANY (relation avec User)


    NE PAS HESITER A POSER DES QUESTIONS...

    ________________________________________________________


    # formation dev fullstack

    https://github.com/form2021/formation

## liveshare

    mardi 16/02

https://prod.liveshare.vsengsaas.visualstudio.com/join?1385DE286D62F362A671381C52D65BAED945

## questions ??

## ENTITE Categorie

    id
    label
    description
    
## ENTITE Annonce

    id
    titre
    slug
    contenu
    image
    datePublication

    UNE FOIS LE make:crud FAIT
    ON PEUT CREER DES ANNONCES DANS LA PARTIE /admin/annonce

    * AJOUTER DANS LA PARTIE PUBLIQUE UNE PAGE /annonces
        QUI VA AFFICHER LES ANNONCES POUR LES VISITEURS 

    RAJOUTER LES RELATIONS DANS UN 2E TEMPS
    user_id     => ONE TO MANY (relation avec User)

    NE PAS HESITER A POSER DES QUESTIONS...


    PAUSE ET REPRISE A 11H15...


## AJOUT DE RELATIONS ENTRE ENTITES

    * DOCUMENTATION UN PEU PLUS DETAILLEE SUR LES ETAPES AVEC make:entity
    https://symfony.com/doc/current/doctrine/associations.html

    LANCER LA COMMANDE make:entity
    ET CREER UNE PROPRIETE
    ET CHOISIR COMME TYPE relation
    REPONDRE AUX QUESTIONS...

## TWIG POUR AFFICHER UN MENU DIFFERENT SI LE VISITEUR EST CONNECTE

    * OBTENIR LES INFOS SUR LE USER CONNECTE
    https://symfony.com/doc/current/security.html#fetch-the-user-in-a-template

    * OBTENIR LES DROITS SUR LE USER CONNECTE
    https://symfony.com/doc/current/security.html#fetch-the-user-in-a-template

    DANS TWIG SYMFONY CREE UNE VARIABLE app 
    QU'ON PEUT UTILISER POUR RETROUVER L'UTILISATEUR CONNECTE

```twig

        {% if app.user %}
            <div class="mb-3">
                Bienvenue {{ app.user.username }}, <a href="{{ path('app_logout') }}">Déconnexion</a>
            </div>
        {% else %}
            <nav>
                <a href="{{ path('app_register') }}">créez votre compte</a>
                <a href="{{ path('app_login') }}">connexion</a>
            </nav>
        {% endif %}


```

    PAUSE ET REPRISE A 14H05...


## OBTENIR LE USER CONNECTE DANS PHP

    * DANS UNE CLASSE ...Controller, ON A LE METHODE  $this->getUser();
        => SIMPLE

    https://symfony.com/doc/current/security.html#a-fetching-the-user-object

    * SI ON N'EST PAS DANS UNE CLASSE ...Controller
        => PLUS COMPLIQUE CAR ON DOIT PASSER PAR INJECTION DE DEPENDANCES

    https://symfony.com/doc/current/security.html#b-fetching-the-user-from-a-service


```php
// src/Service/ExampleService.php
// ...

use Symfony\Component\Security\Core\Security;

class ExampleService
{
    private $security;

    public function __construct(Security $security)
    {
        // Avoid calling getUser() in the constructor: auth may not
        // be complete yet. Instead, store the entire Security object.
        $this->security = $security;
    }

    public function someMethod()
    {
        // returns User object or null if not authenticated
        $user = $this->security->getUser();
    }
}

```


## TESTS AUTOMATISES AVEC PHPUNIT

    * A DECOUVRIR: COMMENT CREER DES TESTS AUTOMATISES DANS SYMFONY AVEC PHPUNIT
    https://symfony.com/doc/current/testing.html#your-first-functional-test


## PROTEGER ESPACE MEMBRE

    AJOUTER UNE REGLE DANS LE FICHIER config/packages/security.yaml
    ET AUSSI METTRE A JOUR LA REDIRECTION DANS src/Security/LoginFormAuthenticator.php


```yaml
    # Easy way to control access for large sections of your site
    # Note: Only the *first* access control that matches will be used
    access_control:
        - { path: ^/admin, roles: ROLE_ADMIN }
        - { path: ^/membre, roles: ROLE_USER }


```


```php

    public function onAuthenticationSuccess(Request $request, TokenInterface $token, string $providerKey)
    {
        if ($targetPath = $this->getTargetPath($request->getSession(), $providerKey)) {
            return new RedirectResponse($targetPath);
        }

        // IL FAUT RECUPERER L'UTILISATEUR CONNECTE
        // ET SUIVANT LE ROLE DE L'UTILISATEUR, ON LE REDIRIGE VERS L'ESPACE ADMIN OU MEMBRE
        // https://symfony.com/doc/current/security.html#b-fetching-the-user-from-a-service
        // https://symfony.com/doc/current/security.html#hierarchical-roles
        // $userConnecte = $this->security->getUser();
        // BAD
        // $isAdmin = in_array("ROLE_ADMIN", $userConnecte->getRoles());

        // GOOD
        $nomRouteRedirection = "index";
        if ($this->security->isGranted("ROLE_ADMIN")) {
            // redirection vers la page /admin
            $nomRouteRedirection = "admin";
        }
        elseif ($this->security->isGranted("ROLE_USER")) {
            // redirection vers la page /membre
            $nomRouteRedirection = "membre";
        }

        // For example : 
        // TODO: CHANGER LA REDIRECTION VERS UNE PAGE ESPACE MEMBRE
        // POUR LE MOMENT, ON REDIRIGE VERS LA PAGE D'ACCUEIL...
        return new RedirectResponse($this->urlGenerator->generate($nomRouteRedirection));
        // throw new \Exception('TODO: provide a valid redirect inside '.__FILE__);
    }


```

## AJOUTER UNB FORMULAIRE POUR CREER UNE ANNONCE


    DANS LA PAGE D'ESPACE MEMBRE
    AJOUTER UN FORMULAIRE POUR PERMETTRE A UN MEMBRE DE PUBLIER UNE ANNONCE

    BONUS:
    CREER LA PAGE /annonces DANS LA PARTIE PUBLIQUE POUR LES VISITEURS
    ET AFFICHER LA LISTE DES ANNONCES DANS CETTE PAGE

    CHECKPOINT A 15H...
    










































