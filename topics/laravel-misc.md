## Resources vs Public
Resources has all the real files we work with. So we work with this folder.

Public is just a folder where compiled versions are put. So we do nothing with this folder. (unless you wanna keep things plain simple and edit `public\css\app.css` directly)

*eg:* go to `\resources\sass\_variables.scss` and change a value. Now in terminal do
```bash
npm run dev
```
Then come to browser and reload with cache clearing (`Ctrl + F5`)

Save effort of typing `npm run dev` every time you change a file. For continuously tracking changes to files and compile on save:
```bash
npm run watch
```

Add your custom css to `resources\sass\_custom.scss` file. The `_` says that this file is an *include*. Now go to `resources\sass\app.scss` and add `@import "custom";`

Then do `npm run dev`

## Installing npm modules
### Bootstrap
*[Source of info](https://www.itechempires.com/2019/09/configure-bootstrap-vue-react-in-laravel-6-with-frontend-scaffolding/amp/#Install_Bootstrap_in_Laravel_6)*

From Laravel 6, UI is separated into its own component. So install it in project using `composer require laravel/ui`.

Check `php artisan ui --help` for which scaffoldings it can set up. Now generate bootstrap scaffolding with it, via `php artisan ui --auth bootstrap`. (This will also add auth system). It will ask to replace `views.layouts.app`, and `views.home`.

Then `npm install && npm run dev`
* install: installs all packages from package.json
* dev: compiles everything

**Changes made**: 
* Bootstrap, jquery, popper.js got added to `package.json`. 
* The resource directory has an updated `bootstrap.js` , `app.scss` and a new `_variables.scss` file

Add your custom css with `@import 'custom';` in the `app.scss` file. It is to be located in same folder as `_custom.scss`

With --auth, you get added HomeController.php, and a route:
```php
Auth::routes();

Route::get('/home', 'HomeController@index')->name('home');
```

with `npm install`, you get updated `public\mix-manifest.json` with location of `app.js` and `app.css` in it, as well as these files in public folder.

# Enable authentication
`php artisan ui:auth`

Fix layout.blade.php, extract navbar from it into an `inc/` folder

You may want to fix all the `home` references in the route, auth files, HomeController name, its home function, and RedirectIfAuthenticated.php, etc to `dashboard`

# Adding user id to posts table
To add more columns to already made tables, make a migration with `php artisan make:migration add_user_id_to_posts`, Make the migration as verbose as possible.

add `$table->integer('user_id');` to up(), and `$table->dropColumn('user_id');` to down().

Then run `php artisan migrate`.

To undo migration, read https://laravel.com/docs/6.x/migrations#rolling-back-migrations

# Model Relationships
Go to Post Model, add 
```php
public function user()
{
    // a single post belongs to a user.
    return $this->belongsTo('App\User');
}
``` 

In User Model, add
```php
public function posts()
{
    // to establish one-to-many relationship.
    return $this->hasMany('App\Post');
}
```

In DashboardController, add
```php
use App\User;

public function index()
{
    $user_id = auth()->user()->id;
    $user = User::find($user_id);
    
    return view('dashboard')->with('posts', $user->posts);
}
```

In dashboard view, add:
```php
<h3>Your listings</h3>
@if (count($posts)>0)
    <table class="table table-striped">
        <tr>
            <th>Company</th>
        </tr>
        @foreach ($posts as $post)
        <tr>
            <td>{{$post->name}}</td>
        </tr>
        @endforeach
        
    </table>
@endif
```

Now for any posts view, just add `{{$post->user->name}}`, 

# Acecss control
Add this constructor to any controller that requires authorization.
```php
/**
* Create a new controller instance.
*
* @return void
*/
public function __construct()
{
    $this->middleware('auth');
}
```
This will force redirect to login page if not logegd in. If you find it blocked an unregistered user from more places than you wanted it to, then add exceptions as an array of views, like:
```php
public function __construct()
{
    $this->middleware('auth', ['except'=>['index','show']]);
}
```
the auth will not work for `index()` and `show()` and these controller methods will work for guests too.

Hide options that shouldn't be visible to a guest or a wrong user. Add this to `views` pages:
```php
@if (!Auth::guest())
//if user is not guest
    @if (Auth::user()->id==$post->user_id)
        //if user is correct owner of post
    @endif
@endif
```

Also modify your `edit()` and `delete()` controller to have this check:

```php
public function edit($id)
{
    $post = Post::find($id);
    if (auth()->user()->id!==$post->user_id) {
        return redirect('/posts')->with('error', 'Unauthorized page');
    }
    return view('posts.edit')->with('post', $post);
}
```

# Final starter Sequence
1. `composer create-project --prefer-dist laravel/laravel project-name`
1. `composer require laravel/ui`
1. `php artisan ui --auth bootstrap`
1. `npm install && npm run dev`
1. `npm run watch`
1. `git init`
1. create `resources/sass/_variables.sass` and add it to `resources/sass/app.sass`
1. (Optional) Rename `home.view.php`->`dashboard.view.php`, `welcome.view.php`->`home.view.php`, and change the Controller, Auth Controllers, Providers, `Middleware/RedirectIfAuthenticated` and route as well.
1. Include into `layouts.app` the standard code.
1. Create Controller as `php artisan make:controller PagesController --resource`
1. Add this to `web.php` 
    ```php
    Route::get('/', 'PagesController@index'); // shows default index mode
    Route::resource('page', 'PagesController'); // responds to all
    ```
1. Run `php artisan make:model Page -m`. This makes a model and a migration file. Make the word start with capital letter. Update the migration file with adequete format. Create a database, link it to `.env` and run `php artisan migrate`
1. 