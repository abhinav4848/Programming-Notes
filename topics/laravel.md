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

## Github download of a Laravel project
Clone project into folder of choice. Then run: 
```bash
composer install
```
to create the `vendor` directory. It's **never** version controlled. Now run
```bash
npm install && npm dev
```
Now make an `.env` file in root directory and copy content from [Laravel's .env.example](https://raw.githubusercontent.com/laravel/laravel/master/.env.example) file into it.

Now create an application encryption key by:
```bash
php artisan key:generate
```

# Local Development Server
If you have PHP installed locally and you would like to use PHP's built-in development server to serve your application, you may use the serve Artisan command. This command will start a development server at http://localhost:8000

```bash
php artisan serve
```


