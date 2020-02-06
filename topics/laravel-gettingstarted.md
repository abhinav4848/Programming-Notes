# Laravel Basics
[Official Documentation](https://laravel.com/docs/), [Brad Traversy YouTube](https://www.youtube.com/playlist?list=PLillGF-RfqbYhQsN5WMXy6VsDMKGadrJ-)

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
composer create-project --prefer-dist laravel/laravel project-name
```

Composer method will do two extra things that laravel method won't. These are executed because of scripts in composer.json
1. `cp .env.example .env`
2. `./artisan key:generate`

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
* **Views**: in `/resources/views/`. All your normal views pages will be saved here in `pages`/`layout`/`includes`, with naming of files as `index.blade.php`
* **Controllers**: in `app/http/Controllers`
* **Routes**: in `routes/wep.php`
* **Config**: in `config/`. In `app.php`, add the plugins under `providers`

Create folders/some files via `artisan` instead of by yourself. Look at Controllers heading below.