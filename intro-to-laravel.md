# Introduction to Laravel

The following practical instructions are a quick start. You should refer to the Laravel website for comprehensive info on building laravel applications and to look up aspects you find confusing.

## Working with routes

The first thing to understand when working with laravel is how routing works, how the URL in the browser maps to code that will be executed. Open *routes/web.php*. This is where the routes for an application are defined. Add the following routes to this file.

```

Route::get('/', function () {
    return view('welcome');
});

Route::get('list',function()
{
    return "show all films";
});
Route::get('details/{filmId}',function($filmId)
{
    return "details for film ".$filmId;
});
Route::get('create',function()
{
    return "create a film";
});
Route::post('save',function()
{
    return "save the new film";
});
Route::get('deletelist',function()
{
    return "show list of films for deleting";
});
Route::post('delete',function()
{
    return "delete films";
});
```

* Open a web browser enter http://localhost/name-of-your-project/public/list and you should get the 'show all films' message.
* Test the other routes. Here are a couple of things to note:
  * When showing the details for a film, we have to specify the film's id number e.g. http://localhost/laravel-project-name/public/details/4 . The route features a parameter ($filmId) that will be assigned this value.
  * You won't be able to test *save* or *delete* as these require a POST action. Eventually these routes will process form data.
* For more info, read the documentation on routing https://laravel.com/docs/routing

## Controllers

Typically, rather than running simple functions, we would call controller methods to execute actions. So the next thing to do is create a controller. Before we do, we need to know about Artisan.

### Artisan

Artisan is a CLI (Command Line Interface) that comes with Laravel. It is used to automate certain tasks for us. One task it can automate is creating controller classes. Here's how to use Artisan.

* From the XAMPP control panel click 'shell'. A command prompt should appear.
* We need to navigate to the laravel installation directory. To do this enter the following commands:
```
cd htdocs
```

```
cd name-of-your-laravel-project
```

* Next we will instruct artisan to make a controller for us. Enter the following command:

```
php artisan make:controller FilmController
```

* If you look inside the *app/Http/Controllers* folder you should see a *FilmController.php* file.

* Open this file in a text editor. The FilmController class will be empty. Next, add some methods inside the FilmController like the following:

```

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class FilmController extends Controller
{
    function index()
    {
        return "controller - show all films";
    }
    function details($filmId)
    {
        return "controller - details for film ".$filmId;
    }
    function create()
    {
        return "controller - show add film form";
    }
    function save(Request $request)
    {
        return "controller - add a new film";
    }
    function deleteList()
    {
        return "controller - show delete films form";
    }
    function delete(Request $request)
    {
        return "controller - delete films";
    }
}
```

* Now we have a controller set up. We need to map the routes to the controller methods.
* Modify your routes so they look like the following:

```
Route::get('list', 'FilmController@index');
Route::get('details/{filmId}','FilmController@details');
Route::get('create','FilmController@create');
Route::post('save','FilmController@save');
Route::get('deletelist','FilmController@deleteList');
Route::post('delete','FilmController@delete');

```

* Test this works by entering some URLs in the browser. You should now be calling the controller methods when a URL is entered.

## Views
Views are simple PHP pages that are loaded by the controller. Create the following view:

```
<html>
    <head>
        <title>Amazing Film App</title>
    </head>
    <body>
        <h1>View All Films</h1>
    </body>
</html>
```

* In the *resources/views* folder create a new folder and name it 'film'. Save your view as *list-view.php*.
* Modify the *index* method in *FilmController.php* so it loads this view instead of simply returning a string.

```
function index()
    {
        return view('film/list-view');
    }
```


* Test this works. Note, how the title of the page is passed to the view.
* To test your understanding create a simple view for the details controller method.
* Test it works.

