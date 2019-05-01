# Installation

Pour installer le projet, on lance la commande composer depuis le terminal:

```powershell
composer create-project themosis/themosis larajobs
cd larajobs
# Dans la version 2.0 de Themosis, le thème de base n'est plus installé par défaut
# La nouvelle invité de commande permet de l'installer facilement 
php console theme:install larajobs
```

# Configuration du projet

On doit modifier les variables d'environnement avant de procéder à l'installation du framework

Dans le fichier `.env`, on va modifier les informations de base de données puis les URL de l'application

```env
APP_NAME=Larajobs
MAIL_ENCRYPTION=null
MAIL_PORT=1025
DATABASE_SOCKET=/Applications/MAMP/tmp/mysql/mysql.sock
```

Puis, installation du projet en nous rendant sur l'addresse principale de notre site-web.

# Structure de notre application

Notre root directory est dans le dossier `htdocs/cms`, dossier dans lequel est installé WordPress. Comme pour les anciennes versions de Themosis, il est toujours possible de développer nos thèmes et plugins dans ce dossier. 
Mais pour faciliter la gestion de notre projet, les développements doivent maintenant se faire dans le dossier `app`

## Structure des fichiers

1. Explication de la structure et l'organisation d'un projet Themosis
2. Différence entre les répertoires root view VS theme view 
3. La commande make disponible dans la console

# Routing

1. Ajouter un paramètre dans l'url: `Route::get(/posts/{slug})`
2. Méthodes pour les routes: `Route::post()`
3. Route qui charge une view: `Route::view('/contact', 'contact')`

**The WordPress $post and $query parameters:**

```php
Route::get('page', function ($post, $query) {
    return view('pages', [
        'page' => $post 
    ]);
});
```

## Routes spéciales Laravel

1. Page d'accueil: `Route::get('home')`
2. Route pour une page spéciale: `Route::get('page', ['contact', 24, function(){}])`
3. Route pour un template: `Route::get('template', ['all-jobs'])`
4. Single custom post route: `Route::get('singular', ['jobs'])`

## Étendre les routes WordPress

Voir le fichier app.php de configuration du projet


# View

## Gestion des assets

1. Installation de laravel-mix
2. Configuration du fichier webpack-mix.js: on va désactiver les notifications avec la méthode `mix.disableNotifications()`
3. Installation de Tailwind.css: `npm install tailwind` et `./node_modules/.bin/tailwind init`
4. Dans le fichier webpack-mix.js, ajouter après sass les options de compilation suivantes:

```
mix.sass('assets/sass/style.sass', 'dist/css/theme.css').options({
    processCssUrls: false,
    postCss: [
        tailwindcss('./tailwind.js') ],
});
```

5. Dans le fichier sass, on va include tailwind en utilisant les commandes suivantes: 

```sass
@tailwind preflight
@tailwind components
@tailwind utilities
```

## Création de notre header et foooter

