# Routes
We can just write html inside routes file itself.
```php
Route::get('/', function () {
    //return view('pages.index');
    return '<h1>Hello World</h1>';
});
```

Can create routes for other pages we add. Eg: If file is located at `\resources\views\pages\about.blade.php`

```php
Route::get('/about', function () {
    return view('pages.about');
});
```

Pass values:
```php
// in this case, both parameters are required to be passed
Route::get('/users/{id}/{name}', function ($id, $name) {
    return 'This is user: '.$name.' with ID: '.$id;
});
```

We normally don't want to return a view from route. But rather, set up a route to a controller which will then return a view..

Writing format: `Controller@method`
```php
Route::get('/', 'PagesController@index');
```

A route like this allows full functional interaction with a controller. See *Make a controller* in Controllers section
```php
Route::resource('page', 'PagesController');
```