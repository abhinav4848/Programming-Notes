# Composer
Initiate composer in a project:

```bash
composer install
```
This reads `composer.json` file and creates a `Vendor` folder with all its stuff for autoloading. Exclude this folder from git. In the entry point of the directory, inside `index.php`, add this:
```php
require vendor/autoload.php
``` 

Update the dependencies subsequently, as in when you want to add new classes you might have created:

```bash
composer dump-autoload
```