```php
<header class="bg-red-light text-white">
    <div class="container mx-auto py-4">
        <h1 class="w-1/2 w-auto text-2xl font-medium">Larajobs Montreal</h1>
    </div>
    <div class="block bg-white border-b">
        <div class="container flex mx-auto">
            <div class="flex -mb-px mr-8">
                <a href="#" class="no-underline text-red-light flex items-center py-4 border-b border-red-light">
                    <svg class="h-6 w-6 fill-current mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M18 8H5.5v-.5l11-.88v.88H18V6c0-1.1-.891-1.872-1.979-1.717L5.98 5.717C4.891 5.873 4 6.9 4 8v10a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-8a2 2 0 0 0-2-2zm-1.5 7.006a1.5 1.5 0 1 1 .001-3.001 1.5 1.5 0 0 1-.001 3.001z" fill-rule="nonzero"></path></svg>
                    <span class="label">Tous les emplois</span>
                </a>
            </div>
            <div class="flex -mb-px mr-8">
                <a href="#" class="no-underline text-white opacity-50 md:text-grey-dark md:opacity-100 flex items-center py-4 border-b border-transparent hover:opacity-100 md:hover:border-grey-dark">
                    <svg class="h-6 w-6 fill-current mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M18.783 12c0-1.049.646-1.875 1.617-2.443a8.932 8.932 0 0 0-.692-1.672c-1.089.285-1.97-.141-2.711-.883-.741-.74-.968-1.621-.683-2.711a8.732 8.732 0 0 0-1.672-.691c-.568.97-1.595 1.615-2.642 1.615-1.048 0-2.074-.645-2.643-1.615-.58.172-1.14.403-1.671.691.285 1.09.059 1.971-.684 2.711-.74.742-1.621 1.168-2.711.883A8.797 8.797 0 0 0 3.6 9.557c.97.568 1.615 1.394 1.615 2.443 0 1.047-.645 2.074-1.615 2.643.173.58.404 1.14.691 1.672 1.09-.285 1.971-.059 2.711.682.741.742.969 1.623.684 2.711.532.288 1.092.52 1.672.693.568-.973 1.595-1.617 2.643-1.617 1.047 0 2.074.645 2.643 1.617a8.963 8.963 0 0 0 1.672-.693c-.285-1.088-.059-1.969.683-2.711.741-.74 1.622-1.166 2.711-.883.287-.532.52-1.092.692-1.672-.973-.569-1.619-1.395-1.619-2.442zM12 15.652a3.653 3.653 0 1 1 0-7.306 3.653 3.653 0 0 1 0 7.306z" fill-rule="nonzero"></path></svg>
                    <span class="label">Réglages</span>
                </a>
            </div>
        </div>
    </div>
</header>
```

```php
<div class="bg-white border-t">
    <div class="container mx-auto flex justify-between items-center text-sm">
        <div class="text-left py-4">
            <a href="#" class="no-underline text-grey-dark mr-4">Tous les emplois</a>
            <a href="#" class="no-underline text-grey-dark mr-4">Réglages</a>
            <a href="#" class="no-underline text-grey-dark">Contact</a>
        </div>
        <div class="text-grey text-center py-4">Meetup Laravel Montréal</div>
    </div>
</div>
```

## Démonstration d'un composant

```php
@component('components.card', ['test' => 'HELLO'])
	My components with errors
	@slot('header')
		HEADER
	@endslot
@endcomponent
```

```php
<div class="flex-grow flex flex-col bg-white border-b sm:rounded sm:border shadow overflow-hidden">
    @isset($header)
        <div class="border-b">
            <div class="flex justify-between px-4"> {{ $header }}</div>
        </div>
    @endisset
    {{ $slot }}
    @isset($footer)
        <div class="px-4 py-10 text-grey">{{ $footer }}</div>
    @endisset
</div>
``

# Création d'un custom post type

On va créer un hook avec la commande `php console make:hook JobsCustomPostType`

Puis on va ajouter le hook dans le fichier de configuration `app.php` dans la section hooks.

On intègre ensuite la facade à nos dépendances `Themosis\Support\Facades\PostType` puis on utilise la méthode `PostType::make`

```php
PostType::make('jobs', 'Jobs', 'Job')->setArguments(['menu_icon' => 'dashicons-store'])->set();
```

# Metabox

Pour créer une metabox, on intègre la facade à nos dépendances `Themosis\Support\Facades\MetaBox` puis on utilise la méthode `MetaBox::make`

```php
Metabox::make('properties', 'jobs')->setTitle('Paramètres du job')->set();
```

## Associer des custom fields à une metabox

Pour créer des fields, on importe la facade `Themosis\Support\Facades\Field`. On importe également la class `Themosis\Support\Section` pour la gestion des sections dans la metabox.

```php
Metabox::make('properties', 'jobs')->setTitle('Paramètres du job')
->add(new Section('general','General', [
	Field::text('company'),
	Field::text('url', ['rules' => 'required|url', 'placeholder' => 'Votre url', 'info' => 'test'])
]))->add(new Section('details', 'Details', [
	Field::text('source'),
	Field::text('date')
]))->set();
```

**Options disponibles pour la classe Field:**

label, attributes, info, placeholder, rules, show_in_rest

# Création de notre page single.php

Création du controller avec la commande `php console make:controller JobsController`

On ajoute au routeur `Route::get('singular', ['jobs', 'uses' => 'JobsController@single'])->name('jobs.single');`

On ajouter à notre controlleur les informations

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class JobsController extends Controller{
    
    public function single(\WP_Post $post){
        return view('jobs.single', compact('post'));
    }

}
```
Enfin, on termine par la création de la vue: single.php

