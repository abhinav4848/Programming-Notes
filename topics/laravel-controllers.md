## Controllers

### Make a controller
This makes a `app\Http\Controllers\PagesController.php` file. You may need to refresh the sidebar.

```bash
php artisan make:controller PagesController --resource
```
(Add `--resource` after controller name to have the file include all the resource methods available via Laravel, for the CRUD functionality needed to interact with the database. We still gotta make the route ourselves.)
```php
Route::resource('page', 'PagesController');
```

### Use a controller
Inside the class of a particular controller, methods are written like:

`view('pages.index')` means look inside `resources/views/pages` folder for `index.blade.php`

and `view('welcome')` means look inside `resources/views/` folder for `welcome.blade.php`

--> [Eloquent Docs](https://laravel.com/docs/6.x/queries#retrieving-results).
```php
// call a model to use database mechanism (or import DB facade as shown later)
use App\Todo;

// Page call with a variable being passed
public function index()
{
    // get all values from database thanx to the model and "Eloquent". Choose either method.
    // $todos = Todo::all();
    $todos = Todo::orderBy('created_at', 'desc')->get();

    // Another method is like
    // use Illuminate\Support\Facades\DB;
    // $todos = DB::table('todos')->orderBy('created_at', 'desc')->get();

    // var_dump($todos);
    return view('todos.index')->with('todos', $todos);
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