## Blade Templates
Blade is a templating engine (https://laravel.com/docs/blade). It allows us to do two things:

* Easily re-use HTML code to create templates, just like when we used *include* in plain PHP.
* Easily integrate PHP into our HTML pages without the need for lots of PHP tags.
* Create the following file
```
<html>
    <head>
        <title>@yield('title')</title>
        <link href="{{asset('/css/style.css')}}" rel="stylesheet" type="text/css">
    </head>
    <body>
        @section('header')
            <ul>
                <li><a href="{{url('list')}}">Read</a></li>
                <li><a href="{{url('create')}}">Create</a></li>
                <li><a href="{{url('deletelist')}}">Delete </a></li>
            </ul>
        @show

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```
* In the *resources/views* folder, create a new folder, name it *layouts*.
* Save the above file as *master.blade.php* in the *layouts* folder
* Next create a child view

```
@extends('layouts.master')

@section('title', 'View All Films')

@section('content')
    <h1>View All Films</h1>
@endsection
```

* Save this as *list-view.blade.php* (in the film folder).
* Test the 'all' route.
* We no longer need *list-view.php*. You can delete it.

Here's how it all works *master.blade.php* is a template that we can base other pages on. *list-view.blade.php* uses this template and adds to it. Here are a couple of things to note:

* In *list-view.blade.php* we need to specify the template to use:

```
@extends('layouts.master')
```

* In *master.blade.php* we use *@yield* to specify content that will be defined by a child view e.g.

```
<title>@yield('title')</title>
```

* In *list-view.blade.php* we use *@section* to add content to the template

```
@section('title', 'View All Films')
```

* We no longer need PHP tags to execute PHP code, instead we can use double curly brackets {{ }} e.g.
```
<li><a href="{{url('deletelist')}}">Delete </a></li>
```
* In this example the url function simply outputs the base url of the site. *deletelist* is added to the end of this url. Use inspect or view source in the browser to see the url that is generated.

* Create additional child views for your other pages, details, create etc.
* Check these work.

## Adding Some CSS

In *master.blade.php* there is already a link to a CSS file.

* Create a simple CSS file and save it in the public/css folder as *style.css*.
* Test this works.

## Working with a Database
The first thing you will need to do is specify your database settings.

* Open the .env file.
* Edit the values for DB_DATABASE, DB_USERNAME and DB_PASSWORD. Before you do this, you might want to set up a new database in XAMPP so you don't get any conflicts with tables from existing projects.

### Creating Database Migrations
Laravel allows us to define database schemas using PHP code. We call these migrations. This makes it very easy to undo changes to the structure of our database and tables, and to completely re-design and re-create our tables without having to import/export .sql files.

We will use Artisan to generate our migrations. Make sure the XAMPP shell is open and you are in the laravel project directory. Enter the following command:

```
php artisan make:migration create_films_table --create=films
```

Look in the *database* folder, in a text editor, open the *create_films_table.php* file. You should see that an id column has been defined for us already.

* Modify this file to add definitions for *title*, *year* and *duration* columns. Your *create_films_table.php* file should then look like the following:

```
use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateFilmsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('films', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title');
            $table->string('year');
            $table->smallInteger('duration');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('films');
    }
}
```

Essentially this code creates a films table for us and defines four columns. The timestamps column is optional but is included by default in every Laravel migration. To run the migration enter the following into the shell:

```
php artisan migrate
```

You might get an error along the lines of 'specified key was too long'. This is a known problem. To fix it open *app/Providers/AppServiceProvider.php*

* Add a use statement at the top
 ```
 use  Illuminate\Support\Facades\Schema;
 ```
 * And change the boot method to
 ```
 public function boot()
     {
         Schema::defaultStringLength(191);
     }
 ```
* Delete any tables that were created in the previous attempt.
* Then try and run your migration again. See here for full explanation https://laracasts.com/discuss/channels/general-discussion/syntax-error-or-access-violation-1071-specified-key-was-too-long

### Seeding the database

In addition to creating migrations we can also seed the database, Laravel will populate our database tables with some sample data. Again, we will use Artisan, this time to create a seeder. Enter the following command into the XAMPP shell window.

```
php artisan make:seeder FilmsTableSeeder
```

Have a look in the *database/seeds* folder and open *FilmsTableSeeder.php* in a text editor.

Modify this file to specify some films for the database. Here's an example:

```
use Illuminate\Database\Seeder;

class FilmsTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        DB::table('films')->insert(['title' => 'Jaws','year' => '1975', 'duration'=>124]);
        DB::table('films')->insert(['title' => 'Winter\'s Bone','year' => '2010', 'duration'=>100]);
        DB::table('films')->insert(['title' => 'Do The Right Thing','year' => '1989', 'duration'=>120]);
    }
}
```


* Next, we need to tell the DatabaseSeeder class to run the *FilmsTableSeeder*. From the seeds folder open *DatabaseSeeder.php*.
* Modify it so that it runs our FilmsTableSeeder. Again, here's an example:

```
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        // $this->call(UsersTableSeeder::class);
        $this->call(FilmsTableSeeder::class);
    }
}
```
* Now, back in the shell, tell Artisan to seed our database table:
```
php artisan db:seed
```
* Check phpmyadmin, you should find that your films table now has three entries.
* That's the database set up

## Models
Again, we will use Artisan to generate our model classes. In the XAMPP shell enter the following:

```
php artisan make:model Film
```

* Have a look in the *app* folder, you should find a Film class has been generated.
* Laravel uses something called Eloquent ORM for object-relational mapping. As we will see, it is based on the Active Record pattern.

>Eloquent ORM works on conventions. If we create a model class called Film, it will assume that this maps to a database table called 'films'. We don't have to do anything else to set up the object-relational mapping.

## Tying everything together

### Displaying all the films
* Open *FilmController.php*.
* Add a use statement to import the Film class.
```
use App\Film;
```
* Modify the index method to use the Film class.

```
function index()
{
    $films = Film::all();
    return view('film/list-view',['films' => $films]);
}
```

* Hopefully, you can see how the active record pattern is working. We want a list of all the films, so we call the *all* method. This has been defined automatically for us by the Eloquent ORM (https://laravel.com/docs/eloquent)
* Also note that the *all* method returns an array of film objects, and this array is passed to the view. Open *list-view.blade.php*. Modify it so that it uses this array of film objects:

```
@extends('layouts.master')

@section('title', 'View All Films')

@section('content')
    <h1>View All Films</h1>
     @foreach ($films as $film)
        <p>
            <a href="{{url('details/'.$film->id)}}">{{$film->title}}</a>        
        </p>
    @endforeach
@endsection
```

* Note the use of @foreach and @if. These are part of the blade templating engine, and make it easy for us to include control structurs in our views.
* Test this works. You should see a list of films.

### Displaying film details
* Next make changes to the details method

```
  function details($filmId)
    {
        $film = Film::find($filmId);
        return view('film/details-view',['film' => $film]);
    }
```

* Again, this uses eloquent, this time the *find* method. Make the following changes to *details.blade.php*

```
@extends('layouts.master')
@section('title', 'Film details')
@section('content')
<h1>{{$film->title}}</h1>
<ul>
<li>Year: {{$film->year}}</li>
<li>Duration: {{$film->duration}}mins</li>
</ul>
@endsection
```

* Test this works

### Adding a new film
* Make the following changes to *create.blade.php*.
```
@extends('layouts.master')
@section('title', 'Create Film')
@section('content')
<form action="{{url('save')}}" method="POST">
{{ csrf_field() }}
<h1>Create New Film</h1>
<label for="title">Title:</label>
<input type="text" id="title" name="title">
<label for="year">Year:</label>
<input type="text" id="year" name="year">
<label for="duration">Duration:</label>
<input type="text" id="duration" name="duration">
<input type="submit" name="submitBtn" value="Add Film">
</form>
@endsection
```
* Again here are a couple of things to note:
  * The action attribute will submit the form to http://localhost/laravel-project-name/public/save . Use inspect element to see how this has been constructed.
  * The form features the line

```
 {{ csrf_field() }}
```
* This injects a hidden field into the form for security purposes to prevent cross-site request forgery. See (https://laravel.com/docs/csrf) for more info. Everytime we build a form in Laravel we need to include a call to this function.

* Next we need to modify the *save* method in *FilmController.php*.

```
function save(Request $request)
{
    $film = new Film();
    $film->title = $request->title;
    $film->year=$request->year;
    $film->duration=$request->duration;
    $film->save();
    return redirect('list');
}
```

* The request object is generated for us automatically by Laravel. We can access form values as properties of this object e.g. $request->title.
* Again, we are using Eloquent ORM, this time to add the new film to the database.
```
$film->save();
```
* Test this works. You should be able to add and view the details of a new film.

### Validating user input
Laravel makes it easy to validate user input. First, change the *save* method to the following:

```
function save(Request $request)
    {
        $this->validate($request, [
            'title' => 'required|max:100',
            'year' => 'required|integer|min:1900',
            'duration' => 'required|integer|min:1|max:300',
        ]);
        $film = new Film();
        $film->title = $request->title;
        $film->year=$request->year;
        $film->duration=$request->duration;
        $film->save();
        return redirect('list');
    }
```

* The validate method is available to all controllers (https://laravel.com/docs/validation). It simply accepts an object to validate and a series of rules.
* It should be fairly obvious what these rules do e.g. the year field must be completed and contain an integer with a value of at least 1900.
* Test this works. If the user enters invalid data they will be re-directed to the form.
* Next we want to display error message for the user. Open *create.blade.php* and make the following changes:

```
@extends('layouts.master')
@section('title', 'Create Film')
@section('content')
@if (count($errors) > 0)
    <div>
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif

<form action="{{url('save')}}" method="POST">
{{ csrf_field() }}
<h1>Create New Film</h1>
<label for="title">Title:</label>
<input type="text" id="title" value="{{old('title')}}" name="title">
<label for="year">Year:</label>
<input type="text" id="year" value="{{old('year')}}" name="year">
<label for="duration">Duration:</label>
<input type="text" id="duration" value="{{old('duration')}}" name="duration">
<input type="submit" name="submitBtn" value="Add Film">
</form>
@endsection
```

* Test this works. Try a variety of different inputs and note the results.
* Any validation errors are automatically generated by Laravel and made available to the view via the *$errors* variable. The code simply tests to see if errors are present and then displays the errors in a list.
* The previously entered form values can be displayed using the *old* function e.g.
```
<input type="text" name="year" value="{{old('year')}}" id="year">
```

### Deleting films
Finally we get on to deleting films. Make the following changes to the *deleteList* and *delete* methods in FilmController.php.

```
function deleteList()
{
    $films = Film::all();
    return view('film/delete-list-view',['films' => $films]);
}
function delete(Request $request)
{
    Film::destroy($request->films);
    return redirect('list');
}
```

* Make the following changes to *delete-list-view.blade.php*.
```
@extends('layouts.master')
@section('title', 'Select Films to Delete')
@section('content')

<form action="{{url('delete')}}" method="POST">
    {{ csrf_field() }}
     @foreach ($films as $film)
        <div>
            <label>{{$film->title}}</label>
            <input type='checkbox' value='{{$film->id}}' name='films[]'/>
        </div>
    @endforeach
    <input type="submit" name="submitBtn" value="Delete Films">
</form>

@endsection
```

* Test this works.
* Hopefully, most of this should make sense. Again, we are using Eloquent to manage the database actions, in this case destroy accepts an array of id numbers that specify rows to delete from a database table.

## On your own

* The above is a whirlwind tour of Laravel. For in-depth explanations see https://laravel.com.
