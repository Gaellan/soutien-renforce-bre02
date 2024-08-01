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


## Étape 2 : lister les utilisateurs

### Étape 2.1 : le UserManager

Dans les UserManager, nous allons devoir rajouter une nouvelle méthode `findAllUsers() : array` qui nous renverra un tableau d'instances de la classe User, récupéré depuis la base de données.

### Étape 2.2 : Le UserController

Dans la méthode `list` du `UserController`, nous allons appeler la méthode `findAllUsers` du `UserManager` pour récupérer la liste des utilisateurs. Nous allons ensuite la passer dans la tableau de données de la méthode `render` pour pouvoir l'utiliser dans notre template.

### Étape 2.3 : le template

Dans le template `admin/users/list.html.twig` vous allez dynamiser le `<tbody>` de la table pour faire en sorte que chacune des rangées (`<tr>`) corresponde à un User de votre liste. Pour le moment laissez les boutons d'action tels qu'ils sont.

### Étape 2.4 : git

Un petit add / commit / push pour sauvegarder notre travail :)


## Étape 3 : Créer un utilisateur

### Étape 3.1 : le UserController

Dans la méthode `checkCreate` du `UserController`, nous allons traiter le formulaire de création d'un utilisateur. Vous pouvez vous inspirer de ce que nous avont fait pour l'inscription côté front. Attention le formulaire n'est pas exactement le même, donc il faut bien vérifier pour ne rien oublier. Une fois l'utilisateur créé nous allons rediriger vers la page de liste des utilisateurs.


## Étape 4 : Détails d'un utilisateur

### Étape 4.1 : le UserManager

Dans le `UserManager` nous allons créer une nouvelle méthode `findUserById(int $id) : ? User` qui permet de trouver un utilisateur dans la base de données à partir de son id.

🚨 Attention à bien respecter le prototype de la méthode, elle retourne soit null, soit un User et prend un int en paramètres.

### Étape 4.2 : le UserController

Dans le UserController nous allons modifier notre méthode `show`, elle ressemblera désormais à ceci : 

```php
public function show(int $id) : void {
    $this->render("admin/users/show.html.twig", []);
}
```

Nous allons dans notre méthode `show` appeler la méthode `findUserById` du `UserManager` pour récupérer les infoemations de l'utilisateur dont l'id a été récu en paramètre (`int $id`).

S'il existe nous allons l'envoyer dans le tableau de données de la méthode `$this->render`, s'il n'existe pas nous allons rediriger vers la liste des utilisateurs.

### Étape 4.3 : le routeur

Dans notre routeur, là où nous vérifions si la route correspond à `admin-show-user` nous allons rajouter une condition qui vérifie si le paramètre `$_GET['user_id']` existe bien. S'il existe nous allons le transformer en int en le castant ou en utilisant `intval()` puis l'envoyer à la méthode `show()`.

### Étape 4.4 : les templates

#### Étape 4.4.1 : show.html.twig

Nous allons utiliser le `User` reçu via la méthode render pour dynmiser les informations de la page.

#### Étape 4.4.2 : list.html.twig

Nous allons rendre dynamique le lien pour voir les détails d'un utilisateur dans les actions du tableau. L'URL du premier des trois liens devra donc ressembler à ça : `index.php?route=admin-show-user&user_id={{user.id}}}`.

🚨 Attention si dans votre boucle d'affichage vous avez utilisé un autre nom de variable que `user`, adaptez à ce que vous avez fait.


## Étape 5 : Modifier un utilisateur

À partir de ce que nous avons fait dans les étapes précédentes, essayez de coder vous-même la fonctionnalité de modification d'un utilisateur.

Vous allez, comme pour le `show`, mettre en place un système qui permet de connaitre l'id de l'utilisateur à modifier et l'injecter directement dans les values du formulaire d'édition.

Vous allez également devoir créer une nouvelle méthode pour modifier un utilisateur dans le `UserManager`, voilà le prototype qu'elle doit avoir : `public function updateUser(User $user) : User`.

Puis vous allez devoir traiter ce formulaire pour appliquer les modifications.


## Étape 6 : Supprimer un utilisateur

### Étape 6.1 : Mettre en place la modale