```php
<div class="flex-grow flex flex-col bg-white border-b sm:rounded sm:border shadow overflow-hidden">
    @isset($header)
        <div class="border-b">
            <div class="flex justify-between px-4"> {{ $header }}</div>
        </div>
    @endisset
    {{ $slot }}
    @isset($footer)
        <div class="px-4 py-10 text-grey">{{ $footer }}</div>
    @endisset
</div>
```

```php
@extends('app')

@section('content')
    <section class="job">
        <div class="job__header py-20 border-b">
            <div class="flex justify-between container mx-auto">
                <h1>{{ $post->post_title }}</h1>
                <p class="text-grey-dark">Publié le {{ Loop::date() }}</p>
            </div>
        </div>
        <div class="job__content">
            <div class="container mx-auto flex py-20">
                <div class="job__description w-2/3 text-grey-darkest text-xl leading-normal mr-32">
                    {!! $post->post_content !!}
                </div>
                <div class="job__details w-1/3">
                    @component('components.card')
                        @slot('header')
                            <h3 class="text-blue-dark py-5 font-medium text-lg">A propos de cet emploi</h3>
                        @endslot
                        <div class="flex-grow flex flex-col text-grey-darker border-b">
                            <div class="w-full px-6 py-6 border-b">{{ get_post_meta($post->ID, 'th_company', true) }}</div>
                            <div class="w-full px-6 py-6 border-b">{{ get_post_meta($post->ID, 'th_source', true) }}</div>
                            <div class="w-full px-6 py-6">{{ get_post_meta($post->ID, 'th_date', true) }}</div>
                        </div>
                        @slot('footer')
                            <a href="{{ get_post_meta($post->ID, 'th_url', true) }}" target="_blank" class="no-underline bg-blue hover:bg-blue-dark text-white border border-blue-dark rounded px-6 py-4">Appliquer</button>
                            <a href="" class="no-underline text-grey-dark ml-5">Partager cet offre</a>
                        @endslot
                    @endcomponent
                </div>
            </div>
        </div>
    </section>
@endsection
```

# Création de notre page index.php

Dans le routeur, `Route::get('/jobs', ['uses' => 'JobsController@index'])->name('jobs.index')`

Dans le controlleur, on ajoute la méthode puis création du fichier blade

```php
@extends('app')

@section('content')
    <section class="jobs">
        <div class="jobs__header py-20">
            <div class="flex justify-between container mx-auto">
                <h1>Tous les emplois <span class="underline font-black text-red-light">Laravel</span> autours de <span class="font-black text-red-light">Montréal</span></h1>
            </div>
        </div>
        <div class="jobs__content mb-20 ">
            @component('components.card')
                <div class="flex-grow flex flex-col text-grey-darker">
                    @query(['post_type' => 'jobs', 'posts_per_page' => 10])
                        <div class="w-full flex px-6 py-6 border-b items-center">
                            <h3 class="mr-5 w-1/3 truncate">{{ Loop::title() }}</h3>
                            <p class="mr-20">Montréal, Canada</p>
                            <p>{{ get_post_meta(Loop::id(), 'th_company', true) }}</p>
                            <a href="{{ get_permalink(Loop::id()) }}" class="no-underline ml-auto text-grey">Plus de détails</a>
                        </div>
                    @endquery
                </div>
            @endcomponent
        </div>
    </section>
@endsection
```

