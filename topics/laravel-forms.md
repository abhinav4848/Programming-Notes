# Forms
## Laravel Collective
Good for building forms. 
1. Go to https://laravelcollective.com/docs/ and follow installation instruction: `composer require laravelcollective/html`
1. Now add provider and alias info in `\config\app.php` as given in documentation. (It may not be needed for higher versions).

## Create
`return view('posts.create');` in the `create()` controller. 

## Edit
in the `edit()` controller,
```php
$todo = Todo::find($id);
return view('todos.edit')->with('todo', $todo);
```
## Example Form
A simple form will look like
```php
{!! Form::open(['url' => 'contact/submit']) !!}

    <div class="form-group">
        {{ Form::label('name', 'Name') }}
        {{ Form::text('name', 'Default Value', ['class'=>'form-control', 'placeholder'=>'Enter Name']) }}
    </div>

    <div>
        {{ Form::submit('Submit', ['class'=>'btn btn-primary']) }}
    </div>

{!! Form::close() !!}
```

## Create
Check this [documentation](https://laravel.com/docs/4.2/html#opening-a-form).
1. You can either
    ```php
    {!! Form::open(['url' => 'contact/submit']) !!}
    ```
    and then add a route like 
    ```php
    Route::post('/contact/submit', 'MessagesController@submit');
    ```

 1. Or use `resource` route (**Best**)
    ```php
    {!! Form::open(['url' => 'contact']) !!}
    ```
    with a route file having this:
    ```php
    Route::resource('contact', 'MessagesController');
    ```
    This will hit the `store()` function in the controller automatically.

1. Or just include the route in the form itself
    ```php
    {!! Form::open(['action' => 'PostsController@store', 'method'=>'POST']) !!}
    ```
    but specify a route in web.php if you aren't using '--resource'
    ```php
    Route::post('/posts/store', 'PostsController@store');
    ```
    If you change the above '/posts/store', then destination also changes accordingly

Now go to function `store()` in the controller, and add validation and save:
```php
use App\Todo;
.
.
.

$this->validate($request, [
    'title'=>'required',
    'body'=>'required'
]);

// Create ToDo
$todo = new Todo;
$todo->text = $request->input('text');
$todo->body = $request->input('body');
$todo->due = $request->input('due');

$todo->save();

return redirect('/')->with('success', 'Todo Created');
```
### Success message
Now we wanna add some messages to reflect the type of error in the form. 

Make `resources\views\inc\messages.blade.php`, add all kinds of messages: 
```php
@if (count($errors)>0)
    @foreach ($errors->all() as $error)
        <div class="alert alert-danger">
            {{$error}}
        </div>
    @endforeach
@endif

{{-- @if (session('success')) --}}

@if ( Session::has('success') )
    <div class="alert alert-success">
        {{-- {{session('success')}} --}}
        {{ Session::get('success') }}
    </div>
    
@endif

{{-- For error we manually create. That is, something the programmers logic decides should be an error --}}
@if (session('error'))
    <div class="alert alert-danger">
        {{session('error')}}
    </div>
@endif


```
Then go to main layout template. eg `resources\views\layouts\app.blade.php` and `@include('inc.messages')`

### WYSIWYG editor (CKEDITOR)
Docs: [Site](https://docs.ckeditor.com/ckeditor4/latest/guide/dev_package_managers.html#composer) and [GitHub](https://github.com/ckeditor/ckeditor5). Now make textbox better by adding a WYSIWYG editor. 

Now go to `resources\views\posts\show.blade.php` and enable html parsing by writing like `{!!$post->body!!}`.

## Edit/Update
For updating anything, use:
```php
{!! Form::open(['action' => ['TodosController@update', $todo->id], 'method'=>'POST']) !!}
    // and then add this somewhere in the form
    {{ Form::hidden('_method', 'put') }}
{!! Form::close() !!}
```

### Update Controller
```php
public function update(Request $request, $id)
{
    $this->validate($request, [
        'text'=>'required'
    ]);

    // Create ToDo
    $todo = Todo::find($id);
    $todo->text = $request->input('text');
    $todo->body = $request->input('body');
    $todo->due = $request->input('due');

    $todo->save();

    return redirect('/')->with('success', 'Todo Updated');
}
```
## Delete
For deleting, put this in the `show` page itself:
```php
{!! Form::open(['action' => ['TodosController@destroy', $todo->id], 'method'=>'POST', 'class'=>'float-right', 'onsubmit'=>'return confirm("Are you sure?")']) !!}
    {{ Form::hidden('_method', 'DELETE') }}
    {{ Form::bsSubmit('Delete', ['class'=>'btn btn-danger']) }}
{!! Form::close() !!}
```

### Delete Controller
```php
public function destroy($id)
{
    $todo = Todo::find($id);
    $todo->delete();
    return redirect('/')->with('success', 'Todo Deleted!');
}
```

# File uploading
In the upload or edit views, modify the form by adding `'enctype'=>'multipart/formdata'`, eg:

```php
{!! Form::open(['action' => 'PostsController@store', 'method'=>'POST', 'enctype'=>'multipart/formdata']) !!}

<div class="form-group">
    {{Form::file('cover_image')}}
</div>

{{Form::submit('Submit', ['class'=>'btn btn-primary'])}}
{!! Form::close() !!}
```

Now add a new table column in database to associate images to posts.
```bash
php artisan make:migration add_cover_image_to_posts
```
then add to `up()`: `
```php
$table->string('cover_image');
```
and to `down()`: 
```php
$table->dropColumn('cover_image');
```
then `php artisan migrate`.


Files get uploaded to `storage/app/public` which isn't accessible by browser so we create symlink to copy this folder to `resources/public`. 

In terminal, do 
```bash
php artisan storage:link
>>> The [public/storage] directory has been linked.
```
This creates `resources/public/storage` folder which is a symlink. Anything put into `storage/app/public` will show up in `resources/public/storage` as well to be used in the website. (The symlink may break on windows. So delete and recreate)

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

return redirect('/')->with('success', 'Todo Created');
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

## Upload multiple files
In form, add 
```php
{{ Form::file('gallery[]', array('multiple'=>true,'accept'=> 'image/*, audio/*')) }}
```
In PostsController, add
```php
//handle multiple files
if ($request->hasFile('gallery')) {
    foreach ($request->file('gallery') as $file) {
        $file->storeAs('public/cover_images', $file->getClientOriginalName());
    }
}
```

# Custom components
[Laravel Collective Docs](https://laravelcollective.com/docs/6.0/html#custom-components)

1.  Create a form provider
    ```bash
    php artisan make:provider FormServiceProvider
    ```
    at the top, of the provider, add `use Form;` else `Class 'App\Providers\Form' not found` error is thrown.

1. then in the `boot()`, put relevant code:
    ```php
    Form::component('bsText', 'components.form.text', ['name', 'value' => null, 'attributes' => []]);
    Form::component('bsTextarea', 'components.form.textarea', ['name', 'value' => null, 'attributes' => []]);
    Form::component('hidden', 'components.form.hidden', ['name', 'value' => null, 'attributes' => []]);
    Form::component('bsSubmit', 'components.form.submit', ['value' => 'Submit', 'attributes' => []]);
    Form::component('file', 'components.form.file', ['name', 'attributes'=>[]]);
    ```

1. Then include the service provider in `config\app.php` as `App\Providers\FormServiceProvider::class,` otherwise the `bsText` variable will not be found.

1.  * Go to `views/components/form/text.blade.php` and put relevant code.
        ```php
        {{-- // resources/views/components/form/text.blade.php --}}
        <div class="form-group">
            {{ Form::label($name, null, ['class' => 'control-label']) }}
            {{ Form::text($name, $value, array_merge(['class' => 'form-control'], $attributes)) }}
        </div>
        ```
    * Do the same for the second component: Create a file called `textarea.blade.php` and change `text` to `textarea`.
    * For the `submit` button, create another file and put
        ```html
        <!-- We leave the attribute as a varaible cuz we may want the button to be different colour at different places -->
        <div>
            {{ Form::submit($value, $attributes) }}
        </div>
        ```
    * For `input type="hidden"`, make new file and put
        ```php
        {{ Form::hidden($name, $value, $attributes) }}
        ```
    * For file,
        ```php
        <div>
            {{ Form::file($name) }}
        </div>
        ```

1. Now just go to your desired form view and put calling code: 
### Create
```php
{!! Form::open(['action' => 'TodosController@store', 'method'=>'POST']) !!}
// or
{!! Form::open(['url' => 'todo']) !!}

// {{-- custom component --}}
{{ Form::bsText('text', 'Default Value', ['placeholder'=>'Title of the Event']) }}
{{ Form::bsTextarea('body') }}
{{ Form::bsText('due') }}
{{ Form::bsSubmit('Submit', ['class'=>'btn btn-primary']) }}
// {{-- <div>
//     {{ Form::submit('Submit', ['class'=>'btn btn-primary']) }}
// </div> --}}
{!! Form::close() !!}
```

### Update
```php
{!! Form::open(['action' => ['TodosController@update', $todo->id], 'method'=>'POST']) !!}

// {{-- custom component --}}
{{ Form::bsText('text', $todo->text) }}
{{ Form::bsTextarea('body', $todo->body) }}
{{ Form::bsText('due', $todo->due) }}
{{ Form::hidden('_method', 'put') }}
{{ Form::bsSubmit('Submit', ['class'=>'btn btn-primary']) }}
// {{-- <div>
//     {{ Form::submit('Submit', ['class'=>'btn btn-primary']) }}
// </div> --}}
{!! Form::close() !!}
@endsection
```

**Note:** Some of the basic stuff is already pre-made, like [seen here](https://laravelcollective.com/docs/6.0/html#text), but we wrote our own code to simplify our life, else it's ugly like:
```php
// We have to mention the label manually everytime
{{ Form::label('website', 'Company Website') }}

// we have to specify class everytime
{{ Form::text('website','',['class'=>'form-control', 'placeholder'=>'Company Website']) }}
```