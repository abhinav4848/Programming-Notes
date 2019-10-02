# PHP Servers
[Docs](https://www.php.net/manual/en/features.commandline.webserver.php). 
*Start a server*
```bash
php -S localhost:8000
```

*End a server*
```bash
PHP 5.4.0 Development Server started at Thu Jul 21 10:43:28 2011
Listening on localhost:8000
Document root is /home/me/public_html
Press Ctrl-C to quit.
```
If you don't have that terminal open, you have to find the PID of the process and kill that.

# Composer
Initiate composer in a project or update the dependencies subsequently:
```bash
composer install
composer dump-autoload
```