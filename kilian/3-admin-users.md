# Administration des utilisateurs

## Étape 1 : Créer un espace admin

### Étape 1.1 : routeur

Dans le routeur on ajoute plusieurs conditions :

- si `$route` vaut `admin`
- si `$route` vaut `admin-connexion`
- si `$route` vaut `admin-check-connexion`
- si `$route` vaut `admin-create-user`
- si `$route` vaut `admin-check-create-user`
- si `$route` vaut `admin-edit-user`
- si `$route` vaut `admin-check-edit-user`
- si `$route` vaut `admin-delete-user`
- si `$route` vaut `admin-list-users`
- si `$route` vaut `admin-show-user`

### Étape 1.2 : les controllers

#### Étape 1.2.1 : AdminController

On crée un fichier `/controllers/AdminController.php` dans lequel on crée une classe `AdminController` :

```php
<?php

class AdminController extends AbstractController
{
    public function __construct(){
        parent::__construct();
    }

    public function home() : void {
        $this->render('admin/home.html.twig', []);
    }

    public function login() : void {
        $this->render('admin/login.html.twig', []);
    }

    public function checkLogin() : void {
        
    }
}
```

#### Étape 1.2.2 : UserController

On crée un fichier `/controllers/UserController.php` dans lequel on crée une classe `UserController` :

```php
<?php

class UserController extends AbstractController
{
    public function __construct(){
        parent::__construct();
    }

    public function create() : void {
        $this->render("admin/users/create.html.twig", []);
    }

    public function checkCreate() : void {

    }

    public function edit() : void {
        $this->render("admin/users/edit.html.twig", []);
    }

    public function checkEdit() : void {

    }

    public function delete() : void {

    }

    public function list() : void {
        $this->render("admin/users/list.html.twig", []);
    }

    public function show() : void {
        $this->render("admin/users/show.html.twig", []);
    }
}
```

#### Étape 1.2.3 : Autoload

```shell
composer dump-autoload
```
#### Étape 1.2.4 : Router

Dans le routeur, on fait correspondre les nouvelles conditions avec les nouvelles méthodes des `AdminController` et `UserController`.

Attention à bien ajouter les controllers en attribut du router :)

#### Étape 1.2.5 : commit

Et un petit add / commit / push pour être sûr-e de rien perdre !

### Étape 1.3 : les templates

#### Étape 1.3.1 : admin/layout.html.twig

```html
<!doctype html>
    <html lang="fr">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>
            {% block title %}
                Admin | Nom du site
            {% endblock %}
        </title>
        {% block stylesheets %}
            <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
            <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
            <link rel="stylesheet" href="assets/styles/css/style.css" />
        {% endblock %}
    </head>
    <body class="container-fluid p-0">
        <header class="navbar navbar-expand-lg bg-primary px-3" data-bs-theme="dark">
            <a class="navbar-brand" href="index.php?route=admin">Espace Admin</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <nav class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="index.php?route=admin">Admin</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="index.php?route=admin-list-users">Utilisateurs</a>
                    </li>
                </ul>
            </nav>
        </header>

        {% block main %}
        {% endblock %}

        {% block javascript %}
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
        <script type="application/javascript" src="/assets/js/main.js"></script>
        {% endblock %}
    </body>
</html>
```

#### Étape 1.3.2 : admin/home.html.twig

```html
{% extends 'admin/layout.html.twig' %}

{% block main %}
    <main class="container py-5">

    </main>
{% endblock %}
```

#### Étape 1.3.3 : admin/login.html.twig

```html
<main class="container py-5">
    <h1 class="mb-4">Connexion</h1>
    <form action="index.php?route=admin-check-login" method="post">
        <input type="hidden" id="csrf_token" name="csrf_token" value="{{ session.csrf_token }}" />
        <fieldset class="mb-2">
            <label for="email" class="form-label">
                Email
            </label>
            <input type="email" name="email" id="email" required class="form-control"/>
        </fieldset>
        <fieldset class="mb-2">
            <label for="password" class="form-label">
                Mot de passe
            </label>
            <input type="password" name="password" id="password" required class="form-control"/>
        </fieldset>
        <fieldset>
            <button type="submit" class="btn btn-primary">Se connecter</button>
        </fieldset>
    </form>
</main>
```