# Amélioration du controlleur, en MVC

Création du model avec la commande `php console make:model Job`

Notre modèle Eloquent fonctionne, mais nous devons filter les posts par type et ne pas récupérer les posts non publiés (brouillons)

Installation du package Corcel: `composer require jgrossi/corcel`, puis configuration *à la Laravel*

On ajoute le provider dans le fichier `app.php` des configurations: `Corcel\Laravel\CorcelServiceProvider::class`

Puis installation de nos vendors du package avec la commande `php console vendor:publish`

Désormais, les modèles doivent hérités de la classe `Corcel\Model\Post`;

Dans le model, la propriété `protected $postType = 'jobs';` permet de spécifier le custom post type que l'on souhaite utiliser.

Modification de la vue

```php
@foreach($jobs as $job)
	@component('components.card')
   		<div class="flex-grow flex flex-col text-grey-darker">
      		<div class="w-full flex px-6 py-6 border-b items-center">
          	<h3 class="mr-5 w-1/3 truncate">{{ $job->post_title }}</h3>
             <p class="mr-20">Montréal, Canada</p>
             <p>{{ $job->meta->th_company }}</p>
             <a href="{{ get_permalink($job->ID) }}" class="no-underline ml-auto text-grey">Plus de détails</a>
          </div>
      </div>
	@endcomponent
@endforeach
```

# Création d'une commande pour alimenter nos jobs depuis Indeed

Création de notre commande `php console make:command DownloadJobsFromIndeedCommand`

Installation de notre librairie CURL: `composer require ixudra/curl`

```php
$jobs = (new CurlService)->to('https://api.indeed.com/ads/apisearch')->withData(['publisher' => '1303284387458115', 'co' => 'CA', 'q' => 'laravel', 'l' => 'Montréal', 'format' => 'json'])->asJson()->get();
foreach($jobs->results as $job):
	$jobInstance = Job::create(['post_title' => $job->jobtitle, 'post_content' => $job->snippet, 'post_excerpt' => $job->snippet, 'to_ping' => false, 'pinged' => '', 'post_content_filtered' => '', 'post_type' => 'jobs']);
	$jobInstance->saveMeta([
		'th_company' => $job->company,
       'th_url' => $job->url,
		'th_source' => $job->source,
      	'th_date' => $job->date,
   	]);
endforeach;
```

On ajoutera ensuite notre commande dans le task scheduler de Themosis

#Création d'un template pour la page s'abonner par courriel

`php console make:migration createJobSubscriptionsTable` 

`php console make:model Models/JobSubscription`

On ajoute à nos dépendances JS Axios pour appeller en AJAX facilement. `npm install axios``

```javascript
Axios.get(themosis.ajaxurl, {params: {action: 'subscribe_job', email: 'jb@deviz.net'}});
```

Et le listener dans une class Hook

```php

use App\Models\JobSubscription;

Ajax::listen('subscribe_job', function($params){
	JobSubscription::create(['email' => request()->get('email')]);
});
```

Puis ajouter la proprité $fillable = ['email'] au model JobSubscription


`php console make:command SendJobsNotificationsCommand`

```php
$subscriptions = JobSubscription::get();
foreach($subscriptions as $subscription):
	$jobs = Job::get();
	Mail::to($subscription->email)->send(new JobsNotificationMail($jobs));
endforeach;
```

`php console make:mail JobsNotificationMail`

```php
@component('mail::message')

<h1>Dernières offres publiées</h1>

@component('mail::table')
| Nom                          | Compagnie                         |
| ---------------------------- |-----------------------------------|
@foreach($jobs as $job)
| {{ $job->post_title }}       | {{ $job->meta->th_company }}      |
@endforeach
@endcomponent

@component('mail::button', ['url' => 'http://larajobs.local'])
Voir les nouveaux jobs disponibles
@endcomponent

@endcomponent
```