Pour éviter de supprimer un utilisateur nous allons rajouter une petite modale fournie par Bootstrap qui permet d'afficher une fenêtre de confirmation avant de supprimer un utilisateur.

Elle demande pas mal de HTML et un peu de JavaScript du coup je vous fournis le code (qui est celui de Bootstrap légèrement modifié) du nouveau template `admin/users/list.html.twig` :

```html
{% extends 'admin/layout.html.twig' %}

{% block main %}
    <main class="container py-5">
        <h1 class="mb-4">Liste des utilisateurs</h1>
        {% if session.success_message %}
            <div class="alert alert-success" role="alert">
                {{ session.success_message }}
            </div>
        {% endif %}
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
                {% for user in users %}
                    <tr>
                        <td>
                            {{ user.id }}
                        </td>
                        <td>
                            {{ user.email }}
                        </td>
                        <td>
                            {{ user.role }}
                        </td>
                        <td>
                            <a class="btn btn-primary" href="index.php?route=admin-show-user&user_id={{ user.id }}" title="Voir l'utilisateur"><span class="bi bi-eye-fill" aria-hidden="true"></span></a>
                            <a class="btn btn-success" href="index.php?route=admin-edit-user&user_id={{ user.id }}" title="Modifier l'utilisateur"><span class="bi bi-pencil-fill" aria-hidden="true"></span></a>
                            <button data-bs-toggle="modal" data-bs-target="#deleteUserModal" class="btn btn-danger" title="Supprimer l'utilisateur" data-bs-user="{{ user.email }}" data-bs-id="{{ user.id }}"><span class="bi bi-trash-fill" aria-hidden="true"></span></button>
                        </td>
                    </tr>
                {% endfor %}
            </tbody>
        </table>
        <div class="modal fade" id="deleteUserModal" tabindex="-1" aria-labelledby="deleteModalTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="deleteModalTitle">Spprimer un utilisateur</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <h2 class="h4">Voulez vous vraiment supprimer cet utilisateur ?</h2>
                        <p class="my-2">

                        </p>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Annuler</button>
                        <button type="button" class="btn btn-danger">Supprimer</button>
                    </div>
                </div>
            </div>
        </div>
    </main>
{% endblock %}
{% block javascript %}
    {{ parent() }}
    <script type="application/javascript">
        window.addEventListener("DOMContentLoaded", function(){
            const modal = document.getElementById('deleteUserModal');
            if (modal) {
              modal.addEventListener('show.bs.modal', event => {
                // Button that triggered the modal
                const button = event.relatedTarget;
                // Extract info from data-bs-* attributes
                const email = button.getAttribute('data-bs-user');
                const id = button.getAttribute('data-bs-id');

                // Update the modal's content.
                const modalP = modal.querySelector('.modal-body p');

                modalP.innerHTML = email;
                const deleteBtn = modal.querySelector('.modal-footer .btn-danger');

                deleteBtn.addEventListener('click', () => {
                    window.location.href = `index.php?route=admin-delete-user&user_id=${id}`;
                });

              })
            }
        });
    </script>
{% endblock %}
```

### Étape 6.2 : modifier le routeur

Comme pour `show` et `edit`, notre route va prendre un paramètre `user_id`, rajoutez la vérification dans le `Router`, transformez le en `int` et envoyez le à la méthode `delete` du `UserController`.

### Étape 6.3 : le manager

Dans le `UserManager` nous allons devoir créer une nouvelle méthode `public function delteUser(int $id) : void` qui efface l'utilisateur qui a l'`id` reçu en paramètres.

### Étape 6.4 : le controlleur

Dans la méthode `delete` du `UserController` pensez à modifier son prototype qui est maintenant `public function delete(int $id)`, ensuite nous allons devoir appeler la méthode `deleteUser` du `UserManager` en lui pssant l'id reçu en paramètres. Une fois que c'est fait redirigez vers la liste des utilisateurs.


### Étape 7 : le login de l'admin

Dernière étape : vous allez devoir mettre en place le login de l'admin (sur la page `admin-connexion`). Faites en sortes que si l'utilisateur qui tente de se connecter a le bon email, le bon mot de passe et est admin, vous le redirigez vers la route `admin` sinon, vous le renvoyez vers la home du front.

