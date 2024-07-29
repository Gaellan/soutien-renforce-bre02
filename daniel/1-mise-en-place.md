# Mise en place du projet

## Étape 1 : le repository

Créez un repository GitHub public, 
avec un README et appelez-le `bre02-soutien-projet`.

Clonez ce repository :

```shell
git clone url-de-votre-repository
```

Puis placez-vous dans son dossier

```shell
cd nom-du-repo
```

## Étape 2 : Les dossiers

Il va falloir créer toute l'architecture des dossiers 
et fichiers comme suit :

- assets
  - styles
    - css
      - style.css
    - sass
      - style.scss
  - js 
    - main.js 
- controllers
  - AbstractController.php
- managers
  - AbstractManager.php 
- models
  - User.php
- services
  - Router.php 
- templates
  - front
    - layout.html.twig 
  - admin
    - layout.html.twig 
- index.php


## Étape 3 : Installer les librairies avec composer

Nous allons devoir installer plusieurs librairies pour 
nous aider à travailler.

Twig, pour avoir un moteur de template

DotEnv pour pouvoir charger un fichier de configuration

VarDumper pour utiliser la commande dump qui fait 
des var_dumps plus jolis et précis.

Pour ajouter ces librairies, faites les commandes 
suivantes dans votre terminal à la racine de votre 
dossier

```shell
composer require vlucas/phpdotenv
```

Si vous êtes sur l'IDE de la 3WA

```shell
composer require "twig/twig:^3.0"
```

sinon

```shell
composer require twig/twig
```

et enfin

```shell
composer require --dev symfony/var-dumper
```


## Étape 4 : configurer l'autoload

Vous allez modifier votre fichier `composer.json` pour le
faire ressembler à celui-ci (attention à garder les 
infos de votre fichier pour la ligne concernant twig)

```json
{
    "autoload": {
        "classmap": [
            "controllers/",
            "managers/",
            "models/",
            "services/"
        ]
    },
    "require": {
        "vlucas/phpdotenv": "^5.6",
        "twig/twig": "^3.0"
    },
    "require-dev": {
        "symfony/var-dumper": "^7.0"
    }
}
```

Une fois que ce fichier est modifier vous n'aurez qu'à faire
la commande suivante pour ajouter une classe à votre autoload :

```shell
composer dump-autoload
```


## Étape 5 : préparer le .env

Créez un fichier `.env.example` à la racine de votre projet, et mettez y le contenu suivant :

```txt
# Database info
DB_NAME="databasename"
DB_USER="username"
DB_PASSWORD="password"
DB_CHARSET="utf8"
DB_HOST="db.3wa.io"
```

ensuite faites une copie de ce fichier que vous appelez
`.env` et ajoutez-y les bonnes informations de connexion
à votre base de données.


## Étape 6 : préparer l'index.php

Mettez le contenu suivant dans votre fichier `index.php`:

```php
<?php

// charge l'autoload de composer
require "vendor/autoload.php";

// charge le contenu du .env dans $_ENV
$dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
$dotenv->load();
```


## Étape 7 : le .gitignore

Créez un fichier `.gitignore` à la racine de votre projet
et mettez-y le contenu suivant (copiez-collez le texte):

```txt
vendor/
.env
composer.lock
```

Ensuite faites les commandes suivantes à la 
racine du projet :

```sh
git add .gitignore
```
```shell
git commit -m "add gitignore"
```
```shell
git push
```

cela vous évitera de versionner les dossiers et fichiers
qui ne devraient pas être en ligne.


## Étape 8 : l'AbstractController

Voici le code de l'AbstractController qui permet de charger
Twig. Placez le dans le fichier 
`controllers/AbstractController.php` :

```php
abstract class AbstractController
{
    private \Twig\Environment $twig;
    public function __construct()
    {
        $loader = new \Twig\Loader\FilesystemLoader('templates');
        $twig = new \Twig\Environment($loader,[
            'debug' => true,
        ]);

        $twig->addExtension(new \Twig\Extension\DebugExtension());

        $this->twig = $twig;
    }

    protected function render(string $template, array $data) : void
    {
        echo $this->twig->render($template, $data);
    }
}
```

Puis faites la commande suivante dans le terminal : 

```shell
composer dump-autoload
```

## Étape 9 : l'AbstractManager

Voici le cde de votre AbstractManager qui se connecte 
à la base de données avec les informations de votre 
fichier `.env`. Placez le dans le fichier 
`managers/AbstractManager.php`:

