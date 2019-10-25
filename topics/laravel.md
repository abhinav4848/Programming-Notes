# Laravel
[Official Documentation](https://laravel.com/docs/)

## Make fresh Laravel installation
First, download the Laravel installer using Composer. (Make sure composer's vendor/bin is added to path.)

```bash
composer global require laravel/installer
```
Once installed, the laravel new command will create a fresh Laravel installation in the directory you specify. For instance, laravel new blog will create a directory named blog containing a fresh Laravel installation with all of Laravel's dependencies already installed:

```bash
laravel new blog
```

will create a folder named blog with Laravel and dependencies already installed. 

Alternatively, you may also install Laravel by issuing the Composer create-project command in your terminal:

```bash
composer create-project --prefer-dist laravel/laravel blog
```

Also run `npm install`.

## Github download of a Laravel project
Clone project into folder of choice. Then run: 
```bash
composer install
```
to create the `vendor` directory. It's **never** version controlled. Now run
```bash
npm install && npm run dev
```
to install all packages AND compile all assets.

Now make an `.env` file in root directory and copy content from [Laravel's .env.example](https://raw.githubusercontent.com/laravel/laravel/master/.env.example) file into it (or rename the `env.example` file given with the github package)

Now create an application encryption key by:
```bash
php artisan key:generate
```

# Local Development Server
If you have PHP installed locally and you would like to use PHP's built-in development server to serve your application, you may use the serve Artisan command. This command will start a development server at http://localhost:8000

```bash
php artisan serve
```

or alternatively, go to the project public folder and open cmd

```bash
php -S localhost:8000
```

### Projects to replicate and test:
* [Monica](https://github.com/monicahq/monica),
* [EdPaper](https://github.com/Edraens/EdPaper)

## Locations of basic MVC files
* **Model**: in `app/`. Can create Models folder or if only few, then add more files next to `User.php`
* **Views**: in `/resources/views/`
* **Controllers**: in `app/http/Controllers`
* **Routes**: in `routes/wep.php`
* **Config**: in `config/`. In `app.php`, add the plugins under `providers`

Create folers via `artisan` instead of by yourself.
Eg: This makes a `app\Http\Controllers\PagesController.php` file. You may need to refresh the sidebar.

```bash
php artisan make:controller PagesController
```

## Routes
We can just write html inside routes file itself.
```php
Route::get('/', function () {
    //return view('welcome');
    return '<h1>Hello World</h1>';
});
```

Can create routes for other pages we add. Eg: If file is located at `\resources\views\pages\about.blade.php`

```php
Route::get('/about', function () {
    return view('pages.about');
});
```

Pass values
```php
// in this case, both parameters are required to be passed
Route::get('/users/{id}/{name}', function ($id, $name) {
    return 'This is user: '.$name.' with ID: '.$id;
});
```

We normally don't want to return a view from route. But rather, set up a route to a controller which will then return a view..

Writing format: `Controller@method`
```php
Route::get('/', 'PagesController@index');
```

## Controllers
Inside the class of a particular controller, methods are written like:

`view('pages.index')` means look inside `resources/views/pages` folder for `index.blade.php`

and `view('welcome')` means look inside `resources/views/` folder for `welcome.blade.php`
```php
// simple static page call
public function index()
{
    return view('pages.index');
}

// to pass a value
public function index()
{
    $title = 'Welcome to Laravel!!!!!!!';
    // return view('pages.index', compact('title'));

    // ->with('name of the variable inside the view', $the_passed_varaible)
    return view('pages.index')->with('title', $title);
}

// to pass multiple values
public function services()
{
    $data = array(
        'title' => 'Services',
        'services' => ['Web Design', 'Programming', 'Medicine', 'SEO']
    );
    // ->with() works for multiple values too. Pass as array
    return view('pages.services')->with($data);
}
```

## Blade Template Engine
Install VS code extension: `Laravel Blade Snippets` for highlighting Laravel blade syntax.

Create `resources\views\layouts\app.blade.php` and put all repetitive html code, etc into it. But add `@yield('unique-name')` to that section of this page where you want other pages' contents to show up.

Then go to those pages in `resources\views\pages\` and add:
```php
@extends('layouts.app')

@section('unique-name')
    <h1>{{$title}}</h1>
    <p><?php echo $title; ?>: This is allowed by Blade</p>
    <p>Text, etc that will display in the yield section</p>

    @if (count($services)>0)
        <ul>
            @foreach ($services as $service)
                <li>{{$service}}</li>
            @endforeach
        </ul>
    @endif
@endsection
```

For `<title>` tag, it searches `.env` for `APP_NAME=` and give its value. If not existing, use `'LSAPP'`

```php
<title>{{config('app.name', 'LSAPP')}}</title>
```

For assets, similar code. Here we import the compiled css which is in `public/css/app.css`

```php
<link rel="stylesheet" href="{{asset('css/app.css')}}">
```

Include other files like navbar. Assuming it's located at `resources\views\inc\navbar.blade.php`, add this to `resources\views\layouts\app.blade.php`:

```php
@include('inc.navbar')
```

## Resources vs Public
Resources has all the real files we work with. So we work with this folder.

Public is just a folder where compiled versions are put. So we do nothing with this folder. (unless you wanna keep things plain simple and edit `public\css\app.css` directly)

eg: go to `\resources\assets\sass\_variables.scss` and change a value. Now in terminal do
```bash
npm run dev
```
come to browser, reload with cache clearing (`Ctrl + F5`)

For frequently tracking changes to files and compile on save:
```bash
npm run watch
```

Add new css to `resources\assets\sass\_custom.scss` file. The `_` says that this file is an *include*. Now go to `resources\sass\app.scss` and add `@import "custom";`
Then do `npm run dev`

## Installing npm modules
### bootstrap
```bash
$ npm install bootstrap
```

Bootstrap gets added to `package.json`. Now go to `resources\sass\app.scss` and add

```scss
// Bootstrap
@import "node_modules/bootstrap/scss/bootstrap";
```
