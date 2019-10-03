# Add PHP, MySQL to path
For windows:
```
C:\xampp\php;
C:\xampp\mysql\bin;
```

# PHP Servers
[Documentation](https://www.php.net/manual/en/features.commandline.webserver.php). 

*Start a server:*
```bash
php -S localhost:8000
```

*End a server:*
```bash
PHP 7.3.9 Development Server started at Fri Oct  4 00:57:52 2019
Listening on http://localhost:8000
Document root is C:\GitHub\Programming-Notes
Press Ctrl-C to quit.
```

If you don't keep that terminal open, you have to find the PID of the process and kill that.