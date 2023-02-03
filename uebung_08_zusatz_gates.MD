**Übung 8: Zusatzbeispiel mit Gates**

Zusammenfassung des Gate Workflow:

1.   Gate mit dem Key im AuthServiceProvider hinzufügen.

2.   Logik im Closure ? »true« lässt das Gate passieren.

3.   Mit der Gate-Facade und den jeweiligen Methoden oder den Blade Shortcuts das Gate unter Angabe des definierten Gate Key aufrufen.


JETZT gehts los !

**Lösung:**

1. und 2.
Im ASP(AuthServiceProvider) fügen wir ein Gate mit dem Key 'post-activity-umschalten' hinzu. Wir geben eine Instanz des User-Models und des Post-Models weiter. 
Im Closure prüfen wir, ob die user_id gleich der user_id des Posts ist.

in App\Providers in AuthServicePorvider.php (ASP) die boot()-Methode erweitern:

```

// oben in der Datei eintragen!
use Illuminate\Support\Facades\Gate;
use App\Models\User;
use App\Models\Post;

....

public function boot()
{
    $this->registerPolicies();
    
    Gate::define('post-activity-umschalten', function (User $user, Post $post) {
        // Diese Logik entscheidet über kann oder kann nicht!
        return $post->user->is($user); //return $user->id === $post->user_id;
    });
}   
```


3. 
in der welcome.blade.php: 

um den bereits existierenden a href für toggle das @can ..@endcan legen zum Schutz:

```
@can('post-activity-umschalten', $post)

    <a href="post/{{$post->id}}/toggle">toggle activity</a>

@endcan
```

in der Route für das toggle muss noch ein Schutz eingebaut werden:

```
Route::get('/post/{post}/toggle', function (App\Post $post) {
    
    Gate::authorize('post-activity-umschalten', $post);
    // hier erfolgt ein Abbruch 403 'This action is unauthorized' , wenn der User dies niicht darf!
    $post->toggleActivity();

    return redirect('/');
});

```

**Test:**

http://advanced-laravel.test

und manuell einen Post toggeln, der dem angemeldeten User NICHT gehört (hier die id=3):

http://advanced-laravel.test//post/3/toggle


4.
Ein weiteres Gate einfügen, zum ansehen aller Posts:

Im ASP:
```
Gate::define('posts-ansehen', function (User $user) { // Nur angemeldete User können die Posts sehen
Gate::define('posts-ansehen', function (?User $user) { // alle können die Posts sehen

    return true;

});
```

In der View:
```
@can('posts-ansehen')
   @foreach ($posts as $post)
....
   @endforeach
@endcan
```