#### Étape 1.3.4 : admin/users/create.html.twig

```html
{% extends 'admin/layout.html.twig' %}

{% block main %}
    <main class="container py-5">
        <h1 class="mb-4">Ajouter un utilisateur</h1>
        <form action="index.php?route=admin-check-create-user" method="post">
            <input type="hidden" id="csrf_token" name="csrf_token" value="{{ session.csrf_token }}" />
            <fieldset class="mb-2">
                <label for="email" class="form-label">
                    Email
                </label>
                <input type="email" name="email" id="email" required class="form-control"/>
            </fieldset>
            <fieldset class="mb-2">
                <label for="password" class="form-label">
                    Mot de passe
                </label>
                <input type="password" name="password" id="password" required class="form-control"/>
            </fieldset>
            <fieldset class="mb-2">
                <label for="confirm_password" class="form-label">
                    Confirmation du mot de passe
                </label>
                <input type="password" name="confirm_password" id="confirm_password" required class="form-control"/>
            </fieldset>
            <fieldset class="mb-2">
                <label for="role" class="form-label">
                    Rôle
                </label>
                <select class="form-select" id="role" name="role">
                    <option value="USER">User</option>
                    <option value="ADMIN">Admin</option>
                </select>
            </fieldset>
            <fieldset>
                <button type="submit" class="btn btn-primary">Ajouter l'utilisateur</button>
            </fieldset>
        </form>
    </main>
{% endblock %}
```

#### Étape 1.3.5 : admin/users/edit.html.twig

```html
{% extends 'admin/layout.html.twig' %}

{% block main %}
<main class="container py-5">
    <h1 class="mb-4">Ajouter un utilisateur</h1>
    <form action="index.php?route=admin-check-edit-user" method="post">
        <input type="hidden" id="csrf_token" name="csrf_token" value="{{ session.csrf_token }}" />
        <fieldset class="mb-2">
            <label for="email" class="form-label">
                Email
            </label>
            <input type="email" name="email" id="email" required class="form-control" value="mari@test.fr"/>
        </fieldset>
        <fieldset class="mb-2">
            <label for="password" class="form-label">
                Mot de passe
            </label>
            <input type="password" name="password" id="password" required class="form-control"/>
        </fieldset>
        <fieldset class="mb-2">
            <label for="confirm_password" class="form-label">
                Confirmation du mot de passe
            </label>
            <input type="password" name="confirm_password" id="confirm_password" required class="form-control"/>
        </fieldset>
        <fieldset class="mb-2">
            <label for="role" class="form-label">
                Rôle
            </label>
            <select class="form-select" id="role" name="role">
                <option value="USER" selected>User</option>
                <option value="ADMIN">Admin</option>
            </select>
        </fieldset>
        <fieldset>
            <button type="submit" class="btn btn-primary">Modifier l'utilisateur</button>
        </fieldset>
    </form>
</main>
{% endblock %}
```

#### Étape 1.3.6 : admin/users/list.html.twig

