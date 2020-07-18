# Composer
Initiate composer in a project:

```bash
composer install
```
This reads `composer.json` file and creates a `Vendor` folder with all its stuff for autoloading. **Exclude this folder from git**. In the entry point of the directory, inside `index.php`, add this:

```php
require vendor/autoload.php
``` 

When you want to update the dependencies/classes subsequently, as in when you want to add new classes you might have created:

```bash
composer dump-autoload
```

# Needs sorting
Initiate composer in a project root folder
```bash
composer init
```

Change vendor location by using 
```json
"config": {
    "vendor-dir": "includes/vendor"
},
```

Different methods of including files
[Documentation](https://getcomposer.org/doc/04-schema.md#autoload).
```json
"autoload": {
    "classmap": [
        // include all folders
        "./", 
        //include only classes in a specified folder
        "myClasses"
    ],
    "psr-4": {
        "Application\\": "app/"
    },
    "files": [
        "helpers/functions.php"
    ]
},
```

require new packages
```bash
composer require webmozart/assert
```