# Artisan
its'a a CLI. You can use `php artisan`-
```bash
list #all options that come with artisan
help migrate
make:controller PostsController
make:model Post -m #to make a migration file for it as well. Keep it singular
make:migration name_of_migration -table=posts
migrate # to run the migration
tinker # Edit database in CLI directly
```

# Making Databases
Go to mysql and make a database. Just the name. No adding tables cuz that's done with migrations.

Now go to Terminal and `php artisan make:controller PostsController --resource`. Then make a **model**. Use singluar form of the above noun: `php artisan make:model Post -m`.

This makes 2 files:
1. app\Post.php
2. database\migrations\datetime_create_posts_table.php

Now go to #2 and add more table columns to `up()`. This is for adding columns. ('timestamp') automatically adds 2 columns. created_at and updated_at)

Similarly, `down()` is for rolling back migrations.

Now go to the `.env` file and update database credentials.

Now run `php artisan migrate`. It will make all the tables.

Now generate some data for populating the database. Go to command line and `php artisan tinker`. Now use Eloquent, which is an ORM.

Eg:
```bash
# import the model (Post) and use functions on it (::count)
# Always use a backslash(\)
>>> App\Post::count()
=> 0

# create an instance of the model
>>> $post = new App\Post();
=> App\Post {#3012} 
# i.e. an instance has been created and saved in memory.

# modify the properties of the instance. Title/body, etc are taken from the model.
>>> $post->title='Post One';
=> "Post One"
>>> $post->body='This is the post body';
=> "This is the post body"

# save the properties into table as a row
>>> $post->save();
=> true

# Show all posts
>>> App\Post::all()

# Show one specific post. Here, id of 1
>>> App\Post::find(1)

# For a second one, you gotta instantiate the thing again.
```

The post table now has an entry with timestamps updated automatically.
To add another post, reinstantiate an instance of the model (ie. `$post = new App\Post();`

Now go to the PostsController and make sure there are resource methods you created with the `--resource` option above. These help with CRUD functionality.

Come to routes and add a route for all these functions.
Or just a shortcut command also exists.

`php artisan route:list` shows us all the routes available. All except `api/user` are created by user. Eg output: 
```bash
+--------+----------+-------------------+------+-----------------------------------------------+--------------+
| Domain | Method   | URI               | Name | Action                                        | Middleware   |
+--------+----------+-------------------+------+-----------------------------------------------+--------------+
|        | GET|HEAD | /                 |      | App\Http\Controllers\PagesController@index    | web          |
|        | GET|HEAD | about             |      | App\Http\Controllers\PagesController@about    | web          |
|        | GET|HEAD | api/user          |      | Closure                                       | api,auth:api |
|        | GET|HEAD | services          |      | App\Http\Controllers\PagesController@services | web          |
|        | GET|HEAD | users/{id}/{name} |      | Closure                                       | web          |
+--------+----------+-------------------+------+-----------------------------------------------+--------------+
```

The shortcut is to open `routes.php` add 
```php
Route::resource('posts', 'PostsController');
```

# Make a provider
```bash
php artisan make:provider FormServiceProvider
```
can be used for custom form. See relevant section