```html
{% extends 'admin/layout.html.twig' %}

{% block main %}
    <main class="container py-5">
        <h1 class="mb-4">Liste des utilisateurs</h1>
        <a href="index.php?route=admin-create-user" class="btn btn-primary mb-4">Ajouter un utilisateur</a>
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>
                        #
                    </th>
                    <th>
                        Email
                    </th>
                    <th>
                        Role
                    </th>
                    <th>
                        Actions
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>
                        1
                    </td>
                    <td>
                        mari@test.fr
                    </td>
                    <td>
                        USER
                    </td>
                    <td>
                        <a class="btn btn-primary" href="index.php?route=admin-show-user" title="Voir l'utilisateur"><span class="bi bi-eye-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-success" href="index.php?route=admin-edit-user" title="Modifier l'utilisateur"><span class="bi bi-pencil-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-danger" href="index.php?route=admin-delete-user" title="Supprimer l'utilisateur"><span class="bi bi-trash-fill" aria-hidden="true"></span></a>
                    </td>
                </tr>
                <tr>
                    <td>
                        2
                    </td>
                    <td>
                        admin@test.fr
                    </td>
                    <td>
                        ADMIN
                    </td>
                    <td>
                        <a class="btn btn-primary" href="index.php?route=admin-show-user" title="Voir l'utilisateur"><span class="bi bi-eye-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-success" href="index.php?route=admin-edit-user" title="Modifier l'utilisateur"><span class="bi bi-pencil-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-danger" href="index.php?route=admin-delete-user" title="Supprimer l'utilisateur"><span class="bi bi-trash-fill" aria-hidden="true"></span></a>
                    </td>
                </tr>
                <tr>
                    <td>
                        3
                    </td>
                    <td>
                        toto@test.fr
                    </td>
                    <td>
                        USER
                    </td>
                    <td>
                        <a class="btn btn-primary" href="index.php?route=admin-show-user" title="Voir l'utilisateur"><span class="bi bi-eye-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-success" href="index.php?route=admin-edit-user" title="Modifier l'utilisateur"><span class="bi bi-pencil-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-danger" href="index.php?route=admin-delete-user" title="Supprimer l'utilisateur"><span class="bi bi-trash-fill" aria-hidden="true"></span></a>
                    </td>
                </tr>
                <tr>
                    <td>
                        4
                    </td>
                    <td>
                        titi@test.fr
                    </td>
                    <td>
                        ADMIN
                    </td>
                    <td>
                        <a class="btn btn-primary" href="index.php?route=admin-show-user" title="Voir l'utilisateur"><span class="bi bi-eye-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-success" href="index.php?route=admin-edit-user" title="Modifier l'utilisateur"><span class="bi bi-pencil-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-danger" href="index.php?route=admin-delete-user" title="Supprimer l'utilisateur"><span class="bi bi-trash-fill" aria-hidden="true"></span></a>
                    </td>
                </tr>
                <tr>
                    <td>
                        5
                    </td>
                    <td>
                        tutu@test.fr
                    </td>
                    <td>
                        USER
                    </td>
                    <td>
                        <a class="btn btn-primary" href="index.php?route=admin-show-user" title="Voir l'utilisateur"><span class="bi bi-eye-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-success" href="index.php?route=admin-edit-user" title="Modifier l'utilisateur"><span class="bi bi-pencil-fill" aria-hidden="true"></span></a>
                        <a class="btn btn-danger" href="index.php?route=admin-delete-user" title="Supprimer l'utilisateur"><span class="bi bi-trash-fill" aria-hidden="true"></span></a>
                    </td>
                </tr>
            </tbody>
        </table>
    </main>
{% endblock %}
```

#### Étape 1.3.7 : admin/users/show.html.twig

```html
{% extends 'admin/layout.html.twig' %}

{% block main %}
    <main class="container py-5">
        <h1 class="mb-4">Détails d'un utilisateur</h1>
        <article>
            <dl>
                <dt>
                    Email
                </dt>
                <dd>
                    mari@test.fr
                </dd>
                <dt>
                    Role
                </dt>
                <dd>
                    USER
                </dd>
            </dl>
        </article>
    </main>
{% endblock %}
```

### Étape 1.4 : sécuriser l'admin

Le principe est assez simple, dans le routeur si quelqu'un veut accéder à une page de l'admin (à part le login) nous allons vérifier si un utilisateur est connecté et si cet utilisateur a le rôle admin. Si ça n'est pas le cas, nous le renvoyons directement sur la page de login de l'admin.

Pour l'instant le login de l'admin n'est pas fonctionnel, donc on va se débrouiller autrement pour connecter un admin. Pour commencer on va passer l'un des utilisateurs dans notre base de données en role ADMIN (utilisez PhpMyAdmin). Une fois que c'est fait, on va utiliser le formulaire de login du front et ensuite allez sur les URLs de l'admin.