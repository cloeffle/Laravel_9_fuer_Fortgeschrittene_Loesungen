**Übung 8:** Vorbereitungen für den Einsatz der Gates

Wir brauchen ein Anwendungsgebiet für die Gates. 

Erstelle deshalb ein Post-Model samt Migration und Controller. 

Außerdem soll eine One-To-Many-Beziehung zu den Nutzern bestehen. 

Es soll möglich sein, im Post einen Titel, Body und einen booleschen Active-Wert anzugeben. 

Außerdem sollen alle Posts in der View welcome.blade.php angezeigt werden. 

Die angezeigten Posts sollen die Möglichkeit bieten, mithilfe eines Anchor-Tags den Active-Wert zu ändern.

**Lösung:**

Vorbereitungen für den Einsatz der Gates
Wir brauchen ein Anwendungsgebiet für die Gates. 

Hier wird nur ein Post System mit 1:n Beziehung zu den Usern aufgebaut!!

Noch keine Gates!!


1. 
Erstelle deshalb ein Post-Model samt Migration und Controller. 

```
php artisan make:Model Post -crm
```

Das Model Post, der PostController vom Typ Resource und die 
Migration für die posts Tabelle, werden erstellt!

2.
In der Migration  xxx_create_posts_table.php

```
 Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id');
            $table->string('title');
            $table->text('body');
            $table->boolean('active')->default(true);
            $table->timestamps();
        });
```

3.
Migrieren!
``` 
php artisan migrate
```

4.
Im Post-Model App\Models\Post.php

```
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    //
    protected $guarded = [];

    public function user()
    {
		// 2. Außerdem soll eine One-To-Many-Beziehung zu den Nutzern bestehen. 
        return $this->belongsTo("App\Models\User");
    }

    public function toggleActivity()
    {
        $this->active = ! $this->active;
        $this->save();
    }
}
```

5.
Im User-Model App\Models\User.php
die post()-Methode hinzufügen:

```
// uebung_08
    public function post()
    {
        return $this->hasMany('App\Models\Post');
    }
```

6.
Routes in der web.php: 

erstmal als CLOSURE, obwohl der Controller ja da ist!!

```
Route::get('/post/{post}/toggle', function (App\Models\Post $post) {
  
    $post->toggleActivity();

    return redirect('/');
});


// in der welcome-Blade die Posts übergeben!! 
Route::get('/', function () {
      return view('welcome', ['posts' => Post::all()]);
});

```

7.
Es soll möglich sein, im Post einen Titel, Body und einen booleschen Active-Wert anzugeben. 
Außerdem sollen alle Posts in der View welcome.blade.php angezeigt werden. 
Die angezeigten Posts sollen die Möglichkeit bieten, mithilfe eines Anchor-Tags den Active-Wert zu ändern.

In der welcome.blade.php ganz unten ein neues div eifügen:
 
 ```
        <div>
            @foreach($posts as $post)
            <h3>{{$post->title}} | active: @if($post->active) ✅ @else ❌ @endif</h3>
            <p>{{$post->body}}</p>
            <p>created by: {{$post->user->name}}</p>
 			<a href="post/{{$post->id}}/toggle">toggle activity</a>
            @endforeach
        </div>
 ```
 		
**Test:**
In tinker per Hand Post erzeugen oder in einer Route in der web.php:

```
art tinker 
```

1.Post:
```
$user = App\Models\User::find(1); // vorher prüfen, ob dein user die id=1 hat! ansonsten anpassen! ;-)
$user->post()->create(['title' => 'erster Post', 'body' => 'das ist doch toll!']);
```
2.Post:
``` 
$user->post()->create(['title' => 'zweiter Post', 'body' => 'das ist noch toller ;-)']);
```

http://advanced-laravel.test/



