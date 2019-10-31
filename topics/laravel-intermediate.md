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

# modify the properties of the instance
>>> $post->title='Post One';
=> "Post One"
>>> $post->body='This is the post body';
=> "This is the post body"

# save the properties into table as a row
>>> $post->save();
=> true
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

# Fetching Data with Eloquent
In the Model, the name of the table is the plural version of the name of the class. To change the name of the table you want to be using, just make `protected $table='posts';`

Same for primary key. The default is 'id'. But if you wanna use something else, just type `public $primaryKey = 'item_id';`

Similarly, timestamps are by default true. So if you wanna specify false, type `public $timestamps = false;`

Go to `PostsController.php` and work on `index()`. Add the view. eg: `return view('posts.index');` to it. Now corresponding to it, create a view at `resources\views\posts\index.blade.php`.

Now also add the 'namespace\Model' file stuff, ie `use App\Post;` (App is the namespace of Post) into PostsController.php at the top of the Controller page.

Now we can use **Eloquent**, an Object Relation mapper. In the PostsController@`index()`, type: 

```php
// $posts =  Post::all();
// $post = Post::where('title', 'Post Two')->get();
// $posts = Post::orderBy('title', 'desc')->take(2)->get();
$posts = Post::orderBy('title', 'desc')->get();
return view('posts.index')->with('posts', $posts);
```
Eloquent has lots of functions inbuilt and all() is a static method in Eloquent class which Post model extends. 

In the `show($id)`, type 
```php
$post = Post::find($id);
return view('posts.show')->with('post', $post);
```
The $id will be obtained from the url as specified by the routes. 

If you do wanna use SQL queries directly, type `use DB` at the top and then `DB::select('SELECT * FROM posts');`

**Pagination**: in the controller in index(), use query like: `$posts = Post::orderBy('title', 'desc')->paginate(1);`

and on the view page, add pagination like: `{{$posts->links()}}`

# Forms
For building Forms, `return view('posts.create');` in the controller. 

Go to https://laravelcollective.com/docs/ and follow installation instruction: `composer require laravelcollective/html`

Now add provider and alias info in `\config\app.php` as given in documentation. (It may not be needed for higher versions) 

Now go to the view ie. `resources\views\posts\create.blade.php` and add info as required. Make the form action as `{!! Form::open(['action' => 'PostsController@store', 'method'=>'POST']) !!}`

Now go to store(), and add validation:
```php
$this->validate($request, [
    'title'=>'required',
    'body'=>'required'
]);
```
Now we wanna add some messages to reflect the type of error in the form. 

make `resources\views\inc\messages.blade.php`, add all kinds of messages. Then go to main layout template. eg `resources\views\layouts\app.blade.php` and `@include('inc.messages')`

Now make textbox better by adding a WYSIWYG editor. 

[Site](https://docs.ckeditor.com/ckeditor4/latest/guide/dev_package_managers.html#composer) and [GitHub](https://github.com/ckeditor/ckeditor5).

Now go to `resources\views\posts\show.blade.php` and enable html parsing by writing like `{!!$post->body!!}`.

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

In Dashboard controller, `use App\User;` and add
```php
public function index()
{
    $user_id = auth()->user()->id;
    $user = User::find($user_id);
    return view('dashboard')->with('posts', $user->posts);
}
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

# File uploading
In the upload or edit views, maodify the form by adding `'enctype'=>'multipart/formdata'`, eg:

```php
{!! Form::open(['action' => 'PostsController@store', 'method'=>'POST', 'enctype'=>'multipart/formdata']) !!}

<div class="form-group">
    {{Form::file('cover_image')}}
</div>

{{Form::submit('Submit', ['class'=>'btn btn-primary'])}}
{!! Form::close() !!}
```

Now add a new table column in database to associate images to posts.

`php artisan make:migration add_cover_image_to_posts`,  then add to up(): `$table->string('cover_image');` and to down(): `$table->dropColumn('cover_image');`, then `php artisan migrate`


Files get uploaded to `storage/app/public` which isn't accessible by browser so we create symlink to copy this folder to `resources/public`. In terminal, do 
```bash
php artisan storage:link
>>> The [public/storage] directory has been linked.
```
This creates `resources/public/storage` folder which is a symlink. Anything put into `storage/app/public` will show up in `resources/public/storage` as well to be used in the website. 

Now add this to `store()`
```php
$this->validate($request, [
    'title'=>'required',
    'body'=>'required',
    'cover_image'=>'image|nullable|max:1999' //nullable means optional
]);

//Handle file upload
if ($request->hasFile('cover_image')) {
    // get filename with extension
    $filenameWithExtension = $request->file('cover_image')->getClientOriginalName();
    // Get just filename
    $filename = pathinfo($filenameWithExtension, PATHINFO_FILENAME);
    // get just extension
    $extension = $request->file('cover_image')->getClientOriginalExtension();
    // filename to store
    $filenameToStore = $filename.'_'.time().'.'.$extension;
    // upload Image
    $path = $request->file('cover_image')->storeAs('public/cover_images', $filenameToStore);
} else {
    $fileNameToStore = 'noimage.jpg';
}

// Create Post
$post = new Post;
$post->title = $request->input('title');
$post->body = $request->input('body');
$post->user_id = auth()->user()->id;
$post->cover_image = $filenameToStore;
$post->save();
```
Then similarly change `update()` and `destroy()`.

For `destroy()`, import the storage library at the top of the controller via 

```php
use Illuminate\SUpport\Facades\Storage;

public function destroy($id)
{
    $post = Post::find($id);

    //make sure only the post owner can delete the post
    if (auth()->user()->id!==$post->user_id) {
        return redirect('/posts/'.$id)->with('error', 'Unauthorized page');
    }

    //delete the physical image only if the image for the post is not noimage.jpg
    if ($post->cover_image!='noimage.jpg') {
        //Delete Image using the Storage library
        Storage::delete('public/cover_images/'.$post->cover_image);
    }

    //delete the post from database itself
    $post->delete();

    return redirect('/posts')->with('success', 'Post removed');
}
```