```php
abstract class AbstractManager
{
    protected PDO $db;

    public function __construct()
    {
        $connexion = "mysql:host=".$_ENV["DB_HOST"].";port=3306;charset=".$_ENV["DB_CHARSET"].";dbname=".$_ENV["DB_NAME"];
        $this->db = new PDO(
            $connexion,
            $_ENV["DB_USER"],
            $_ENV["DB_PASSWORD"]
        );
    }
}
```

Puis faites la commande suivante dans le terminal :

```shell
composer dump-autoload
```

## Étape 10 : Un petit commit pour la route

Nous allons maintenant add, commit et push tout ça sur notre repo Github.

```shell
git add assets
```

```shell
git add controllers
```

```shell
git add managers
```

```shell
git add models
```

```shell
git add services
```

```shell
git add templates
```

```shell
git add index.php
```

```shell
git add .env.example
```

```shell
git commit -m "folders and files structure"
```

```shell
git push
```

## Étape 11 : Afficher une homepage pour tester

### Étape 11.1 : Router

On va créer un routeur et la route qui correspond à la page d'accueil :

Dans le fichier `services/Router.php` :

```php
<?php

class Router {

    public function handleRequest(? string $route) : void {
        if($route === null)
        {
            // le code si il n'y a pas de route ( === la page d'accueil)
        }
        else
        {
            // le code si c'est aucun des cas précédents ( === page 404)
        }
    }
}
```

### Étape 11.2 : index.php

Dans votre fichier index.php vous allez rajouter un 
comportement :

Créez une variable `$route` que vous initialisez comme 
valant `null`.

Ensuite si la variable `$_GET['route']` existe, 
donnez sa valeur à `$route`. Sinon ne faites rien.

Ensuite instanciez un routeur, et appelez sa méthode 
handleRequest en lui passant `$route`.


### Étape 11.3 : tester le tandem index.php + routeur

Dans votre routeur, vous allez rajouter un affichage de test dans le if : 

```php
echo "je dois afficher la page d'accueil";
```

et un autre dans le else

```php
echo "je dois afficher la page 404";
```

Ensuite testez en changeant l'URL dans votre navigateur :

`index.php` sans route doit vous envoyer la page d'accueil

toute autre url du site devrait vous envoyer la page 404


### Étape 11.4 : rajoutons un controller

Créez un fichier `controllers/DefaultController.php` et placez-y le code suivant.

```php
<?php

class DefaultController extends AbstractController
{
    public function __construct() {
        // j'appelle le constructeur de l'AbstractController pour lui permettre de charger Twig
        parent::__construct();
    }

    public function homepage() : void
    {
        $this->render('front/home.html.twig', []);
    }
    
    public function notFound() : void
    {
        $this->render('front/error404.html.twig', []);
    }
}
```

Faite ensuite la commande :

```shell
composer dump-autoload
```

### Étape 11.5 : les templates

#### front/layout.html.twig

Placez le code de base suivant dans le fichier `templates/front/layout.html.twig`.

```html
<!doctype html>
<html lang="fr">
    <head>
        <meta charset="utf-8">
        <title>
            {% block title %}
                | Nom du site
            {% endblock %}
        </title>
        {% block stylesheets %}
            <link rel="stylesheet" href="assets/styles/css/style.css" />
        {% endblock %}
    </head>
    <body>
        <header>

        </header>

        {% block main %}
        {% endblock %}

        <footer>

        </footer>
        {% block javascript %}
            <script type="application/javascript" src="/assets/js/main.js"></script>
        {% endblock %}
    </body>
</html>
```

#### front/home.html/twig

Créez un fichier `templates/front/home.html.twig`.

Dans ce fichier, vous allez étendre `front/layout.html.twig`. Puis ensuite
vous allez surcharger le bloc `main` pour y afficher : 

```html
<h1>Page d'accueil</h1>
```

#### front/error404.html/twig

Créez un fichier `templates/front/error404.html.twig`.

Dans ce fichier, vous allez étendre `front/layout.html.twig`. Puis ensuite
vous allez surcharger le bloc `main` pour y afficher :

```html
<h1>404 : Page introuvable</h1>
```

### Étape 11.6 : Appelez le controller dans le router

Vous allez rajouter un attribut dans votre routeur : il s'appelera `$dc` et sera une instance d'un `DefaultController`.

Rajoutez un constrcteur à votre Router et instanciez cet attribut dedans.

Ensuite dans la méthode `handleRequest`, appelez les bonnes méthodes du controller dans le `if` et dans le `else`.

Ensuite testez en changeant l'URL dans votre navigateur :

`index.php` sans route doit vous envoyer la page d'accueil

toute autre url du site devrait vous envoyer la page 404

### Étape 11.7 : Add/Commit/Push

N'oubliez pas de add / commit / push votre travail :)