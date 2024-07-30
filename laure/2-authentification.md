# L'authentification

## Étape 1 : le modèle User

Tu vas commencer par créer ton modèle User qui représentera tes utilisateurs.

Tous ses attributs sont privés, avec des getters et des setters. Tous ses
attributs sauf l'id, sont initialisés en les passant au constructeur.
L'id lui, a la valeur `null` par défaut.

La liste des attributs :

- id : ? int
- email : string
- password : string
- role : string

(tous les champs de ton User ne sont pas là mais ceux nécessaires pour l'authentification, si, nous verrons les autres lors de l'étape d'administration des utilisateurs)

Une fois que tu as fini de créer ta classe n'oublie pas de la rajouter à ton autoload :

```shell
composer dump-autoload
```

## Étape 2 : la base de données

(si tu veux être tranquille, créé une nouvelle base de données et mets son nom dans ton .env)

Dans ta base de données tu vas créer une table `users` qui va contenir les colonnes suivantes :

- id : primary key, autoincrémenté
- email : varchar(255), non null
- password: varchar(255), non null
- role: varchar(5), non null


## Étape 3 : le Manager

### Étape 3.1 :la classe et son constructeur

Crée un fichier `managers/UserManager.php`, dans lequel tu vas créer une classe `UserManager` avec le code suivant :

```php
<?php

class UserManager extends AbstractManager {

    public function __construct() {
        // J'appelle le constructeur de l'AbstractManager pour qu'il initialise la connexion à la DB
        parent::__construct();
    }
}
```

### Étape 3.2 : la méthode createUser

Dans ton `UserManager` tu vas rajouter et compléter la méthode suivante qui permet d'entrer un nouvel utilisateur dans la base de données.

🚨 Attention à bien respecter le prototype de la méthode, elle prend une classe User en paramètre et pas autre chose.

```php
public function createUser(User $user) : User
{
    
}
```

Dans cette méthode tu vas utiliser les informations contenues dans l'instance de classe `$user` reçue en paramètres pour créer un nouvel User dans la base de données.

Ne cherches pas à la tester pour le moment, nous verrons comment faire dans une étape suivante.

### Étape 3.3 : la méthode findUserByEmail

Dans ton `UserManager` tu vas rajouter et compléter la méthode suivante qui permet de trouver un utilisateur dans la base de données à partir de son email.

🚨 Attention à bien respecter le prototype de la méthode, elle retourne soit null, soit un User et prend une string en paramètres.

```php
public function findUserByEmail(string $email) : ? User
{
    
}
```

Dans cette méthode tu vas interroger la base de données pour savoir si il existe un utilisateur avec l'email passé en paramètres. Si oui retourne cet utilisateur, si non, retourne `null`.

Ne cherche pas à la tester pour le moment, nous verrons comment faire dans une étape suivante.


### Étape 3.4 : autoload

Une fois que tu as fini de créer ta classe n'oublie pas de la rajouter à ton autoload :

```shell
composer dump-autoload
```

## Étape 4 : le Router

Dans la méthode `handleRequest` de ton `Router` tu vas rajouter des conditions pour les routes que tu vas créer.

- si la `$route` vaut `inscription`
- si la `$route` vaut `check-inscription`
- si la `$route` vaut `connexion`
- si la `$route` vaut `check-connexion`
- si la `$route` vaut `deconnexion`

## Étape 5 : AuthController

### Étape 5.0 : modifier l'AbstractController

Dans la classe `AbstractController` tu vas rajouter une méthode pour rediriger l'utilisateur vers une route :

```php
protected function redirect(? string $route) : void 
{
    if($route !== null)
    {
        header(`Location: index.php?route=$route`);
    }
    else
    {
        header(`Location: index.php`);
    }
}
```

### Étape 5.1 : la classe et son constructeur

Crée un fichier `controllers/AuthController.php` et mets-y le code de base suivant :

```php
<?php

class AuthController extends AbstractController {

    private UserManager $um;
    
    public function __construct() {
        parent::__construct();
        $this->um = new UserManager();
    }
}
```

### Étape 5.2 : méthode register

Dans votre `AuthController`, rajoutez la méthode suivante :

```php
public function register() : void
{
    $this->render('front/register.html.twig', []);
}
```

### Étape 5.3 : méthode checkRegister

Pour l'instant nous n'avons pas encore de formulaire à disposition donc on va se contenter d'utiliser la méthode pour tester si notre UserManager fonctionne.

Dans ton `AuthController`, rajoute la méthode suivante :

```php
public function checkRegister() : void
{
    $password = password_hash("test", PASSWORD_BCRYPT);
    $user = new User("test@test.fr", $password, "USER");
    
    $this->um->createUser($user);
}
```

### Étape 5.4 : la méthode login

Dans votre `AuthController`, rajoutez la méthode suivante :

```php
public function login() : void
{
    $this->render('front/login.html.twig', []);
}
```

### Étape 5.5 : la méthode checkLogin

Pour l'instant nous n'avons pas encore de formulaire à disposition donc on va se contenter d'utiliser la méthode pour tester si notre UserManager fonctionne.

Dans ton `AuthController`, rajoute la méthode suivante :

```php
public function checkLogin() : void {

    $user = $this->um->findUserByEmail("test@test.fr");
    dump($user);
}
```

### Étape 5.6 : la méthode logout

```php
public function logout() : void
{

}
```

### Étape 5.7 : autoload

```shell
composer dump-autoload
```

## Étape 6 : session et sécurité

Pour pouvoir faire de l'authentification nous allons avoir besoin les sessions, à la fois pour la connexion et pour gérer la sécurité.

### Étape 6.1 : session_start

Dans ton index.php, démarre une session avant la gestion des routes.

### Étape 6.2 : CSRFTokenManager

Crée un fichier `services/CSRFTokenManager.php` et mets-y la classe suivante :

```php
<?php

class CSRFTokenManager
{
    public function generateCSRFToken() : string
    {
        $token = bin2hex(random_bytes(32));

        return $token;
    }

    public function validateCSRFToken($token) : bool
    {

        if (isset($_SESSION['csrf_token']) && hash_equals($_SESSION['csrf_token'], $token))
        {
            return true;
        }
        else
        {
            return false;
        }
    }
}
```

```shell
composer dump-autoload
```

### Étape 6.3 : générer le token

Pour générer un token CSRF que nous utiliserons pour la sécurité, il faut rajouter le code suivant dans l'`index.php` après le démarrage de la session.

```php
if(!isset($_SESSION["csrf_token"]))
{
    $tokenManager = new CSRFTokenManager();
    $token = $tokenManager->generateCSRFToken();

    $_SESSION["csrf_token"] = $token;
}
```

## Étape 7 : les templates

### Étape 7.1 : layout.html.twig

Dans les templates `templates/front/layout.html.twig` et `templates/admin/layout.html.twig` il faut rajouter une meta viewport :

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

puis il faut ajouter cette ligne dans le bloc `stylesheets` :

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
```

et celle-ci dans le bloc `javascript` :

```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
```

Rajoute également la classe `container-fluid` à la balise `<body>`.

### Étape 7.2 : register.html.twig

Le contenu du template `templates/front/register.html.twig` :

```html
<main class="container py-5">
    <form action="index.php?route=check-inscription" method="post">
        <input type="hidden" id="csrf_token" name="csrf_token" value="" />
        <fieldset>
            <label for="email" class="form-label">
                Email
            </label>
            <input type="email" name="email" id="email" required class="form-control"/>
        </fieldset>
        <fieldset>
            <label for="password" class="form-label">
                Mot de passe
            </label>
            <input type="password" name="password" id="password" required class="form-control"/>
        </fieldset>
        <fieldset>
            <label for="confirm_password" class="form-label">
                Confirmer le mot de passe
            </label>
            <input type="password" name="confirm_password" id="confirm_password" required class="form-control"/>
        </fieldset>
        <fieldset>
            <button type="submit" class="btn btn-primary">Créer un compte</button>
        </fieldset>
    </form>
</main>
```

### Étape 7.3 : login.html.twig

Le contenu du template `templates/front/login.html.twig` :

```html
<main class="container py-5">
    <form action="index.php?route=check-connexion" method="post">
        <input type="hidden" id="csrf_token" name="csrf_token" value="" />
        <fieldset>
            <label for="email" class="form-label">
                Email
            </label>
            <input type="email" name="email" id="email" required class="form-control"/>
        </fieldset>
        <fieldset>
            <label for="password" class="form-label">
                Mot de passe
            </label>
            <input type="password" name="password" id="password" required class="form-control"/>
        </fieldset>
        <fieldset>
            <button type="submit" class="btn btn-primary">Connexion</button>
        </fieldset>
    </form>
</main>
```

## Étape 8 : Routing et tests

### Étape 8.1 : un nouvel atribut pour le routeur

Il va falloir ajouter l'AuthController comme attribut du routeur, initialisé dans le constructeur.

### Étape 8.2 : appeler les bonnes méthodes

Remplissez les conditions créées à l'étape 4 avec les appels aux bonnes méthodes du `AuthController`.

### Étape 8.3 : tester

- `index.php?route=inscription` doit afficher le formulaire d'inscription
- `index.php?route=connexion` doit afficher le formulaire de connexion
- `index.php?route=check-inscription` créé l'utilisateur en dur dans la base de données
- `index.php?route=check-connexion` affiche l'utilisateur récupéré
- `index.php?route=deconnexion` ne fait rien


## Étape 9 : les méthodes d'authentification

Maintenant que nous avons des formulaires et des sessions en place, nous allons pouvoir remplacer le code qui servait simplement à tester les méthodes du UserManager.

### Étape 9.1 : checkRegister

`checkRegister` est la méthode qui doit vérifier ce qui a été envoyé par le formulaire d'inscription et l'utiliser pour créer un utilisateur en base de données.

Son comportement doit être le suivant :

- Elle vérifie que tous les champs du formulaire (email, password, confirm_password, csrf_token) sont bien présents. Si ce n'est pas le cas elle redirige vers la page d'inscription et affiche un message d'erreur.


- Elle utilise le CSRFTokenManager pour vérifier que le token reçu est le bon, si ça n'est pas le cas elle redirige vers la page d'inscription et affiche un message d'erreur.


- Elle utilise la méthode `findUserByEmail` du `UserManager` pour vérifier si un compte n'existe pas déjà avec l'email transmis. Si un compte existe déjà elle redirige vers la page d'inscription et affiche un message d'erreur.


- Elle vérifie que le mot de passe et la confirmation du mot de passe sont identiques, si ça n'est pas le cas elle renvoie vers la page d'inscription en affichant un message d'erreur.


- Optionnel : si dans ton cahier des charges tu as des règles particulières pour les mots de passe tu peux les vérifier ici et si elles ne sont pas remplies, rediriger vers la page d'inscription avec un message d'erreur.


- Si tout va bien elle chiffre le mot de passe, instancie un User avec le role `"USER"` puis appelle la méthode `createUser` du `UserManager` pour créer le nouvel utilisateur. Ensuite elle redirige vers la page de connexion.

### Étape 9.2 : checkLogin

`checkLogin` est la méthode qui doit vérifier ce qui a été envoyé par le formulaire d'inscription et l'utiliser pour créer un utilisateur en base de données.

Son comportement doit être le suivant :

- Elle vérifie que tous les champs du formulaire (email, password, csrf_token) sont bien présents. Si ce n'est pas le cas elle redirige vers la page de connexion et affiche un message d'erreur.


- Elle utilise le CSRFTokenManager pour vérifier que le token reçu est le bon, si ça n'est pas le cas elle redirige vers la page de connexion et affiche un message d'erreur.


- Elle utilise la méthode `findUserByEmail` du `UserManager` pour vérifier si le compte existe bien avec l'email transmis. S'il n'existe déjà elle redirige vers la page de connexion et affiche un message d'erreur.


- Elle vérifie que le mot de passe transmis en clair correspond bien au mot de passe chiffré de la base de données. Si ça ne correspond pas elle redirige vers la page de connexion et affiche un message d'erreur.

- Si tout va bien elle stocke en session l'id de l'user et son role et redirige vers la page d'accueil.


### Étape 9.3 : logout

Elle détruit la session et redirige vers la page d'accueil.